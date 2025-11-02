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

## Screenshots/ Diagrams
### Network Layout
![](/assets/)

## Challenges and Lessons Learned