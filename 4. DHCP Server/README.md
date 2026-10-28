# DHCP and Internet Access
<img width="1920" height="983" alt="2  Installation Complete" src="https://github.com/user-attachments/assets/194fb307-09ff-4ec1-a5b3-6bdbefdb4e72" />

## Overview
This stage extends the lab by adding a DHCP server to the domain controller so that clients receive their IP address and DNS settings automatically, instead of being configured by hand. It also begins preparing the server to act as a gateway so that domain clients can reach the internet.

## Tasks

- [X] Install the DHCP Server role on the domain controller.
- [X] Complete the DHCP post-install configuration and authorize the server.
- [X] Create an IPv4 scope for the lab network.
- [X] Configure the DNS domain name and DNS server handed out to clients.
- [X] Switch the client from a manual IP to DHCP and verify the lease.

## Implementation

### 1. Install the DHCP Server role
The role was added through **Server Manager > Add Roles and Features**, alongside the existing AD DS and DNS roles. After installation, the DHCP post-install wizard was run to create the *DHCP Administrators* and *DHCP Users* security groups and to authorize the DHCP server in Active Directory

### 2. Create the DHCP scope
A new IPv4 scope was created with the **New Scope Wizard**:

* **Address range:** `192.168.1.50` to `192.168.1.100`
* **Subnet mask:** `255.255.255.0` (length 24)
* **Parent domain:** `crashdummies.za`
* **DNS server:** `192.168.1.15`

The server's own address (`192.168.1.15`) sits outside the pool, so it cannot be handed out to a client by mistake.

### 3. Move the client from a manual IP to DHCP
The client had originally been given a manual IP address so it could join the domain. It was then switched to **Obtain an IP address automatically** and **Obtain DNS server address automatically**, and the lease was renewed.

## Status

* **Completed:** DHCP role, authorization, scope, client lease, second server adapter, DNS forwarders, Router option.
* **In progress:** installing the **Routing** role (Remote Access) and enabling **NAT** in Routing and Remote Access. The role installation was very slow on the VM's current resources and is to be retried (with more RAM and CPU assigned to the server VM) before testing client internet access. Until NAT is enabled, clients are pointed at the server as their gateway but cannot yet reach the internet through it.


## Key Learnings
This stage helped me understand how DHCP works alongside DNS and Active Directory: how a scope hands out addresses, DNS servers and a domain suffix, why a DHCP server must be authorized in a domain, and why the server's static address should sit outside the pool.
