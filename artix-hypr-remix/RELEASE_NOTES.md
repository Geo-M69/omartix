# Artix Hypr Remix — Release Notes

## v0.2.0-beta2 — Phase 3 framework release candidate

**Status:** Public-source framework-delivery validation of `68b7532` was
recorded on 2026-09-07. This candidate is not tagged or published.
**Framework metadata:** `0.2.0-beta2`, `beta` channel, with the existing public
source `https://github.com/Geo-M69/artix-installer.git`.
**Base system:** Artix Linux with OpenRC

### Included in this candidate

- Phase 3a–3e: the versioned Flatpak catalog, read-only catalog CLI,
  catalog-managed install/launch/remove/update, category-driven Install/Remove
  menus, offline catalog-entry validation, lifecycle/drift/menu regressions,
  and normalized Flatpak diagnostics.
- Framework delivery of the catalog, libraries, `ahr flatpak` command, menu
  integration, and the Hyprland OBS share-picker placement rules.
- A narrow, idempotent share-picker migration for existing user-edited
  `hyprland.conf` files. It appends only missing rules and leaves an exact
  `hyprland.conf.bak.*` recovery copy.
- Exact framework-rollback namespace restoration: AHR-managed links introduced
  after the selected snapshot are removed, while unrelated user links are
  preserved.

### Public-source validation record

The 2026-09-07 live transaction records directly confirm committed applies
`tx-wXBhTBJ3` and `tx-03ibl18k` of `68b7532`, with completed manual rollback
`tx-EB26D1uj` against the initial backup. The initial apply remains committed.
Rollback recorded a degraded doctor result; its captured output identifies
inactive Bluetooth, which the restored older policy treated as required.
Pre-apply and post-rollback backup contents compared equal for the framework
targets, metadata, migration snapshot, and namespace manifest. All transaction
records present at inspection had terminal completion values.

The earlier live report also stated exact live restoration and passing final
doctor and baseline smoke. Those observations lack a preserved contemporaneous
transcript and remain reported rather than directly verified. See the
[sanitized evidence and comparison limits](docs/validation/2026-09-07-beta2-framework/README.md).
This evidence supports beta2 framework-delivery readiness for the tested
revision; it does not close the full Phase 3 product exit gate or establish a
new release revision without review.

On 2026-11-18, current host doctor and baseline smoke both passed with
optional-service and MIME warnings. The installed framework content and
terminal transaction inventory were also checked; see the
[current health record](docs/validation/2026-11-18-beta2-health/README.md).
These checks do not replace the missing September transcripts.

---

## v0.1.0-beta1 — Released 2026-06-06

---

## Tested base system

The following base system was used for the current validation run:

