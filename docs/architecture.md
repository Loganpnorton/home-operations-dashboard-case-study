# Architecture

## System boundary

The dashboard sits between people, Home Assistant, household service providers, media devices, and mobile notification platforms. The browser is treated as an untrusted presentation client. Credentials and integration-specific operations stay behind the service boundary.

```text
Web or native client
        |
        | HTTPS and authenticated WebSocket
        v
Application service layer ---- SQLite operational state
        |
        +---- Home Assistant WebSocket API
        +---- Camera and media adapters
        +---- Notification providers
        +---- Household workflow adapters
```

## Client application

The React and TypeScript client uses a room-oriented information architecture. High-frequency actions are available from compact cards, while secondary controls open in bottom sheets that work well on small screens.

The client maintains a normalized view of entity state. Initial state is loaded when the session connects, then updated from Home Assistant events. Reconnection logic restores subscriptions and reconciles state after a network interruption.

Progressive web app support provides an installable shell, cached static assets, and an application-like launch experience. Dynamic device state is never treated as safely cacheable.

## Service layer

The Express service layer handles work that should not run in a browser:

- Secret-bearing provider requests
- Notification dispatch and delivery callbacks
- Receipt and bill processing workflows
- Durable operational state
- Request validation and authorization
- Rate limiting and failure isolation

Provider behavior is separated into adapters. This keeps account-specific details out of shared workflow code and makes an unreliable upstream service less likely to affect the dashboard as a whole.

## Real-time state

Home Assistant WebSocket events provide immediate device updates. The design avoids optimistic assumptions for safety-relevant or externally controlled entities. A command enters a pending state, and the interface confirms the resulting state from the event stream.

Connection state is visible to the user. When the event stream is unavailable, controls that require current state are disabled or clearly marked stale.

## Storage

SQLite stores application-specific workflow state rather than duplicating the full Home Assistant state model. Data access is isolated behind a small repository layer so retention, backup, and schema migration behavior can be tested independently.

## Media

Camera and doorbell surfaces use a media adapter to translate device-specific streams into formats suitable for web and native clients. Stream startup, timeout, and unavailable states are explicit UI states. Media endpoints are authenticated and are not exposed as public URLs.

## Testing strategy

The highest-value tests cover boundaries that commonly fail:

- Dashboard layout editing and persistence
- Home Assistant session restoration
- Client and server state synchronization
- Receipt extraction and correction flows
- Profile and notification preferences
- Mobile navigation and degraded connection behavior

Unit tests cover state reducers and adapters. Browser-level tests exercise complete workflows against controlled fixtures.

