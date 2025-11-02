# Two Networks One Router
## Goal
- creating a network in packet tracer where two networks interconnect and communicate with each other.
- There will be end devices on each side of each network that will attach to a switch and that switch will connect to the router.
- the router will act as the intermediate point between the two networks.

## Architecture/Design
- This will be a hybrid model composed of multiple star topologies.
- each switch will be a star topology and the router will act as a star topology.

## Problem Solved
- This will simulate a pretty common setup for most small businesses where they may have multiple floors or office buildings and will need the networks to communicate with each other.
- This setup is going to all be static IP addresses and directly connected routes but is usually a good simulation starting point for most small businesses.

## Setup Instructions
- first we will need to figure out the subnets for each network for the company.
- we will assume that each network is only going to need about 50 devices on each network starting off and will be using a 192.168.0.0/16 private address space.
- For 50 devices on each network we will need 10 bits two create our networks as there will be 6 host bits remaining.
- The networks will be: 
    - 192.168.0.0 - 192.168.0.63 /26
    - 192.168.0.64 - 192.168.0.127 /26
- Once the subnets are configured setup the PCs on each side with static IP addresses in each range and configure the default gateway as the interface on the router on their side of the network.
- Connect each pc with UTP straight through cables to the interfaces on the switch and the switch with UTP straight through cable to the router interface. 
- Next, set up the static IP addresses on each interface of the router with the correct address for the network to act as the default gateway for each network.
- Because these will be directly connected networks there is no need for setting up any routing rules at this time.
- Once configured, test that a PC can ping a PC on the same network, a PC can ping its default router and finally see if the PC can ping another PC on the other network.
## Configuration and Verification Snippets
- Setting up the IP address to the interfaces on the router use the following commands.
```
R1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#interface gig0/0/0
R1(config-if)#ip address 192.168.0.1 255.255.255.192
R1(config-if)#no shutdown

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

R1(config-if)#interface gig0/0/1
R1(config-if)#ip address 192.168.0.65 255.255.255.192
R1(config-if)#no shutdown

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#exit
R1(config)#exit
R1#
```
- you can then check each interface settings with the command `show interface <interface>`
- To see gig0/0/0 interface:
```
R1#show interface gig0/0/0
GigabitEthernet0/0/0 is up, line protocol is up (connected)
  Hardware is ISR4331-3x1GE, address is 0002.161b.a901 (bia 0002.161b.a901)
  Internet address is 192.168.0.1/26
  MTU 1500 bytes, BW 1000000 Kbit, DLY 10 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported
  Full Duplex, 1000Mbps, link type is auto,  media type is Auto Select
  ...
```
- Ping a device to make sure the network is setup correctly.
- this shows pinging PC1 to PC2 the PC1 to the default gateway and then PC1 to PC6
```
C:\>ping 192.168.0.3

Pinging 192.168.0.3 with 32 bytes of data:

Reply from 192.168.0.3: bytes=32 time=1ms TTL=128
Reply from 192.168.0.3: bytes=32 time=1ms TTL=128
Reply from 192.168.0.3: bytes=32 time<1ms TTL=128
Reply from 192.168.0.3: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.0.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

C:\>ping 192.168.0.1

Pinging 192.168.0.1 with 32 bytes of data:

Reply from 192.168.0.1: bytes=32 time<1ms TTL=255
Reply from 192.168.0.1: bytes=32 time<1ms TTL=255
Reply from 192.168.0.1: bytes=32 time<1ms TTL=255
Reply from 192.168.0.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.0.66

Pinging 192.168.0.66 with 32 bytes of data:

Request timed out.
Reply from 192.168.0.66: bytes=32 time=1ms TTL=127
Reply from 192.168.0.66: bytes=32 time<1ms TTL=127
Reply from 192.168.0.66: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.0.66:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

``` 
- to see the mac address tables while in privileged exec mode use `show mac-address-table`
```
SW1#show mac-address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
```
- here is the outcome after the success of a ping from PC1 to PC6
```
SW1#show mac-address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0002.161b.a901    DYNAMIC     Gig0/1
   1    0010.1170.482d    DYNAMIC     Fa0/1
   1    0030.a337.ac2d    DYNAMIC     Fa0/2
```
## Screenshots/ Diagrams
### Network Layout
![How the network is setup](/assets/images/network_design.png)

### PC GUI setup
![Setting the default gateway](/assets/images/pc_default_gateway_setup.png)

![Setting the IP address and subnet mask](/assets/images/pc_ip_address_setup.png)

### Router Configuration
![Basic setup of the router and interfaces](/assets/images/router_configuration.png)

### Show MAC address tables on switch 
![Show MAC address table before and after a ping](/assets/images/switch_mac_address_table.png)

## Challenges and Lessons Learned
- This setup was easy to implement but did bring up how to think about breaking up networks into subnets.
- The basic configuration on a router interface is easy to do however I can see where making sure IP addresses are set correctly is important.
- These skills will carry over into the next packet tracer assignment where I will start to use VLANs and VLAN routing. 