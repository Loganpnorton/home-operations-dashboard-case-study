# Operations

## Container layout

The web client and service layer run in a container behind a reverse proxy. The image is built in stages so development dependencies and source tooling do not remain in the runtime layer.

A health endpoint checks application readiness without exposing configuration. Container health checks drive recovery automation, and repeated failures are surfaced through an external notification channel.

## Deployment checks

A release is accepted only after:

1. Type checking and linting pass.
2. Unit and browser-level tests pass.
3. The production bundle builds without embedded secrets.
4. Database migrations are tested against a backup copy.
5. The container starts and reaches a healthy state.
6. Home Assistant reconnect and notification delivery are exercised.

## Backup and recovery

SQLite backups are created from a consistent snapshot and stored separately from the host. Recovery exercises verify both schema compatibility and the application startup path. Configuration is rebuilt from protected deployment inputs rather than restored from an unreviewed working directory.

## Observability

Useful operational signals include:

- Home Assistant connection and reconnection counts
- Integration request latency and error rate
- Notification acceptance and delivery outcomes
- Media startup failures
- Database migration and backup status
- Container restarts and health-check failures

Metrics and logs use redacted identifiers. Household data is not required to diagnose service health.

## Failure handling

External providers are assumed to fail independently. Adapters use bounded timeouts, limited retries, and clear error categories. The dashboard continues serving unrelated controls when a billing, media, or notification integration is unavailable.

## Incident response

For a suspected credential disclosure, the response sequence is to restrict ingress, rotate the affected credential, invalidate sessions, inspect access logs, redeploy from reviewed inputs, and document the cause. Git history is treated as permanently disclosed even if a later commit removes the value.

