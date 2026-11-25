# Server day checklist

Work top to bottom. Change one thing at a time and test after each step. Take a Proxmox snapshot before risky changes.

## Before you start

- [ ] `docs/network-plan.md` is filled in, and the router can reserve addresses and set DNS
- [ ] A second computer, an 8 GB or larger USB stick and balenaEtcher (or similar) are ready
- [ ] The Proxmox installer is on the USB stick
- [ ] The server is placed near the router, with airflow, plugged into the UPS
- [ ] The password manager is ready for new logins

## 1. BIOS

- [ ] Update the BIOS if it is very old
- [ ] Turn on virtualization (Intel VT-x, "Virtualization Technology")
- [ ] Set power-on after power loss ("AC power recovery" or "restore on AC power loss")
- [ ] Boot from the USB stick

## 2. Proxmox

- [ ] Install Proxmox on the NVMe SSD
- [ ] Use a fixed address that matches the reservation in the router
- [ ] Log in to the web interface and store the login in the password manager
- [ ] Add the photo hard drive as storage
- [ ] Add a monthly reminder to check drive health (SMART) and updates

## 3. Pi-hole

- [ ] Create the `pihole` container and give it its own fixed address
- [ ] Test it from one device by setting that device's DNS to the Pi-hole address
- [ ] Only then set the router's DNS to Pi-hole, with a fallback DNS
- [ ] Check that normal browsing still works
- [ ] Test the streaming apps on the TV (Netflix, Hulu, HBO Max, Prime, Disney+, YouTube, YouTube TV, Paramount) and allowlist any blocked domain

## 4. Home Assistant

- [ ] Install the official Home Assistant OS image as the `homeassistant` VM
- [ ] Plug the Zigbee and Z-Wave sticks into USB 2.0 ports or extension cables
- [ ] Pass each stick through to the VM
- [ ] Set up the ZHA (Zigbee) and Z-Wave integrations
- [ ] Pair the Shelly BLU H&T ZB sensor over Zigbee (check Shelly's manual for the button sequence)
- [ ] Pair the Shelly Wave Plug US over Z-Wave, using SmartStart or the 5-digit PIN from its DSK label (photo in the password manager)
- [ ] Place the plug in a central spot, because it repeats the Z-Wave signal
- [ ] Check that the plug reports power (W) and energy (kWh)
- [ ] Name the devices and areas using `docs/naming-scheme.md`
- [ ] Commit the configuration to git (not `secrets.yaml`)

## 5. Immich

- [ ] Create the `immich` VM and install Docker
- [ ] Keep the compose file in `docker/` in this repo, with secrets in a `.env` file that is not committed
- [ ] Import a small batch of test photos first
- [ ] Install the phone app and test auto-upload on home Wi-Fi
- [ ] Do the Google Takeout import only after the backup drive exists

## 6. Jellyfin

- [ ] Create the `jellyfin` container or VM and give it a fixed address
- [ ] Mount the media drive and follow the folder layout in `docs/media-library.md`
- [ ] Pass the Intel integrated graphics through to Jellyfin if you need hardware transcoding (it is free)
- [ ] Keep Jellyfin's automatic port mapping (UPnP) off
- [ ] Add the libraries and test direct play from the Samsung TV
- [ ] Install the Jellyfin app on the TV (store first, sideload if the TV is too old)

## 7. Forgejo

- [ ] Create the `forgejo` container
- [ ] Create a private `homelab` repo there and add it as a second remote
- [ ] Set up a push mirror to the private GitHub repo

## 8. Backups and power

- [ ] Schedule Proxmox backups of the VMs to the external USB drive
- [ ] Back up the photo library to the external drive
- [ ] Include the Jellyfin VM in the Proxmox backups (the media files themselves are optional)
- [ ] Restore one small backup to prove it works
- [ ] Set up an encrypted cloud copy, if you decide to use one
- [ ] Connect the UPS by USB and set up NUT for automatic shutdown
- [ ] Plug the router and modem into the UPS

## 9. Final checks

- [ ] No port forwarding, and UPnP is off on the router
- [ ] Every service has a unique password in the password manager
- [ ] The household guide is updated with the real names and who to ask
