- [1. **Open PowerShell as Administrator**:](#1-open-powershell-as-administrator)
- [2. **Check if `ssh-agent` is installed**:](#2-check-if-ssh-agent-is-installed)
- [3. **Start the `ssh-agent` service and set it to start automatically**:](#3-start-the-ssh-agent-service-and-set-it-to-start-automatically)
- [4. **Create or modify your PowerShell profile script**:](#4-create-or-modify-your-powershell-profile-script)
- [5. **Add the following script to your PowerShell profile**:](#5-add-the-following-script-to-your-powershell-profile)
- [6. **Save and close the profile script**:](#6-save-and-close-the-profile-script)
- [7. **Restart PowerShell**:](#7-restart-powershell)

To automatically start `ssh-agent` and load your SSH keys in PowerShell, you can set up a profile script that runs every time you start a new PowerShell session. Here's a step-by-step guide to do this:

### 1. **Open PowerShell as Administrator**:

- Right-click on the PowerShell icon and select "Run as Administrator."

### 2. **Check if `ssh-agent` is installed**:

```powershell
Get-Service ssh-agent
```

If `ssh-agent` is not installed, you can install the OpenSSH client which includes `ssh-agent` by running:

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client*
```

### 3. **Start the `ssh-agent` service and set it to start automatically**:

```powershell
Start-Service ssh-agent
Set-Service -Name ssh-agent -StartupType Automatic
```

### 4. **Create or modify your PowerShell profile script**:

- The profile script is located at `~\Documents\PowerShell\Microsoft.PowerShell_profile.ps1` for a user-specific profile or `C:\Program Files\PowerShell\7\profile.ps1` for an all-users profile.

To create or edit your user-specific profile, run:

```powershell
notepad $PROFILE
```

If the file does not exist, you'll be prompted to create it.

### 5. **Add the following script to your PowerShell profile**:

```powershell
# Start ssh-agent
if (-not (Get-Service ssh-agent).Status -eq 'Running') {
    Start-Service ssh-agent
}

# Add SSH keys to the agent
$sshKeys = @("$env:USERPROFILE\.ssh\id_rsa", "$env:USERPROFILE\.ssh\id_ed25519")

foreach ($key in $sshKeys) {
    if (Test-Path $key) {
        ssh-add $key | Out-Null
    }
}

# Optional: Add a message to indicate the keys were added
Write-Host "SSH keys added to ssh-agent"
```

This script starts the `ssh-agent` service if it's not already running and adds your SSH keys to the agent.

### 6. **Save and close the profile script**:

- Save the file and close Notepad.

### 7. **Restart PowerShell**:

- Close and reopen PowerShell to apply the changes. You should see the message indicating that SSH keys were added to `ssh-agent`.

Now, every time you start a new PowerShell session, `ssh-agent` will start automatically, and your SSH keys will be loaded.
