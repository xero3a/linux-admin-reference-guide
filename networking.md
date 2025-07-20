# Networking

This document tracks all networking-related configuration and diagnostics performed on my 
RHEL system, including IP address checks, firewall management, interface configuration, 
and DNS settings.

__________________________________________________________________________________________
------------------------------------------------------------------------------------------

## Basic Network Inspection

These commands were used to inspect the network status of the system:

- `ip a` – Lists all network interfaces and their assigned IP addresses.
- `nmcli device status` – Shows the status of all available network interfaces.
- `ping -c 4 8.8.8.8` – Tests connectivity to the internet via ICMP.
- `hostname -I` – Prints the system's current IP address(es).
- `ss -tuln` – Displays listening ports and their associated services.

> These tools help ensure connectivity is established and confirm what services (if any) are exposed.

