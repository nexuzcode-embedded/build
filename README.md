<h3 align="center">
  <a href=#><img src="https://raw.githubusercontent.com/armbian/.github/master/profile/logosmall.png" alt="Armbian logo"></a>
  <br><br>
</h3>

## Purpose of This Repository

The **Armbian Linux Build Framework** creates customizable OS images based on **Debian** or **Ubuntu** for **single-board computers (SBCs)** and embedded devices.

It builds a complete Linux system including kernel, bootloader, and root filesystem, giving you control over versions, configuration, firmware, device trees, and system optimizations.

The framework supports **native**, **cross**, and **containerized** builds for multiple architectures (`x86_64`, `aarch64`, `armhf`, `riscv64`) and is suitable for development, testing, production, or automation.

> **Looking for prebuilt images?** Use [Armbian Imager](https://github.com/armbian/imager/releases) — the easiest way to download and flash Armbian to your SD card or USB drive. Available for Linux, macOS, and Windows.

## Quick Start

```bash
git clone https://github.com/armbian/build
cd build
./compile.sh
```

<a href="#quick-start"><img src=".github/README.gif" alt="Build demonstration" width="100%"></a>

The entry point `compile.sh` sources `lib/single.sh` and dispatches to the CLI. Configuration is done through configuration files (e.g. `config-default.conf`) and command-line variables, not by editing `compile.sh` directly.

## Build Host Requirements

### Hardware
- **RAM:** ≥8 GB (less with `KERNEL_BTF=no`)
- **Disk:** ~50 GB free space
- **Architecture:** `x86_64`, `aarch64`, or `riscv64`

### Operating System
- **Native builds:** Armbian or Ubuntu 24.04 (Noble)
- **Containerized:** any Docker-capable Linux
- **Windows:** WSL2 with Armbian or Ubuntu 24.04

### Software
- Superuser privileges (`sudo` or root)
- Up-to-date system (outdated Docker or other tools can cause failures)

## Repository Layout

| Path | Purpose |
|:--|:--|
| `compile.sh` | Top-level entry point; delegates to `lib/`. |
| `lib/` | Bash libraries implementing the build framework. |
| `config/` | Board, family, distribution, CLI and boot configurations. |
| `config/boards/` | Per-board configuration files (`.conf`, `.csc`, `.wip`, `.eos`, `.tvb`). |
| `config/sources/families/` | SoC/family-level build logic and defaults. |
| `config/bootenv/`, `config/bootscripts/` | U-Boot environment templates and boot scripts. |
| `config/distributions/` | Debian/Ubuntu userland release definitions. |
| `patch/` | Patches for kernel, U-Boot, ATF and other components, per branch/version. |
| `packages/` | Armbian-specific packaging: `bsp/`, `bsp-cli/`, `bsp-desktop/`, `armbian/`, `blobs/`, `extras-buildpkgs/`. |
| `extensions/` | Optional build-time extensions that hook into the build lifecycle. |
| `tools/` | Auxiliary helpers (e.g. `mk_format_patch`, `unifying_configs`). |
| `action.yml` | Composite GitHub Action `Rebuild Armbian` (used by armbian/os). |
| `.github/workflows/` | CI, data-sync, maintenance and infrastructure workflows. |

## Board Configuration Status

Boards are declared in `config/boards/` and their support status is expressed by the file extension:

| Extension | Meaning |
|:--|:--|
| `.conf` | Supported — actively maintained |
| `.csc` | Community / Staging Contribution — work in progress or without an active maintainer |
| `.wip` | Work in progress |
| `.eos` | End of support / former stable |
| `.tvb` | TV box, unofficial support |

Board configuration variables (e.g. `BOARD_NAME`, `BOARDFAMILY`, `BOOTCONFIG`, `KERNEL_TARGET`, `SERIALCON`, `MODULES`, `DEFAULT_OVERLAYS`, `BOOT_FDT_FILE`, `CPUMIN`/`CPUMAX`, …) are documented in [`config/boards/README.md`](config/boards/README.md).

## Distributions

Upstream Debian and Ubuntu release status is defined in `config/distributions/`:

| Status | Description |
|:--|:--|
| supported | Current package base |
| csc | Unstable, work in progress, old-stable |
| eos | Former stable, end of life |

## Using as a GitHub Action

This repository exposes a composite action (`action.yml`, `Rebuild Armbian`) that checks out `armbian/os`, this build framework and an optional customisation repo, then runs `./compile.sh` with the selected inputs. Notable inputs include:

- `armbian_target` — `kernel` (default) or `image`
- `armbian_board` — hardware platform (default `uefi-x86`)
- `armbian_branch` — framework branch (default `main`)
- `armbian_kernel_branch` — `legacy`, `current` (default), `edge`, …
- `armbian_release` — userspace release (default `noble`)
- `armbian_ui` — `minimal` (default), `server`, or a desktop environment
- `armbian_compress` — output compression method (default `sha,img,xz`)
- `armbian_extensions` — extra build extensions to enable
- `armbian_pgp_key` / `armbian_pgp_password` — optional signing of `*.img*.xz`
- `armbian_download_base_url`, `armbian_download_repository`, `armbian_index_url` — used to compose the release assets manifest

See `action.yml` for the full input list.

## Continuous Integration

Workflows in `.github/workflows/` cover several concerns:

- **Data sync** — `data-jira-ticket.yml`, `data-sync-board-list.yml`, `data-sync-labels.yml`, `data-sync-maintainers.yml`, `data-sync-tools.yml`
- **Infrastructure** — `infrastructure-dispatch-to-fork.yml`, `infrastructure-mirror-to-codeberg.yml`
- **Maintenance** — auto-labeling, PR/merge announcements, board asset checks, kernel security checks, script linting, kernel config/patch rewrites, board config validation, security scanning, welcome bots, and artifact builds

Details on self-hosted runner tags (`small`, `big`, `arm64`) and how forks can consume dispatched events are documented in [`.github/workflows/README.md`](.github/workflows/README.md).

## Resources

- **[Documentation](https://docs.armbian.com/Developer-Guide_Overview/)** — Comprehensive guides for building, configuring, and customizing
- **[Website](https://www.armbian.com)** — News, features, and board information
- **[Blog](https://blog.armbian.com)** — Development updates and technical articles
- **[Forums](https://forum.armbian.com)** — Community support and discussions

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on reporting issues, working on existing issues, preparing an environment, generating patches, and submitting pull requests. Repository labels are defined in [`.github/labels.yml`](.github/labels.yml) and synchronised automatically.

Board maintainers are tracked via `BOARD_MAINTAINER` fields and the auto-generated [`CODEOWNERS`](.github/CODEOWNERS); to become a maintainer, [adjust the contact data](https://www.armbian.com/update-data/).

## Support

### Community Forums
Get help from users and contributors on troubleshooting, configuration, and development.
👉 [forum.armbian.com](https://forum.armbian.com)

### Real-time Chat
Join discussions with developers and community members on IRC or Discord.
👉 [Community Chat](https://docs.armbian.com/Community_IRC/)

### Paid Consultation
For commercial projects, guaranteed response times, or advanced needs, paid support is available from Armbian maintainers.
👉 [Contact us](https://www.armbian.com/contact)

## Contributors

Thank you to everyone who has contributed to Armbian!

<a href="https://github.com/armbian/build/graphs/contributors">
  <img alt="Contributors" src="https://contrib.rocks/image?repo=armbian/build" />
</a>

See also [CREDITS.md](CREDITS.md), the [Armbian team](https://github.com/orgs/armbian/people) and the [authors page](https://www.armbian.com/authors).

## Armbian Partners

Our [partnership program](https://forum.armbian.com/subscriptions) supports Armbian's development and community. Learn more about [our Partners](https://armbian.com/partners).

## License

This project is licensed under the terms of the **GNU General Public License, version 2**. See [LICENSE](LICENSE) for the full text.
