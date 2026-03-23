# Host Machine Storage Audit

**Date:** 2026-03-23  
**Auditor:** kiro  
**Issue:** #1

---

## Summary

Root filesystem (`/dev/sda1`, 49G) is at **89% used (43G)** with only **5.6G free**.

---

## Top Consumers

| Size | Path | Notes |
|------|------|-------|
| 20G | `/home/ubuntu` | See breakdown below |
| 4.2G | `/tmp` | Temp files, some large |
| 8.5G | `/usr` | System — expected |
| 2.2G | `/var` | Logs + apt cache |
| 1.8G | `/snap` | Snap packages |

### `/home/ubuntu` breakdown

| Size | Path | Notes |
|------|------|-------|
| 3.3G | `/home/ubuntu/tmp/oos12-h40.zip` | Single zip file, likely safe to delete |
| 3.0G | `/home/ubuntu/Android/Sdk` | Android SDK |
| 4.2G | `/home/ubuntu/.gradle` | Gradle build cache (3.5G in caches) |
| 2.3G | `/home/ubuntu/projects` | Project source |
| 2.1G | `/home/ubuntu/flutter` | Flutter SDK (415M is `.git` history) |
| 1.1G | `/home/ubuntu/.cursor-server` | Cursor IDE server binaries |
| 981M | `/home/ubuntu/.local` | Local binaries (562M in `bin`) |
| 824M | `/home/ubuntu/.nvm` | Node version manager |
| 687M | `/home/ubuntu/.cache` | Caches (591M is `uv` Python cache) |

### `/tmp` breakdown

| Size | Path | Notes |
|------|------|-------|
| 2.0G | `/tmp/arm64_check/system.img` | Android system image — likely stale |
| 70M | `/tmp/flutter_tools.XJTTGQ` | Flutter tool cache |
| 42M | `/tmp/flutter_tools.VEUURG` | Flutter tool cache |
| 21M | `/tmp/platform-tools` | Android platform tools |

### `/var` breakdown

| Size | Path | Notes |
|------|------|-------|
| 587M | `/var/cache/apt` | APT package cache |
| 296M | `/var/log/journal` | systemd journal logs |

---

## Quick Wins (safe to reclaim)

| Action | Estimated Reclaim |
|--------|------------------|
| Delete `/home/ubuntu/tmp/oos12-h40.zip` | **3.3G** |
| Delete `/tmp/arm64_check/` (stale Android images) | **2.0G** |
| `sudo apt clean` (APT cache) | **587M** |
| `sudo journalctl --vacuum-size=50M` (journal logs) | **~246M** |
| `rm -rf /tmp/flutter_tools.*` (Flutter temp caches) | **~112M** |
| `rm -rf /home/ubuntu/.gradle/caches` (rebuild on demand) | **3.5G** (if Android dev paused) |
| `rm -rf /home/ubuntu/.cache/uv` (pip cache, rebuilds) | **591M** (if acceptable) |

**Immediate no-risk reclaim (top 5): ~6.2G**  
**Aggressive reclaim (all above): ~10.3G**

---

## Recommendations

1. **Immediate:** Delete `/home/ubuntu/tmp/oos12-h40.zip` and `/tmp/arm64_check/` — these are clearly stale artifacts.
2. **Routine:** Run `sudo apt clean && sudo journalctl --vacuum-size=50M` as a cron job or after upgrades.
3. **Consider:** Trimming `.gradle/caches` and `.cache/uv` if Android/Python dev is not active.
4. **Long-term:** The Flutter SDK `.git` directory (415M) can be pruned with `git gc --aggressive` if Flutter version is pinned.
