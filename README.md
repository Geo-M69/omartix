# Omartix

[![Quality gate](https://github.com/Geo-M69/artix-installer/actions/workflows/artix-hypr-remix-quality-gate.yml/badge.svg)](https://github.com/Geo-M69/artix-installer/actions/workflows/artix-hypr-remix-quality-gate.yml)
[![Framework smoke](https://github.com/Geo-M69/artix-installer/actions/workflows/artix-hypr-remix-framework-smoke.yml/badge.svg)](https://github.com/Geo-M69/artix-installer/actions/workflows/artix-hypr-remix-framework-smoke.yml)

**Omartix** is a phase-based installer for a polished Hyprland desktop on **Artix Linux with OpenRC**. It provides an Artix-native alternative to systemd-oriented desktop workflows, with a TTY-first session by default, optional `greetd`, managed configuration, and recovery tooling.

> **Beta software.** The current target is `v0.1.0-beta1`. NVIDIA laptop and VM-based AMD paths have validation coverage; Intel support is still limited. Review the [support matrix](artix-hypr-remix/BETA_SUPPORT_MATRIX.md) and [known issues](artix-hypr-remix/MILESTONE6_KNOWN_ISSUES.md) before installing.

![Omartix desktop](artix-hypr-remix/docs/screenshots/screenshot-01-first-login-geoartix-20260614.png)

## What it sets up

- Hyprland, Waybar, Mako, PipeWire/WirePlumber, portals, lock/idle, screenshot, clipboard, and theming components
- OpenRC service configuration and a TTY-first Hyprland session; `greetd` is available as an option
- Hardware-aware package and OpenRC-module handling for AMD, Intel, NVIDIA, and laptop profiles
- Managed desktop configuration with timestamped backups before replacement
- `ahr` commands for menus, theme management, updates, migrations, diagnostics, and repair
- Optional Docker, printing, AUR, Flatpak, and development-baseline profiles

## Requirements

- A fresh or minimal **x86_64 Artix Linux installation using OpenRC**
- A non-root desktop user with `sudo` access
- `pacman`, `bash`, and working network access

This project does not support systemd-based hosts or display managers other than `greetd`. It is a script installer, not an ISO or distribution image.

## Install

Clone the repository, review the plan, then run the installer from a non-root shell:

```bash
git clone https://github.com/Geo-M69/artix-installer.git
cd artix-installer/artix-hypr-remix
./install.sh --dry-run
./install.sh
```

The installer elevates with `sudo` when needed. It installs packages, enables the required OpenRC services, backs up and deploys managed configuration, configures startup, and prepares the first-run framework. Reboot when it completes.

Useful installation variants:

```bash
# Use greetd instead of the default TTY startup
./install.sh --startup-mode greetd

# Install detected hardware packages without a confirmation prompt
./install.sh --hardware-mode auto

# Enable optional profiles
./install.sh --docker-profile on --printing-profile on
```

See the [installer reference](artix-hypr-remix/README.md#reference-all-installer-options) for phases, Flatpak/AUR controls, re-run workflows, and all supported flags.

## After installation

On first login, use `Super` + `K` to open the desktop menu. The installer creates the `ahr` command namespace for common maintenance tasks:

```bash
ahr repair --dry-run
ahr repair --apply
ahr update --dry-run
ahr migrate --status
```

For recovery boundaries, locations of backups and logs, and safe repair options, read [Recovery and reset](artix-hypr-remix/RECOVERY_AND_RESET.md).

## Repository layout

| Path | Purpose |
| --- | --- |
| [`artix-hypr-remix/install.sh`](artix-hypr-remix/install.sh) | Phase-based installer entry point |
| [`artix-hypr-remix/packages/`](artix-hypr-remix/packages/) | Pacman and AUR package manifests |
| [`artix-hypr-remix/services/`](artix-hypr-remix/services/) | OpenRC service manifests |
| [`artix-hypr-remix/config/`](artix-hypr-remix/config/) | Desktop configuration and AHR runtime files |
| [`artix-hypr-remix/lib/`](artix-hypr-remix/lib/) | Installer libraries |
| [`artix-hypr-remix/scripts/`](artix-hypr-remix/scripts/) | Validation, diagnostics, and helper scripts |
| [`artix-hypr-remix/docs/screenshots/`](artix-hypr-remix/docs/screenshots/) | Visual validation artifacts |

## Development and contributions

Run the host-independent quality checks before proposing a change:

```bash
cd artix-hypr-remix
./scripts/quality-gate.sh --no-aur
```

Please open an issue before substantial changes to installer behavior, hardware profiles, or OpenRC service handling. Bug-report guidance, validation-bundle instructions, and pull-request expectations are in [CONTRIBUTING.md](CONTRIBUTING.md).

## Documentation

- [Full installer guide and command reference](artix-hypr-remix/README.md)
- [Beta support matrix](artix-hypr-remix/BETA_SUPPORT_MATRIX.md)
- [Release notes](artix-hypr-remix/RELEASE_NOTES.md)
- [Known issues and troubleshooting](artix-hypr-remix/MILESTONE6_KNOWN_ISSUES.md)
- [Project roadmap](artix-hypr-remix/ROADMAP.md)

## License

This repository does not currently declare a project-wide license. The bundled [`omarchy/`](omarchy/) reference directory has its own [license](omarchy/LICENSE). If you need to reuse or redistribute material from this repository, please open an issue with the maintainer first.
