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

## PC

```bash
# show ip configuration
ipconfig /all
```

## Switch

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
login
exit

# config vty line to allow Telnet access
line vty 0 4
password cisco
login
end

# banner
banner motd #
Unauthorized access is strictly prohibited and prosecuted to the full extent of the law. #

interface vlan1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit

# save your configuration
copy running-config startup-config
show running-config
```

## Router

```bash

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

-   Copper straight: different devices (switch to router, PC to switch)
-   Copper crossover: same devices (switch to switch, router to router)

## Devices

-   can ping other devices in same network

### Switch

-   doesn't have power switch

### PC

-   Desktop >> IP Configuration >> IP address, subnet mask, default gateway
