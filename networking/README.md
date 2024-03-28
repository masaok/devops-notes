- [1. Install DHCP Server](#1-install-dhcp-server)
- [2. Configure Network Interfaces](#2-configure-network-interfaces)
- [3. Enable IP Forwarding](#3-enable-ip-forwarding)
- [4. Set Up NAT](#4-set-up-nat)
- [5. Configure DHCP Server](#5-configure-dhcp-server)
- [6. Start DHCP Server](#6-start-dhcp-server)
- [7. Verify Configuration](#7-verify-configuration)

To use an Ubuntu Linux machine with two Ethernet interfaces as a router and set up a local DHCP server, you'll need to follow a series of steps. This will involve configuring the network interfaces, enabling IP forwarding, setting up NAT (Network Address Translation), and configuring a DHCP server. Below are the general steps to accomplish this. Please note that you might need to adjust commands and configuration files based on your specific Ubuntu version and network setup.

### 1. Install DHCP Server

First, you need to install the DHCP server package. Open a terminal and run:

```bash
sudo apt update
sudo apt install isc-dhcp-server
```

### 2. Configure Network Interfaces

Identify your network interfaces with `ip addr` or `ifconfig` (you might need to install `net-tools` for `ifconfig`). Usually, they are named something like `eth0`, `eth1`, `wlan0`, etc. Decide which one will be connected to your upstream internet (WAN) and which one will serve your local network (LAN).

Edit `/etc/netplan/01-netcfg.yaml` or the appropriate netplan configuration file for your system, to configure your interfaces. For example:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.1/24]
    eth1:
      dhcp4: true
```

Here, `eth0` is set with a static IP and will be used for the LAN side, while `eth1` gets its IP via DHCP from your ISP.

After making changes, apply them with `sudo netplan apply`.

### 3. Enable IP Forwarding

Edit `/etc/sysctl.conf` and uncomment or add the following line:

```conf
net.ipv4.ip_forward=1
```

Apply the changes with `sudo sysctl -p`.

### 4. Set Up NAT

Use `iptables` to set up NAT. This example assumes `eth1` is your WAN interface:

```bash
sudo iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE
sudo apt install iptables-persistent
```

The `iptables-persistent` package will help keep your NAT configuration across reboots.

### 5. Configure DHCP Server

Edit `/etc/dhcp/dhcpd.conf` to configure your DHCP server. Add the following configuration for your LAN network (adjust according to your network settings):

```conf
subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.10 192.168.1.100;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  option routers 192.168.1.1;
  option broadcast-address 192.168.1.255;
  default-lease-time 600;
  max-lease-time 7200;
}
```

Replace `192.168.1.1` with your LAN interface's IP address, and adjust the `range` to fit your needs.

### 6. Start DHCP Server

Specify the interface on which the DHCP server should listen by editing `/etc/default/isc-dhcp-server` and setting the `INTERFACESv4` variable to your LAN interface, for example:

```bash
INTERFACESv4="eth0"
```

Restart the DHCP server to apply changes:

```bash
sudo systemctl restart isc-dhcp-server
```

### 7. Verify Configuration

Ensure the DHCP server is running correctly with:

```bash
sudo systemctl status isc-dhcp-server
```

Check your IP forwarding and NAT setup by connecting a device to your LAN and verifying it receives an IP address and can access the internet.

This setup turns your Ubuntu machine into a basic router with DHCP capabilities. Depending on your requirements, you might want to explore additional configurations, such as setting up firewall rules, DHCP reservations, or more complex networking scenarios.
