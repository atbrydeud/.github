# At Bryde Ud — the ecosystem foundation

At Bryde Ud is the foundation beneath a much larger ecosystem of independent
ventures, capital organizations, trust institutions, communities and protocols.
It gives them a common rulebook, shared digital foundations and a way to work
together without forcing them into one corporation.

It is **not a holding company** that owns every participant. It is **not a public
company, public marketplace or open public network** where anyone automatically
receives access or authority. It is a governed foundation: a neutral coordinating
layer that protects the integrity of the whole while each organization keeps its
own identity, ownership, data, infrastructure and decision-making authority.

## Three connected ecosystem triangles

At Bryde Ud connects three mutually reinforcing domains:

| Triangle | Purpose | What it enables |
|---|---|---|
| **Ventures** | Creates and grows companies, projects, products and protocols | Ideas become real organizations and useful infrastructure |
| **Capital** | Structures, finances and scales credible opportunities | Good ventures can attract resources and move from potential to execution |
| **Trust** | Makes identity, evidence, authority, relationships and commitments verifiable | Organizations can collaborate without relying only on informal relationships or one central operator |

**Ventures creates what can exist. Capital makes it financeable. Trust makes
cooperation credible.** At Bryde Ud connects the three so that each ecosystem
strengthens the others instead of developing as an isolated collection of
companies and tools.

This creates a foundation broad enough to support:

- **for-profit organizations** building products, services and economic value;
- **non-profits and public-interest institutions** stewarding communities,
  standards and shared missions; and
- **protocols** encoding durable rules, rights and interoperability that should
  survive any one company or leadership team.

No participant needs to surrender its independence to benefit from the whole.
Shared standards and capabilities can compound across the ecosystem, while
ownership, liability, secrets, data and production authority remain where they
belong.

## The neutral referee

At Bryde Ud acts as the ecosystem's **ultimate referee, not its operating boss**.
Its legitimacy comes from two things working together:

1. **A governed rule system** defines authority, responsibilities, boundaries,
   approvals, security expectations and how exceptions or disputes are handled.
2. **A shared platform** makes those rules usable by coordinating identities,
   organizations, capabilities, evidence, status and protocol interactions.

The rules constrain the platform; the platform makes the rules observable and
actionable. Neither should quietly replace the other. Consequential authority
remains human-governed, decisions are auditable, and downstream organizations
must continue operating even when the central platform is unavailable.

## From foundation to daily operation

The repository architecture turns that larger vision into a practical operating
model:

```text
CONNECT → RULE → DEPLOY → EQUIP → OPERATE
```

| Layer | Repository | Responsibility |
|---|---|---|
| **CONNECT** | [`ecosystem-bootstrap`](https://github.com/atbrydeud/ecosystem-bootstrap) | Establishes accounts, identities, secrets architecture and service connections |
| **RULE** | [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance) | Defines what must be true and who has authority |
| **DEPLOY** | [`ecosystem-blueprints`](https://github.com/atbrydeud/ecosystem-blueprints) | Deploys infrastructure, shared systems and agent runtimes |
| **EQUIP** | [`ecosystem-library`](https://github.com/atbrydeud/ecosystem-library) | Supplies reusable agents, skills, MCPs, tools and components |
| **OPERATE** | [`ecosystem-operations`](https://github.com/atbrydeud/ecosystem-operations) | Composes those capabilities into repeatable human and agent work |

Connection comes before rule: you cannot govern a tool that is not connected
yet. Once Bootstrap has established the accounts, identities and credentials,
Governance has something real to enforce its rules onto.

[`ecosystem-platform`](https://github.com/atbrydeud/ecosystem-platform) is the
At Bryde Ud ecosystem/Web3 product and coordination surface.
[`ecosystem-protocols`](https://github.com/atbrydeud/ecosystem-protocols) is the
canonical home for the three At Bryde Ud protocol definitions. Together, the
seven repositories provide the constitutional, technical and operational
foundation on which the larger Ventures, Capital and Trust ecosystems can grow.

## Why this matters

Without a shared foundation, an ecosystem becomes a collection of disconnected
companies, SaaS accounts, agents and protocols. Knowledge is duplicated,
relationships depend on particular people, controls diverge, and collaboration
breaks as the network grows.

At Bryde Ud makes scale possible without turning scale into central ownership.
It lets independent organizations reuse trusted foundations, interoperate under
known rules and contribute improvements back to the whole—while remaining
sovereign enough to operate, evolve or leave without being trapped by a central
company or platform.

## About this repository

This public `.github` repository is the ecosystem's GitHub front door. It
provides organization orientation, shared GitHub-facing defaults and the
canonical high-level repository map. It is not an eighth implementation layer
and does not replace authoritative policy in `ecosystem-governance`.

## Start here

| Document | Purpose |
|---|---|
| [`profile/README.md`](profile/README.md) | Public organization landing page |
| [`docs/ECOSYSTEM.md`](docs/ECOSYSTEM.md) | Canonical ownership model for the seven repositories |
| [`docs/PROJECT_MAP.md`](docs/PROJECT_MAP.md) | “I need to change X — where does it belong?” |
| [`docs/END_TO_END.md`](docs/END_TO_END.md) | Step by step: connecting, governing and deploying one organization |
| [`docs/ARCHITECTURE_PRINCIPLES.md`](docs/ARCHITECTURE_PRINCIPLES.md) | Design principles behind the model |
| [`docs/GETTING_STARTED.md`](docs/GETTING_STARTED.md) | Contributor and agent orientation |
| [`docs/MIGRATION_FOLLOW_UPS.md`](docs/MIGRATION_FOLLOW_UPS.md) | Known misplaced content requiring separate migration |
| [`AGENTS.md`](AGENTS.md) | Instructions for agents working in this repository |

Community health/default files remain at the repository root or GitHub-required
`.github/ISSUE_TEMPLATE/` paths: `CONTRIBUTING.md`, `GOVERNANCE.md`,
`SECURITY.md`, `SUPPORT.md`, `CODE_OF_CONDUCT.md`, the pull-request template and
issue forms.

## Repository boundaries

This repository owns navigation, orientation, the organization profile and
shared GitHub templates. It does not own policy, provider setup, infrastructure,
runtimes, reusable intelligence, operating workflows, product implementation or
protocol definitions.

Changes that alter requirements must first be authorized in Governance. Changes
here go through pull request review and must remain consistent with
[docs/ECOSYSTEM.md](docs/ECOSYSTEM.md).
