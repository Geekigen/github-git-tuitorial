# SSH Setup Guide (Linux, Mac, Windows)

SSH (Secure Shell) lets you securely connect to and control a remote computer/server from your own machine. This guide shows how to install SSH and generate SSH keys on Linux, macOS, and Windows.

---

## 1. Linux

Most Linux distros already have the SSH client installed. To check:

```bash
ssh -V
```

If it's missing, install it:

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install openssh-client

# Fedora/RHEL/CentOS
sudo dnf install openssh-clients

# Arch
sudo pacman -S openssh
```

If you also want your Linux machine to **accept** SSH connections (act as a server):

```bash
# Debian/Ubuntu
sudo apt install openssh-server
sudo systemctl enable --now ssh

# Fedora/RHEL
sudo dnf install openssh-server
sudo systemctl enable --now sshd
```

### Generate an SSH key
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
Press Enter to accept the default file location, and optionally set a passphrase.

### Connect to a server
```bash
ssh username@server_ip
```

---

## 2. macOS

SSH is built into macOS — no installation needed. Just open **Terminal** (Cmd + Space, type "Terminal").

Check it's available:
```bash
ssh -V
```

### Generate an SSH key
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
Press Enter to accept the default location, and optionally set a passphrase.

### Add your key to the macOS keychain (so you don't retype the passphrase)
```bash
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

### Connect to a server
```bash
ssh username@server_ip
```

---

## 3. Windows

Windows 10/11 come with a built-in OpenSSH client, but it may need to be turned on.

### Check/install via Settings
1. Open **Settings** → **Apps** → **Optional Features**
2. Click **Add a feature**
3. Search for **OpenSSH Client**, install it (and **OpenSSH Server** too if you want to receive connections)

### Or install via PowerShell (run as Administrator)
```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

### Generate an SSH key
Open **PowerShell** or **Command Prompt**:
```powershell
ssh-keygen -t ed25519 -C "your_email@example.com"
```
Press Enter to accept the default location, and optionally set a passphrase.

### Connect to a server
```powershell
ssh username@server_ip
```

> **Alternative:** Many Windows beginners use **PuTTY** instead of the built-in client — it has a simple graphical interface. Download it from putty.org if you prefer a GUI over the command line.

---

## Quick Reference: Common SSH Commands

| Action | Command |
|---|---|
| Check SSH version | `ssh -V` |
| Generate a key pair | `ssh-keygen -t ed25519 -C "email"` |
| Connect to a server | `ssh username@server_ip` |
| Connect on a custom port | `ssh username@server_ip -p 2222` |
| Copy your public key to a server | `ssh-copy-id username@server_ip` |
| View your public key | `cat ~/.ssh/id_ed25519.pub` (Linux/Mac) or `type $env:USERPROFILE\.ssh\id_ed25519.pub` (Windows) |

---

## Tips for Beginners
- Your **private key** (`id_ed25519`) stays on your computer — never share it.
- Your **public key** (`id_ed25519.pub`) is what you share/copy to servers.
- Use `ssh-copy-id` (Linux/Mac) to easily copy your public key to a remote server for passwordless login.
- Always set a passphrase on your key for extra security.
