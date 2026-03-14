# 📋 CachyOS_pkglist

A snapshot of all installed packages on this machine — auto-generated and kept up to date by the [`pkgsync`](https://github.com/Joslef/scripts/tree/main/pkgsync) script from the [`scripts`](https://github.com/Joslef/scripts) repository.

> **This repo is auto-generated. Do not edit the package list files by hand.**
> All changes are committed automatically by `pkgsync` on a schedule or on demand. See the [`pkgsync` README](https://github.com/Joslef/scripts/tree/main/pkgsync) for full documentation on how syncing works.

## 📦 Contents

| File | Source command | What it contains |
|------|----------------|------------------|
| `pkglist-native.txt` | `pacman -Qqen` | All explicitly installed packages from the official Arch/CachyOS repositories |
| `pkglist-aur.txt` | `pacman -Qqem` | All installed AUR and other foreign packages |

Each file is a plain newline-separated list of package names with no version pins, suitable for piping directly into `pacman` or an AUR helper.

## 🖥️ Restoring Packages on a New Machine

The easiest way to restore your packages is the built-in restore option in `pkgsync`. From the interactive menu, press `c`. pkgsync will:

1. Pull the latest `pkglist-native.txt` and `pkglist-aur.txt` from your configured GitHub repo.
2. Diff them against locally installed packages and show a count of what is missing.
3. Ask for confirmation before installing anything.
4. Install missing native packages via `sudo pacman -S --needed --noconfirm`.
5. Install missing AUR packages via `paru` or `yay` (whichever is detected). If neither is found, AUR packages are skipped with an error message.

Make sure your AUR helper (`paru` or `yay`) is installed before running the restore if you have AUR packages to recover.

**Manual alternative** — if `pkgsync` is not yet set up on the new machine, clone this repo directly and replay the lists:

```bash
# 1. Clone this repo
git clone https://github.com/Joslef/CachyOS_pkglist
cd CachyOS_pkglist

# 2. Install all native (official repo) packages
sudo pacman -S --needed - < pkglist-native.txt

# 3. Install AUR packages — requires an AUR helper (e.g. yay or paru)
yay -S --needed - < pkglist-aur.txt
```

## 🔄 How This Repo Is Updated

`pkgsync` runs on a user-level systemd timer and optionally on boot (30 seconds after login, once the network is up). Each run:

1. Regenerates `pkglist-native.txt` and `pkglist-aur.txt` from the current package state.
2. Checks whether either file has changed (`git diff`). If nothing changed, it exits silently with no commit.
3. Stages both files, commits with a `YYYY-MM-DD_HH-MM` timestamp as the message, and pushes to `origin main`.

## 🖥️ Requirements

- Arch Linux or an Arch-based distro (this machine runs CachyOS)
- [`pkgsync`](https://github.com/Joslef/scripts/tree/main/pkgsync) installed and configured to point at this repo
