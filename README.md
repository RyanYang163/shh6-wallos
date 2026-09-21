# Wallos

> TOS 7 application package for **Wallos** — platform integration only.
> The application itself is provided by the upstream project, unmodified.

## Overview

Self-hosted subscription tracker with renewal reminders and spending statistics.

上游项目 / Upstream: <https://github.com/ellite/Wallos>
上游许可证 / License: **GPL-3.0**

## Features

- Track recurring subscriptions and their renewal dates
- Multi-currency support with exchange rates
- Spending statistics and category breakdowns
- Email notifications for upcoming renewals

## Installation

1. Requirements: TOS 7.0+ and Docker Engine (install from the TOS App Center)
2. Install from the TOS App Center
3. Open the app and complete initial configuration

## Usage

1. Access URL: `http://${ip}:18806`
2. Default credentials: see upstream documentation
3. Key settings: see upstream documentation

## Permissions

| Permission | Rationale |
|---|---|
| Network: port 18806 | Web UI access |
| File system: `/Volume*/DockerAppData/shh6-wallos/` | Application data persistence |
| User: shh6wallos | Isolated non-root service execution |

## Configuration

See `config.ini` for platform metadata; see `docker-compose.yml` for runtime configuration.

## Ports

| Port | Protocol | Purpose |
|---|---|---|
| 18806 | TCP | Web UI (Wallos) |

## Support

- Documentation: https://github.com/ellite/Wallos
- Issue tracker: https://github.com/ellite/Wallos/issues
- Community: https://github.com/ellite/Wallos

## Security & Compliance

- **License**: GPL-3.0 — full text in [`LICENSE`](./LICENSE)
- **Attribution**: see [`NOTICE`](./NOTICE)
- **Privacy Policy**: see [`PRIVACY.md`](./PRIVACY.md)
- **Vulnerability scan**: `trivy-report.txt` attached to each Release (HIGH/CRITICAL must be 0)
- Runs as a non-root dedicated user; no privileged mode, no host network

## Changelog

### v1.0.1 (2026-09-20)
- Compliance update: added LICENSE / NOTICE / PRIVACY materials,
  declared upstream license inside the package, added container healthcheck

### v1.0.0
- Initial release

## License

**GPL-3.0** — this packaging repository is distributed under the same license as the
upstream project. Full text: [`LICENSE`](./LICENSE).
