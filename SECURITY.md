# Security Policy

Organization-wide security entry point for At Bryde Ud. It is intentionally
short. The **authoritative security baseline** — controls, audit requirements,
secrets requirements, backup and recovery requirements — lives in
[`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance).
Where a repository's own security policy is stricter, that one applies.

---

## Reporting a vulnerability

**Do not open a public issue, pull request or discussion for a suspected
vulnerability.**

Report it privately through GitHub:

1. Go to the affected repository → **Security** → **Report a vulnerability**
   (GitHub private vulnerability reporting).
2. If private reporting is unavailable on that repository, report it in
   [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance/security)
   instead and name the affected repository.

Include what you can: affected repository and version or commit, impact, steps
to reproduce, and any proof of concept. Report suspected exposure of a secret or
credential the same way — and never paste the secret itself into the report.

We will acknowledge the report, investigate, and coordinate on disclosure and
remediation. Please give us reasonable time to remediate before disclosing
publicly.

> A dedicated security contact address has not been published for this
> organization. Until organization owners designate one, GitHub private
> vulnerability reporting is the correct channel.

---

## Rules that apply everywhere

- **Never disclose secrets** in issues, pull requests, commits, logs, test
  fixtures, screenshots or agent transcripts. If a secret is exposed, treat it
  as compromised: rotate first, then report.
- **Never bypass repository security controls** — branch protection, required
  review, signed commits, required checks, secret scanning. If a control blocks
  legitimate work, request an exception (see [GOVERNANCE.md](GOVERNANCE.md)).
- **Use approved secret management.** Secrets belong in the approved secret store
  for the environment, never in a repository, an issue, a wiki or a chat message.
- **Prefer identity over long-lived credentials.** Use OIDC and workload identity
  where the provider supports it; long-lived credentials are an exception that
  must be scoped and rotated.
- **Do not extend agent authority beyond your own.** Agents operate under the AI
  policy in `ecosystem-governance` and the permission profiles in
  `ecosystem-library`.
- **Follow repo-local security policy where it is stricter.**

## Scope

This document covers how to report and the baseline expectations for anyone
working in At Bryde Ud repositories. It does not define the controls themselves —
that is `ecosystem-governance`.
