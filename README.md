# Homelab

Plans, configs and notes for my home server: Proxmox, Pi-hole, Home Assistant, Immich and Forgejo. No outside access, local first.

Last updated: 2026-11-25

## Getting Started

- If you have high level questions, read `/docs/homelab_guide_for_everyone.md`

### Documentation

- If you have more technical questions about our plan, read `/docs/homelab_plan.md`
- If you want to see diagrams laid out, read `/docs/diagrams/homelab_flow_diagrams.md`




## Folder map

```text
homelab/
  README.md                         this file and the tracker
  .gitignore                        keeps secrets and big files out
  docs/
    devices.md                      inventory of sensors and devices
    naming-scheme.md                how to name rooms, devices, VMs
    network-plan.md                 addresses, DNS, router checks
    server-day-checklist.md         what to do when the hardware arrives
    home-assistant-study-guide.md   concepts, examples and a short quiz
    password-manager.md             short version, with a link to the full guide
    ssh-setup.md                    SSH key walkthrough for Windows
    marketplace-shopping.md         what to buy used, what to check, how to set alerts
  homeassistant/
    secrets.example.yaml            placeholders only, never real values
  esphome/                          ESPHome device files, later
  docker/                           compose files (Immich and others), later
```

## Rules

- Never commit `secrets.yaml`, passwords, API keys, Wi-Fi passwords, or Z-Wave PINs, QR codes and DSKs. Those go in the password manager.
- Commit small and often. One change, one commit, with a message that says what changed.
- Barcodes (EAN numbers), FCC IDs and model names are fine to commit. They are public product IDs.
- **Never use the Z-Wave smart plug with an EV charger.** GM says not to use wall adapters or extension cords of any kind with its charge cord, and the plug's documented load types do not include EV charging. See `docs/devices.md`.

## Pre-hardware tracker

Status key: `[x]` done, `[ ]` to do, `[-]` skipped on purpose.

### 1. Check the two sensors

- [x] Sensor 1 identified: Shelly BLU H&T ZB (Zigbee and Bluetooth temperature and humidity sensor), barcode 3800238072414
- [x] Sensor 2 identified: Shelly Wave Plug US (Z-Wave smart plug with power metering), FCC ID 28DC6-WAVEPLUGUS
- [x] Z-Wave region confirmed: the plug is a US version at 908.4 MHz, so buy a US Z-Wave stick
- [ ] Photograph each box and label. For the plug, photograph the Z-Wave DSK label (it has the 5-digit PIN). Store the photos in the password manager, not in git
- [ ] Do not pair either device to anything else yet
- [ ] Read the safety note in `docs/devices.md` before choosing what to plug into the Wave Plug

### 2. Practice on the PC

- [-] Skipped on purpose: no VM managers on the PC
- [ ] Read `docs/home-assistant-study-guide.md` instead, and try the quiz at the end

### 3. Prep the repo

- [x] Local repo and private GitHub repo set up
- [ ] Commit this starter set of files
- [ ] Fill in the rooms in `docs/devices.md`
- [ ] Agree the room and device names in `docs/naming-scheme.md`
- [ ] Fill in the subnet and reservations in `docs/network-plan.md`

### 4. Router check

- [ ] Log into the router and confirm it can reserve a fixed address (DHCP reservation)
- [ ] Confirm it lets me change the DNS server it hands out
- [ ] Note the router's model and any limits in `docs/network-plan.md`

### 5. Photo move prep

- [ ] Check library size in Google Photos or Takeout, to confirm the 1 TB estimate
- [ ] Delete obvious junk and duplicates first
- [ ] Do not start a full Takeout until the photo drive and backup drive exist

### 6. Housekeeping

- [ ] Pick and set up a password manager (`docs/password-manager.md` and the full guide)
- [ ] Generate an SSH key and add it to GitHub (`docs/ssh-setup.md`)
- [ ] Work through the Home Assistant study guide
- [ ] Set up Marketplace and eBay alerts from `docs/marketplace-shopping.md`

## Related docs

- Homelab plan for beginners (Claude Doc): https://claude.ai/artifact/3WD3cZwmQWyBf69dWfvY4i
- Home system guide for everyone (Claude Doc): https://claude.ai/artifact/EK3qYxEh1GnvbNwvT8KciU
- Password manager guide for the homelab (Claude Doc): https://claude.ai/artifact/JF3u9Fiz3Z7crv5aYFtN6B