| Detail | Value |
|--------|-------|
| Distribution | Artix Linux (OpenRC) |
| Image | `artix-base-openrc-20250501-x86_64.iso` |
| Validation date | 2026-06-05 – 2026-06-06 |
| Validation hosts | `geoartix` (VM, QEMU/KVM, Virtio GPU) + NVIDIA laptop (real hardware) |
| Image source | [artixlinux.org/download](https://artixlinux.org/download) |
| Package state | Fresh `pacman -Syu` prior to install; live Artix repos as of validation date |

> **Note:** Package availability depends on live Artix repository state at install time.
> The beta release will pin the exact ISO filename and a recommended minimum `pacman -Syu` date.
> Intel GPU validation is still pending — expected to work but no real-host logs yet.

---

## What's included

This beta release provides an opinionated, phase-based installer that configures a complete Hyprland desktop on Artix Linux (OpenRC). The installer is idempotent and safe to re-run.

### Core desktop stack

- Hyprland (TTY launch by default, optional `greetd`)
- Waybar (status bar with toggle/restart support)
- Mako (notification daemon with silencing and restart)
- PipeWire / WirePlumber (audio)
- `grim` / `hyprlock` / `hypridle` (screenshots, lock, idle)
- `wl-clipboard` (clipboard)
- `xdg-desktop-portal-hyprland` (portals)
- Theme engine with `artix-dark` default
- Command namespace (`ahr`, `ahr-menu`, `ahr-repair`, `ahr-migrate`, `ahr-update`, etc.)

### Optional profiles

| Profile | Enabling flag |
|---------|---------------|
| Docker | `--docker-profile on` |
| Printing | `--printing-profile on` |
| AUR packages | Phase 6 (default: on) |
| Flatpak (default/optional/all) | `--flatpak-profile` |
| Dev baseline (Git/GPG/SSH) | `--dev-baseline on` (Phase 8) |

### Startup modes

- `tty` — default, launches Hyprland via managed block in `~/.profile`
- `greetd` — optional display-manager flow (greeter or autologin)

---

## Supported hardware

See [BETA_SUPPORT_MATRIX.md](BETA_SUPPORT_MATRIX.md) for the full frozen matrix.

| Profile | Status | Hosts validated |
|---------|--------|-----------------|
| AMD (desktop/VM) | 🟢 Validated | 1 (virtualized) |
| Intel | 🟡 Limited | 0 — stub and OpenRC module present, no real host yet |
| NVIDIA | 🟢 Validated | 1 (real laptop, TTY + greetd) |
| Laptop | 🟢 Validated | 1 (NVIDIA laptop) |

### Validation hosts

| Host | GPU | Startup | Date | Logs |
|------|-----|---------|------|------|
| `geoartix` (VM) | AMD (Virtio) | tty | 2026-06-05 | Quality gate ✅, smoke ✅, milestone2 ✅ |
| `geoartix` (VM) | AMD (Virtio) | greetd (greeter) | 2026-06-05 | Doctor ✅, post-install smoke ✅, milestone2 ✅ |
| `geoartix` (host) | AMD (Virtio) | greetd (autologin) | 2026-06-05 | Daily-use validation ✅ |
| NVIDIA laptop | NVIDIA (real) | tty | 2026-06-06 | Doctor ✅, post-install smoke ✅, milestone2 ✅, bundle collected |
| NVIDIA laptop | NVIDIA (real) | greetd (greeter) | 2026-06-06 | milestone2 ✅ (second pass) |

---

## Key known issues

See [MILESTONE6_KNOWN_ISSUES.md](MILESTONE6_KNOWN_ISSUES.md) for the full list.

- Real-host validation is still limited — Intel GPU profile needs logs before strong support claims. NVIDIA and laptop profiles are now validated on real hardware.
- `greetd` mode is present and validated for both greeter and autologin variants. Fresh-install log bundles collected for TTY and greetd modes (NVIDIA laptop).
- `ahr repair --config` is detect-only and does not rewrite user-edited config automatically.
- Theme background fallback: `artix-dark` has no bundled background image; falls back to solid color.
- Package availability depends on live Artix repos and selected package profiles.
- Screenshots / visual gallery are deferred to future work.

---

## Fast start

```bash
git clone https://github.com/<you>/artix-installer.git
cd artix-installer/artix-hypr-remix
sudo ./install.sh
```

See `README.md` for phase options, flags, and validation commands.

---

## Rollback and recovery

See [RECOVERY_AND_RESET.md](RECOVERY_AND_RESET.md) for repair boundaries.

- Config backups are created at `~/.config/<name>.bak.<timestamp>` during Phase 4.
- Safe repair commands: `ahr repair --namespace --apply`, `ahr repair --theme --apply`.
- Installer phase 4 can re-deploy managed config: `sudo ./install.sh --phase 4 --user <username> -y`.
- No full uninstall/reset command exists yet.

---

## Intentionally unsupported (beta)

- systemd, UWSM, SDDM, Plymouth, Limine, and other systemd-specific Omarchy maintenance paths
- Non-OpenRC init systems
- Display managers other than `greetd`
- Full uninstall/reset command
- ISO / distribution profile work
- Web app installer
- Rich theme gallery, previews, and theme install/update/remove
- Broad install/remove menus for gaming, Windows, AI, dev stacks, security tools
- Hardware-specific toggles not validated on real Artix hardware

---

## How to contribute validation logs

See [MILESTONE6_BETA_CHECKLIST.md](MILESTONE6_BETA_CHECKLIST.md) for the exact commands to run.

To submit a validation bundle:
1. Run the minimum validation commands on your Artix host.
2. Gather logs from `/var/log/artix-hypr-remix-install.log` and `/var/lib/artix-hypr-remix/hardware-profile.json`.
3. Package into `validation-<host>-<gpu>-<date>.tar.gz`.
4. Attach to the beta release or open an issue.

Profiles most needed: **Intel desktop**, **NVIDIA desktop**, **laptop (any GPU)**.
