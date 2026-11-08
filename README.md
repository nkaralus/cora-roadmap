# cora-roadmap
Community &amp; personal roadmap for native software, UI upgrades, and missing tools.
# Cora Ecosystem Roadmap

Community & personal roadmap for native software, UI upgrades, and missing tools. A sovereign, local-first Linux architecture pairing deterministic kernel enforcement with local AI telemetry. Built to run without corporate toll booths, cloud authentication gates, or opaque client bloat.

---

## 1. Substrate Matrix & Repository Index

| Repository | Role | Substrate / Stack | Status |
| :--- | :--- | :--- | :--- |
| **[engine-telemetry-bus](https://github.com/nkaralus/engine-telemetry-bus)** | Deterministic safety gates (GI_1–GI_3), virtual IPv4 addressing (`10.0.0.0/8`), strict geometry & path validation. | Python 3.12, zero external runtime deps. | **v1.0.0 Stable** (166/166 passing; vendored in sentinel) |
| **[cora-security](https://github.com/nkaralus/cora-security)** | Community blocklists, two-key rotation (K1/K2), automated violation ingestion, and `violation.schema.json` (v1). | JSON Schema, Ed25519 signatures, GitHub Actions. | **v1.0 Active** (Manifest signing verified) |
| **[cora-sentinel](https://github.com/nkaralus/cora-sentinel)** | Out-of-band kernel sentinel: cgroup v2 process tracking, connect-time SYN/UDP/QUIC flow taps via nftables/NFLOG, kernel floor (`--floor`), dynamic policy (`--dyn-policy`), and continuous PSI feed (`uihub.py`). | Python 3.12, Linux 6.x+, cgroup v2, nftables, netlink. | **v0.10.1 Active** (Pre-isolated tap tests passing) |
| **cora-desktop / cora-hud** | Low-overhead native desktop interface: live cgroup scope monitors, PSI pressure gauges, and one-click panic breakers. | Python, PySide6 / native Wayland. | **Designing** (Direct consumer of `uihub.py` status feed) |
| **[cora-roadmap](https://github.com/nkaralus/cora-roadmap)** | Single source of truth for ecosystem alignment, release planning, and cross-project architecture. | Markdown, Git. | **Active** |

---

## 2. Active Development Tracks[ cora-sentinel daemon ]
                               │
                ┌──────────────┴──────────────┐
                ▼                             ▼
   [ uihub.py / Status Feed ]       [ Kernel Enforcers ]
                │                   • nftables egress cut (ISOLATE)
                ▼                   • cgroup.freeze (SUSPEND)
    [ cora-hud Desktop UI ]         • kernel floor drop
    • Task PSI memory meters
    • Egress breach indicators
    • Manual panic switches### Track A: Desktop Runtime & Live HUD (`cora-hud`)
*Goal: Provide a lightweight, native GUI for the desktop session that consumes the daemon's status feed without shell access.*
- [x] **PSI Telemetry Engine** (`cora-sentinel/psi.py`, `uihub.py`): Continuous `some`/`full` memory pressure percentages and cumulative `total_us` stalls emitted per task alongside monotonic clocks (`12e55b3`).
- [ ] **Local IPC Transport**: Bind `uihub.py` broadcast socket (e.g., `/run/cora-sentinel/events.sock`) with standard unprivileged read permissions.
- [ ] **System Tray Daemon**: Desktop tray icon displaying overall daemon health (Normal, Evaluating, Breaker Tripped).
- [ ] **Task Monitor Dashboard**: Real-time flyout showing active `cora-run` scopes, current RAM vs `memory.max`, and PSI stall trends.
- [ ] **Session Panic Breakers**: UI triggers wired directly to `cora-firewall release`, `cora-firewall purge`, and scope freeze/thaw commands.

### Track B: Deterministic Workload Harness
*Goal: Stress-test isolation boundaries, memory pressure ceilings, and dynamic policies using clean, fully owned binaries.*
- [x] **Sever Closed Runtimes**: Drop dependencies on proprietary game clients requiring remote authentication, closed launchers, or external session states.
- [ ] **Clean Workload Targets**: Standardize on:
  1. Headless Godot engine scenarios (deterministic scene compilation and rendering).
  2. Synthetic memory pressure generators (allocators cycling up to `--memory-max` to trip PSI signals).
  3. Synthetic egress beacon suites (validating sub-millisecond containment across TCP/UDP).
- [ ] **Standardized Metric Ingestion**: Write verified frame pacing, CPU time, and memory stall fractions into structured JSON logs for automated regression testing.

### Track C: Daemon Hardening & Packaging
*Goal: Turn the sentinel from an active checkout into an unattended system service.*
- [x] **Status Feed & Monotonic Time**: Verified clock stamping and PSI tracking across all tasks.
- [x] **Two-Key Rotation**: Standby key rotation protocol and verification tools landed.
- [ ] **Systemd Unit Distribution**: Finalize `deploy/cora-sentinel.service` with automatic `RuntimeDirectory=/run/cora-sentinel` provisioning.
- [ ] **Native `.deb` Packaging**: Verify end-to-end service installation and unprivileged execution on Ubuntu LTS environments.

---

## 3. Engineering Operating Principles

1. **Sovereignty First**: Never introduce a test harness or dependency that relies on third-party license servers, cloud logins, or proprietary desktop launchers.
2. **Deterministic Hot Paths**: Hard rules and kernel taps detect and drop violations immediately; models only evaluate ambiguities out-of-band.
3. **Fail-Closed**: Any unmeasured metric, broken pipe, missing cgroup scope, or ambiguous telemetry packet registers as an explicit error, never a silent pass.
4. **Zero-Overhead UI**: Desktop interfaces must consume read-only broadcast feeds without injecting latency into the daemon's kernel polling loops.
