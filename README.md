# AYN Thor Config

Obtainium and Cocoon configuration for the [AYN Thor](https://www.ayntec.com/products/ayn-thor), a dual-screen Android 13 handheld (Snapdragon 8 Gen 2).

Sideloaded apps don't auto-update. [Obtainium](https://github.com/ImranR98/Obtainium) fixes that by tracking each app's own release page. This repo provides a curated Obtainium list for the Thor — emulators covering every major system the device handles plus the usual companion apps, each pointed at its canonical release source — and Cocoon platform configs for systems Cocoon doesn't ship with.

## Contents

| File | Purpose |
|---|---|
| `obtainium.json` | Full Obtainium app list (20 apps) |
| `cocoon/xbox360-platform.json` | Custom Cocoon platform: Xbox 360 via X360 Mobile |
| `cocoon/psvita-platform.json` | Custom Cocoon platform: PS Vita via Vita3K or EmuCoreV |
| `cocoon/ps3-platform.json` | Custom Cocoon platform: PS3 via RPCSX |

## Import

Obtainium → **Settings → Import/Export → Obtainium Import**, then paste the raw URL:

```
https://raw.githubusercontent.com/dreulavelle/ayn-thor-config/main/obtainium.json
```

Apps already on your device are recognized by package id; everything else shows as available to install. Nothing installs without your confirmation, so import the full list and skip what you don't want.

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
| [melonDS](https://github.com/rafaelvcaetano/melonDS-android) | DS | GitHub | Native dual-screen support since 2.0.0; see [notes](#melonds-on-two-screens) below |
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
| [O2P Tweaks](https://github.com/FeralAI/o2ptweaks.app) | AYN device tweaks | GitHub | Built for the Odin 2 line; runs on the Thor. Optional |
| [AdrenoToolsDrivers](https://github.com/K11MCH1/AdrenoToolsDrivers) | GPU drivers | GitHub | Track-only: notifies of new Adreno driver releases, installed manually inside each emulator |
| [Obtainium](https://github.com/ImranR98/Obtainium) | App updater | GitHub | Updates itself |

## melonDS on two screens

For DS games on the Thor, use official [melonDS Android](https://github.com/rafaelvcaetano/melonDS-android) 2.0.0 or newer. Older guides point to the [MelonDualDS fork](https://github.com/SapphireRhodonite/melonDS-android) because the official app couldn't drive both screens — that changed in April 2026, when rafaelvcaetano merged the fork's dual-screen work into 2.0.0 (AYN supplied test devices). This config tracks the official app.

The fork still exists and ships two exclusives as RC-tagged prereleases, a Vulkan renderer and per-ROM controller mapping, but its source is unpublished — the GitHub repo only distributes APKs. If you want those features, it installs alongside the official app under its own package id (`me.magnum.melondualds`).

Recommended settings on the Thor:

| Setting | Value |
|---|---|
| Renderer | OpenGL |
| Internal resolution | 4x |
| Dual screen preset | Internal: Top, External: Bottom |
| Soft input behavior | Always Invisible |

## Source durability

Track DMCA-prone emulators (Switch especially) from the developer's own git host, never from a GitHub mirror. Eden points at `git.eden-emu.dev` (Forgejo) because the GitHub releases mirror has a history of DMCA/451 takedowns — when a mirror dies, Obtainium silently stops updating that app.

For the same reason, avoid bulk-import packs that re-add GitHub mirrors on every sync.

## Cocoon platform configs

Cocoon consumes Daijishō-style platform configs (`databaseVersion: 14`). The ones in `cocoon/` add platforms Cocoon doesn't ship with, each validated against the emulator's exported activities and intent extras:

| Platform | Player | Launch mechanism |
|---|---|---|
| Xbox 360 | X360 Mobile | `VIEW` intent with `{file.uri}`; accepts `iso/xex/zar/zip/7z` |
| PS Vita | Vita3K | `AppStartParameters` string-array extra: `-r <titleID>` |
| PS Vita | EmuCoreV | `com.sbro.emucorev.action.LAUNCH` with `--es titleId <titleID>` |
| PS3 | RPCSX | `rpcsx.intent.action.Emulator` with `--es path <game dir>` |

Import each from Cocoon's platform settings.

### `.dpt` stub files

Vita and PS3 games aren't loose ROM files — they're installed inside the emulator. The ROM folder instead holds Daijishō Player Template stubs (one per game) whose tags get substituted into the launch intent:

```
# Daijishou Player Template
[vita_game_id] PCSE00383
```

```
# Daijishou Player Template
[ps3_game_path] /storage/emulated/0/Android/data/net.rpcsx/files/config/games/BCUS98362
```

Vita title IDs come from the game's `param.sfo` (or a lookup tool like vitagameid); RPCSX game paths are listed in its `games.json` (`Android/data/net.rpcsx/files/games.json`).

Note: RPCSX does not boot `.iso` files. Install the game in RPCSX first (PKG or extracted disc folder), then point the stub at the installed path.
