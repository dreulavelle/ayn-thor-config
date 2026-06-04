# AYN Thor — Config Backup

Version-controlled backup of my AYN Thor (purple, 16GB, Snapdragon 8 Gen 2, Android 13) setup.

## Contents
- obtainium-config.json — cleaned Obtainium app list (28 apps) for keeping sideloaded emulators/apps up to date.

## Why this exists
Sideloaded apps (emulators, Cocoon, GameNative) don't auto-update — Obtainium tracks their release pages and updates them. This repo is the durable, version-controlled source of truth so the config can be re-imported any time it breaks.

## How to restore (private repo = file import)
1. Pull obtainium-config.json to the device (already pushed to /sdcard/backups/obtainium-config-cleaned.json).
2. Obtainium -> Settings -> Import/Export -> Import -> pick the JSON.

## Recommended manual fixes (do in Obtainium UI for correct per-source settings)
- **Eden**: re-add from the dev-hosted git **https://git.eden-emu.dev/eden-emu/eden** instead of the GitHub  /Releases  mirror (the mirror is DMCA/451-prone — the main cause of breakage).
- **RJNY "Obtainium Emulation Pack"**: consider removing — it auto-bulk-adds emulators (Winlator, duplicates) and re-bloats the list on each sync. With this repo as the source of truth, it's redundant.

## Durability rule
For DMCA-prone emulators (Switch especially), always point Obtainium at the developer's own git host, never a GitHub mirror.

_Last updated: 
