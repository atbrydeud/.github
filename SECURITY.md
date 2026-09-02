# Security Policy

This is the organization-wide reporting entry point. Authoritative security,
secrets, audit, data-boundary, backup/recovery and agent-authority requirements
live in
[`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance).

## Reporting a vulnerability

**Do not open a public issue, pull request or discussion for a suspected
vulnerability. Do not paste the secret or exploit data into a report.**

Use GitHub private vulnerability reporting in the affected repository. If it is
unavailable, use the private security reporting path on
[`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance/security)
and identify the affected repository.

Include the affected version/commit, impact, safe reproduction details and a
contact method where possible. For a suspected credential exposure, rotate or
revoke it immediately when authorized, preserve non-sensitive evidence and report
the incident privately.

## Rules applying across repositories

- Never commit or paste secrets, tokens, private keys, recovery codes,
  connection strings or unredacted sensitive logs.
- Git stores approved secret **references and metadata**, not secret values.
- Bootstrap owns provider connections and credential-reference plumbing;
  Governance owns the requirements those mechanisms must satisfy.
- Prefer OIDC/workload identity. Scope, rotate and revoke unavoidable long-lived
  credentials.
- Do not bypass branch, review, scanning, signing, approval or environment controls.
- Agents cannot exceed the operator's authority or approve their own actions.
- Follow a repository's stricter local security instructions where present.

Do not describe an unresolved vulnerability in an ordinary PR. Coordinate
disclosure and remediation through the private reporting path.

