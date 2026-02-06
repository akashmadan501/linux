
# File Management

## Make Directory
```bash
mkdir <dirname>
mkdir -p <path>
```

## Create File
```bash
touch <filename>
```

## Edit File
```bash
vim <filename>
```

## Navigation
```bash
cd ..        # Go one directory back
cd ~         # Go to home directory
ls           # List all directories/files
```

---

# SSH (Secure Shell)

SSH is a program for logging into a remote machine and executing commands remotely. It provides secure encrypted communications between two machines (client & host) using port 22.

## Key Distribution

**Local to Server:**
- Client (local) needs a private key
- Server (remote) needs a public key

**Server to Server:**
- Server A needs a private key
- Server B needs a public key

## SSH Connection Flow

```
LOCAL TO SERVER:
┌──────────────┐                    ┌──────────────┐
│   Client     │                    │   Server     │
│  (Local)     │                    │  (Remote)    │
│ Private Key  │◄──────Port 22─────►│ Public Key   │
│              │   Encrypted        │              │
└──────────────┘                    └──────────────┘

SERVER TO SERVER:
┌──────────────┐                    ┌──────────────┐
│  Server A    │                    │  Server B    │
│ Private Key  │◄──────Port 22─────►│ Public Key   │
│              │   Encrypted        │              │
└──────────────┘                    └──────────────┘
```

## Generate SSH Keys

1. **Change to .ssh directory:**
    ```bash
    cd ~/.ssh
    ```

2. **Generate key:**
    ```bash
    ssh-keygen
    ```

3. **List files:**
    ```bash
    ls
    ```

4. **View public key:**
    ```bash
    cat id_ed25519.pub
    ```

5. **Paste public key to server's ~/.ssh directory**

6. **Connect to server:**
    ```bash
    ssh -i 'private_key_path' user@DNS
    ssh -i "/home/user/Desktop/key.pem" ubuntu@ec2-3-129-67-210.us-east-2.compute.amazonaws.com
    ```

## View SSH Configuration
```bash
cat /etc/ssh/sshd_config
```

---

# Package Installer

`apt` provides a high-level command-line interface for the package management system.

| OS | Package Manager |
|---|---|
| Ubuntu | apt |
| RedHat | rpm, dnf |
| CentOS | yum |

- **update:** Download packages
- **upgrade:** Install downloaded packages

```bash
apt install package_name
```

---

# Process Management

The first system process is `systemd` (PID 1).

```bash
systemctl status service_name      # Get service status
systemctl stop service_name        # Stop service
systemctl start service_name       # Start service
```

---

# Journalctl

View logs for services:
```bash
journalctl -u service_name
journalctl -u nginx
```

---

# User Management

## Add User
```bash
sudo useradd -m <username>
sudo useradd -m <username> -s /usr/bin/bash
```
- `-m`: Create home directory
- `-s`: Set login shell

## Set Password
```bash
sudo passwd <username>
```

## Delete User
```bash
sudo userdel <username>
```

## Switch User
```bash
su <username>
whoami    # See current user
```

## Group Management
```bash
sudo groupadd <groupname>                   # create new group
cat /etc/group                              # View groups
sudo gpasswd -a <username> <groupname>      # add user to group
sudo gpasswd -a ubuntu docker               # Run Docker without sudo
```

## Modify User
```bash
sudo usermod -l <new_username> <username>   # Change login name
sudo usermod -s <shell> <username>          # Change shell
sudo usermod -g <groupname> <username>      # Change primary group
sudo usermod -aG <groupname> <username>     # Add to group
sudo usermod -aG docker ubuntu              # Run Docker without sudo
```

## Change File Ownership
```bash
sudo chown <username> <file_name>
```

## Change Group and Ownership of File
```bash
sudo chown user:group filename
```

## Change Group
```bash
chgrp group filename
```

---

# Permission Management

File/directory permissions format:

```
-rwxrwxr-x
│││││││││
││││││││└─ Others: execute
│││││││└── Others: write
││││││└─── Others: read
│││││└──── Group: execute
││││└───── Group: write
│││└────── Group: read
││└─────── Owner: execute
│└──────── Owner: write
└───────── Owner: read (- for file, d for directory)
```

## Permission Octal Values

| Octal | r | w | x |
|-------|---|---|---|
| 0     | 0 | 0 | 0 |
| 1     | 0 | 0 | 1 |
| 2     | 0 | 1 | 0 |
| 3     | 0 | 1 | 1 |
| 4     | 1 | 0 | 0 |
| 5     | 1 | 0 | 1 |
| 6     | 1 | 1 | 0 |
| 7     | 1 | 1 | 1 |

Example: `rwx rw- r--` = `764`

- 1 - x
- 2 - w
- 4 - r

- 1 - x
- 2 - w
- 3 - -wx
- 4 - r--
- 5 - r-x
- 6 - rx-
- 7 - rwx

## Change Permissions
```bash
chmod 777 <filename>
```

---

# Additional Notes

## Pipe Operator
Pipe operator (`|`) passes output of one command as input to another

## /etc/shadow
Stores encrypted user password

## Disk Usage
```bash
df -h
```
Used to see disk usage in human readable format

