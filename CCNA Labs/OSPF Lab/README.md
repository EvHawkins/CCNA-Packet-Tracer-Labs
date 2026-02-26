# OSPF Configuration Lab

## Objective
Configure Multi-area OSPF network with a backbone area 0, and two branch areas.

## Skills Demonstracted
- OSPF single-area and multi-area configuration
- Router IDs and passive interfaces
- Basic network verification

## Topology
<img width="730" height="397" alt="Topology" src="https://github.com/user-attachments/assets/3df33291-b91b-4f13-804e-9c6c25727405" />

## Step 1: Configure interface IP address

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

enable

conf t

int gig 0/0

ip address 10.0.23.2 255.255.255.252

no shut

int gig 0/1

ip address 192.168.20.1 255.255.255.0

no shut

<img width="1440" height="706" alt="Router Config" src="https://github.com/user-attachments/assets/7a8c6dbb-3d65-4077-89f8-581bfc3971a6" />


### Configuring ip addresses on interfaces allows routers to communicate and advertise their networks with routing protocols.

## Step 2: Configure PCs

#### PC1

IP - 192.168.10.10

Gateway - 192.168.10.1

#### PC2

IP 192.168.10.20

Gateway - 192.168.10.1

#### PC3

IP - 192.168.20.10

Gateway - 192.168.20.1

#### PC4

IP - 192.168.20.20

Gateway - 192.168.20.1

<img width="744" height="751" alt="PC Config" src="https://github.com/user-attachments/assets/7e3f88ea-5859-4208-8bab-74c18e0a740a" />

## Step 3: Configure OSPF(Multi-Area)

#### R1

conf t

router ospf 1

router-id 1.1.1.1

network 10.0.12.0 0.0.0.3 area 0

network 192.168.10.0 0.0.0.255 area 10

#### R2

conf t

router ospf 1

router-id 2.2.2.2

network 10.0.12.0 0.0.0.3 area 0

network 10.0.23.0 0.0.0.3 area 0

#### R3

conf t

router ospf 1

router-id 3.3.3.3

network 10.0.23.0 0.0.0.3 area 0

network 192.168.20.0 0.0.0.255 area 20

<img width="691" height="788" alt="OSPF Config" src="https://github.com/user-attachments/assets/dbfcae4f-db44-4b9a-8537-334ff100e976" />





