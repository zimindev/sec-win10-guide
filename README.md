# 🛡️ Windows 10 Security Guide

A practical security guide for hardening **Windows 10** against malware, unauthorized access, data theft, and common network attacks.

> ⚠️ **Important:** Windows 10 reached end of support on **October 14, 2025**. For systems that must remain on Windows 10, apply these recommendations and use an appropriate supported security/patching strategy. Where possible, upgrade to a supported Windows version.

---

## 🔄 1. Keep Windows Updated

Install all available security updates.

Open:

**Settings → Update & Security → Windows Update**

Or run:

```powershell
Start-Process "ms-settings:windowsupdate"
```

Check for updates regularly.

> 🔐 Security updates are one of the most important protections against known vulnerabilities.

---

## 👤 2. Use a Standard User Account

Avoid using an administrator account for everyday activities.

Check your account type:

```powershell
net user $env:USERNAME
```

For daily browsing, email, and normal applications, use a **Standard User** account whenever possible.

Use administrator privileges only when required.

---

## 🔑 3. Use a Strong Windows Password

Use a long and unique password.

Good examples:

```text
Raven!Cloud-72-Matrix
BlueForest!91-Window
Orbit#54-Lemon-Quartz
```

Avoid:

```text
password
123456
qwerty
admin
windows10
YourName123
```

### Recommended

* At least **14–16 characters**
* Use upper/lowercase letters
* Include numbers
* Include symbols
* Never reuse passwords
* Never share your password

Consider using a password manager such as **KeePassXC**.

---

## 🔐 4. Enable Windows Hello

If your hardware supports it, configure:

* PIN
* Fingerprint
* Facial recognition
* Security key

Go to:

**Settings → Accounts → Sign-in options**

A Windows Hello PIN is device-specific and can provide a convenient alternative to repeatedly entering your Microsoft account password.

---

## 🛡️ 5. Keep Microsoft Defender Enabled

Do not disable Microsoft Defender without a strong reason.

Check Defender status:

```powershell
Get-MpComputerStatus
```

You should normally see protections such as:

```text
AntivirusEnabled
RealTimeProtectionEnabled
BehaviorMonitorEnabled
IoavProtectionEnabled
```

Update Defender signatures:

```powershell
Update-MpSignature
```

---

## 🚨 6. Enable Real-Time Protection

Open:

**Windows Security → Virus & threat protection → Manage settings**

Enable:

* Real-time protection
* Cloud-delivered protection
* Automatic sample submission
* Tamper Protection

These features provide additional protection against malware and suspicious activity.

---

## 🧠 7. Enable SmartScreen

SmartScreen helps protect against malicious websites, downloads, and applications.

Open:

**Windows Security → App & browser control**

Enable reputation-based protection where available.

Recommended protections include:

* Check apps and files
* SmartScreen for Microsoft Edge
* Potentially unwanted app blocking

---

## 🔥 8. Enable Windows Firewall

Check firewall status:

```powershell
Get-NetFirewallProfile | Select Name, Enabled
```

Enable all profiles if they are disabled:

```powershell
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
```

Check individual profiles:

```powershell
Get-NetFirewallProfile
```

Do not disable the firewall simply because you are connected to a trusted network.

---

## 🌐 9. Secure Network Profiles

Check your current network profile:

```powershell
Get-NetConnectionProfile
```

For untrusted networks, use:

```text
Public
```

Avoid unnecessarily setting unknown networks as:

```text
Private
```

Public networks should have more restrictive network discovery and sharing settings.

---

## 📡 10. Disable Network Discovery When Not Needed

Go to:

**Control Panel → Network and Sharing Center → Change advanced sharing settings**

Disable:

* Network discovery
* File and printer sharing

when they are not required.

This reduces unnecessary exposure on public or untrusted networks.

---

## 📁 11. Secure File Sharing

Avoid sharing entire drives such as:

```text
C:\
D:\
```

Prefer sharing only specific directories.

Review current SMB shares:

```powershell
Get-SmbShare
```

Remove unnecessary shares:

```powershell
Remove-SmbShare -Name "ShareName"
```

> ⚠️ Be careful when modifying existing shares on systems used by other users.

---

## 🔒 12. Disable SMBv1

SMBv1 is an obsolete protocol and should generally not be used.

Check SMB1:

```powershell
Get-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

Disable it:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```

Restart Windows afterward if requested.

---

## 🔑 13. Disable Unnecessary Remote Access

If you do not use Remote Desktop, disable it.

Check the configuration:

```powershell
Get-ItemProperty "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name fDenyTSConnections
```

