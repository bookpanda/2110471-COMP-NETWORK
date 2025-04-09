# Commands
```bash
# turn on interfaces that are administratively down
no shutdown

# save your configuration, ctrl+s
copy running-config startup-config

```

## Show
```bash
show ip interface brief

```
## ?
```bash
int vlan ?
show ip ?
```

# Tips
1. you do not need to assign an IP to the switch interface itself for the PC to ping the router as long as:
- The PC and router are on the same subnet
- The switch port connected to the PC and the one connected to the router are in the same VLAN
- The interfaces are up

# Keys
- ctrl+shift+6: interrupt e.g. Translating "ip"...domain server
