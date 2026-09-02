# At Bryde Ud

**AI-first. Infrastructure-as-code-first.**

At Bryde Ud is the upstream ecosystem from which standalone organizations are
created, governed, operated, and continuously improved. Standards, capabilities
and protocols are built once here and inherited downstream.

> **Shared code and standards; separate state and authority.**

Git is the source of truth for ecosystem configuration. SaaS is used where it
provides leverage, never as the only definition of the ecosystem.

---

## The seven ecosystem projects

| Project | Answers | Owns |
|---|---|---|
| [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance) | What must be true. | Policy, authority, security baseline, audit, AI policy, exceptions, change control. |
| [`ecosystem-bootstrap`](https://github.com/atbrydeud/ecosystem-bootstrap) | How an organization is created or upgraded. | Organization declarations, bootstrap workflows, initial Azure/GitHub trust, remote state, baseline application. |
| [`ecosystem-blueprints`](https://github.com/atbrydeud/ecosystem-blueprints) | How the technical environment is built. | OpenTofu modules, Azure reference architecture, AKS, networking, zero trust, workload identity, Argo CD, observability, data, backups. |
| [`ecosystem-platform`](https://github.com/atbrydeud/ecosystem-platform) | The ecosystem control and product layer. | Dashboard, organization registry, lifecycle management, Web3 and treasury, approvals, shared APIs, cross-organization visibility. |
| [`ecosystem-operations`](https://github.com/atbrydeud/ecosystem-operations) | How organizations operate. | Plane configuration-as-code, work-item structures, workflows, SOPs, release and incident processes, Definitions of Done. |
| [`ecosystem-library`](https://github.com/atbrydeud/ecosystem-library) | What reusable intelligence exists. | Eve and reusable agents, skills, tools, MCP servers, instructions, evals, tool permission profiles. |
| [`ecosystem-protocols`](https://github.com/atbrydeud/ecosystem-protocols) | The shared protocol layer. | Protocol specifications, contracts, SDKs, primitives, token and governance mechanics, versioning. |

---

## How the layers fit together

```text
                        ┌────────────────────────────┐
                        │    ecosystem-governance    │   what must be true
                        └──────────────┬─────────────┘
                                       │ constrains everything below
      ┌────────────────┬───────────────┼───────────────┬────────────────┐
      ▼                ▼               ▼               ▼                ▼
  blueprints       operations       library        protocols        platform
  how technical    how orgs         reusable       shared           ecosystem
  environments     work             intelligence   protocol         control /
  are built                         capabilities   layer            product layer
      └────────────────┴───────────────┼───────────────┴────────────────┘
                                       │ consumed as versioned baselines
                        ┌──────────────▼─────────────┐
                        │    ecosystem-bootstrap     │   how environments
                        └──────────────┬─────────────┘   are instantiated
                                       ▼
              ┌────────────────────────────────────────────────┐
              │           Downstream organizations             │
              │  own identity · state · secrets · production   │
              │  authority · infrastructure · workloads · data │
              └────────────────────────────────────────────────┘
```

---

## Start here

| If you are… | Start with |
|---|---|
| New to the ecosystem | [Ecosystem overview](https://github.com/atbrydeud/.github/blob/main/docs/ECOSYSTEM.md) |
| Looking for the right repo to change | [Project map](https://github.com/atbrydeud/.github/blob/main/docs/PROJECT_MAP.md) |
| Onboarding as a contributor or agent | [Getting started](https://github.com/atbrydeud/.github/blob/main/docs/GETTING_STARTED.md) |
| Proposing a policy or baseline change | [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance) |
| Reporting a security issue | [Security policy](https://github.com/atbrydeud/.github/blob/main/SECURITY.md) |

Contribution baseline: [CONTRIBUTING.md](https://github.com/atbrydeud/.github/blob/main/CONTRIBUTING.md) ·
Governance model: [GOVERNANCE.md](https://github.com/atbrydeud/.github/blob/main/GOVERNANCE.md) ·
Where to ask: [SUPPORT.md](https://github.com/atbrydeud/.github/blob/main/SUPPORT.md)