Remote Desktop should not be exposed directly to the public Internet.

If remote access is required:

* Use a VPN
* Restrict firewall rules
* Use strong authentication
* Keep the system patched
* Avoid exposing TCP/3389 directly to the Internet

---

## 🔐 14. Secure Remote Desktop

If RDP is required:

**Settings → System → Remote Desktop**

Enable:

```text
Require devices to use Network Level Authentication
```

Use strong passwords and restrict access to trusted networks or VPN connections.

---

## 💾 15. Encrypt Your Disk

Use **BitLocker** where supported.

Check BitLocker status:

```powershell
manage-bde -status
```

Encryption protects your files if the physical device is lost or stolen.

### Important

Store your BitLocker recovery key securely.

Do not keep the only copy on the same computer.

---

## 🧰 16. Enable Secure Boot

Check Secure Boot:

```powershell
Confirm-SecureBootUEFI
```

Expected result:

```text
True
```

Secure Boot helps prevent unauthorized bootloaders and certain types of pre-OS malware.

---

## 🧪 17. Avoid Pirated Software

Avoid:

* Cracked applications
* Keygens
* Unknown activators
* Pirated games
* Modified installers
* Random executable files

These are common sources of:

* Trojans
* Stealers
* Ransomware
* Remote access malware
* Credential theft

Download software from trusted official sources whenever possible.

---

## 📥 18. Be Careful With Downloads

Do not automatically trust files such as:

```text
.exe
.msi
.bat
.cmd
.ps1
.vbs
.js
.scr
```

Especially when they arrive through:

* Email
* Discord
* Telegram
* Unknown websites
* File-sharing services
* Unknown USB drives

Verify the source before executing anything.

---

## 📧 19. Secure Email Attachments

Treat unexpected attachments as suspicious.

Be especially careful with:

```text
.exe
.iso
.zip
.rar
.docm
.xlsm
.js
.vbs
```

Never enable Office macros simply because a document asks you to.

---

## 🧹 20. Remove Unnecessary Software

Review installed applications regularly.

Open:

**Settings → Apps → Apps & features**

Remove applications you no longer use.

You can also inspect installed software with PowerShell:

```powershell
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |
Select DisplayName, DisplayVersion
```

---

## 🚀 21. Review Startup Applications

Check startup programs:

**Task Manager → Startup**

Disable applications that:

* You do not recognize
* You do not need
* Start unnecessarily
* Consume resources

Do not disable security components unless you know exactly what they do.

---

## 🔍 22. Review Running Processes

Use:

```powershell
Get-Process
```

For network-related processes:

```powershell
Get-NetTCPConnection
```

Investigate unusual applications, unexpected network connections, or unknown processes.

---

## 🌐 23. Use Secure DNS

Consider using a reputable DNS provider.

Examples:

```text
1.1.1.1
1.0.0.1
```

or:

```text
8.8.8.8
8.8.4.4
```

DNS security does not replace antivirus, firewall, or endpoint protection.

---

## 🔒 24. Use HTTPS

When browsing the web, prefer:

```text
https://
```

Avoid entering passwords or sensitive information on websites that do not use HTTPS.

---

## 🧑‍💻 25. Secure PowerShell

Avoid blindly executing commands copied from the Internet.

Never run commands such as:

```powershell
irm example.com/script.ps1 | iex
```

unless you completely understand and trust the source.

A remote script can execute arbitrary commands with your privileges.

---

## 📜 26. Enable Auditing

For security-sensitive systems, Windows auditing can help identify suspicious activity.

Review:

**Event Viewer → Windows Logs → Security**

Useful events include:

```text
4624  Successful logon
4625  Failed logon
4634  Logoff
4648  Explicit credential logon
4672  Special privileges assigned
4688  New process created
```

---

## 🕵️ 27. Monitor Failed Logins

Repeated failed authentication attempts can indicate password guessing or unauthorized access.

Use:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName='Security'
    Id=4625
} -MaxEvents 20
```

Review unexpected login attempts.

---

## 💽 28. Make Regular Backups

Use the **3-2-1 backup principle**:

```text
3 copies of your data
2 different types of storage
1 copy stored offline/off-site
```

Example:

```text
PC
 ├── Internal SSD
 ├── External HDD
 └── Cloud Backup
