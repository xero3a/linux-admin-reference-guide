# User and Permissions Management

## Best Practices for Users, Groups, Sudoers, and File Permissions

This section outlines essential system administration practices for managing users and groups, configuring sudo privileges securely, and setting appropriate file and directory permissions. Following these guidelines helps ensure system access is controlled, auditable, and aligned with security standards.

### User Management

- Use `useradd` or `adduser` to create users with specific home directories and shells.
- Set strong passwords with `passwd` and encourage password complexity.
- Lock inactive accounts with `usermod -L` or remove them if no longer needed.
- Use `/etc/login.defs` to configure password aging and expiration policies.
- Use `chage` to view and modify user account expiration and aging details.

### Special Permission Bits

- **Setuid (Set User ID)**:  
  Allows users to execute a file with the file owner’s privileges.  
  Example:  
  ```bash
  sudo chmod u+s /path/to/executable
- Setgid (Set Groupd ID)
 sudo chmod g+s /path/to/dir
- Sticky Bit
Restricts file deletion within a directory to the file's owner or root
sudo chmod +t /path/to/dir


### Password Policies and Expiration

Managing password aging helps enforce regular password changes, improving security.

- View current settings for a user:
  ```bash
  chage -l johndoe
- Password Expiration Policy
sudo chage -M 90 user
- Set warning period before   expiration
sudo chage -W 7 user
- Lock account # days before expiration
sudo chage -I 14 user
- System-wide password policies
  a. /etc/login.defs
sudo nano /etc/login.defs
~ PASS_MAX_DAYS 90
~ PASS_MIN_DAYS 0
~ PASS_WARN_AGE 7



### Securing User Data with Permissions


### Group Management ###
- Creating new groups
sudo groupadd group_name
- Add an Existing user to a group
sudo usermod -aG group_name user
- Veryify group membership
groups user
- Changing users primary gorup
sudo usermod -g group_name user
- Remove a group
sudo groupdel group_name



### File and Permissions Management ###
- Changing File Ownership
sudo chown username file.txt
sudo chown user:group file.txt
- Changing Group Ownership
sudo chgrp groupname file.txt
- Changing Permissions
chmod u+x script.sh	# gives user execute permissions
chmod g-w file.txt	# remove write from group
chmod o=r file.txt	# set "others" to read-only
- Numeric Notation
chmod 755 script.sh	# rwxr-xr-x
chmod 644 file.txt	# rw-r--r--
- Setting default Permissions
umask
- Setting a new umask (current session)
umask 027

# Special Permissions
- SetGID on a directory
chmod g+s /shared_dir
chmod +t /shared_dir


### Sudoers Configuration ###
- Editing /etc/sudoers
sudo visudo
- Granting sudo Access to a user
user ALL=(ALL) ALL	# ALL= execute any command: (ALL)=as any user: ALL=from any host
~ :wq 
$ sudo whoami		# verifies user has been added to sudoers file
- Granting sudo Access to a Group
sudo usermod -aG wheel user
sudo visudo 
~ %wheel ALL=(ALL) ALL		# must be uncommentted 
- Restricting Commands
sudo visudo 
~ user ALL=(ALL) ALL /usr/sbin/systemctl restart nginx	# restricted command
- Disabling Password Promts (Not recommended)
sudo visudo
~ user ALL=(ALL) ALL NOPASSWD: ALL



### Monitoring and Managing Logins
- Keeping track of user login activity helps recognize unauthorized or suspicious user behavior patterns
- Viewing Recent Login Activity
$ last			# read from the /var/log/wtmp file
- View Failed Login Attempts
$ sudo lastb		# read from the /var/log/btmp file
- Real-Time Login Monitoring
$ who			# identify who is logged into what terminal
$ w			# identify more detail as to what a user is doing
- Check Login Attempts with journalctl (Systemd)
sudo journalctl _COMM=sshd | grep "Failed password"
- Install <acct> for Detailed Tracking (optional)
sudo dnf install psacct
sudo systemctl start psacct
sudo systemctl enable psacct
sudo lastcomm		# prints executed commands
sudo sa			# summarizes command usage


### Managing File and Directory Permissions
