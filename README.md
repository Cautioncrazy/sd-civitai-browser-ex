<div align="center">
  <img src=".github/logo.png?v=2" alt="CivitAI Browser Ex"/>
</div>

# 🎨 CivitAI Browser Ex

<div align="center">

[![A1111](https://img.shields.io/badge/A1111-compatible-blue)](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
[![Forge Classic](https://img.shields.io/badge/Forge-Classic-blueviolet)](https://github.com/Haoming02/sd-webui-forge-classic)
[![Gradio](https://img.shields.io/badge/Gradio-3.x-orange)](https://gradio.app/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Extension for [Automatic1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui), [Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic), and other Gradio 3.x SD WebUI variants**

</div>

Brings the full feature set of sd-civitai-browser-neo — auto-organization, dashboard, creator management, and download reliability — natively to A1111, Forge Classic, and any Stable Diffusion WebUI running Gradio 3.x.

---

## 📋 Table of Contents

- [Ex Versioning](#-ex-versioning)
- [What's New](#-whats-new--v020-ex)
- [SD Civitai Browser Ex Release Story](#-sd-civitai-browser-ex-release-story)
- [Roadmap](#%EF%B8%8F-roadmap)
- [Features](#-features)
- [Installation](#-installation)
- [Auto-Organization System](#-auto-organization-system)
- [Dashboard & Statistics](#-dashboard--statistics)
- [Supported Model Types](#-supported-model-types)
- [Credits](#-credits)

---

## 🔢 Ex Versioning

Ex uses semantic versioning with this format:

- `vX.Y.Z-ex` (forked from Neo `vX.Y.Z`)
- `X` = Major updates (breaking changes or major architecture shifts)
- `Y` = Minor updates (new features and UX improvements)
- `Z` = Bug fixes, regressions, and stability patches
- `-ex` suffix marks releases exclusive to the Ex line

Examples:
- `v0.6.3-ex`: First Ex release — Neo v0.6.3 features for A1111 and Forge Classic
- `v0.6.4-ex`: Ex-specific bug fix on top of the v0.6.3 base
- `v0.7.0-ex`: New feature set, backward-compatible

---

## 🆕 What's New — v0.2.1-ex

### Wildcard Download Improvements *(synced from Neo v0.7.1)*

- **Wildcards get their own subfolder** — each wildcard pack downloads into `wildcards/<model-name>/` by default, compatible with sd-dynamic-prompts `__subfolder/name__` syntax
- **Flat zip extraction for wildcards** — files inside the zip are placed directly in the target folder with no internal folder structure, preventing double-nesting
- **No preview images for wildcards** — `save_preview` and `save_images` are skipped for Wildcards
- **Configurable base-model split for wildcards** — new setting `Organize wildcards by base model` (off by default)

## 🆕 What's New — v0.2.0-ex

> **Stability & Feature Sync** — all Neo v0.7.0 fixes and improvements, now on A1111 and Forge Classic.

- **☑️ Update Selected** — in Update Mode, check any subset of outdated cards and click "Update Selected (N)" to queue only those; button reverts to "Update All" when nothing is selected
- **🎯 Smart version selection** — clicking a card with an active base model filter (e.g. Illustrious) auto-selects the most recent version for that family
- **🔒 Downloads survive screen lock** — Win+L, RunPod reconnects, and SSE disconnects no longer abort downloads in progress
- **🛡️ EARLY_ACCESS / NO_API safety** — API-blocked downloads no longer save partial `.json`/`.html` files or delete unrelated existing files
- **📁 Embeddings folder auto-detection** — supports both old (`embeddings/`) and new (`models/embeddings/`) layouts; warns if both have files
- **🖼️ Fixed send-to-txt2img** — intermittent failure sending prompt/settings to txt2img resolved
- **🐛 Session restore guard** — download queue no longer crashes restoring a session when the model list hasn't loaded yet
- **🐛 Model info crash fix** — model info panel no longer errors on lazy fetch when queue enriches a model not in current search results
- **🐛 `save_images` guard** — no crash when preview HTML is unavailable for a model
- **🐛 Debug log format** — debug messages now use the correct `[CivitAI Browser Ex]` prefix and respect the debug setting

---

## 📖 SD Civitai Browser Ex Release Story

### v0.2.0-ex
> **Theme: Stability & Feature Sync** — Neo v0.7.0 fixes and improvements ported to A1111 and Forge Classic.

- [x] Update Selected — queue only checked cards, dynamic button label
- [x] Smart version selection by base model filter
- [x] Downloads survive screen lock / SSE disconnect (Win+L, RunPod)
- [x] EARLY_ACCESS/NO_API: no more stray saves or unrelated file deletes
- [x] Embeddings folder auto-detection (old and new layout support)
- [x] Warn when both embeddings folders have content
- [x] Fixed send-to-txt2img intermittent failure
- [x] Guard against None `json_data` in session restore
- [x] Fixed `UnboundLocalError` for `model_folder` in update flow
- [x] Guard `None preview_html` in `save_images`
- [x] Fixed `_debug_log` message format

---

### v0.1.0-ex
> **Theme: A1111 & Forge Classic Edition** — all Neo v0.6.3 features working natively on Gradio 3.x.

- [x] Native Gradio 3.x compatibility — fully tested on A1111 and Forge Classic
- [x] SHA256 post-download integrity check — corrupted or truncated files detected and removed automatically
- [x] Instant batch enqueue — queuing 10 models is as fast as queuing 1
- [x] Thread-safe cancel — reliable cancel for individual and all downloads
- [x] Aria2 auto-reconnect — RPC process auto-restarts if unreachable during a download
- [x] Creator Management — favorite / ban creators with instant filtering
- [x] Dashboard — disk usage, pie chart, top file rankings, CSV/JSON export
- [x] Auto-Organization — sort models into subfolders by architecture


---

## 🗺️ Roadmap

### v0.1.0-ex — Gradio 3 Port *(complete)*
- Full Gradio 3.x compatibility ✅
- All Neo v0.6.3 features inherited ✅

### v0.2.1-ex — Wildcard Improvements *(current)*
- Wildcards get their own subfolder per download
- Flat zip extraction for wildcards — no double-nesting
- Skip preview/images for wildcards
- New settings: `wildcard_own_folder` (ON) and `wildcard_organize_by_base` (OFF)

### v0.2.0-ex — Stability & Feature Sync *(complete)*
- Neo v0.7.0 fixes and improvements ✅
- Downloads survive screen lock / SSE disconnect ✅
- EARLY_ACCESS/NO_API safety ✅
- Embeddings folder auto-detection ✅
- send-to-txt2img intermittent fix ✅

### v0.3.0-ex — Stabilization *(planned)*
- A1111-specific path handling improvements
- Forge Classic quirks and fixes
- Testing on different Gradio 3.x minor versions

### v0.4.0-ex — Extended Features *(planned)*
- Saved search presets
- Favorites in creator search dropdown
- SHA256 cache injection (read `.json` sidecars → populate `cache.json` instantly)
- **Organization by Tag — Phase 1**: save CivitAI tags to `.json` sidecar; editable user-tags in model panel
- **Organization by Tag — Phase 2**: pick anchor tags in Manage tab → models sort into `<type>/<tag>/` subfolders

---

## 🎯 Features

> All features work natively on A1111, Forge Classic, and any Gradio 3.x SD WebUI

### 🔍 Browse & Search

- **Browse CivitAI** directly inside the WebUI — no browser switching needed
- **Search by keyword, tag, or username** — multiple search modes
- **Filter by content type**: Checkpoint, LORA, LoCon, DoRA, VAE, ControlNet, Upscaler, TextualInversion, Wildcards, Workflows, and more
- **Filter by base model**: SD 1.x, SDXL, Pony, Illustrious, NoobAI, and more — **auto-updated from CivitAI API** at startup (no hardcoded stale list)
- **Sort by**: Highest Rated, Most Downloaded, Newest, Most Liked, Most Discussed
- **Filter by time period**: Day, Week, Month, Year, All Time
- **NSFW toggle**: Show/hide NSFW content
- **Liked models only**: Filter to models you've liked on CivitAI (requires API key)
- **Hide installed models**: Declutter the browser by hiding already-downloaded models
- **Hide banned creators**: Client-side toggle that instantly hides cards from banned creators without a new search
- **Exact search**: Match search terms exactly instead of fuzzy
- **Search settings persist**: Sort, NSFW state, base model filter — all saved across restarts

### 📥 Download

- **Download any model, version, and file** directly from the browser
- **Aria2 high-speed multi-connection downloads** — optional, enabled by default
- **Download queue** — multiple downloads run in sequence without blocking the UI
- **Queue persistence** — the download queue is logged server-side; if the browser disconnects, a restore banner appears on reconnect
- **Cancel downloads** individually or all at once
- **Auto-set save folder** based on content type — no manual path typing needed
- **Custom sub-folders** — choose or create sub-folders per download
- **Custom save folder per type** — configure paths in Settings
- **Download URL override** — paste a direct URL to download a specific file
- **Proxy support** — SOCKS4/SOCKS5 for regions with restricted access
- **API key support** — download early access and private models with your CivitAI API key

### 🔄 Model Updates

- **Outdated card detection** — orange border on cards that have a newer version available
- **Batch update** — select multiple outdated models via checkbox and download all at once
- **Precise version comparison** — compares model family + version string (configurable)
- **Retention policy** — when updating, choose: `keep` both files, `move to _Trash`, or `replace` (permanent delete)
- **Audit log** — `ex_update_audit.jsonl` records every scan and retention action for traceability
- **Dashboard update summary** — after scanning, the Dashboard shows a live banner with outdated counts per type

### 🗂️ Auto-Organization

- **Organize new downloads automatically** into subfolders by base model type (SDXL/, Pony/, Illustrious/, etc.)
- **Organize existing models** in one click from the Update Models tab
- **Validate organization** — read-only scan that checks every model against its `.json` metadata and reports ✅ correct / ❌ misplaced / ⚠️ no-metadata, with a per-file table
- **Fix misplaced files** — after validation, move only the flagged models to their correct subfolders in one click; backup created automatically
- **Backup before organizing** — automatic snapshot of current folder structure
- **One-click rollback** — restore previous structure at any time (keeps last 5 backups)
- **Custom category patterns** — define your own base model → folder mapping in Settings (JSON)
- **Associated files moved together** — `.json`, `.png`, `.preview.png`, `.txt` files travel with the model

### 🖼️ Model Info & Preview

- **Model information panel** — shows name, version, base model, type, trained tags, permissions, description
- **Sample images** with a **"Send to txt2img"** button per image — fills prompt, negative, sampler, steps, CFG all at once
- **Individual meta field buttons** — click any field (Prompt, Negative, Seed, CFG...) to send just that value to txt2img. **Shift+click appends** to your existing prompt instead of replacing
- **Trained tags / trigger words** — displayed in the model info panel and in the **model overlay popup** (CivitAI icon on txt2img/img2img cards). The **"➕ Add to prompt" button** appends activation tags directly to your txt2img prompt; for **LORA models it automatically prepends `<lora:filename:1>`**
- **SHA256 in Version Information** — the model info overlay shows the SHA256 hash of the selected file; click once to select all for easy copy
- **Video preview** support — model cards with video samples play on hover (muted, loops automatically)
- **Image viewer** — click any preview image to open it fullscreen
- **Resize preview images** in cards — configurable max resolution (128–1024px) for faster loading
- **Save model info** — saves model data as `.json` and HTML with all sample images
- **Save images** — downloads all sample images locally
- **Use local HTML** — when clicking the CivitAI button on a model card in txt2img, open the locally saved HTML instead of fetching from the internet

### 📊 Dashboard & Statistics

- **Disk usage by category** — see exactly how much space each model type and architecture uses
- **File count per category** — know exactly what you have
- **Organized by base model** — Checkpoints and LORAs broken down by type (Pony, SDXL, Illustrious, etc.)
- **Visual progress bars** and percentage breakdown
- **Pie chart** with legend
- **Hide empty categories** toggle for a cleaner view
- **Scan summary** — folders scanned, scan duration, skipped files, read errors
- **Export CSV / JSON** — download dashboard data to a file

### 🃏 Model Cards

- **Color-coded borders**: green = installed, orange = update available, blue = early access, none = not installed
- **Color legend bar** — always-visible reference above the card grid
- **NSFW badge** on cards marked as adult content (configurable)
- **"Paid" badge** (💎) for early access models
- **Model type badge** on each card
- **Tile size** — configurable card size (smaller = more cards per row)
- **Sort by date** — group cards by upload date
- **Hide installed models** — remove already-downloaded models from the grid
- **Multi-select** — checkbox on outdated cards to select multiple for batch download
- **Quick delete** on installed/outdated cards — removes model directly from the card
- **👤 Creator Management** — ⭐ favorite or 🚫 ban a creator directly from the browser; favorited creator cards get a gold glow and ⭐ badge; banned creators can be hidden in one click

### 🔒 Safety & Integrity

- **Send deleted models to Trash** (OS recycle bin) instead of permanent delete — configurable in Settings (default: ON)
- **SHA256 post-download integrity check** — corrupted or truncated files caught automatically
- **Filename length limit (246 bytes / UTF-8)** — prevents filesystem errors on Linux
- **Illegal character sanitization** in filenames — removes characters forbidden by the OS automatically

### ⚙️ Settings

All settings are in **Settings → CivitAI Browser Ex**:

| Setting | Default | Description |
|---------|---------|-------------|
| Personal API key | — | Required for some downloads and liked-only search |
| Hide early access models | OFF | Hides models behind a paywall |
| Individual prompt buttons | ON | Click each meta field to send it to txt2img |
| Shift+click meta fields | — | Appends to existing prompt instead of replacing |
| Move deleted to Trash | ON | OS recycle bin instead of permanent delete |
| Resize preview cards | ON | Resizes card thumbnails for faster loading |
| Resize preview size | 512px | Max width for card thumbnails |
| Video playback | ON | Card thumbnails play video on hover (muted) |
| Use local HTML | OFF | Open local HTML when clicking CivitAI button |
| Page navigation as header | OFF | Sticky top navigation bar |
| Auto-organize downloads | OFF | Organize new downloads by base model type |
| Retention policy | replace | What to do with old files on update |
| Debug prints | OFF | Verbose console output for troubleshooting |
| Use Aria2 | ON | High-speed multi-connection downloads |
| Proxy address | — | SOCKS4/SOCKS5 proxy for restricted regions |

---

## 📦 Installation

1. Open your WebUI
2. Navigate to **Extensions** → **Install from URL**
3. Paste: `https://github.com/eduardoabreu81/sd-civitai-browser-ex`
4. Click **Install** and reload WebUI

### Requirements

- ✅ [Automatic1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui), [Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic), or any Gradio 3.x SD WebUI
- ✅ Python 3.10+
- ✅ Gradio 3.15+ (comes with the WebUI)

> 💡 **Forge Neo user?** We recommend [sd-civitai-browser-neo](https://github.com/eduardoabreu81/sd-civitai-browser-neo) for an even better experience on Forge Neo.

---

## 📁 Auto-Organization System

### How It Works

The organization system analyzes your models based on their `baseModel` metadata (stored in `.json` files) and automatically organizes them into subfolders.

#### **Before Organization:**
```
models/Lora/
├── model1.safetensors (SDXL)
├── model2.safetensors (Pony)
├── model3.safetensors (Illustrious)
├── random_folder/
│   └── model4.safetensors (SD1.5)
└── ...
```

#### **After Organization:**
```
models/Lora/
├── SDXL/
│   ├── model1.safetensors
│   ├── model1.json
│   └── model1.png
├── Pony/
│   └── model2.safetensors
├── Illustrious/
│   └── model3.safetensors
├── SD/
│   └── model4.safetensors
└── ...
```

### Safety Features

- ✅ **Automatic Backup**: Creates backup before any operation
- ✅ **One-Click Undo**: Restore original structure anytime
- ✅ **Associated Files**: Moves `.json`, `.png`, `.txt` files together
- ✅ **Conflict Detection**: Skips files that already exist at destination
- ✅ **Error Recovery**: Continues operation even if some files fail

---

## 📊 Dashboard & Statistics

### Overview

The Dashboard provides comprehensive insight into your model collection with detailed disk usage statistics organized by model type and architecture.

### Example Output

```
2294 files (1.4 TB) → 12 categories

┌────────────────────────┬───────┬──────────────┬────────────┐
│ MODEL TYPE             │ FILES │ TOTAL SIZE   │ % OF TOTAL │
├────────────────────────┼───────┼──────────────┼────────────┤
│ Checkpoint → Other     │ 148   │ 968.3 GB     │ 70.0%      │
│ LORA → Other           │ 1960  │ 366.6 GB     │ 26.5%      │
│ LORA → SDXL            │ 59    │ 17.7 GB      │ 1.3%       │
│ Checkpoint → Illust... │ 2     │ 13.2 GB      │ 1.0%       │
│ LORA → SD              │ 66    │ 6.6 GB       │ 0.5%       │
│ LORA → Pony            │ 2     │ 386.7 MB     │ 0.0%       │
│ VAE                    │ 5     │ 1.2 GB       │ 0.1%       │
└────────────────────────┴───────┴──────────────┴────────────┘
```

---

## 🎨 Supported Model Types

> Forge Classic and A1111 are focused on **SD1 and SDXL** checkpoints. The organization system can still categorize any model type found in your collection, but only SD and SDXL families can be used for generation.

| Category | Detection patterns | Notes |
|----------|-------------------|-------|
| **SD** | SD 1, SD1, SD 2, SD2 | SD 1.x and SD 2.x |
| **SDXL** | SDXL | Base SDXL |
| **Pony** | PONY | Pony V6 and variants (SDXL-based) |
| **Illustrious** | ILLUSTRIOUS | Illustrious XL (SDXL-based) |
| **NoobAI** | NOOBAI, NOOB AI, NAI | NoobAI v-pred (SDXL-based) |
| **Other** | (unrecognized) | Configurable |

### Custom Categories

You can define your own categories in **Settings** → **Model Organization** using JSON format:

```json
{
  "SD": ["SD 1", "SD1", "SD 2", "SD2"],
  "SDXL": ["SDXL"],
  "MyCustomType": ["CUSTOM", "PATTERN"]
}
```

---

## ⚠️ Known Issues

- Some models may not have `baseModel` metadata (download from CivitAI to get it)
- Dashboard shows "Unorganized" for files placed directly in root model folders
- Rollback only works for the last operation
- Maximum 5 backups are kept (older ones are deleted)
- Queue restore banner: after a browser disconnect, click the banner manually to restore your queue; the feature is fully functional

## 💡 Tips

- **First time using?** Update model info & tags to generate `.json` files with metadata
- **Want to see your collection?** Use Dashboard tab to analyze disk usage
- **Want custom folders?** Edit JSON in Settings → Model Organization
- **Made a mistake?** Use "Undo Organization" button immediately
- **Need help?** Check console logs (`[CivitAI Browser Ex]` prefix)

---

## 📄 Credits

### Project Lineage

```
sd-civitai-browser (Vetchems)
  └── sd-civitai-browser-plus (BlafKing)
        ├── sd-civitai-browser-plus (anxety-solo fork)
        └── sd-civitai-browser-neo (Eduardo Abreu)
              └── sd-civitai-browser-ex (Eduardo Abreu)  ← this project
```

### Original Project
- **[sd-civitai-browser](https://github.com/Vetchems/sd-civitai-browser)** by Vetchems
- **[sd-civitai-browser-plus](https://github.com/BlafKing/sd-civitai-browser-plus)** by BlafKing

### Feature Inspiration
- **[sd-webui-civbrowser](https://github.com/SignalFlagZ/sd-webui-civbrowser)** by SignalFlagZ — Creator Management pattern (UserInfo class, `.txt` storage, accordion UI, mutually exclusive lists)

### Anxety-Solo Fork
- **[sd-civitai-browser-plus](https://github.com/anxety-solo/sd-civitai-browser-plus)** by anxety-solo
  - Modern UI redesign
  - Quality of life improvements
  - Multiple bugfixes

### Neo Version (direct parent)
- **[sd-civitai-browser-neo](https://github.com/eduardoabreu81/sd-civitai-browser-neo)** by Eduardo Abreu
  - Forge Neo compatibility (Gradio 4.x)
  - Auto-organization system
  - Dashboard & statistics
  - Download reliability features (SHA256, batch deferral, aria2 reconnect)
  - Creator management (favorite/ban)

### Ex Version (this project)
- **[sd-civitai-browser-ex](https://github.com/eduardoabreu81/sd-civitai-browser-ex)** by Eduardo Abreu
  - Native A1111 and Forge Classic support (Gradio 3.x)
  - All Neo v0.6.3 features brought to classic WebUIs

### Special Thanks
- All contributors to the original projects
- CivitAI for their amazing API

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🤝 Contributing

Contributions are welcome! Feel free to:
- 🐛 Report bugs
- 💡 Suggest new features
- 🔧 Submit pull requests
- 📖 Improve documentation

---

## ☕ Support

If you find this extension helpful, consider:
- ⭐ Starring the repository
- 🐛 Reporting issues
- 📢 Sharing with others
- ☕ [Buy me a coffee](https://ko-fi.com/eduardoabreu81)

---

<div align="center">

Made with ❤️ for the Stable Diffusion community

**[Report Bug](https://github.com/eduardoabreu81/sd-civitai-browser-ex/issues)** • **[Request Feature](https://github.com/eduardoabreu81/sd-civitai-browser-ex/issues)** • **[Discussions](https://github.com/eduardoabreu81/sd-civitai-browser-ex/discussions)**

</div>
