# Commands

```bash
# turn on interfaces that are administratively down
no shutdown

# set clock rate on DCE side
clock rate 128000

ping 192.168.1.11
```

## Show

```bash
# display IOS version
show version

# show vlans and their ports
show vlan

show ip interface
# show all interfaces with IP (if vlan has IP, it will show up here)
show ip interface brief

show interfaces
show interfaces fastEthernet 0/5

# show IP routes of interfaces (router only)
show ip route

show ip nat translations
show ip nat statistics
```

## Setup

```bash
enable
configure terminal
hostname S1
# prevent switch from trying to resolve mistyped commands
no ip domain-lookup

# add password
enable secret class
line con 0
password cisco
# log out after 5 minutes of inactivity
exec-timeout 5 0
# make system message come after your input
logging synchronous
login
exit

# config vty line to allow Telnet access
line vty 0 4
password cisco
login
end
# now PC can telnet 192.168.1.2 (vlan ip of switch)

# set password encryption
service password-encryption

# banner
banner motd #Unauthorized access is prohibited#

interface vlan1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit

# save your configuration (reload won't cause loss of config)
copy running-config startup-config
show running-config
```

# PC

```bash
# show ip configuration
ipconfig /all
```

# Router

```bash
clock set 17:00:00 18 Feb 2013
```

## GigabitEthernet

```bash
int g0/0
description Connection to PC-B
# set interface's IP address and subnet mask
ip address 192.168.0.1 255.255.255.0
no shutdown
```

## Serial DCE

-   even after DCE ip config, PC cannot ping stuff cross DCE (the router doesn't have routes to distant networks)

```bash
int s0/0/0
ip address 10.1.1.2 255.255.255.252
clock rate 128000 # do only on DCE side
no shutdown
```

## Loopback

```bash
interface Loopback0
ip address 198.133.219.1 255.255.255.0
```

## Static Route

```bash
# recursive static route on R1
# dest network, subnet mask, next hop ip (dest router side)
ip route 192.168.1.0 255.255.255.0 10.1.1.2
# show ip route: ip route 192.168.1.0/24 [1/0] via 10.1.1.2
# PC A still cannot ping PC C (R3 does not have return route)
# recursive static route on R3
ip route 192.168.0.0 255.255.255.0 10.1.1.1
# now PC A can ping PC C

# directly connected static route on R3
# dest network, subnet mask, interface (R3 router side)
ip route 192.168.0.0 255.255.255.0 s0/0/0
# show ip route: 192.168.0.0/24 is directly connected, Serial0/0/0
no ip route 192.168.0.0 255.255.255.0 s0/0/0
# A static route cannot be "directly connected" unless the destination network is on that interface

# these 2 routes are equivalent
ip route 198.133.219.0 255.255.255.0 S0/0/1 # this router side
ip route 198.133.219.0 255.255.255.0 10.1.1.2 # dest router side
```

## Default Route

```bash
ip route 0.0.0.0 0.0.0.0 s0/0/1
# gateway of last resort is 0.0.0.0 to 0.0.0.0

```

# OSPF

```bash
# enable OSPF on router
router ospf 1
# enable OSPF in each interface e.g. first one 192.168.1.0/24 subnet
network 192.168.1.0 0.0.0.255 area 0
network 192.168.12.0 0.0.0.3 area 0
network 192.168.13.0 0.0.0.3 area 0
# do for all routers

show ip ospf neighbor
# show subnets that are not directly connected to the router + cost
show ip route ospf
# verify OSPF settings
show ip protocols
# verify OSPF process
show ip ospf
# detail + cost
show ip ospf interface
show ip ospf interface s0/0/0
```

## Router ID

order of finding router ID:

1. IP addr configured with router-id

```bash
router ospf 1
router-id 11.11.11.11
end
clear ip ospf process
```

2. highest IP addr of loopback interface

```bash
interface lo0
ip address 1.1.1.1 255.255.255.255
end
reload # must reload to take effect
```

3. highest active IP addr of any interface

## Passive Interface

prevent OSPF from sending hello packets on an interface

```bash
show ip ospf interface g0/0 # Hello due
router ospf 1
passive-interface g0/0
end
show ip ospf interface g0/0 # no Hello

# R2
passive-interface default # all interfaces passive
# R1: when R2 dead timer expires, R1 will remove R2 from its OSPF neighbor table
# R1, R3 no longer have a route passing through R2 subnet
show ip ospf neighbor

# R2: open back one interface, R1, R3 can only go to R2 through that interface
router ospf 1
no passive-interface s0/0/0
```

## Change OSPF metrics

```bash
# change OSPF cost
router ospf 1
# cost = 10000 / speed in Mbps e.g. 1 Gbps cost = 10, default is 100
auto-cost reference-bandwidth 10000

# change interface bandwidth
interface s0/0/0
bandwidth 128
show ip route ospf # some best route will change

# change ospf cost directly
interface s0/0/1
ip ospf cost 1565
```

# Switch

```bash
# give vlan interface addr
interface vlan 1
ip address 192.168.1.11 255.255.255.0
no shutdown

# deactivate range of interface
interface range f0/2-5, f0/7-24, g0/1-2
shutdown

# create vlans, assign switch ports

```

# Tips

1. you do not need to assign an IP to the switch interface itself for the PC to ping the router as long as:

-   The PC and router are on the same subnet
-   The switch port connected to the PC and the one connected to the router are in the same VLAN
-   The interfaces are up

# Keys

-   ctrl+shift+6: interrupt e.g. Translating "ip"...domain server
-   ?: help
-   tab: auto-complete

# Hardware

## Wiring

-   Copper straight: PC <-> switch, router <-> switch
-   Copper crossover: same devices or PC <-> router
-   Serial DCE: add HWIC-2T to router
-   connecting remote accessp
    -   wire console cable from PC's RS 232 to router's console port
    -   enter PC's terminal, can now access switch's terminal

## Devices

-   can ping other devices in same network

### Switch

-   doesn't have power switch

### PC

-   Desktop >> IP Configuration >> IP address, subnet mask, default gateway

### Router

-   default gateway is the IP address of the router's interface on that local network
-   GigabitEthernet IP = default gateway IP (first usable ip in the network after the network ip)
