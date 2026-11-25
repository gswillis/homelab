# Password manager (short version)

The full guide is a Claude Doc: https://claude.ai/artifact/JF3u9Fiz3Z7crv5aYFtN6B

## Pick one

- **Easiest:** Bitwarden Free or Proton Pass Free. Synced across PC and phone.
- **Most local:** KeePassXC. Your vault is a file on your PC. You handle sync and backups.
- **Later:** Vaultwarden on your own server, but only after backups are proven.

Check current pricing and plan limits on each vendor's site before signing up.

## First day

- Make a long master passphrase of five or six random words, unique to this vault.
- Write it down once and keep it somewhere physically safe.
- Turn on two-factor authentication for the vault, and keep the recovery codes on paper.
- Import browser passwords, then delete the browser copy and the export file.

## What to store

- Wi-Fi and guest Wi-Fi passwords
- Router admin login
- Proxmox, Pi-hole, Home Assistant, Immich and Forgejo logins
- GitHub login and recovery codes
- Jellyfin admin login
- SSH key passphrase
- Z-Wave PINs, QR codes and DSK labels (attach photos), including the Wave Plug US PIN
- Cloud backup login and encryption key

## Backups

- Keep a copy of the vault on the external backup drive, and a third copy away from home.
- Encrypt exports and delete plain-text exports right away.
- Test a restore once a quarter.

## Never

- Store passwords in git, in this repo, or in plain notes.
- Reuse a password between services.
- Paste passwords into chat.
