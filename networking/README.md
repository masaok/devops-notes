- [1. Update Your System](#1-update-your-system)
- [2. Install Necessary Packages](#2-install-necessary-packages)
- [3. Enable IP Forwarding](#3-enable-ip-forwarding)
- [4. Configure NAT](#4-configure-nat)
- [5. Save Your IPTABLES Rules](#5-save-your-iptables-rules)
- [6. Configure Network Interfaces](#6-configure-network-interfaces)
- [7. DHCP Server (Optional)](#7-dhcp-server-optional)
- [Final Steps](#final-steps)

Setting up an Ubuntu machine to act as a router involves several steps, including enabling IP forwarding, setting up NAT (Network Address Translation), and configuring your network interfaces. Here's a basic guide to get you started. This guide assumes you have two network interfaces: `eth0` (connected to your WAN or internet source) and `eth1` (connected to your local network).

### 1. Update Your System

First, make sure your system is up-to-date:

```bash
sudo apt-get update && sudo apt-get upgrade
```

### 2. Install Necessary Packages

Install `iptables`, the tool you'll use to set up NAT and firewall rules:

```bash
sudo apt-get install iptables iptables-persistent
```

During the installation of `iptables-persistent`, it will ask if you want to save current rules. You can choose "Yes" or "No" since you haven't created any rules yet.

### 3. Enable IP Forwarding

Edit the `sysctl.conf` file to enable IP forwarding:

```bash
sudo nano /etc/sysctl.conf
```

Find the line `#net.ipv4.ip_forward=1` and remove the `#` to uncomment it. Save and close the file. Apply the changes by running:

```bash
sudo sysctl -p
```

### 4. Configure NAT

Use `iptables` to configure NAT. This step allows devices on your local network to share your Ubuntu machine's internet connection.

```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
```

### 5. Save Your IPTABLES Rules

Save the `iptables` rules so they persist after reboot:

```bash
sudo netfilter-persistent save
```

### 6. Configure Network Interfaces

You need to configure your network interfaces. This can involve setting a static IP for your `eth1` interface and ensuring `eth0` is configured correctly for your WAN connection. This setup can vary based on whether you're using a static IP, DHCP, or another method for your WAN connection.

For a static IP on `eth1`, edit the Netplan configuration or the `/etc/network/interfaces` file, depending on your Ubuntu version and configuration.

Example for Netplan (`/etc/netplan/01-netcfg.yaml`):

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth1:
      addresses:
        - 192.168.1.1/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

Apply the changes with:

```bash
sudo netplan apply
```

### 7. DHCP Server (Optional)

If you want your Ubuntu router to assign IP addresses to your local network devices, you'll need to set up a DHCP server. Install `isc-dhcp-server` and configure it to serve your local network.

Install the DHCP server:

```bash
sudo apt-get install isc-dhcp-server
```

Configure it by editing `/etc/dhcp/dhcpd.conf` to define your subnet and range of IP addresses that can be assigned.

### Final Steps

- Restart networking services or reboot your Ubuntu machine to apply all changes.
- Test your configuration from a client device by attempting to access the internet through your new Ubuntu router.

Remember, this is a basic setup. Depending on your specific requirements (e.g., setting up firewall rules, more complex networking scenarios), you may need to perform additional configurations.
