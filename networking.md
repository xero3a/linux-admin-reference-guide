# Networking

This document tracks all networking-related configuration and diagnostics performed on my 
RHEL system, including IP address checks, firewall management, interface configuration, 
and DNS settings.


------------------------------------------------------------------------------------------

### Basic Network Inspection

These commands were used to inspect the network status of the system:

- `ip a` – Lists all network interfaces and their assigned IP addresses.
- `nmcli device status` – Shows the status of all available network interfaces.
- `ping -c 4 8.8.8.8` – Tests connectivity to the internet via ICMP.
- `hostname -I` – Prints the system's current IP address(es).
- `ss -tuln` – Displays listening ports and their associated services.

> These tools help ensure connectivity is established and confirm what services (if any) are exposed.


### Configuring Network Interfaces

> Use `nmtui` (Network Manager Text User Interface) for interactive network configuration.

- To view all interfaces and their status:
```
$ nmcli device status
```
- Bringing an interface up/down
```
$ sudo ip link set <interface> up
$ sudo ip link set <interface> down
```
- Assigning a static IP address (temporarily)
```
$ sudo ip addr add <IP_address>/<prefix_length> dev <interface>
```
- Deleting a static IP address from interface
```
$ sudo ip addr del <IP_address>/<prefix_length> dev <interface>
```
- Permanent network configuration performed by the following actions:
        a. /etc/sysconfig/network-scripts
        b. nmcli

### Firewall Configuration (firewalld)

> RHEL 9 uses `firewalld` as its default firewall management tool.

- Check firewalld status (start & enable)
```
$ sudo systemctl status firewalld
$ sudo systemctl start firewalld
$ sudo systemctl enable firewalld
```
- View default and active zones
```
$ firewall-cmd --get-default-zone
$ firewall-cmd --get-active-zones
```
- List current rules for a zone
```
$ firewall-cmd --zone=public --list-all
```
- Opening ports & reload the firewall rules to apply new changes.
```
$ sudo firewall-cmd --zone=public --add-port=<port>/<protocol> --permanent
$ sudo firewall-cmd --reload
```
