# Commands
```bash
# turn on interfaces that are administratively down
no shutdown

# save your configuration, ctrl+s
copy running-config startup-config

# set clock rate on DCE side
clock rate 128000
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

# Tips
1. you do not need to assign an IP to the switch interface itself for the PC to ping the router as long as:
- The PC and router are on the same subnet
- The switch port connected to the PC and the one connected to the router are in the same VLAN
- The interfaces are up

# Keys
- ctrl+shift+6: interrupt e.g. Translating "ip"...domain server
- ?: help
- tab: auto-complete