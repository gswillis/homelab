# Network plan

Fill in the blanks when you log into your router. Do not guess addresses. Use the ones your router shows.

## Router facts to find

| Item | Value |
|---|---|
| Router model | |
| Router address (the one you log in at) | |
| Home subnet (the first three numbers of any device address) | |
| DHCP range | |
| Can reserve fixed addresses (DHCP reservation)? | yes / no |
| Can change the DNS server it hands out? | yes / no |

If the router cannot do both of the last two, Pi-hole needs a different setup, such as a different router or Pi-hole acting as the DHCP server. Find out before buying anything.

## Address plan

Reserve a block outside the router's automatic range for infrastructure. Write the real addresses in the table.

| Device | Reserved address | Notes |
|---|---|---|
| Router | | Fixed |
| Proxmox host (`pve01`) | | Wired Ethernet, fixed |
| Pi-hole (`pihole`) | | Own address, fixed |
| Home Assistant (`homeassistant`) | | Fixed |
| Immich (`immich`) | | Fixed |
| Forgejo (`forgejo`) | | Fixed |
| Control tablet | | Reserved so it is easy to find |
| Info display | | Reserved |

## DNS design

- The router hands out the Pi-hole address as the DNS server to every device.
- Set a fallback DNS in the router so the internet still works if the server is off. The cost is that some devices will bypass ad blocking when they use the fallback.
- Do not experiment with Pi-hole while others are streaming.

## Rules

- The server uses Ethernet, not Wi-Fi.
- No port forwarding on the router, and turn off UPnP.
- Wi-Fi uses WPA2 or WPA3 with a strong password, plus a guest network for visitors.
- Smart-home devices may move to their own network (VLAN) later. Not needed to start.
