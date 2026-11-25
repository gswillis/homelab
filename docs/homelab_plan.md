# Homelab plan for beginners

Prepared November 24, 2026. Prices are ballpark US figures, so check current listings before buying.

## The plan in short

One small, quiet computer does everything. Proxmox is the operating system on it. It lets you carve the machine into separate virtual machines and containers, one each for Pi-hole, Home Assistant and the photo app. If you break one while learning, the others keep running.

### Software choices

- **Pi-hole** runs in a lightweight container (LXC). It is tiny.
- **Home Assistant** runs as a full VM using the official HAOS image. This is the easiest and best-supported route.
- **Immich** is the best Google Photos replacement. It has phone apps with auto-upload, face and object search, and shared albums. It runs in a Docker container inside a small VM.
- **No remote access, by design: nothing here is reachable from outside your hom**e, so you never open ports on your router.

### Three things worth knowing up front

1. **RAID or mirrored drives are not a backup.** Follow the 3-2-1 idea: the live copy, a second copy on an external drive, and an encrypted cloud copy (Backblaze B2 costs a few dollars a month for a typical library). Your family photos are the one thing here you cannot replace.
2. **Pi-hole is a single point of failure.** If the server reboots, your whole house loses DNS and the internet seems down. Set a fallback DNS in your router, and do not experiment on the box while others are streaming.
3. **Wire the server with Ethernet.** Wi-Fi works badly for servers.

## How it connects

- Internet → ISP modem → router and Wi-Fi. The router hands out Pi-hole as the DNS server.
- Router → Ethernet cable → the Proxmox server (one mini PC).
- Inside the server: Pi-hole (ad blocking), Home Assistant (smart home hub), Immich (photos and video) and room for extras like Jellyfin or Vaultwarden later.
- Hardware inside the server: an NVMe SSD for Proxmox and the VMs, a hard drive for the photo and video library, and a Zigbee stick and a Z-Wave stick as the smart home radios.
- Backups: the hard drive is copied to an external USB drive (local copy) and to an encrypted cloud backup (offsite copy).
- Your devices (phones, TVs, PCs) connect through the router.

## Buying in order

