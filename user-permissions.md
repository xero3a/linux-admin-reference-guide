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

