# `.github` — At Bryde Ud organization defaults

At Bryde Ud organization profile, shared GitHub standards, templates, and
ecosystem documentation.

This is the organization's **front door**, not one of the seven ecosystem
projects. It provides orientation, navigation and shared GitHub-facing defaults.
Implementation and authoritative policy live in the projects that own them.

**Start here:** [Ecosystem overview](docs/ECOSYSTEM.md) ·
[Project map](docs/PROJECT_MAP.md) ·
[Getting started](docs/GETTING_STARTED.md)

---

## What is in here

| Path | Purpose |
|---|---|
| [`profile/README.md`](profile/README.md) | The public organization landing page rendered on github.com/atbrydeud |
| [`docs/ECOSYSTEM.md`](docs/ECOSYSTEM.md) | Canonical high-level description of the ecosystem and the seven projects |
| [`docs/PROJECT_MAP.md`](docs/PROJECT_MAP.md) | "I need to change X — which repo owns it?" |
| [`docs/ARCHITECTURE_PRINCIPLES.md`](docs/ARCHITECTURE_PRINCIPLES.md) | The principles behind the model |
| [`docs/GETTING_STARTED.md`](docs/GETTING_STARTED.md) | Orientation for contributors and agents |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Org-wide contribution baseline |
| [`SECURITY.md`](SECURITY.md) | How to report vulnerabilities; org-wide security expectations |
| [`GOVERNANCE.md`](GOVERNANCE.md) | Governance ownership model and how changes are proposed |
| [`SUPPORT.md`](SUPPORT.md) | Where to ask, and which repo owns which request |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | Expected conduct across organization repositories |
| [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md) | Default pull request template |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE) | Default issue forms: bug, feature, governance change |

Files here are **defaults**: GitHub applies them to organization repositories
that do not define their own. A repository that needs something stricter or more
specific overrides it locally, and the local version wins.

## The seven ecosystem projects

| Project | Answers |
|---|---|
| [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance) | What must be true. |
| [`ecosystem-bootstrap`](https://github.com/atbrydeud/ecosystem-bootstrap) | How an organization is created or upgraded. |
| [`ecosystem-blueprints`](https://github.com/atbrydeud/ecosystem-blueprints) | How the technical environment is built. |
| [`ecosystem-platform`](https://github.com/atbrydeud/ecosystem-platform) | The ecosystem control and product layer. |
| [`ecosystem-operations`](https://github.com/atbrydeud/ecosystem-operations) | How organizations operate. |
| [`ecosystem-library`](https://github.com/atbrydeud/ecosystem-library) | What reusable intelligence capabilities exist. |
| [`ecosystem-protocols`](https://github.com/atbrydeud/ecosystem-protocols) | The shared protocol layer. |

Full detail and ownership boundaries: [docs/ECOSYSTEM.md](docs/ECOSYSTEM.md).

## Boundary rules

**This repository contains** orientation, navigation, shared GitHub defaults and
templates, organization-level contribution and security entry points, and the
canonical high-level architecture documentation.

**This repository does not contain** production OpenTofu modules, Azure runtime
definitions, AKS manifests, shared Eve agents or skills, Plane implementation
configuration, protocol implementations, At Bryde Ud platform source, or
authoritative governance policy. Those belong to the seven projects — and
duplicating them here would create a second, conflicting source of truth.

If you are about to add implementation here, check
[docs/PROJECT_MAP.md](docs/PROJECT_MAP.md) first.

## File placement

GitHub conventions decide the layout, not preference:

- Community health files (`CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md`,
  `GOVERNANCE.md`, `CODE_OF_CONDUCT.md`, `PULL_REQUEST_TEMPLATE.md`) are at the
  repository root, where GitHub picks them up as organization defaults.
- Issue forms live in `.github/ISSUE_TEMPLATE/` — the location GitHub requires
  for organization-wide default issue templates.
- `profile/README.md` is the organization profile page and is rendered on
  github.com/atbrydeud. It uses absolute links, because it is rendered outside
  this repository's context.

## Contributing to this repository

Pull requests welcome for the profile, templates and documentation here. Changes
that alter what contributors are *required* to do need a governance change in
[`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance)
first — see [GOVERNANCE.md](GOVERNANCE.md).
