# Security and privacy

## Trust model

The client can request permitted operations, but it never receives provider credentials or unrestricted Home Assistant access. The server validates identity, authorization, input shape, and resource ownership before crossing an integration boundary.

## Credential handling

- Secrets are supplied through the runtime environment or a managed secret store.
- Browser bundles contain only non-sensitive configuration.
- Home Assistant tokens use the narrowest practical permissions.
- Webhook endpoints require a signature, shared secret, or authenticated session.
- Mobile push keys and signing material remain in protected build environments.
- Credentials are rotated after suspected disclosure and are never retained in Git history.

## Data minimization

The application stores only data needed for a user-facing workflow. Raw provider responses are transformed into a minimal internal representation and discarded when they are no longer required.

Logs use stable event names and internal correlation identifiers. They exclude addresses, camera URLs, message bodies, account identifiers, receipt contents, and tokens. Error reporting applies the same redaction rules.

## Retention

Operational records have explicit retention periods. Users can remove stored records, and backups follow the same expiration policy. Media is streamed when possible instead of being retained by default.

## Network boundaries

The service is placed behind TLS termination and an authenticated reverse proxy. Home Assistant and local media services remain on trusted networks. Public ingress is limited to documented application and callback routes.

## Repository policy

Production configuration, database snapshots, test receipts, household screenshots, and device inventories do not belong in a public repository. This case study uses generalized descriptions so it can be reviewed without weakening the deployment.