| Step | Item | What to look for | Approx. cost |
| --- | --- | --- | --- |
| 1. Required | Server | Used small-form-factor desktop (HP EliteDesk or ProDesk, Dell OptiPlex, 8th-gen Intel i5 or newer) or an Intel N100/N305 mini PC, with 16 GB RAM and a 500 GB NVMe SSD. If RAM or SSD is not included, add them | $250–400 |
| 1. Required | Ethernet cable | Cat6, long enough to reach the router | $5–10 |
| 2. Before moving photos | Photo drive | 4 TB NAS-rated HDD (WD Red Plus or Seagate IronWolf). Your 1 TB library fits with room to grow. The case needs a 3.5" bay | $90–120 |
| 2. Before moving photos | Backup drive | External USB HDD, 4 TB or larger, unplugged between backups | $90–110 |
| 3. Before trusting it | UPS | Line-interactive, 600–850 VA, pure sine wave, with a USB data port. See the UPS section | $70–120 |
| 4. Smart home radios | Zigbee stick | Home Assistant Connect ZBT-2 or similar, for your Zigbee devices | $30–50 |
| 4. Smart home radios | Z-Wave stick | Home Assistant Connect ZWA-2 or similar. Buy the version for your region (US Z-Wave runs on a different frequency than Europe's) | $40–65 |
| 4. Smart home radios | USB extension cables (2) | Keeps the sticks away from the server's USB 3 ports, which can interfere with the radios | $5–10 each |
| 5. Displays, buy last | Control display | Tablet, or a Raspberry Pi with a touchscreen. See the Displays section | $50–100 |
| 5. Displays, buy last | Info display | E-ink panel, ESP32 touch panel or a second read-only tablet. See the Displays section | $50–150 |
| Optional | 8-port gigabit switch | Only if your router is out of ports | about $20 |
| Optional | Cloud backup | Encrypted offsite copy, e.g. Backblaze B2 | a few dollars a month |

Steps 1–3 come to about $505–760. With the two radio sticks and the optional items the build lands near $600–900. Adding both displays brings it to roughly $650–1,150, which is under your $1,200 ceiling but close at the top end, so go lean on the info display if you want a buffer. Zigbee and Z-Wave are different radios, so you need one stick for each, and you can add the second later. Do not move your photos off Google until the backup drive is set up, because one drive can fail.

You can start with your existing ISP router. A managed switch, VLANs and a dedicated firewall like OPNsense or UniFi can come later, once you are comfortable.

## Security with no outside access

Not needing remote access removes most homelab risk, because the common problems come from exposing things to the internet.

- **Never forward ports on your router.** Turn off UPnP too, since it can open ports without you knowing.
- **Skip remote-access tools.** No Tailscale and no Home Assistant paid cloud link, so the server is not reachable from outside your home.
- **Use unique passwords.** Give Proxmox, Pi-hole, Home Assistant and Immich each a strong password from a password manager. Anyone on your Wi-Fi can reach those login pages.
- **Lock down your Wi-Fi.** Use WPA2 or WPA3 with a strong password, and put visitors on a guest network.
- **Keep it updated.** Proxmox, Home Assistant and Immich all ship security fixes, so check for updates about once a month.
- **Protect your backup.** Keep the external drive unplugged except when backing up, so one mistake or ransomware on a machine cannot hit both copies.

The trade-off: Immich uploads from your phone only while you are on your home Wi-Fi, so photos sync when you walk in the door. A separate network for smart-home devices (a VLAN) is a good later upgrade but not needed to start.

## Displays

Plan two displays: one touchscreen for controlling things and one for glancing at information. Buy them last, because they only work once Home Assistant is running and your sensors are paired. Prices are approximate.

| Option | Best for | Pros | Cons | Approx. cost |
| --- | --- | --- | --- | --- |
| Tablet (old iPad, old Android, or a basic new Android) | Control | Cheap or free, crisp touchscreen, Fully Kiosk Browser makes an always-on dashboard easy | Old iOS versions lose app support. A battery left plugged in 24/7 can swell | $0–100 |
| E-ink panel | Info | Very low power, readable in sunlight, no glow at night | Slow refresh, no real touch control, limited color, mostly DIY | $50–150 |
| ESP32 touch panel (ESPHome) | Control or info | Very cheap, fully local, low power, no battery | Small screen of a few inches, DIY setup, simple layouts | $20–60 |
| Raspberry Pi with a touchscreen | Control | No battery, fully local, runs a normal dashboard in a browser | More setup work, and you buy the Pi, screen, case and power supply | $100–200 |
| In-wall PoE touch panel | Control | One cable for power and data, built for 24/7 use, clean look | Most expensive, needs a cable run, and some models rely on the maker's cloud | $150–300 |
| LED matrix clock (such as a Ulanzi TC001 with Awtrix firmware) | Info | Cheap, glanceable, can run locally with custom firmware | Tiny display, scrolling text only, needs reflashing | about $50 |
| Spare monitor or TV with a small Raspberry Pi | Info | Big and readable across a room, reuses hardware | Wastes power if always on, and OLED panels can burn in | $50–100 |
| Google Nest Hub or Echo Show | Not recommended | Easy to set up | Sends data to the maker's cloud, which conflicts with your local-only goal | $50–250 |

### Recommended split

- **Control display:** a touchscreen tablet, wired to power in a spot you pass often, like the kitchen or entryway. If you want no battery at all, use a Raspberry Pi with a touchscreen instead.
- **Info display:** an e-ink panel built with ESPHome, which stays fully local. A cheap ESP32 touch panel or a second read-only tablet does the same job with less effort.
- **Order:** start with the control display, since it is useful right away. Add the info display later as a project.

### Display tips

- **Protect tablet batteries.** Put the tablet on a smart plug and have Home Assistant charge it only between about 40% and 80%. A lithium battery held at 100% for years is the main hardware fire risk in this plan.
- **E-ink suits information, not control.** It is good for weather, who is home, door and window status and a calendar. It is poor for tapping lights on and off.
- **Stay local.** Avoid displays that need the maker's cloud service. Check this before buying.
- **Dim at night.** Set screens to dim or turn off after dark so they do not glow in a hallway.

### Buying a control tablet on Marketplace

The tablet only has to run one dashboard, so you can aim at a narrow target. A reasonable price is about $50–100 for a decent older model. Used prices vary by model and region, so compare several listings. At that price a new budget Android or Fire tablet can be a better deal, so compare the two before committing.

**What to look for**

- **Screen size:** 8–10 inches. Smaller is hard to tap, and bigger is awkward on a wall.
- **iPads:** a standard iPad (not the cheapest, oldest ones), an iPad mini or an iPad Air from a recent-ish generation. The key is that it still gets a current iPadOS. Before you buy, look up the model's latest supported version and compare it with what the Home Assistant app requires.
- **Android:** a Samsung Galaxy Tab A or S series, a Lenovo Tab or a similar mainstream model. Avoid no-name brands, which are often stuck on old software with no updates.
- **Amazon Fire tablets:** cheap and they work well with Fully Kiosk Browser. They push Amazon ads and a lock-screen layer you will want to switch off. A new Fire HD on sale can beat a used tablet, since you get a fresh battery and a warranty.
- **Skip cellular models.** You do not need the extra cost or the blacklist risk.
- **Accessories:** budget for a charging cable and a mount. The tablet must stay plugged in.

**What to check before you pay**

- **Locks:** make sure the seller has signed out of their account. An iPad tied to an Apple ID (Activation Lock) or an Android tied to a Google account (factory-reset protection) can be unusable. Ask them to do a full reset in front of you, and walk away if they cannot.
- **Battery swelling:** look for a screen lifting at the edges, a bulging back or a case that does not sit flat. Do not buy a swollen one, because it is a safety risk.
- **Battery behavior:** run it off the charger for a few minutes and check that the percentage does not plunge. Heavy wear matters less here, because the tablet will live on power.
- **Screen:** check for dead pixels, burn-in marks and cracks. Open a white page and a black page, and swipe across every corner to check for dead or phantom touches.
- **Charging port:** wiggle the cable gently. A loose port gets worse over time.
- **Software:** confirm the OS version in Settings, and make sure it can run the Home Assistant app or a modern browser.
- **Ownership:** for an iPad, check the serial number on Apple's site. Meet in a public place and bring your own charger.

## UPS and battery backup

A UPS only needs to keep the server running long enough to shut down safely. A small one does that.

- **Type:** line-interactive. This is the standard home choice. Online (double-conversion) units are louder, hotter and pricier than you need.
- **Size:** 600–850 VA is plenty. The server, hard drive and router together probably draw about 40–70 W. Check the maker's runtime chart rather than the VA number, and expect roughly half an hour to an hour.
- **Waveform:** choose pure sine wave if the server is a full-size or small-form-factor desktop, because some modern power supplies dislike cheaper simulated output. A mini PC with a power brick is usually fine either way.
- **USB data port:** make sure it has one, so the server can talk to the UPS.
- **Plug in the router and modem too.** Otherwise the internet drops in an outage even though the server stays up, and Pi-hole cannot answer anything.
- **Set up automatic shutdown.** The free tool NUT (Network UPS Tools) lets Proxmox shut everything down cleanly when the battery runs low. Home Assistant also has an integration to show battery status. This turns a UPS into real protection.
- **Keep heaters and laser printers off it.** They draw too much power.
- **Expect to replace the battery every 3–5 years.** Buy a model with a user-replaceable battery.
- **Optional:** a small 12 V mini DC UPS (about $30–60) can power just the router or modem, if you want the internet to stay up without a full UPS.
- **Brands to compare:** APC, CyberPower and Eaton. Prices vary a lot, so check current listings.

## How future-proof is it

This build should serve a home for years, with a few known limits. A sensible upgrade path is a second mini PC for redundancy, or a dedicated NAS if your library outgrows one drive, without redoing what you have built.

### What holds up well

- **The software is portable.** Proxmox runs on ordinary x86 hardware, so you can restore your VMs onto a newer machine if the server dies or you outgrow it. Immich and Home Assistant keep their data in standard formats, so you are not locked in.
- **The hardware is upgradeable.** RAM and SSDs are swappable, and an 8th-gen or newer Intel CPU handles this workload for years.
- **Storage has headroom.** A 4 TB drive gives your 1 TB library about four times the space. Growing past that means swapping in a bigger drive and copying over.
- **The radios are mature standards.** Zigbee and Z-Wave are widely supported, and the Home Assistant Connect sticks can be updated through firmware. Check the exact model for Thread and Matter support when you buy.
- **Gigabit Ethernet is plenty.** Nothing here comes close to saturating it.

### Where it will pinch

- **Single drive, no redundancy.** The plan relies on backups rather than a mirror. If you want a mirrored pair later, you need a case with two 3.5" bays, so check this before buying if it matters to you.
- **The first photo import is slow.** Immich's face recognition and video transcoding are the heaviest load, and an older CPU will take a while on your 1 TB library. It is a one-time cost, and new photos are light work afterward.
- **Used hardware has age.** Expect to replace the server in roughly 5–8 years.
- **One box is a single point of failure.** If it dies, the house loses Pi-hole and Home Assistant until you restore from backup. A second small machine later can reduce that.

## Setup and organization tips

A little structure up front saves the most pain later. Most homelab trouble comes from forgetting what you changed, not from the hardware.

### Before the hardware arrives

- **Check your router.** Log in and confirm it can reserve a fixed address for a device (often called a DHCP reservation) and lets you change the DNS server. Pi-hole needs both. If it cannot, you may need a different router or a small change in how Pi-hole is set up.
- **Make a USB installer.** You need a second computer, a USB stick of 8 GB or more, and a free tool such as balenaEtcher to put the Proxmox installer on it.
- **Pick a spot for the server.** Choose a place near the router with airflow and little dust, away from heat. Plan to plug it into the UPS.
- **Start a notes doc.** Record the server's fixed address, what each VM or container is for, and what you changed and when. Do not store passwords there. Use a password manager.

### First-day settings

- **Turn on virtualization in the BIOS.** It is usually called Intel VT-x or Virtualization Technology. Proxmox will not run VMs without it.
- **Set the server to power on after a power cut.** The BIOS setting is often called "AC power recovery" or "restore on AC power loss." Without it, the server stays off after an outage.
- **Give the server a fixed address.** Reserve one in your router, and use the same address inside Proxmox.
- **Update the BIOS if it is very old.** Do this once, before you put your data on the machine.

### Keeping it organized

- **Use clear names.** Name each VM or container for its job, such as `pihole`, `homeassistant` and `immich`, rather than defaults like `ct101`.
- **Use a number pattern for IDs.** For example, 100s for infrastructure, 200s for apps and 900s for tests. It keeps the Proxmox list readable as it grows.
- **Put notes on each VM.** Proxmox has a notes field for each one. Write what it does and any special setup.
- **Keep photos and system data on separate drives.** The SSD holds Proxmox and the VMs, and the hard drive holds the photo library. A problem with one does not touch the other.
- **Label the hardware.** Mark the server, the UPS and each cable at both ends, and write which USB stick is Zigbee and which is Z-Wave.

### Working safely

- **Change one thing at a time.** Test after each change, so you know what broke something.
- **Take a snapshot before risky changes.** Proxmox can snapshot a VM in seconds, and rolling back is quick. Do it before updates and before trying new settings.
- **Back up the VMs themselves.** Proxmox can schedule backups of whole VMs to the external drive, which is separate from the photo backup.
- **Test a restore early.** Restore one small backup to confirm it works before you depend on it.
- **Check drive health.** Proxmox shows SMART health for each drive. Look at it monthly and replace a drive that reports warnings.
- **Do not experiment on the live box.** Pi-hole and Home Assistant run your home, so try new ideas in a separate test VM.

### Moving your Google photos

- **Use Google Takeout to export.** Export your library in large chunks, and keep the original files until the import is checked.
- **Expect messy metadata.** Takeout stores dates and locations in separate files, so a plain import can scramble photo dates. Look for an import tool made for Takeout, such as the community tool immich-go, and check its current instructions before you start.
- **Import a small batch first.** Check that dates, albums and video playback look right before importing the rest.
- **Delete from Google last.** Wait until the photos are on the photo drive and on the backup drive.

## Source control

Use Forgejo on your own server as the main home for your configs and docs. Use a private GitHub repo as the starting home now, and as the offsite copy later. Forgejo is a lightweight, community-run fork of Gitea. It feels like GitHub and fits your local-first goal.

### Why Forgejo, with GitHub alongside

- **GitHub:** no setup, free private repos and offsite by default. Your files live on someone else's cloud, so keep secrets out.
- **GitLab:** powerful, but heavy to self-host and overkill for one person.
- **Gitea and Forgejo:** lightweight and GitHub-like. Forgejo is the community-governed fork. Check both projects' current status before you commit to one.

### Two rules

- **Do not let the only copy live on the server it describes.** If the server dies, the repo holding your configs dies with it. Back up the Forgejo VM like your others, and mirror the repos to a private GitHub repo. Forgejo can push mirrors automatically.
- **Keep secrets out of git.** Home Assistant's `secrets.yaml`, API keys, passwords and Wi-Fi credentials go in a `.gitignore` file. Commit a `secrets.example.yaml` with fake values so you remember what is needed.

### What belongs in git

- **Yes:** Home Assistant YAML (configuration, automations, dashboards), ESPHome files, Docker compose files (for Immich), your notes and docs, Pi-hole settings exports and your setup checklist.
- **No:** the backups themselves, which are large binary files that may contain secrets. They stay on the backup drive and the cloud backup. Photos and videos stay out too.
- **Layout:** start with one `homelab` repo with folders such as `homeassistant/`, `esphome/`, `docker/` and `docs/`. Split it later if you want.

### Start now, before you have a server

1. **Install Git.** On Windows, download Git for Windows from git-scm.com, or run `winget install --id Git.Git -e` in PowerShell. On a Mac, run `xcode-select --install`. On Linux, run `sudo apt install git`. Close and reopen the terminal, then run `git --version` to check.
2. **Tell Git who you are.** Run these two commands once, with your own name and email:

   ```powershell
   git config --global user.name "Your Name"
   git config --global user.email "you@example.com"
   ```
3. **Make the folder and the repo.** Run these commands:

   ```powershell
   mkdir homelab
   cd homelab
   git init -b main
   ```
4. **Add your first files.** In that folder, use a text editor to create a `README.md` and a `.gitignore` file. Put these lines in `.gitignore`:

   ```text
   secrets.yaml
   .env
   *.key
   ```
5. **Make the first commit.** From inside the folder:

   ```powershell
   git add .
   git commit -m "First commit"
   ```
6. **Create a private GitHub repo.** Make it empty, with no README. Then connect your folder to it and upload. Replace the address with the one GitHub shows you:

   ```powershell
   git remote add origin https://github.com/YOUR-USERNAME/homelab.git
   git push -u origin main
   ```
7. **Repeat as you work.** Run `git add .`, then `git commit -m "what changed"`, then `git push`. On Windows, a sign-in window opens in your browser the first time you push.

### Which terminal to use

Use PowerShell on your Windows PC. Windows Terminal is a nicer window for it. Git commands are the same on Windows, Mac and Linux. You will meet a Linux terminal later, when you connect to the server with SSH, but you do not need it to start.

### Setting up GitHub

- **Secure the account first.** Turn on two-factor authentication under Settings, then Password and authentication. A passkey or an authenticator app is better than SMS.
- **Keep your email private.** Under Settings, then Emails, turn on "Keep my email addresses private". Use the `noreply` address GitHub gives you for your commits: `git config --global user.email "YOUR-NOREPLY-ADDRESS"`.
- **Create the repo.** Name it `homelab` and set it to **Private**. Leave "Add a README", ".gitignore" and "license" unchecked, because you already have those files locally and a non-empty repo causes push errors.
- **Connect and push.** Use `git remote add origin ADDRESS` and `git push -u origin main`, or do it in VS Code (next section).
- **If a secret is ever committed,** treat it as leaked. Change the credential first, then clean up the repo. Deleting the file later does not remove it from history.

### Using VS Code instead of the terminal

VS Code works on the same repo, so terminal commands and the Source Control panel can be mixed freely.

- **Open the folder.** Use File, then Open Folder, and pick `homelab`.
- **Commit.** The Source Control icon in the left bar lists changed files. Stage with the `+`, type a short message and click Commit.
- **Upload.** Click Sync Changes or Push to send commits to GitHub.
- **Publishing from VS Code.** If you use "Publish to GitHub", choose Private when it asks, and check the repo name it creates.
- **Check before you commit.** Glance at the staged files and make sure `secrets.yaml` or any other sensitive file is not in the list.
- **Check your commit email.** Run `git config --global user.email` in the VS Code terminal and confirm it shows your `noreply` address.
- **YAML extensions.** The "YAML" extension from Red Hat checks YAML syntax, and the "Home Assistant Config Helper" extension helps with Home Assistant files. They catch indentation mistakes early. Check the Extensions panel for current options.

### If the browser sign-in closes right away

The window closing quickly does not always mean failure. Work through these in order:

1. **Check if it worked.** Run `git push -u origin main` again. "Everything up-to-date" or files uploading means you are done.
2. **Clear old saved logins.** Open Windows Credential Manager, choose Windows Credentials, and remove entries that start with `git:https://github.com`. Try again.
3. **Confirm the sign-in helper.** `git config --global credential.helper` should print `manager`. If it is blank, set it with `git config --global credential.helper manager`.
4. **Use a code instead of a pop-up.** Try `git config --global credential.gitHubAuthModes device`, then push and enter the code at github.com/login/device. Check the Git Credential Manager docs if the setting name errors.
5. **Use the GitHub CLI.** Run `winget install --id GitHub.cli -e`, reopen PowerShell and run `gh auth login`. Or sign in through VS Code.

### If GitHub does not show your files

- **Check you made a commit.** `git log --oneline` should list at least one. Staged files that are not committed do not upload.
- **Check you pushed.** `git status` saying you are ahead of origin, or a Sync Changes button with a number, means commits are still local.
- **Check the remote.** `git remote -v` should show your GitHub repo address. A wrong address sends files somewhere else.
- **Check the branch.** Use the branch dropdown on the repo page. If your files are on `master` or another branch, switch to it or run `git branch -M main` and push again.
- **Check which repo you are looking at.** If VS Code published a new repo, it may have a different name or visibility than the one you created by hand.
- **Refresh the page.** An empty repo page can show old setup instructions until you reload.

### Moving to Forgejo later

Once the server runs, install Forgejo in its own container or VM and create a private repo there. Add it as a second remote with `git remote add forgejo ADDRESS`, then push to it. Keep GitHub as the offsite mirror. Your history moves with the repo.

## Suggested build order

1. Install Proxmox and give the server a fixed IP address.
2. Add Pi-hole and point your router's DHCP at it, with a fallback.
3. Add Home Assistant, then your Zigbee stick and devices.
4. Add Immich, test with one phone, then import your Google Takeout.
5. Set up backups and work through the security checklist before you trust it with everything.

## Decisions and open questions

- Budget ceiling: $1,200, which leaves room for every optional item.
- Library size: about 1 TB today. A 4 TB drive gives about four times that, and the backup drive must be at least as large.
- Smart home radios: your devices use both Zigbee and Z-Wave, so the plan includes one stick for each.
- Displays: one for controlling things and one for information. You have no spare tablet, so you plan to look on Marketplace.
- Source control: Forgejo on the server, with a private GitHub repo as the starting home and later the offsite mirror.
