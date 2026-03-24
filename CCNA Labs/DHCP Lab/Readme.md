# Dynamic Host Configuration Protocol Lab

## Objective

Build a small network where a router provides DHCP services to automatically assign IP address to client devices


## Skills Shown In This Lab

- DHCP client configuration
- DHCP relay configuration
- IP addressing
- Basic network verification

## Topology

<img width="788" height="408" alt="Topology" src="https://github.com/user-attachments/assets/739da678-9ee1-4172-965b-1cd57c75eb91" />

 ## Step 1: Configure Router Interfaces

 #### R1

- enable
- conf t
- int gig 0/0
- ip address 192.168.10.1 255.255.255.0
- no shut
- int gig 0/1
- ip address 10.0.12.1 255.255.255.252
- no shut

 #### R2

- enable
- int gig 0/1
- ip address 192.168.20.1 255.255.255.0
- no shut

### Configuring each router to act as default gateway to client PC's on their respective networks

## Step 2: Exclude Reserved Addresses

### R1

- conf t
- ip dhcp excluded-address 192.168.10.1 192.168.10.10
- ip dhcp excluded-address 192.168.20.1 192.168.20.10
- ip dhcp exluded-address 10.0.12.1

### This reserves addresses for other devices such as routers, servers, printers, etc. This also prevents DHCP from assigning addresses already in use by other devices such as the routers default gateway.


## Step 3: Create The DHCP Pools on R1

conf t

ip dhcp pool 1

network 192.168.10.0 255.255.255.0

default-router 192.168.10.1

dns-server 8.8.8.8

exit

ip dchp pool 2

network 192.168.20.0 255.255.255.0

default-router 192.168.20.0

dns-server 8.8.8.8

exit

ip dhcp pool 3

network 10.0.12.0

default-router 10.0.12.1 255.255.255.252

dns-server 8.8.8.8

<img width="745" height="750" alt="DHCP Pool" src="https://github.com/user-attachments/assets/b3f5089f-ba5a-426b-a3e5-021d7f22be9f" />


### DHCP pools tell the router which network to assign assign IP addresses from, what gateway the clients should use, and which DNS server to use.

## Step 4: Configure PC's on the 192.168.10.0 network to use DHCP

### PC1

#### Go to the IP configuration menu and switch to DHCP from the static option

#### PC1 DHCP hasn't yet been enabled so PC1 hasn't requested an IP address

<img width="744" height="750" alt="DHCP Router Verify 1" src="https://github.com/user-attachments/assets/2ff53f0e-7032-4765-9401-e3bbf0f34ec9" />

#### PC1 has now been assigned an IP address from R1

<img width="748" height="753" alt="DHCP Router Verify" src="https://github.com/user-attachments/assets/e3f81486-1ed8-470e-a16c-3e1bff234224" />

### Alternatively you can use the /renew from the command prompt to have the DHCP client request a new IP address from the router.

<img width="743" height="752" alt="Screenshot 2026-03-09 at 1 26 00 PM" src="https://github.com/user-attachments/assets/6f78fd27-78cd-44d0-afd6-d1ad4f3af662" />


### DHCP clients go through a process know as "DORA". This is discover, offer, request, and acknowledge. The DHCP client sends out a message to locate or "discover" any available DHCP servers. The DHCP server then responds or "offers" an available IP address and configuration details. The DHCP client selects the offer and then formally puts in a "request" to use that IP address. The DHCP server will then "acknowledge" the request and finalize the lease of the IP address.

## Step 5: Configure an IP address on an interface using DHCP

### R2

conf t

int gig 0/0

ip address dhcp

<img width="742" height="283" alt="Interface DHCP" src="https://github.com/user-attachments/assets/db9eee48-bbdf-412c-a4c0-373e1dcf1d12" />

## Step 6: Configure R2 as a relay agent for the 192.168.20.0 network

### R2

conf t

int gig 0/1

ip helper-address 10.0.12.1

### R2 will now forward DHCP broadcast messages from clients to a DHCP server on a different subnet or network.

## Step 7: Configure PC's on the 192.168.20.0 network to use DHCP

### Repeat the same process as in Step 4

### PC5

<img width="747" height="712" alt="Relay Verify 1" src="https://github.com/user-attachments/assets/e8b0532b-549d-4a93-b847-7d922ff56a8a" />

### PC6

<img width="748" height="753" alt="PC relay verify 2" src="https://github.com/user-attachments/assets/11a21132-a7c7-4184-8d9a-528e68e3d041" />

## Step 8: Verify DHCP on R1

### R1

show ip dhcp binding

<img width="698" height="664" alt="DHCP Binding Table" src="https://github.com/user-attachments/assets/a6d614c9-6672-43bc-95de-602779cd5fcd" />

### This shows which devices recieved which addresses and active leases.























































