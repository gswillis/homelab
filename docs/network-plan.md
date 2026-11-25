Last updated: 2026-11-25

Fill in the blanks when you open the eero app. Do not guess addresses. Use the ones the app shows.

Router facts
Item	Value
Router	eero (rented from the ISP). Model: ______
Modem	Owned. Connects to the eero. Assumed to be modem-only (no router or Wi-Fi of its own). Confirm: ______
Management	eero phone app only
Home subnet (first three numbers of any device address)	
DHCP range	
Can reserve fixed addresses (DHCP reservation)?	Yes. Settings, then Network Settings, then Reservations & Port Forwarding
Can change the DNS server it hands out?	Yes. Settings, then advanced network settings, then DNS, then Custom DNS
eero Plus subscription?	No, so the DNS setting is not blocked
IPv6	Leave off for now (see below)
Port forwarding	Exists, stays unused
Router check result

Both required features exist, so the plan works with the current router. No upgrade is needed.

eero specifics
Reservations
Reserve a fixed address for the Proxmox host and for each container or VM that needs one (see the address plan below).
Make the reservation after the device has connected once, so the eero lists it.
Custom DNS and Pi-hole
Saving a DNS change reboots the eero network, so do it when nobody is online.
eero sends DNS traffic from its gateway, so Pi-hole sees most requests as coming from the eero. Blocking works. Per-device stats in Pi-hole do not.
Test Pi-hole from one PC first by setting that PC's own DNS to the Pi-hole address. Only then change the eero's Custom DNS, which applies to the whole network.
To go back: turn Custom DNS off in the eero app, and the eero returns to the provider's DNS.
If you enter more than one DNS server, check how the eero uses them. A public fallback keeps the internet working if the server is off, but some lookups may bypass Pi-hole.
If you ever subscribe to eero Plus, the DNS setting will be grayed out until the Plus filters are turned off.
IPv6
Leave it off for now. Nothing in this plan needs it.
With IPv6 on, devices may use an IPv6 DNS server and skip Pi-hole. The eero app can set custom IPv6 name servers, but then Pi-hole must handle IPv6 too.
IPv6 addresses can be reached from the internet, which conflicts with the no-outside-access goal. A router firewall normally blocks unsolicited inbound traffic, but it is one more thing to check.
Revisit it after everything runs. If it is already on, leave it for now.
Port forwarding and UPnP
Do not create port forwards.
If the eero app has a UPnP option, keep it off.
Rented gear
The rented equipment is the eero. Note the monthly fee from the bill: ______
Owning is a cost question, not a technical one, because the eero already does what the plan needs.
Work out the break-even: purchase price divided by the monthly fee.
If you buy, cover the same space as the rented units, check the model, and move the network to your own eero account.
Swapping the eero causes a short outage, so do it when nobody is online.
Revisit after the server and UPS are running.
Address plan

Reserve a block outside the router's automatic range for infrastructure. Write the real addresses in the table.

Device	Reserved address	Notes
Router (eero gateway)		Fixed by the eero
Proxmox host (pve01)		Wired Ethernet, fixed
Pi-hole (pihole)		Own address, fixed
Home Assistant (homeassistant)		Fixed
Immich (immich)		Fixed
Forgejo (forgejo)		Fixed
Control tablet		Reserved so it is easy to find
Info display		Reserved
DNS design
The eero's Custom DNS points to the Pi-hole address.
Do not experiment with Pi-hole while others are streaming.
The server is the single point of failure for DNS. Decide on a fallback before switching the whole network.
Rules
The server uses Ethernet, not Wi-Fi.
No port forwarding, and UPnP stays off.
Wi-Fi uses WPA2 or WPA3 with a strong password, plus a guest network for visitors.
Smart-home devices may move to their own network later. Not needed to start.