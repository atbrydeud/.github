# Architecture Principles

These principles explain the ecosystem model. Binding requirements are authored
in [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance).

## 1. Clear ownership: RULE → CONNECT → DEPLOY → EQUIP → OPERATE

Governance defines requirements. Bootstrap establishes accounts and
connections. Blueprints deploys systems. Library provides reusable intelligence.
Operations composes those capabilities into work. Platform and Protocols remain
the Web3 product/protocol domain.

## 2. AI-first

Agents are first-class participants. Repositories, interfaces and processes must
be legible to people and agents, while authority remains explicit and auditable.

## 3. Git is canonical for definitions

Configuration, policy, schemas, workflows and reusable components are reviewed
as code where practical. A SaaS console can be an operational surface but must
not be the only record of how the ecosystem is shaped.

## 4. Infrastructure and configuration as code where practical

Prefer repeatable, reviewable automation. Some provider steps remain assisted or
manual because billing, legal agreements, MFA, consent and identity proofing are
appropriately human.

## 5. Secrets stay outside Git

Secret values belong in approved stores. Bootstrap records provider/credential
references and connection metadata. Prefer OIDC/workload identity to long-lived
credentials; scope, rotate and revoke unavoidable credentials.

## 6. Shared standards, isolated organizations

Share code, protocols, skills and baselines upstream. Keep each organization's
identity, state, secrets, data, infrastructure and production authority separate.
Inheritance never implies central access.

## 7. Humans retain consequential authority

Agents operate under explicit authority boundaries. Financial actions,
production changes, security-control relaxation, destructive actions and other
consequential decisions retain the human authorization Governance requires.

## 8. Downstream independence

An organization must keep operating, deploying and recovering if At Bryde Ud's
central Platform or services are unavailable. Central coordination must not
become a production dependency.

## 9. Versioned baselines and explicit contracts

Consume immutable releases/tags/digests rather than moving branches. Define
inputs, outputs, permissions, compatibility and migrations at every layer
boundary.

## 10. Detect drift

Compare declared and actual state. Surface drift with evidence, then correct it
or record a time-boxed Governance exception.

## 11. Minimal direct dependencies

Repositories should depend on stable contracts, not each other's internal
layout. Bootstrap is the natural connector/assembler, but it must not become a
monolithic owner of systems, intelligence and workflows.

## 12. Pragmatic SaaS; deliberate sovereignty

Use SaaS where it provides meaningful leverage and exit/recovery is understood.
Prefer open source or self-hosting where sovereignty, ownership, portability or
economics justify the operating cost.

## 13. Minimal sufficient complexity

Build the smallest capability that meets the requirement. Every service,
abstraction and integration creates a long-lived operational obligation.

## Applying the principles

| Situation | Expected response |
|---|---|
| A principle conflicts with policy | Policy wins; reconcile the documents through review |
| A deadline pressures a binding requirement | Request a Governance exception; do not silently bypass it |
| Ownership is ambiguous | Use the project map and define the cross-layer contract before implementation |
| A change deliberately violates a principle | State the reason, risk, duration, owner and rollback in the pull request |

