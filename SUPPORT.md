# Support

**Ask in the repository that owns the concern.** This repository is the front
door, not the help desk, and not a backlog.

---

## Where to go

| I want to… | Go to |
|---|---|
| Report a bug in a project | That project's issue tracker |
| Request a feature | The project that owns the surface — see [docs/PROJECT_MAP.md](docs/PROJECT_MAP.md) |
| Propose a policy or baseline change | [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance/issues) |
| Report a security vulnerability | [SECURITY.md](SECURITY.md) — **never a public issue** |
| Understand how the ecosystem fits together | [docs/ECOSYSTEM.md](docs/ECOSYSTEM.md) |
| Find out which repo owns something | [docs/PROJECT_MAP.md](docs/PROJECT_MAP.md) |
| Onboard as a contributor or agent | [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md) |
| Fix the organization profile, org templates or these documents | This repository |

## Which repository owns which kind of request

| Request type | Repository |
|---|---|
| Policy, authority, security baseline, audit, AI policy, exceptions | [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance) |
| Creating or upgrading an organization, trust, remote state, baseline application | [`ecosystem-bootstrap`](https://github.com/atbrydeud/ecosystem-bootstrap) |
| OpenTofu modules, Azure/AKS architecture, networking, observability, data, backups | [`ecosystem-blueprints`](https://github.com/atbrydeud/ecosystem-blueprints) |
| Dashboard, registry, APIs, lifecycle, approvals, Web3 and treasury | [`ecosystem-platform`](https://github.com/atbrydeud/ecosystem-platform) |
| Plane configuration, workflows, SOPs, templates, release and incident process | [`ecosystem-operations`](https://github.com/atbrydeud/ecosystem-operations) |
| Agents, skills, tools, MCP servers, evals, permission profiles | [`ecosystem-library`](https://github.com/atbrydeud/ecosystem-library) |
| Protocol specifications, contracts, SDKs, compatibility | [`ecosystem-protocols`](https://github.com/atbrydeud/ecosystem-protocols) |

## What belongs in *this* repository

Only the organization front door: the profile page, org-wide contribution,
security and support entry points, shared issue and pull request templates, and
the canonical high-level ecosystem documentation.

**Do not use this repository as a catch-all implementation backlog.** Issues that
describe work in one of the seven projects will be redirected there. If you are
unsure which project owns something, that ambiguity is itself worth an issue
here — against [docs/PROJECT_MAP.md](docs/PROJECT_MAP.md), so the map gets fixed.

## Response expectations

These are public repositories maintained by a small team. Issues are triaged in
the owning repository; security reports take priority over everything else.
