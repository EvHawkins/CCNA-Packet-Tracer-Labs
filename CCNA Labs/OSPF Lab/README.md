# OSPF Configuration Lab

## Objective
Configure Multi-area OSPF network with a backbone area 0 and two branch areas.

## Skills Demonstracted
- OSPF single-area and multi-area configuration
- Router IDs and passive interfaces
- Basic network verification

## Topology
<img width="730" height="397" alt="Topology" src="https://github.com/user-attachments/assets/3df33291-b91b-4f13-804e-9c6c25727405" />

## Step 1: Configure Interface IP address

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

### Area 0 is required for OSPF to work and is the central area all OSPF areas must connect to. All non backbone areas must connect to Area 0 for routing to work correctly. So Area 0 acts as a sort of bridge between the other areas. The routet ID's here help identify each router as well as influence the election process for DR(designated router),BDR(backup designated router). Using the "netowrk" command has the router check all interfaces for matching ip addresses matching the wildcard mask and activates OSPF on that interface. It then starts sending out hello packets, forming neighbor adjacencies, and advertises that interfaces network in the specified area.

## Step 4: Verify Routing

#### Run on each router:

show ip route

<img width="822" height="649" alt="Routing Table" src="https://github.com/user-attachments/assets/49b8f18b-94b4-45a8-b650-e483000b98be" />

#### You should be able to see:

"O" = OSPF routes

Routes learned from other routers

#### Test from PC1

ping 192.168.20.10

ping 192.168.20.20

<img width="745" height="751" alt="Successful Ping" src="https://github.com/user-attachments/assets/7d6722d6-b539-4a43-8273-443c9e1aa40d" />

### If the pings are successful that means OSPF is working.

## Step 5: Configure Passive Interfaces.

#### R1:

router ospf 1

passive-interface gig 0/1

#### R2

router ospf 1

passive-interface gig 0/1

<img width="745" height="753" alt="Ip Protocols" src="https://github.com/user-attachments/assets/54fe2130-1e80-4d2d-9c81-ebc04ba3c837" />

### Adding passive interfaces helps reduce unecessary traffic and improves security. If the interface is connected to an end device there is no reason for the router to advertise networks. As far as improving securtiy, configuring a passive interface could prevent a connected rouge router forming an OSPF adjacency and injecting fake routes.










