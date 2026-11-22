# Garry's Mod x86-64 + Git — Pterodactyl Egg

Garry's Mod dedicated server on the **x86-64** Steam branch, using **only official Pterodactyl images**, with optional Git deployment.

| | Image |
|---|---|
| Installer | `ghcr.io/pterodactyl/installers:debian` |
| Runtime | `ghcr.io/ptero-eggs/games:source` |

## How Git works without a custom image

The official runtime image does not ship `git`. During installation, the egg copies the installer's `git` binary and the libraries it needs into `/home/container/.egg/git`. On every start, `.egg/git-sync.sh` uses this bundled git to sync the repository before `srcds_run_x64` launches.

- **Install / Reinstall**: clones the repository set in *Git Repository* into *Git Folder*.
- **Start**: if *Git Auto Update* = 1, fetches the branch and runs `git reset --hard`. Tracked files are overwritten, untracked files (data, logs, etc.) are kept.
- Any Git failure is logged and **never prevents the server from starting**.
- Credentials are passed via `GIT_ASKPASS` and are never written to `.git/config` or the remote URL.

## Installation

1. Panel → Admin → Nests → Import Egg → `egg-garrysmod-x64-git.json`
2. Create a server with the egg.
3. (Optional) Set *Git Repository*, then **Reinstall** to deploy it.

To switch an existing server from the official Garry's Mod egg: variable names are the same (`STEAM_TOKEN`, `SRCDS_MAP`…). Change the egg, then reinstall to switch to the x86-64 branch and install the bundled git.

## Private repositories

Use a dedicated **read-only** token (GitHub fine-grained PAT with *Contents: Read-only*, or GitLab `read_repository`).

## Files

- `egg-garrysmod-x64-git.json` — the egg to import
- `scripts/install.sh` — readable copy of the installation script embedded in the egg
