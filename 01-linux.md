
# Linux Beginner's Guide

## What is Linux?

Linux is a free, open-source operating system kernel created by Linus Torvalds in 1991. It manages hardware resources and enables communication between software and physical devices.

## Linux Architecture

```
┌─────────────────────┐
│   Applications      │
├─────────────────────┤
│   Shell (bash, sh)  │
├─────────────────────┤
│   Kernel            │
├─────────────────────┤
│   Hardware          │
└─────────────────────┘
```

### Components:
- **Hardware**: Physical devices (CPU, RAM, disk)
- **Kernel**: Core managing hardware and processes
- **Shell**: Command interpreter (bash, zsh)
- **Applications**: User programs

## Boot Process

1. **BIOS**: Initializes hardware
2. **Systemd**: Init system (replaces older init)
3. **Systemctl**: Tool to manage services and processes

## Everything is a File or Directory

Linux treats everything as files:
- Regular files
- Directories
- Devices


## Everything Starts with a Process

Processes are running programs. Each has a unique PID (Process ID). The first process is `systemd` (PID 1).

## Linux Distributions

- **Ubuntu**: User-friendly, Debian-based
- **Fedora**: Cutting-edge, Red Hat-sponsored
- **CentOS**: Stable, enterprise-focused
- **Arch**: Minimal, customizable
- **Debian**: Stable foundation

## Filesystem Hierarchy

```
/            Root directory
├── /bin     Essential binaries
├── /home    User home directories
├── /etc     Configuration files
├── /var     Variable data (logs)
├── /usr     User programs
├── /tmp     Temporary files
├── /boot    Boot files
└── /root    Root user home
```
