# Governance

**This file is not the authoritative governance policy.** It explains how
governance works across At Bryde Ud and where the authoritative source lives.

> The authoritative governance policy — security baseline, audit requirements,
> AI policy, change control, exception policy, authority model — lives in
> [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance).
> Where this document and `ecosystem-governance` disagree, `ecosystem-governance`
> wins and this document is the defect.

---

## Ownership model

| Concern | Owner |
|---|---|
| What must be true (policy, baselines, authority, security, AI policy) | `ecosystem-governance` |
| How policy is applied to an organization | `ecosystem-bootstrap`, or the relevant applier |
| How the technical environment is built | `ecosystem-blueprints` |
| How organizations operate day to day | `ecosystem-operations` |
| Reusable intelligence and automation | `ecosystem-library` |
| The ecosystem control/product layer | `ecosystem-platform` |
| Shared protocol contracts | `ecosystem-protocols` |
| Organization-level GitHub defaults (this repository) | Organization owners, under governance intent set by `ecosystem-governance` |

GitHub *governance intent* — what branch protection, review and access rules
must exist — is owned by `ecosystem-governance`. The org-level defaults in this
repository (profile, templates, contribution and security entry points) are
conveniences that implement that intent for GitHub's own surfaces; they never
extend or relax it.

---

## Proposing a governance change

1. Open a **Governance change** issue in
   [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance/issues/new/choose)
   — not here. State the policy affected, the reason, downstream impact,
   organizations affected, migration required, risk, and the proposed version bump.
2. Get agreement on the *requirement* before proposing the mechanism.
3. Land the policy change in `ecosystem-governance` with a version bump where it
   changes what downstream organizations must do.
4. Land the mechanism in the repository that owns it (see
   [docs/PROJECT_MAP.md](docs/PROJECT_MAP.md)), referencing the governance change.
5. Downstream organizations adopt the new baseline version through their own
   change process.

**Policy before mechanism.** A mechanism change that quietly changes what is
required is a governance change wearing a disguise.

---

## Exceptions and escalation

- Exceptions to policy are **requested, recorded and time-boxed** — never
  implicit and never local. The exception process itself is defined in
  `ecosystem-governance`.
- An exception is scoped: to an organization, an environment and an expiry.
- If you cannot comply and cannot get an exception, stop and escalate rather
  than deviating silently.
- Escalation path: the owning project's maintainers → organization owners.
  Security issues follow [SECURITY.md](SECURITY.md) instead, and are never
  escalated through public issues.

---

## Changing this repository

Changes to org-level GitHub defaults (profile, issue and pull request templates,
these documents) are made by pull request in this repository and reviewed by
organization owners. Changes that alter what contributors are *required* to do
need a corresponding change in `ecosystem-governance` first.

## What does not belong here

Authoritative policy, implementation, infrastructure, protocol or platform code.
See the boundary rules in [README.md](README.md).
