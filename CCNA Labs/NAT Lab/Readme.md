# Network Address Translation Lab

## Objective
- Simulate a small enterprise network
- Use Network address translation (NAT) to route private IPs to the public internet
- Use Port address translation (PAT) to share one public IP and use port numbers to track sessions

## Skills Show In This Lab
- Inisde vs outside interface roles
- Dynamic NAT with overload (PAT)
- ACL based NAT matching
- NAT verifaction

## Topology
<img width="828" height="336" alt="Topology" src="https://github.com/user-attachments/assets/00128840-ef6d-4ab9-9689-6b646ff70671" />

## Step 1: Configure IP addresses

#### R1(Edge Router)

enable

conf t

interface gig 0/0

ip addresss 192.168.10.1 255.255.255.0

no shut

interface gig 0/1

ip address 209.165.200.225 255.255.255.252

no shut

#### R2(ISP Router)

enable

conf t

interface gig 0/0

ip address 209.168.200.226 255.255.255.252

no shut

interface gig 0/1

ip address 8.8.8.1 255.255.255.0

no shut

<img width="744" height="595" alt="Screenshot 2026-03-05 at 12 16 45 PM" src="https://github.com/user-attachments/assets/c5a67318-0e12-4476-abcf-d2796f14744b" />

#### PC's

PC1 - 192.168.10.10

PC2 - 192.168.10.20

PC3 - 8.8.8.8

<img width="730" height="871" alt="Screenshot 2026-03-04 at 11 18 20 AM" src="https://github.com/user-attachments/assets/a1e1640f-3816-4240-bf30-9c5877b89e69" />


### In this step I used the 192.168.10.0 network because it belongs in the private IP address range. These addresses can't be routed on the public internet and are meant to be used for internal company networks. This range of addresses would require NAT to accesss the internet. I used the IP address of 209.165.200.224 because it is meant to simulate a public IP address assigned by an Internet Service Provider(ISP). For PC3, I used the IP address of 8.8.8.8 to represent a public internet address or in simple terms, its supposed to act as the internet. In the real world, 8.8.8.8 is Google's DNS and is commonly used for ping testing.

## Step 2: Add A Default Route On The Edge Router

#### R2(Edge Router)

conf t

ip route 0.0.0.0 0.0.0.0 209.165.200.226

### The purpose of using a deafault route is if the router is sent an unknown address, it will say "I don't know where this IP address is" and will send it to the ISP router, which will in turn, send it out to the internet.

## Step 3: Configure NAT on R1(Edge Router)

conf t

interface gig 0/0

ip nat inside

int gig 0/1

ip nat outside

### In this step we are telling the router which interface in used for the inside private network and which interface is used for the outside public network. This tells the router which addresses need to be translated and when the translation should happen.

## Step 4: Configure Dynamic NAT(PAT)

#### Create an ACL to define inside traffic:

access-list 1 permit 192.168.10.0 0.0.0.255

#### Enable NAT Overload:

ip nat inside source list 1 interface gig 0/1 overload

### By using an ACL we are telling NAT the addresses NAT is allowed to translate. Using the ACL as the source list and using the "overload" command on the gig 0/1 interface tells NAT to translate the inside addresses in the 192.168.10.0 network, use the outside interfaces IP address, and overload allows multiple users to share one public IP address. 

## Step 5: Test NAT

From PC1 and PC2 ping 8.8.8.8

Check NAT Translations on R1(Edge Router)

<img width="743" height="752" alt="NAT Translations" src="https://github.com/user-attachments/assets/4122181f-6f12-4774-8860-efa66ab9d43d" />

### You can see that traffic from each PC is sharing the one public IP address and using port numbers to track each session.










