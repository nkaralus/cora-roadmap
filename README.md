# Cora Ecosystem Roadmap: The Human Runtime

A sovereign, local-first Linux architecture pairing deterministic kernel enforcement with local AI telemetry. Built to run without corporate toll booths, cloud authentication gates, or opaque client bloat.

---

## 1. Physical Hardware Boundaries & Windows Quarantine

- **Host Machine**: `Dimgeon` (Ubuntu 24.04 LTS, NVIDIA RTX 5070 GPU, 16GB RAM).
- **Authorized Storage**:
  - `nvme0n1p5` (49.1 GB, ext4): Root Linux `/` & `/home/nick`
  - `sda1` (1.8 TB, ext4): Secondary storage `/media/nick/UE5_Storage`
  - Ingress Sandbox: `/home/nick/Downloads` (`10.1.1.0/24`)
- **Quarantined Partitions (Permanent Gate 3 Block)**:
  - `nvme0n1p6` (866.9 GB, NTFS): Primary Windows Partition
  - `nvme0n1p2` (16 MB), `nvme0n1p3` (14 GB, `WININSTALL`), `nvme0n1p4` (1 GB, Windows Recovery)
  - **Rule**: Absolute exclusion. Any action targeting NTFS, exFAT, or `nvme0n1p[2346]` is rejected at the bus boundary before execution.

---

## 2. Core Execution Phases

### Phase 1: Lock the Basement (Feature Complete & Frozen)
- `cora-sentinel` is locked at v0.10.2.
- Out-of-band kernel sentinel: cgroup v2 tracking, `nftables` floor drops, connect-time taps (NFLOG), and early boot ordering (`Type=notify`, `Before=network-pre.target`).
- **Policy**: Zero further synthetic packaging runs, zero maintainer script audits, zero interrupted-signal tests, and zero `systemd-analyze` score chasing.

### Phase 2: The Glass Surface (`cora-hud` / Tokonoma UI)
- **Zero-Engine Architecture**: Standalone hardware-accelerated HTML5, CSS, and Web Audio API running in borderless kiosk mode (`pywebview` or dedicated browser app window) at 60 FPS on the RTX 5070.
- **Theme Dashboard (Character Select)**:
  - *Traditional Samurai Nature*: Scorched cedar (`#121113`), gold-leaf accents (`#D4AF37`), washi textures, falling blossoms, and sumi-e ink styling.
  - *Vibrant Anime Sanctuary*: Cyber-neon pastels, playful aesthetic, glowing borders, and companion presence.
- **The 5 Persistent Carved Tiles** (Unmoving bottom navigation bar):
  1. `[ SANCTUARY ]` — Live wallpaper canvas, weather, falling blossoms, ambient audio, lighting.
  2. `[ COMPANION ]` — Full-screen conversational Hearth and learning space with Cora.
  3. `[ WORKSHOP ]` — Terminal co-pilot, code editor, and D&D battlemap canvas.
  4. `[ LEISURE ]` — One-click launcher for Minecraft and Steam, music player.
  5. `[ SYSTEM ]` — Storage gauges, memory pressure dials, and master audio knobs.
- **3-Click Guarantee**: No action in the OS ever takes more than three clicks or navigates through a nested sub-menu.

### Phase 3: The Dual-Brain Switchboard
- **3B Reflex Engine** (Local Qwen 2.5 3B via GBNF grammar slots):
  - Mechanical solenoid executing deterministic JSON actions.
  - Terminal typo and syntax auto-correction via shell hook (`command_not_found_handle`).
  - Downloads valet mapping `~/Downloads` into `engine-telemetry-bus` virtual IPs (`10.1.1.0/24`) without touching the Windows drive.
  - Desktop hygiene: Bluetooth mouse trust persistence (`org.bluez`) and keyring auto-unlock (`org.freedesktop.secrets`).
- **9B / Frontier Engine** (Companion & Hearth):
  - Drives aesthetic mood adaptation, theme transitions, ambient sound generation, and zero-shame technical learning in plain, grounded language (voltage, substrate, load, gravity).

### Phase 4: Tactile Playground (Games & Tools)
- **Minecraft Kinetic Launcher**: Optimized launcher profile with locked CPU priorities, allocated memory pools, and zero disk thrashing.
- **Tabletop GM Canvas**: Lightweight HTML5 battlemap and token HUD for tabletop RPG sessions.
- **Sonic Player**: Integrated audio player and visualizer for original music tracks and focus soundscapes.

---

## 3. Substrate Matrix & Repository Index

| Repository | Role | Stack | Status |
| :--- | :--- | :--- | :--- |
| **`engine-telemetry-bus`** | Deterministic safety gates (GI_1–GI_3), virtual IPv4 addressing (`10.0.0.0/8`), strict geometry & path validation (`fsguard.py`). | Python 3.12, zero external runtime deps. | **v1.0.0 Stable** (166/166 passing) |
| **`cora-sentinel`** | Out-of-band kernel sentinel: cgroup v2 process tracking, connect-time SYN/UDP/QUIC flow taps via nftables/NFLOG, kernel floor (`--floor`), boot window closed. | Python 3.12, Linux 6.x+, cgroup v2, nftables, netlink. | **v0.10.2 Frozen** (Baseline verified) |
| **`mule-protocol`** | Deterministic token-level syntax guard: strict slot template filling, rejecting fences, markdown noise, and leaks. | Python 3.10+, regex slot parser. | **v1.0 Active** |
| **`cora-desktop / cora-hud`** | Living canvas UI: Tokonoma aesthetic selector, tactile bottom tray, and desktop hygiene runtime. | HTML5, Modern CSS, Web Audio, Python D-Bus bridge. | **Active Target** |
| **`cora-roadmap`** | Single source of truth for ecosystem alignment, release planning, and cross-project architecture. | Markdown, Git. | **Active** |

---

## 4. Engineering Operating Principles

1. **Sovereignty First**: Never introduce a test harness or dependency that relies on third-party license servers, cloud logins, or proprietary desktop launchers.
2. **Deterministic Hot Paths**: Hard rules and kernel taps detect and drop violations immediately (34–79 µs); models only evaluate ambiguities out-of-band.
3. **Fail-Closed**: Any unmeasured metric, broken pipe, missing cgroup scope, or ambiguous telemetry packet registers as an explicit error, never a silent pass.
4. **Zero-Overhead UI**: Desktop interfaces must consume read-only broadcast feeds without injecting latency into the daemon's kernel polling loops.
5. **Windows Partition Quarantine**: Hard boundary on `nvme0n1p[2346]`. Any action targeting NTFS/exFAT is killed at Gate 3.
