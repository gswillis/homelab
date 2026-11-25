# SSH key setup on Windows

An SSH key lets you connect to GitHub, and later to your server, without typing a password each time. The key has two halves: a **private key** that stays on your PC and is never shared, and a **public key** that you give to other systems.

## 1. Check that SSH is available

Open PowerShell and run:

```powershell
ssh -V
```

If it prints a version, you have it. If not, turn on "OpenSSH Client" under Windows optional features.

## 2. Make a key

```powershell
ssh-keygen -t ed25519 -C "homelab"
```

- Press Enter to accept the default location (inside `.ssh` in your user folder).
- Set a passphrase when asked. It protects the key if someone copies your files. Save the passphrase in your password manager.

This creates two files: `id_ed25519` (private, never share) and `id_ed25519.pub` (public, safe to share).

## 3. Let Windows remember the key

Run PowerShell as Administrator once:

```powershell
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
```

Then, in a normal PowerShell window:

```powershell
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

## 4. Add the public key to GitHub

Copy the public key to the clipboard:

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard
```

On GitHub, go to Settings, then SSH and GPG keys, then New SSH key. Give it a title like "Windows PC", paste the key, and save. Make sure you paste the `.pub` file, not the private one.

## 5. Test

```powershell
ssh -T git@github.com
```

The first time, it asks you to confirm GitHub's fingerprint. Check it against the fingerprints GitHub publishes in its documentation, then type `yes`. You should see a greeting with your username.

## 6. Switch your repo to SSH

From inside the `homelab` folder:

```powershell
git remote set-url origin git@github.com:YOUR-USERNAME/homelab.git
git remote -v
git push
```

## Later: your server

When the server exists, copy your public key into the server's `authorized_keys` file, and log in with `ssh root@SERVER-ADDRESS` or a normal user. Once key login works, you can turn off password login on the server.

## Rules

- Never share, email, paste or commit the private key (`id_ed25519`). The `.gitignore` in this repo blocks `id_*` files for safety.
- Use a different key for each machine if you have more than one PC.
- If a private key is ever exposed, remove its public half from GitHub and the server, and make a new key.
