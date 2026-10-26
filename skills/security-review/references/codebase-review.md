# Codebase Review

Use this mode for an application-wide or pre-release review. Read the shared report format before reporting.

## Map the application

Identify the framework, public entry points, privileged operations, data stores, background tasks, external integrations, and deployment definitions that are actually available. Enumerate the inspected paths and the parts left unreviewed; a partial repository view is partial coverage.

Work through applicable trust boundaries rather than assigning one finding to every checklist item.

| Area | Evidence to follow |
| --- | --- |
| Authentication | Route composition, session or token verification, expiry and revocation, account recovery, server-side checks before protected work |
| Authorization | Actor-to-object access, tenant boundaries, roles, ownership, administrative operations, delegated permissions |
| Injection | Input flowing into SQL or NoSQL queries, shells, templates, deserializers, redirects, server-side network requests, or filesystem paths |
| Browser boundaries | Rendering context, framework escaping and raw HTML paths, cookie flags, browser-accessible tokens, state-changing request CSRF defenses, CORS behavior |
| Secrets and data | Committed credentials, logging and error paths, response fields, password storage, encryption and key handling for recoverable sensitive data |
| Files and resources | Upload size and type enforcement, storage paths, archive handling, path traversal, resource limits, brute-force and abuse controls |
| Dependencies and configuration | Lockfile-resolved versions, primary advisories, debug settings, deployment policies, database reachability, least-privilege grants |
| Business actions | Payment or entitlement checks, webhook authentication and replay handling, idempotency, race-sensitive state transitions when present |

## Avoid checklist false positives

- Trace composed authentication and authorization; a handler can be protected by a parent route or middleware.
- Password storage calls for a suitable salted password hash, not reversible password encryption. Review recoverable sensitive fields separately.
- Browser token storage requires a concrete threat model. JavaScript-readable tokens increase the impact of XSS; moving to cookies also changes CSRF considerations. Do not classify every localStorage reference as a verified exploit.
- A wildcard CORS header on intentionally public, non-credentialed content is different from an origin policy exposing private data. Inspect request credentials and browser enforcement.
- Missing CSP, rate limiting, or encryption settings in an incomplete snapshot is not proof that a deployed upstream control is absent.
- Read database exposure from deployment/network evidence; a connection string or local port alone does not prove public reachability.
- Use current primary documentation or advisories for version-dependent claims. If lookup is unavailable, keep the affected claim conditional and name the gap.

## Complete this mode

For each applicable area, provide a verified finding, a scoped Not observed observation, or a concrete coverage gap. Prioritize validated paths; do not make a broad completion claim because a single search or scanner returned no hits.
