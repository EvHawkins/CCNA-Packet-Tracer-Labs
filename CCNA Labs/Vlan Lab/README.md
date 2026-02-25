# VLAN Configuration Lab

## Objective
Configure VLANs and trunking between switches.

## Skills Demonstrated
- VLAN creation
- Trunk configuration (802.1Q)
- Inter-VLAN routing
- Switchport configuration

## Topology
<img width="793" height="433" alt="Topology" src="https://github.com/user-attachments/assets/81dfc5e5-9dde-44f0-8adb-44796a3a1042" />

## Step 1: Create Vlans on each switch.

enable

conf t

vlan 10

name SALES

vlan 20

name HR

vlan 30

name IT

### Vlans seperate traffic. Even if all PC's are connected to one switch, they wont be able to communicate unless routing happens.

## Step 2: Assign access ports.

int fa 0/1

switchport mode access

switchport acces vlan 10

int fa 0/2

switchport mode access

switchport access vlan 20

int fa 0/3

switchport mode access

switchport access vlan 30

### This configures each port to it's own vlan, making the port belong only to that vlan.

## Step 4: Configure trunk ports

SW1(to SW2)

int fa 0/4 

switchport mode trunk

SW2(to SW1)

int fa 0/4

switchport mode trunk

SW2(to R1)

int gig 0/2

switchport mode trunk

### This allows multiple vlans to travel over one cable.

<img width="1440" height="478" alt="Vlan&#39;s and Trunk" src="https://github.com/user-attachments/assets/7d355f60-619f-416a-a821-8ddc72213d8a" />

## Step 5: Configure router sub-interfaces

enable

conf t

int gig 0/0/0

no shut

int gig 0/0/0.10

encapsulate dot1q 10

ip address 192.168.10.1 255.255.255.0

int gig 0/0/0.20

encapsulate dot1q 20

ip address 192.168.20.1 255.255.255.0

int gig 0/0/0.30

encapsulate dot1q 30

ip address 192.168.30.1 255.255.255.0

### This allows port gig 0/0/0 to act as multiple virtual interfaces, each assigned to a different vlan. "encapuslate dot1q" tells the router to expect vlan tagged traffic.

<img width="750" height="755" alt="Sub Interfaces" src="https://github.com/user-attachments/assets/eec3ca1a-21f1-4420-b13e-d17460e0ee99" />

# Step 6: Configure IP address on PCs.

Vlan 10 PCs

PC1 - 192.168.10.10

PC4 - 192.168.10.20

Gateway - 192.168.10.1

Vlan 20 PCs

PC2 - 192.168.20.10

PC5 - 192.168.20.20

Gateway - 192.168.20.1

Vlan 30 PCs

PC3 - 192.168.30.10

PC6 - 192.168.30.20

Gateway 192.168.30.1

<img width="749" height="317" alt="PC IP&#39;s" src="https://github.com/user-attachments/assets/992cb567-2a53-4f10-8ba3-159cdc020d63" />


### The gateway is the routers sub-interface for the vlan. Now if a PC needs to talk to another vlan, it will send traffic to the router, the router routes it, and then sends it to the correct vlan.




