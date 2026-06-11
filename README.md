# AYN Thor Config

Obtainium and Cocoon configuration for the [AYN Thor](https://www.ayn.hk/products/thor) — a dual-screen Android 13 handheld (Snapdragon 8 Gen 2).

Sideloaded apps don't auto-update. [Obtainium](https://github.com/ImranR98/Obtainium) fixes that by tracking each app's own release page. This repo is the version-controlled source of truth for that setup: every emulator and companion app on the device, each pointed at its canonical release source.

## Contents

| File | Purpose |
|---|---|
| `obtainium.json` | Full Obtainium app list (20 apps) |
| `cocoon/xbox360-platform.json` | Custom Cocoon platform: Xbox 360 via X360 Mobile |

## Import

Obtainium → **Settings → Import/Export → Obtainium Import**, then pick the file (or paste the raw URL of `obtainium.json` from this repo).

Apps already on the device are recognized by package id; everything else shows as available to install.

## App list

### Emulators

| App | System | Source | Notes |
|---|---|---|---|
| [Azahar](https://github.com/azahar-emu/azahar) | 3DS | GitHub | Citra successor |
| [Cemu](https://github.com/SapphireRhodonite/Cemu) | Wii U | GitHub | SapphireRhodonite's dual-screen fork |
| [Dolphin](https://dolphin-emu.org/download/) | GameCube / Wii | Official site | |
| [DuckStation](https://github.com/stenzek/duckstation) | PS1 | Mirror | Official Android APK isn't published on GitHub releases; tracked via community mirror |
| [Eden](https://git.eden-emu.dev/eden-emu/eden) | Switch | Dev-hosted Forgejo | See [source durability](#source-durability) below. `optimized` build filtered for the Thor's SoC |
| [EmuCoreV](https://github.com/sashkinbro/EmuCoreV) | PS Vita | GitHub | Custom UI over a Vita3K core; standard (non-parallel) build |
| [MelonDualDS](https://github.com/SapphireRhodonite/melonDS-android) | DS | GitHub | Dual-screen fork — see [notes](#melondualds--ds-on-two-screens) below |
| [NetherSX2 Classic](https://github.com/Trixarian/NetherSX2-classic) | PS2 | GitHub | AetherSX2 continuation |
| [PPSSPP](https://www.ppsspp.org/download) | PSP | Official site | |
| [RetroArch](https://buildbot.libretro.com/stable) | Multi | libretro buildbot | AArch64 build |
| [RPCSX](https://github.com/RPCSX/rpcsx-ui-android) | PS3 | GitHub | |
| [Vita3K](https://github.com/Vita3K/Vita3K-Android) | PS Vita | GitHub | |
| [X360 Mobile](https://github.com/Ashnar2602/X360-Mobile---OFFICIAL) | Xbox 360 | GitHub | Experimental; paired with the Cocoon platform config |

### Frontend, PC gaming & utilities

| App | Role | Source | Notes |
|---|---|---|---|
| [Cocoon](https://github.com/inssekt/CocoonFE) | Dual-screen launcher | GitHub | [cocoon-shell.com](https://cocoon-shell.com/) |
| [GameNative](https://github.com/utkarshdalal/GameNative) | PC games (Steam/Epic/GOG) | GitHub | |
| [GameHub Lite](https://github.com/Producdevity/gamehub-lite) | PC games | GitHub | Upstream ships under the spoofed package id `com.antutu.ABenchMark` — that's intentional, not an error |
| [Steam Shortcut Generator](https://github.com/NaviVani-dev/Steam-Shortcut-Generator) | Steam shortcuts for frontends | GitHub | |
| [O2P Tweaks](https://github.com/FeralAI/o2ptweaks.app) | Device tweaks | GitHub | |
| [AdrenoToolsDrivers](https://github.com/K11MCH1/AdrenoToolsDrivers) | GPU drivers | GitHub | Track-only: notifies of new Adreno driver releases, installed manually inside each emulator |
| [Obtainium](https://github.com/ImranR98/Obtainium) | App updater | GitHub | Updates itself |

## MelonDualDS — DS on two screens

The official [melonDS Android](https://github.com/rafaelvcaetano/melonDS-android) app doesn't drive both Thor screens. [MelonDualDS](https://github.com/SapphireRhodonite/melonDS-android) is the community fork built for dual-screen devices (AYN Thor, AYANEO Pocket DS) and is the version recommended by the [Retro Game Corps dual-screen guide](https://retrogamecorps.com/2025/10/27/dual-screen-android-handheld-guide/).

Two things worth knowing:

- **It's a separate app.** Since v0.6.1 the fork uses its own package id (`me.magnum.melondualds`), so it installs alongside official melonDS rather than replacing it, and has standalone RetroAchievements support.
- **Releases ship as prereleases.** All 0.7.0 builds are RC-tagged, so this config enables *Include prereleases* — otherwise Obtainium pins you to 0.6.1 (March 2026) forever.

Recommended settings on the Thor:

| Setting | Value |
|---|---|
| Renderer | OpenGL |
| Internal resolution | 4x |
| Dual screen preset | Internal: Top, External: Bottom |
| Soft input behavior | Always Invisible |

## Source durability

DMCA-prone emulators (Switch especially) should always be tracked from the developer's own git host, never a GitHub mirror. Eden is configured against `git.eden-emu.dev` (Forgejo) — its GitHub releases mirror has a history of DMCA/451 takedowns and was the main cause of breakage in earlier versions of this config.

For the same reason, avoid bulk-import packs that re-add GitHub mirrors on every sync.

## Cocoon platform configs

Cocoon has no built-in Xbox 360 platform. `cocoon/xbox360-platform.json` adds one wired to X360 Mobile's launch activity (`emu.x360.mobile`), accepting `iso/xex/zar/zip/7z`. Import it from Cocoon's platform settings.
