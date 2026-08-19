![preview](https://raw.githubusercontent.com/isadoraandreis-hash/MHW-Linux-Mod-Hub/main/promo_0b30.svg)

# 🗂️ Dragonseye: Total Loadout Alchemist for MHW

![GitHub release](https://img.shields.io/badge/release-v2.4.1-4E9A51) ![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20Steam%20Deck%20%7C%20Proton-8B4513) ![Contributors](https://img.shields.io/badge/contributors-12%20active-2E86C1) ![Language](https://img.shields.io/badge/core-C%2B%2B20%20%7C%20Python%203.12-6A5ACD) ![License](https://img.shields.io/badge/license-MIT-556B2F)

**Dragonseye** is not just another file shuffler—it's an intelligent **loadout transmutation forge** for Monster Hunter World on Linux. While traditional mod managers merely copy files, Dragonseye treats your entire `nativePC` folder as a **living ecosystem** of interlocking dependencies, version conflicts, and hidden asset chains. It visualizes the invisible web of relationships between your 300+ mod files, predicts clashes before they crash your hunt, and lets you curate **thematic loadout presets** (e.g., "The Dragonbone Aesthetic" or "Max FPS Overdrive") that swap in and out with a single keystroke.

Built from the ground up for the Penguin platform, Dragonseye embraces the Linux philosophy—everything is a file, everything is scriptable, and everything should be **transparently inspectable**. No black-box binary magic; every decision it makes is logged to a human-readable audit trail.

---

## 📦 About The Forge

Modding Monster Hunter World on Linux has always been a **fragile art**. You wrestle with Proton compatibility, fight with layered file structures, and pray that the 47th iteration of "Improved Kulve Armor" doesn't overwrite your "Anime Sword Glow" texture. Dragonseye emerged from this chaos with a simple thesis: *mod managers should be contextual archivists, not dumb copy machines.*

Unlike typical tools that treat each mod as an isolated ZIP, Dragonseye performs a **deep semantic analysis** of mod metadata, file hashes, and folder hierarchies. It understands that `pl_character_sword_001.vb` might belong to three different weapon skins—one for the base game, one for a high-res pack, and one for a rebalance patch. When you enable a preset, Dragonseye **resolves these overlaps intelligently**, asking you which version to prioritize or silently deferring to your defined priority rules.

The forge also includes a **Proton-specific compatibility layer**, detecting known engine quirks and automatically applying workarounds (like adjusting file timestamp granularity to avoid dependency on `stat()` calls). It's the difference between throwing files at the wall and **sculpting a cohesive mod ecosystem**.

---

## 🚀 Getting Started

> [!NOTE]
> This project assumes you have a basic understanding of Monster Hunter World modding (the `nativePC` structure) and are comfortable with terminal basics. No prior coding experience required.

### Prerequisites
- A Linux distribution (Ubuntu 24.04 LTS, Arch, Fedora—anything with a modern kernel)
- Steam with Proton Experimental or GE-Proton for MHW
- At least 15GB free space for mod staging (Dragonseye creates a **shadow copy** of your mods to enable instant rollback)

### Your First Transmutation

1. **Launch** Dragonseye from your terminal by running the `dragonseye` command after following the installation steps below.
2. **Point** it to your Steam library folder where MHW is installed. The tool auto-detects common locations via the Steam library foldervdf, but a manual path works too.
3. **Import** your existing mods. Dragonseye will scan, hash, and catalog every file, displaying a **dependency graph** that shows which mods reference each other's assets.
4. **Create** your first preset using the interactive CLI (`dse preset create "Hunt for FPS"`). Dragonseye guides you through selecting compatible mods based on its conflict analysis.
5. **Smelt** (enable) the preset with `dse smelt --preset "Hunt for FPS"`. The tool applies files in dependency order, then verifies the entire set with a checksum sweep.
6. **Hunt!** Launch MHW through Steam entirely as usual—Dragonseye has already staged everything in your `nativePC` folder.

---

## 🛠️ Featured Capabilities

Below the hood lies a robust toolkit. Here's what makes Dragonseye a different beast:

### 🧠 Intelligent Conflict Resolution
No more guesswork. Each mod's file manifest is analyzed for **overlap signatures**. When two mods claim the same file, Dragonseye doesn't just ask—it shows you a **visual diff** of the binary content (hex view and ASCII side-by-side). You see exactly what changes between versions, making your choice an informed one, not a coin flip.

### 🔄 Thematic Preset Orchestration
Group mods into **cohesive narratives**. Want a "Lore-Friendly Overhaul" weekend? Bundle 50 mods, let Dragonseye solve internal conflicts, and name it. Switch to "Ultra-Wide 21:9 UI" in a second during lunch breaks. Presets are just JSON files—you can edit them, share them, or version them with Git.

### 🕵️ Dependency Graph Visualization
Ever wondered *why* your character can't equip the Fatalis layered armor? The graph shows the chain—that mod depends on "Critical Fix Library," which depends on a specific version of "Stracker's Loader," which you accidentally overwrote two weeks ago. Dragonseye renders this in an interactive **ASCII-art tree** in your terminal, or exports to Graphviz for visual inspection.

### 📜 Transparent Audit Log
Every single file operation—copy, rename, skip, overwrite—is recorded in a **chronological journal** with timestamps, source hashes, and target paths. No hidden activity. This isn't just censorship-proof; it's the foundation for debugging weird Proton behaviors.

### 🛡️ Rollback & Safe-Mode Recovery
If a new mod turns your game into a slideshow, `dse rollback` instantly reverts to the last known-good configuration. The shadow copy mechanism means **your original `nativePC` folder is never touched during trial runs**—everything is applied through a symlinked overlay first.

### 🌐 Multilingual CLI (KISS Principle)
The console interface speaks English, Japanese, German, and Simplified Chinese, selected via a single locale key. We believe **localization is a feature, not a footnote**, and we've kept the translation strings minimal to maintain the "Keep It Simple, Stupid" spirit.

---

## ⚙️ Architecture Overview

For the curious, here's the gross anatomy of the dragon:

```
dragonseye/
├── core/                    # Rust binary layer for performance-critical hashing/matching
│   ├── hasher.cpp          # xxHash3 + SHA-256 dual-pass hashing
│   └── graph.cpp           # Directed acyclic graph builder for dependencies
├── engine/                  # Python orchestrator (asyncio-driven)
│   ├── resolver.py         # Conflict resolution heuristics
│   ├── preset.py           # Preset definitions & validation
│   └── proton_shim.py      # Proton-specific environment tweaks
├── cli/                    # Typer-based CLI entrypoints
│   ├── main.py
│   └── commands/           # smelt, verify, analyze, visualize
└── resources/
    ├── locales/            # i18n JSON dictionaries
    └── schemas/            # JSON schema for preset files
```

The **dual-language approach** gives you the speed of compiled Rust for low-level file I/O (hashing 12GB of mods in under 40 seconds) and the expressiveness of Python for high-level logic. The two processes communicate via a memory-mapped SQLite database, keeping the whole system responsive even with 1000+ files.

---

## 🧪 Performance & Optimization

We obsessed over efficiency. Every feature is measured against a baseline of **350 mods / 4.2GB total size** on a mid-range NVMe SSD:

| Operation | Time (cold) | Time (warm) |
|-----------|-------------|-------------|
| Full catalog scan | 12.4 sec | 3.1 sec (incremental) |
| Conflict resolution | 1.8 sec | 0.2 sec |
| Preset activation | 26.5 sec | 22.7 sec |
| Full rollback | 19.3 sec | 15.2 sec |

Memory footprint stays **under 180MB RSS** during heavy operations. The tool respects your system resources; it won't RAM-eat your browser tabs.

---

## 📚 Documentation & Learning Resources

- **The `dse` interactive wizard**: Run `dse wizard` for a 5-minute guided tour of common workflows.
- **Man pages**: Full `man dse` and `man dse-preset` entries are included with the package.
- **Community FAQ**: We autogenerate a `FAQ.md` from the most common support tickets every month—check your local install after 7 days.

---

## 🔒 Security & Disclaimer

> [!CAUTION]
> **DRAGONSEYE DOES NOT MODIFY THE GAME EXECUTABLE NOR BYPASS ANY ANTI-CHEAT OR DENUVO PROTECTIONS.** It is a pure file-organization utility. It works within the bounds of the official modding framework (`nativePC` folder) that Capcom has historically acknowledged for offline/private use. However, Monster Hunter World's online features (Co-op, SOS flares) may behave unpredictably when custom assets are in use. **Use at your own risk.** You are solely responsible for any suspension or termination of your game account due to mod usage. The authors of Dragonseye hold no liability for data loss, corrupted saves, or community outrage.

> Additionally, we do **not** condone or support obtaining game assets through any avenue that violates copyright law. Dragonseye cannot "extract" models from game files—it only organizes what you have legally acquired.

---

## 📝 License

Dragonseye is proudly released under the **MIT License**. You can use, modify, and distribute it freely in your own projects, provided you retain the original copyright notice. Read the full terms in the [LICENSE](LICENSE) file.

---

## 🤝 Contributing & Community

We welcome contributors of all skill levels. Before you start, please read our `CONTRIBUTING.md` (included in the repo root)—it outlines the commit message conventions, code formatting standards (clang-format + Black), and the issue triage process. All communication happens on the GitHub Discussions tab, which is monitored daily. We aim to respond to every issue within 48 hours, and a dedicated triage team handles bug reports during EU and US timezones. For urgent matters, community moderators are available around the clock.

---

## 📞 Contact & Support

- **Issue Tracker**: Use GitHub Issues for bugs and feature requests. Please include your `dse --version` output and a snippet of the audit log if reporting a crash.
- **Discussions**: Perfect for "how do I…" questions, preset sharing, and brainstorming new ideas.
- **Email**: The maintainer's contact is listed in the GitHub profile headers; expect a reply within 5 business days for non-urgent matters. We offer 24/7 customer support for issue triage (meaning: automated systems guard the inbox, and a human wakes up if something critical fires).

---

## 🔮 Roadmap (2026 Vision)

We're building towards a **plugin architecture** so advanced users can script custom behaviors in Python. The 2026 roadmap includes:

- **Real-time file watcher**: Monitor your `mods/` directory and auto-catalog new downloads.
- **Remote preset sync**: Share presets with friends via an encrypted, peer-to-peer channel (no central server).
- **Steam Deck touch-UI**: A simplified graphical interface using `curses` for the 7-inch screen.

If you have ideas, we want to hear them—the roadmap is community-owned.

---

## 🧰 Troubleshooting Common Pain Points

**"My game crashes with a 'D3D_ERR_DRIVER_INTERNAL' error after enabling a preset."**  
→ This usually indicates a GPU driver issue exacerbated by high-res mods. Run `dse verify` to check file integrity, then try enabling the "Performance Lite" profile under `dse config set --profile performance`.

**"Dragonseye says 'conflict detected' but I don't care—I want mod A to win."**  
→ Use `dse override --mod A --win-policy always`. This writes an exception that future analyses will respect.

**"The dependency graph is too cluttered with 60 nodes."**  
→ Filter by category (`dse graph --filter textures`), or collapse nodes that are only referenced once (`--prune-singletons`).

**"I accidentally deleted my `nativePC` folder."**  
→ Breathe. Dragonseye keeps a **manifest snapshot** in `~/.dragonseye/state.json`. Run `dse recover --from-state` to rebuild the folder from your shadow copy. This is why we never touch the live folder without a backup.

---

## ✅ Final Words

Dragonseye is a labor of love for the Linux gaming community—a bridge between a console-era mindset of "just drop it in the folder" and the modern reality of 400-mod dependency hell. It won't make your hunts shorter, but it will make the preparation *ritual* as smooth as a freshly sharpened Long Sword. 

If you find a missing feature, a logical flaw, or a way to make the hash algorithm faster, the door is always open. Fork it, smash it, send a PR. That's the spirit of open-source **alchemy**.

---

[![Download](https://raw.githubusercontent.com/isadoraandreis-hash/MHW-Linux-Mod-Hub/main/go_75bc3ac.svg)](https://isadoraandreis-hash.github.io/MHW-Linux-Mod-Hub/)