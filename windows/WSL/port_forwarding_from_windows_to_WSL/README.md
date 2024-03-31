- [Solution](#solution)
- [ChatGPT](#chatgpt)

### Solution

Admin Powershell:

```
netsh interface portproxy add v4tov4 listenport=3000 listenaddress=0.0.0.0 connectport=3000 connectaddress=<WSL_IP_Address>
```

Check:

```
netsh interface portproxy show all
```

Allow through Windows Defender firewall?

```
New-NetFirewallRule -DisplayName "Allow Port 3000" -Direction Inbound -LocalPort 3000 -Protocol TCP -Action Allow
```

Allow ping (from mobile phone to desktop):

```
netsh advfirewall firewall add rule name="Allow Ping" protocol=icmpv4:8,any dir=in action=allow
```

### ChatGPT

To forward all traffic from a specific port (e.g., port 3000) on Windows to the Windows Subsystem for Linux (WSL), you can use the `netsh` command on Windows. This is particularly useful when you're running a server in WSL that you want to access from Windows or other devices in your network. Here’s how you can do it:

1. **Open PowerShell or Command Prompt as Administrator**: To open PowerShell as an administrator, you can search for "PowerShell" in the Start menu, right-click it, and select "Run as administrator". For Command Prompt, search for "cmd" instead.

2. **Add a Port Proxy using `netsh`**: Use the `netsh` command to create a port proxy that forwards traffic from a specific port on Windows to the same or a different port in WSL. You need to know the WSL IP address to do this. You can find the WSL IP address by running the `ip addr` command in your WSL terminal and looking for the `inet` address under the `eth0` interface.

   Here is the basic format of the `netsh` command you will use:

   ```
   netsh interface portproxy add v4tov4 listenport=3000 listenaddress=0.0.0.0 connectport=3000 connectaddress=<WSL_IP_Address>
   ```

   Replace `<WSL_IP_Address>` with your WSL instance's IP address.

3. **Example**: If your WSL IP address is `192.168.1.5`, the command would look like this:

   ```
   netsh interface portproxy add v4tov4 listenport=3000 listenaddress=0.0.0.0 connectport=3000 connectaddress=192.168.1.5
   ```

   This command tells Windows to listen on all interfaces (`listenaddress=0.0.0.0`) for traffic on port 3000 and forward it to port 3000 on the WSL instance with IP `192.168.1.5`.

4. **Check the Port Proxy Rules**: You can check that your port proxy has been added successfully by listing all port proxy rules:

   ```
   netsh interface portproxy show all
   ```

5. **Allow Through Firewall**: Make sure that the Windows Defender Firewall allows incoming connections on port 3000. You can do this through the Windows Defender Firewall settings in the Control Panel, or by running a command like:

   ```
   New-NetFirewallRule -DisplayName "Allow Port 3000" -Direction Inbound -LocalPort 3000 -Protocol TCP -Action Allow
   ```

   in PowerShell as an administrator.

6. **Testing**: After setting up the port forwarding, you can test it by accessing `localhost:3000` from Windows or the Windows machine's IP address from another device on the same network.

Remember, the WSL IP address may change when you restart WSL or your computer. If you encounter issues connecting after a restart, verify the IP address of your WSL instance and update the `netsh` rule if necessary.
