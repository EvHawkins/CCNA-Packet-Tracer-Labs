# OSPF Configuration Lab

## Objective
Configure Multi-area OSPF network with a backbone area 0, and two branch areas.

## Skills Demonstracted
- OSPF single-area and multi-area configuration
- Router IDs and passive interfaces
- Basic network verification

## Topology
<img width="730" height="397" alt="Topology" src="https://github.com/user-attachments/assets/3df33291-b91b-4f13-804e-9c6c25727405" />

## Step 1 Configure interface IP address

#### R1

enable 

conf t

interface gig 0/0

ip address 10.0.12.1 255.255.255.252

no shut

int gig 0/1

ip address 192.168.10.1 255.255.255.0

no shut

#### R2

enable 

conf t

interface gig 0/0

ip address 10.0.12.2 255.255.255.252

no shut

int gig 0/1

ip address 10.0.23.1 255.255.255.252

no shut

#### R3

int gig 0/0

ip address 10.0.23.2 255.255.255.252

no shut

int gig 0/1

ip address 192.168.20.1 255.255.255.0

no shut

### Configuring ip addresses on interfaces allows routers to communicate and advertise their networks with routing protocols.



