# Governance

This file explains governance routing. It is **not** the authoritative policy.
Binding policy, authority, security, audit, secrets, change-control, exception
and break-glass requirements live in
[`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance).

## Governance in the ecosystem

```text
RULE → CONNECT → DEPLOY → EQUIP → OPERATE
```

- Governance decides what must be true.
- Bootstrap establishes accounts, identities, credential references and connections.
- Blueprints deploys systems and runtimes that satisfy requirements.
- Library supplies reusable agents, skills, MCPs and tools.
- Operations defines the process, handoffs and approvals used in practice.
- Platform and Protocols remain the Web3 product/protocol domain.

An implementation choice does not become a requirement merely because it is the
current default. A requirement does not belong in an implementation repository
merely because that repository can enforce it.

## Proposing a governance change

1. Open a Governance change issue in `ecosystem-governance`.
2. State the current and proposed requirement, reason, authority, affected
   organizations, risk, exceptions, version impact and downstream migration.
3. Obtain the required human decision on the requirement.
4. Land/version the policy change.
5. Implement it through linked pull requests in the owning repositories, using
   [docs/PROJECT_MAP.md](docs/PROJECT_MAP.md).
6. Verify adoption and drift without claiming that “prepared” means “applied.”

## Examples

| Change | Owner |
|---|---|
| Require two reviewers for production | Governance |
| Configure GitHub rulesets to enforce it | Bootstrap |
| Define the review/exception workflow | Operations |
| Display compliance status | Platform |

## Exceptions and escalation

Exceptions are requested, approved by the proper human authority, recorded,
scoped and time-boxed. An agent cannot grant itself or its operator an exception.
If policy and implementation conflict, stop, preserve evidence and escalate to
the owning maintainers and organization owners. Security issues follow
[SECURITY.md](SECURITY.md), never a public issue.

## This repository

The `.github` repository owns orientation and shared GitHub-facing defaults. It
does not own authoritative policy or implementation. Changes here that alter a
requirement need an authorized Governance change first.

