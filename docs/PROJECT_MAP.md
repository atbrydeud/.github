# Project Map

**"I need to change X. Which repo owns it?"**

Use the first rule that matches. If two rules seem to match, the *policy vs.
mechanism* test below resolves it.

---

## The one test that resolves most ambiguity

| Ask | Answer | Repo |
|---|---|---|
| Am I changing what is **required**? | Yes | [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance) |
| Am I changing what **builds** it? | Yes | [`ecosystem-blueprints`](https://github.com/atbrydeud/ecosystem-blueprints) |
| Am I changing what **applies** it to an organization? | Yes | [`ecosystem-bootstrap`](https://github.com/atbrydeud/ecosystem-bootstrap) |
| Am I changing how **people and agents work**? | Yes | [`ecosystem-operations`](https://github.com/atbrydeud/ecosystem-operations) |
| Am I changing a **reusable agent/skill/tool**? | Yes | [`ecosystem-library`](https://github.com/atbrydeud/ecosystem-library) |
| Am I changing the **At Bryde Ud product surface**? | Yes | [`ecosystem-platform`](https://github.com/atbrydeud/ecosystem-platform) |
| Am I changing a **protocol contract**? | Yes | [`ecosystem-protocols`](https://github.com/atbrydeud/ecosystem-protocols) |

---

## Lookup table

| I want to change… | Repo |
|---|---|
| GitHub ruleset **policy** (what branch protection must exist) | `ecosystem-governance` |
| GitHub ruleset **application mechanism** (what applies the ruleset) | `ecosystem-bootstrap`, or the relevant applier |
| Security baseline, audit requirement, retention requirement | `ecosystem-governance` |
| AI policy — what agents may do and with what authority | `ecosystem-governance` |
| Exception process, change-control requirement, approval authority | `ecosystem-governance` |
| Secrets **requirements** (where secrets must live, rotation rules) | `ecosystem-governance` |
| Secrets **plumbing** (how a vault or key store is deployed) | `ecosystem-blueprints` |
| Backup/recovery **requirements** (RPO/RTO) | `ecosystem-governance` |
| Backup/recovery **implementation** | `ecosystem-blueprints` |
| An AKS, networking, storage or Postgres/Redis OpenTofu module | `ecosystem-blueprints` |
| Azure reference architecture, zero-trust or workload-identity pattern | `ecosystem-blueprints` |
| Argo CD, observability or runtime deployment pattern | `ecosystem-blueprints` |
| Organization **bootstrap declaration** (a new or upgraded organization) | `ecosystem-bootstrap` |
| Initial Azure/GitHub trust, OIDC federation setup, remote state init | `ecosystem-bootstrap` |
| Which baseline **version** an organization is on | `ecosystem-bootstrap` |
| Organization-level integration wiring | `ecosystem-bootstrap` |
| Plane workflow, work-item type, project template or state machine | `ecosystem-operations` |
| SOP, engineering process, Definition of Done | `ecosystem-operations` |
| Infrastructure **request** process (how you ask for a cluster) | `ecosystem-operations` |
| Release, incident or approval process | `ecosystem-operations` |
| Marketing/content workflow | `ecosystem-operations` |
| A shared skill, MCP server, connector or tool | `ecosystem-library` |
| An Eve or other reusable agent, agent template, reusable instruction | `ecosystem-library` |
| Evals, shared context conventions, tool permission profiles | `ecosystem-library` |
| At Bryde Ud dashboard, registry, API or provisioning interface | `ecosystem-platform` |
| Ecosystem lifecycle management, cross-organization visibility | `ecosystem-platform` |
| Web3, treasury or on-platform governance functionality | `ecosystem-platform` |
| Protocol specification, contract, SDK or shared primitive | `ecosystem-protocols` |
| Token/protocol mechanics, interoperability or compatibility rules | `ecosystem-protocols` |
| Organization profile page, org-wide issue/PR templates, this map | [`.github`](https://github.com/atbrydeud/.github) |

---

## Worked examples

**"Branch protection should require two reviewers."**
The requirement is policy → `ecosystem-governance`. The code that configures the
ruleset on a GitHub organization is a mechanism → `ecosystem-bootstrap` (or the
applier that owns it). Two changes, two repos, governance first.

**"Our AKS module should enable a private control plane by default."**
Module default → `ecosystem-blueprints`. If private control planes are to become
*mandatory*, that is a second change in `ecosystem-governance`.

**"Add a `deployment-request` work-item type in Plane."**
Operations → `ecosystem-operations`. If it introduces a new mandatory approval
gate, add the requirement in `ecosystem-governance`.

**"Agents should have a shared `terraform-plan-review` skill."**
Capability → `ecosystem-library`. The permission profile it runs under is also
`ecosystem-library`; the policy limiting what such a profile may include is
`ecosystem-governance`.

**"Show baseline version drift on the dashboard."**
Product surface → `ecosystem-platform`. The definition of what drift means may
belong to `ecosystem-governance`.

**"Bump the protocol to v2 with a breaking change."**
Specification and compatibility rules → `ecosystem-protocols`. Platform
integration of v2 → `ecosystem-platform`.

**"Onboard a new standalone organization."**
Declaration → `ecosystem-bootstrap`. It consumes an approved baseline version;
it does not redefine one.

---

## Rules of thumb

1. **Policy before mechanism.** If the requirement is changing, land the
   governance change first, then the mechanism.
2. **One concern, one repo.** If a change needs edits in three repos, it is
   probably three changes with a stated order.
3. **Don't fork upstream downstream.** Fix it where it is owned and consume the
   new version.
4. **This repo is not a backlog.** `.github` holds orientation, navigation and
   shared GitHub defaults — nothing implementable. See [SUPPORT.md](../SUPPORT.md).