```

Keep at least one backup disconnected from the computer to reduce ransomware risk.

---

## 🦠 29. Protect Against Ransomware

Enable protections available through:

**Windows Security → Virus & threat protection → Ransomware protection**

Consider using:

```text
Controlled folder access
```

Test your backups regularly.

A backup that has never been restored is not a proven backup.

---

## 🔌 30. Be Careful With USB Devices

Do not connect unknown USB drives to your primary computer.

For unknown devices:

* Scan them
* Avoid executing files automatically
* Disable unnecessary AutoPlay behavior
* Treat unknown USB storage as untrusted

---

## 📱 31. Secure Your Browser

Recommended practices:

* Keep the browser updated
* Remove unnecessary extensions
* Use HTTPS
* Block suspicious pop-ups
* Avoid unknown extensions
* Do not save passwords on shared computers
* Review browser permissions

Periodically inspect installed extensions.

---

## 🍪 32. Review Browser Permissions

Check which websites have access to:

* Camera
* Microphone
* Location
* Notifications
* Clipboard

Remove permissions you do not need.

---

## 🔐 33. Use Multi-Factor Authentication

Enable MFA/2FA for important accounts:

* Microsoft account
* Email
* GitHub
* Cloud storage
* Banking
* Social networks
* VPN
* Password manager

Prefer authenticator applications or hardware security keys over SMS when possible.

---

## 🗝️ 34. Use a Password Manager

Do not reuse the same password across multiple services.

A password manager can generate unique passwords such as:

```text
vT8!qL2#nP7@xZ4$wK9
```

Use one unique password per account.

---

## 🧱 35. Minimize Administrator Privileges

Follow the principle:

> **Use the minimum privileges required to perform a task.**

Avoid running everyday applications as Administrator.

Do not permanently disable UAC.

---

## ⚙️ 36. Keep UAC Enabled

Check:

**Control Panel → User Accounts → Change User Account Control settings**

Recommended:

```text
Always notify
```

or the default secure setting.

UAC provides an additional barrier against unauthorized administrative actions.

---

## 🧯 37. Create a Recovery Plan

Prepare for system failure.

Keep:

* Windows installation/recovery media
* BitLocker recovery key
* Backup copies
* Important account recovery codes
* Network configuration information
* Emergency contact information

Do not store every recovery method exclusively on the PC itself.

---

## 🚨 38. What To Do After a Suspected Infection

If you suspect malware:

### 1. Disconnect from the network

Disable:

```text
Wi-Fi
Ethernet
Bluetooth
```

if appropriate.

### 2. Do not log into important accounts

Avoid entering:

* Banking passwords
* Email passwords
* Password manager passwords

### 3. Run Microsoft Defender scans

Use Windows Security and perform an appropriate scan.

### 4. Investigate from a trusted device

Change important passwords from a clean device if credential theft is suspected.

### 5. Restore from a known-good backup

For serious compromise, a clean OS reinstall may be safer than attempting to manually remove every malicious component.

---

# ✅ Windows 10 Security Checklist

```text
[ ] Windows security updates installed
[ ] Microsoft Defender enabled
[ ] Real-time protection enabled
[ ] SmartScreen enabled
[ ] Firewall enabled
[ ] Strong unique password configured
[ ] Standard user account used for daily work
[ ] UAC enabled
[ ] BitLocker enabled where available
[ ] Secure Boot enabled
[ ] SMBv1 disabled
[ ] Unnecessary services disabled
[ ] Unnecessary software removed
[ ] Startup applications reviewed
[ ] Remote Desktop disabled if unused
[ ] RDP protected if required
[ ] Network discovery disabled on untrusted networks
[ ] Browser extensions reviewed
[ ] MFA enabled
[ ] Password manager configured
[ ] Important data backed up
[ ] Offline/off-site backup available
[ ] Recovery keys stored securely
[ ] Security logs periodically reviewed
```

---

## 🔗 Useful Resources

* [Microsoft Windows Security](https://support.microsoft.com/windows/stay-protected-with-windows-security-2ae0365d-0ada-cd3d-0f5d-6f1e-9e3e2c8d5d4f?utm_source=chatgpt.com)
* [Microsoft Windows 10 End of Support](https://www.microsoft.com/windows/windows-10-specifications?utm_source=chatgpt.com)
* [Microsoft BitLocker Documentation](https://learn.microsoft.com/windows/security/operating-system-security/data-protection/bitlocker/?utm_source=chatgpt.com)
* [Microsoft Defender Documentation](https://learn.microsoft.com/defender/?utm_source=chatgpt.com)
* [Microsoft Security Documentation](https://learn.microsoft.com/security/?utm_source=chatgpt.com)
🌐 [zimin.dev](https://zimin.dev?utm_source=chatgpt.com)
