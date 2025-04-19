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
banner motd #Unauthorized access prohibited#

interface vlan1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit

# save your configuration (reload won't cause loss of config)
copy running-config startup-config
show running-config
```

## PC

```bash
# show ip configuration
ipconfig /all
```

## Router

```bash
clock set 17:00:00 18 Feb 2013
```

### Interface

```bash
int g0/0
description Connection to PC-B
# set interface's IP address and subnet mask
ip address 192.168.0.1 255.255.255.0
no shutdown
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
