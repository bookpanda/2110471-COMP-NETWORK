```bash
no
enable
config terminal
no ip domain-lookup
service password-encryption
enable secret class
banner motd #Unauthorized access is strictly prohibited.#
line con 0
password cisco
login
logging synchronous
line vty 0 4
password cisco
login

hostname R1

```
