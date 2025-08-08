# RHEL 9 Virtual Machine Setup

#### This guide outlines the setup of a RHEL 9 virtual machine using VirtualBox, including post-install configuration and Git setup.
---


## 1. Operating System Installation
- RHEL 9 ISO Used
- Disk Partitioning
- Installation Summary
- Root/User Setup

## 2. Post-Installation System Configuration
- Enabling Repositories
- System Updates
- EPEL Release Installation
- Essential Packages Installed

## 3. Neovim Setup
- Installation Method
- Config Files (init.lua or init.vim)
- Clipboard Setup

## 4. Git & GitHub Setup
- Git Installation
- Creating GitHub Repo
- SSH Key Generation & Setup
- Connecting to GitHub via SSH
- First Commit and Push

## 5. Misc Notes / Issues Encountered
- Drag & Drop from Host to Guest
- Clipboard Issues
- .swp File Conflicts


_____________________________________________________________________________________
-------------------------------------------------------------------------------------


###     VirtualBox Configuration

- **VM Name:** RHEL9
- **Base Memory:** 2048 MB
- **Processors:** 2
- **Hard Disk:** 20 GB (Dynamically Allocated)
- **ISO Used:** rhel-9.3-x86_64-dvd.iso
- **Network Adapter:** NAT (Adapter 1 Enabled)


### 1.    Installation Summary

- **Installation Type:** Minimal Install
- **Root Password:** Set
- **Root Account:** Locked
- **User Created:** `adduser -m user`
- **Hostname:** system.domain
- **Timezone:** Default (or specify)
- **Post-Install Boot:** Successful (into CLI)


### 2.    Initial Setup Tasks

- Enable network settings using `nmtui`
- Verify internet connectivity with `ping`
- Update system using:
```
$ sudo dnf clean all
$ sudo dnf update
```
- Enable EPEL repository
```
$ sudo dnf install epel-release
$ sudo dnf update 
```
- Install essential tools
```
$ sudo dnf install vim git curl wget net-tools htop 
```

### 3.    Git Configuration
- Create GitHUB user account

- Install Git
```
$ sudo dnf install git
$ git --version
```  
- Configure user info
```
$ git config --global user.name "my.name"
$ git config --global user.email "my.email@example.com"
```
- Generate SSH keys (ed25519) and add to public key in GH
```
$ sudo ssh-keygen -t ed25519 -C user@example.com
```
- Start the agent & add your key
```
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_ed25519
```
- Copy your key
```
$ cat ~/.ssh/id_ed25519.pub  # output (starts with ssh-ed25519...)
```
- Add your public key to GitHub or GitLab
```
    For GitHub:
      Go to GitHub > Settings > SSH and GPG keys
      Click New SSH key
      Paste the public key
      Give it a name (e.g., “RHEL 9.4 VM”)

    For GitLab:
      User icon > Preferences > SSH Keys
      Paste the key there
```
- Test your SSH connection
```
$ ssh -T git@github.com
```
- Initialize local git repo and perform initial commit
```
$ mkdir my-project && cd my-project
$ git init
$ echo "# My Project" > README.md
$ git add README.md
$ git commit -m "Initial commit note"
```
- Set Remote origin:
```
$ git remote add origin git@github.com:username/git/repos/setup.git
```
- Push to (main|master) branch
```
$ git push -u origin main|master
```

### 4. Summary
| Task                         |   Status                              |
| ---------------------------- | --------------------------------------|
| Git installed                |   Recommended                         |
| Name & email configured      |   Recommended                         |
| Editor, colors, branch set   |   Optional, recommended               |
| SSH key generated            |   Optional but secure                 |
| SSH key added to platform    |   If using GitHub/GitLab              |
| First repo initialized       |   For testing/config confirmation     |

## 5. Notes
- Drag and Drop functionality between Host and Guest VM need to be configured
- Clipboard support needed for ease of installation and updates
- Neovim installed and functioning
