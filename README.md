# Home Operations Dashboard

This repository is a source-free architecture case study for a private home operations platform. It describes the engineering decisions behind a real-time Home Assistant dashboard, installable web application, native mobile shell, and containerized service layer without publishing household data, deployment configuration, or integration credentials.

## Project scope

The system brings device control and household workflows into one mobile-first interface:

- Real-time entity updates over the Home Assistant WebSocket API
- Room-oriented navigation and focused control sheets
- Camera and doorbell video surfaces
- Installable progressive web app behavior
- Receipt, bill, and household operations workflows
- Native iOS and Android packaging with push notifications
- iOS Live Activities and home-screen widgets
- Container health checks and automated service recovery
- End-to-end coverage for high-value user journeys

## Architecture at a glance

| Layer | Responsibilities |
| --- | --- |
| React and TypeScript client | Navigation, room views, controls, dashboards, offline shell, and responsive presentation |
| Express service layer | Integration boundaries, operational workflows, notification delivery, and server-only logic |
| Home Assistant connection | Authenticated WebSocket state updates and service calls |
| Operational storage | Local application state and workflow records in SQLite |
| Media services | Camera stream coordination and device-specific presentation |
| Native shell | Capacitor packaging, push delivery, Live Activities, and widgets |
| Runtime platform | Docker, reverse proxying, health checks, and recovery automation |

The detailed design is documented in [docs/architecture.md](docs/architecture.md).

## Engineering priorities

1. Keep integration credentials and household identifiers outside the client bundle.
2. Treat the Home Assistant event stream as the source of truth for device state.
3. Isolate provider-specific behavior behind server-side adapters.
4. Make common controls usable with one hand on a phone.
5. Preserve useful degraded behavior when a service or network connection is unavailable.
6. Test workflows that cross UI, persistence, notifications, and external integrations.

## Documentation

- [Architecture](docs/architecture.md)
- [Security and privacy](docs/security-and-privacy.md)
- [Operations](docs/operations.md)
- [Mobile delivery](docs/mobile-delivery.md)
- [Disclosure](DISCLOSURE.md)

## What this repository demonstrates

This case study focuses on system design, integration boundaries, privacy controls, reliability, and delivery strategy. The production source remains private because it is tightly coupled to a real household deployment.

