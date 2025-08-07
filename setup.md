# RHEL 9 Virtual Machine Setup

## 1. VirtualBox Configuration
- VM Name
- Base Memory
- Processors
- Storage Settings
- Network Adapter

## 2. Operating System Installation
- RHEL 9 ISO Used
- Disk Partitioning
- Installation Summary
- Root/User Setup

## 3. Post-Installation System Configuration
- Enabling Repositories
- System Updates
- EPEL Release Installation
- Essential Packages Installed

## 4. Neovim Setup
- Installation Method
- Config Files (init.lua or init.vim)
- Clipboard Setup

## 5. Git & GitHub Setup
- Git Installation
- Creating GitHub Repo
- SSH Key Generation & Setup
- Connecting to GitHub via SSH
- First Commit and Push

## 6. Misc Notes / Issues Encountered
- Drag & Drop from Host to Guest
- Clipboard Issues
- .swp File Conflicts


_____________________________________________________________________________________
-------------------------------------------------------------------------------------


1.    VirtualBox Configuration

- **VM Name:** RHEL9
- **Base Memory:** 2048 MB
- **Processors:** 2
- **Hard Disk:** 20 GB (Dynamically Allocated)
- **ISO Used:** rhel-9.3-x86_64-dvd.iso
- **Network Adapter:** NAT (Adapter 1 Enabled)


2.    Installation Summary

- **Installation Type:** Minimal Install
- **Root Password:** Set
- **User Created:** <user>
- **Hostname:** <user>.<domain>
- **Timezone:** Default (or specify)
- **Post-Install Boot:** Successful (into CLI)


3.    Initial Setup Tasks

- Enabled network using `nmtui`
- Verified internet connectivity with `ping`
- Updated system using:
$ sudo dnf update

- Enabled EPEL repository
$ sudo dnf install epel-release
$ sudo dnf update 

- Installed base admin tools
$ sudo dnf isntall vim git curl wget net-tools htop 


4.    Git Configuration
- Created GitHUB user account
- Installed Git
- Configured user info
$ git config --global user.name "my.name"
$ git config --global user.email "my.email@example.com"

- Generated SSH keys (ed25519) and added to public key in GH
- Initialized local git repo and performed initial commit
- Remote origin set to:
$ git@github.com:user.name/git/repos/setup.git

- Pushed to main branch successfully


5.    Notes
- Drag and drop functionality between Host and Guest VM needed to be configured
- Clipboard support needed for ease of installation and updates
- Neovim installed and functioning
