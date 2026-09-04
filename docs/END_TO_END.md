# End to End: Connect, Rule, Deploy

How one organization goes from nothing to a running system, using the first three
repositories in the stack.

[ECOSYSTEM.md](ECOSYSTEM.md) says what each layer *owns*. This document says what you
actually *do*, and which command does it. It stops where DEPLOY stops; EQUIP and OPERATE
are named at the end but not covered.

## The shape is not a straight line

The canonical order is `CONNECT → RULE → DEPLOY`, and that order is real: you cannot
govern a tool that is not connected yet. But it describes when each layer *becomes
active*, not a queue in which each finishes before the next begins.

**Bootstrap and Blueprints are sequential. Governance is transversal.**

```text
                    ┌──────────────────────────────────────────────┐
   BOOTSTRAP  ────▶  │              GOVERNANCE                      │
    CONNECT          │  becomes enforceable once things are         │
   (a milestone)     │  connected, then applies across everything   │
                    └──────────────────────────────────────────────┘
                              │              │              │
                              ▼              ▼              ▼
                         BLUEPRINTS       LIBRARY       OPERATIONS
                           DEPLOY          EQUIP          OPERATE
```

Bootstrap is something you finish: the organization is connected, and you move on.
Governance is not. It starts after Bootstrap because rules land on real accounts, and
from that moment it cuts *across* every layer — the GitHub organization's rules, the
subscription's access control, what a deployment is allowed to do, what authority an
agent has at runtime. There is no point at which you are "done with governance" and
proceed to deploying.

So read the sequence below as: **connect once, turn governance on, and from then on every
deployment happens inside it.**

**The three layers do not work the same way, and that is the second thing to know.**

| Layer | Repository | How you drive it | What it produces |
|---|---|---|---|
| CONNECT | `ecosystem-bootstrap` | A CLI you run on a laptop: `bryde-connect` | A declaration file — YAML describing the organization's accounts and credential *references* |
| RULE | `ecosystem-governance` | No CLI **yet** — see below. You edit YAML and open a pull request; workflows plan and apply | Requirements, and enforcement applied onto connected things |
| DEPLOY | `ecosystem-blueprints` | No CLI. You call its modules from your own root module and run `tofu` | Running infrastructure and runtimes |

Only Bootstrap has a CLI. Governance and Blueprints are driven by editing files and
running standard tools. Anyone expecting three command-line tools will be looking for two
that do not exist — and in Governance's case, for one that arguably should.

### The missing enforcement CLI

Governance today applies its rules through Terraform, invoked from its own workflows. That
works, and it is worth naming why it is nonetheless the wrong long-term shape.

**Infrastructure-as-code tooling belongs to DEPLOY.** OpenTofu is Blueprints' instrument
for building systems. When Governance reaches for the same class of tool to apply a GitHub
branch-protection rule or an organization setting, the RULE layer takes on a dependency
that belongs to the layer below it, and the boundary the model is built on gets blurred at
exactly the point it matters most.

The shape that matches the model is the one Bootstrap already has: **Governance owning an
enforcement CLI of its own**, which reads the declaration, reads the control catalogue, and
applies organization rules directly through each provider's API — reporting one recorded
outcome per rule and target, as
[`APPLYING_RULES.md`](https://github.com/atbrydeud/ecosystem-governance/blob/main/docs/APPLYING_RULES.md)
already requires of any applier.

Until that exists, treat the Terraform in Governance's workflows as an implementation
detail of the RULE layer rather than as a pattern to copy. **Do not conclude from it that
Governance and Blueprints share a toolchain.** They do not, and Blueprints explicitly
forbids the Terraform CLI in its own repository.

---

## Before you start

- `gh`, authenticated as yourself. Bootstrap's CLI reads GitHub through it and never
  asks for a token of its own.
- `node` for the Bootstrap CLI.
- `tofu` (OpenTofu) for Blueprints. **Not** the Terraform CLI — see the note in step 3.
- A directory to hold your organization's checkouts, which is **not** inside
  `ecosystem-bootstrap`.

No credential is needed to begin. Credentials appear only at the two points this
document marks explicitly.

---

## Step 1 — CONNECT: describe the organization

**Repository:** `ecosystem-bootstrap`. **Tool:** `bryde-connect`.

The CLI asks questions and writes YAML. The declaration is the artifact — reviewable,
diffable, version-controlled — and execution happens elsewhere, from that file. The CLI
holds no token and cannot act with administrative rights unattended.

```bash
cd ~/where-your-organization-checkouts-live

# No checkout needed; npm clones and runs it.
npx github:atbrydeud/ecosystem-bootstrap init
```

Run it from a directory that is *not* the Bootstrap repository. Writing an organization
into `ecosystem-bootstrap` is refused.

`init` reads GitHub, and only reads it — the organization already describes itself in
public, so its identity is discovered rather than retyped. Those reads go through your
own `gh`, which already holds your authorization.

Then add the providers the organization needs and record where their secrets live:

```bash
bryde-connect add <provider>            # add a provider to the declaration
bryde-connect secret ref <provider>     # record a credential REFERENCE, never a value
bryde-connect status                    # what we believe, from the record. No network.
```

### The one distinction worth learning here

**`status` reads the record. `verify` goes and looks.**

`status` makes no network call, needs no credential, and answers "what do we believe"
from a laptop with nothing open. `verify` contacts the provider, so it needs the
administrative credential, so it needs the human who can unlock it — it refuses outright
when no terminal is attached.

```bash
bryde-connect verify [provider]   # contacts a provider. Needs a human, always.
bryde-connect plan                # what would be established
bryde-connect apply               # authorization gate; writes nothing itself
```

`apply` writes nothing, not even the declaration. It is an authorization gate, and what
it authorizes is the declaration — not an action the CLI performs.

**Secret values never enter Git.** What the declaration holds is a *reference*: which
store, which path. Everything downstream consumes the reference.

**Output of this step:** a declaration file describing accounts, identities, provider
relationships and credential references. Governance and Blueprints both read it.

---

## Step 2 — RULE: state what must be true, and enforce it

**Repository:** `ecosystem-governance`. **Tool:** none — pull requests and workflows.

Governance comes second because rules are enforced *onto connected things*. There is no
access control to set on a subscription that does not exist yet. Its position in the
sequence does not limit its authority: every other layer, Bootstrap included, operates
within its requirements.

Governance defines **requirements**, not the mechanism that satisfies them. "Database
traffic must be encrypted" is Governance. The input that turns encryption on is
Blueprints.

You work here by editing YAML and opening a pull request:

```text
enforcement/controls/control-catalog.yaml     the controls themselves
enforcement/controls/risk-levels.yaml         how severity is classified
enforcement/controls/exceptions.yaml          what is exempt, and why
enforcement/companies/<organization>.yaml     which controls apply to whom
enforcement/github/                           GitHub organization enforcement
```

The workflows do the rest:

| Workflow | Fires on | What it does |
|---|---|---|
| `validate` | pull request | Checks the YAML and formatting |
| `plan-governance` | pull request | Shows what applying would change |
| `apply-governance` | push to the default branch | Applies it |
| `audit-drift` | schedule | Reports where reality has diverged from the rules |

So the loop is: edit YAML → open a PR → read the plan on the PR → get the human approval
Governance itself requires → merge, and the apply workflow runs.

Read [`docs/APPLYING_RULES.md`](https://github.com/atbrydeud/ecosystem-governance/blob/main/docs/APPLYING_RULES.md)
before your first change. It is normative, and it defines the split between *defining* a
rule and *applying* one — including which layer applies which class of rule, and how
Governance uses a credential without ever holding one.

**A rule with no target is not a failure.** If a control names something Bootstrap has
not connected yet, it simply has nothing to apply to. That is expected and reported, not
an error.

**Output of this step:** requirements that are enforceable, and enforcement applied to
what Bootstrap connected.

---

## Step 3 — DEPLOY: build the systems

**Repository:** `ecosystem-blueprints`. **Tool:** `tofu`.

Blueprints is a library of reusable modules. **Nothing in it applies anything.** There is
no `apply` in any of its workflows and no environment it could target. You consume it
from your own root module — the one that holds your organization's values and your state
backend — and you run `tofu` there.

> **OpenTofu, not Terraform.** Blueprints requires the `tofu` CLI. Note that
> Governance's own workflows currently invoke `terraform`; the two repositories differ
> here, and if you are moving between them, use each repository's own documented command
> rather than assuming they match.

### 3a. Point a root module at it

```hcl
module "landing_zone" {
  source = "github.com/atbrydeud/ecosystem-blueprints//modules/landing-zone?ref=<version>"

  organization = "<short code>"
  # subscription, tenant and region are declared inputs, never ambient
}
```

Pin a version. Consumers pin releases rather than tracking `main`.

The values you pass are the ones Bootstrap established — subscription, tenant, identity,
and credential *references*. That is the seam between the two layers: Bootstrap decides
what exists and where its secrets are; Blueprints consumes those as typed inputs.

### 3b. Run it

```bash
tofu init                 # your backend config comes from your pipeline, not from Blueprints
tofu validate
tofu plan
tofu apply                # your authorization, in your root module, against your subscription
```

Blueprints' own repository checks — the ones its contributors run — are different, and
apply to changing the modules rather than using them:

```bash
tofu fmt -check -recursive .
tofu init -backend=false && tofu validate     # per module and example directory
tflint --chdir=<dir> --config="$PWD/.tflint.hcl"
trivy config --config trivy.yaml .
trivy fs --scanners secret .
```

### 3c. The steps a human runs

Some steps cannot be declarative, and Blueprints says which. On the self-managed cluster
path, the cluster's etcd bootstrap is one: it needs the Talos API of a control-plane node
on a port a runner outside the virtual network does not have.

```bash
talosctl bootstrap --nodes <control-plane node>
```

See [`docs/TALOS_CLUSTER.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/docs/TALOS_CLUSTER.md)
for that sequence, and
[`docs/AGNTCY_DEPLOYMENT.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/docs/AGNTCY_DEPLOYMENT.md)
for a worked example of a shared runtime landing on it — including which of its steps are
declarative and which a person performs.

**Output of this step:** running infrastructure and runtimes.

---

## The whole sequence, in order

| # | Step | Layer | Who | Command |
|---|---|---|---|---|
| 1 | Describe the organization | CONNECT | Human | `bryde-connect init` |
| 2 | Add providers | CONNECT | Human or agent | `bryde-connect add <provider>` |
| 3 | Record credential references | CONNECT | Human or agent | `bryde-connect secret ref <provider>` |
| 4 | Confirm what is real | CONNECT | **Human, always** | `bryde-connect verify` |
| 5 | Authorize the declaration | CONNECT | **Human** | `bryde-connect apply` |
| 6 | State the requirements | RULE | Human or agent | Edit YAML, open a PR |
| 7 | Read the plan, get approval | RULE | **Human** | Review the PR |
| 8 | Enforce | RULE | Workflow | Merge; `apply-governance` runs |
| 9 | Compose modules in a root module | DEPLOY | Human or agent | Edit HCL |
| 10 | Plan and apply the foundation | DEPLOY | **Human authorization** | `tofu plan` / `tofu apply` |
| 11 | Bootstrap the cluster, if self-managed | DEPLOY | **Human** | `talosctl bootstrap` |
| 12 | Deploy the runtimes | DEPLOY | Human or agent | `tofu apply` in the chart layer |

Steps 4, 5, 7, 10 and 11 need a person. That is deliberate in each case, and each layer
documents why rather than leaving it implicit.

**Steps 6 to 8 are the exception to the numbering.** They are the *first* pass through
governance, not the only one. Every later change — a new control, a new organization
brought under an existing control, a deployment that introduces something to govern —
re-enters at step 6, while steps 9 to 12 continue independently. The drift audit runs on a
schedule and can send you back to step 6 without anything having changed on your side.

The one hard ordering constraint is that **step 6 cannot usefully precede step 1**: a
control naming a provider nobody has connected has nothing to apply to. That is reported
rather than treated as a failure, but it is also not progress.

---

## Where this stops

DEPLOY leaves you with running systems and nothing running *on* them.

- **EQUIP** — `ecosystem-library` supplies the agents, skills, tools and MCP servers.
- **OPERATE** — `ecosystem-operations` composes those into actual work: Plane
  configuration, SOPs, approval flows, n8n workflows and the human/agent handoffs.

The recurring distinction, in one line: **the n8n runtime is Blueprints; the n8n flow is
Operations.** A generic agent is Library; the process deciding when it runs is Operations.

---

## Mistakes this order prevents

**Governing something that is not connected.** Rules land on real accounts. Running RULE
before CONNECT gives you a catalogue of controls with nothing to apply to.

**Treating governance as a phase you complete.** It is the transversal layer: it turns on
after Bootstrap and then applies across every deployment, every capability and every agent
that follows. A team that "did governance" in week one and moved on has a control
catalogue, not enforcement.

**Putting a secret in Git.** No layer here stores a secret value. Bootstrap records a
reference; Governance uses a credential without holding one; Blueprints accepts
references as typed inputs and creates none.

**Deploying from the Blueprints repository.** It applies nothing. If you are looking for
where to run `apply`, it is your root module, against your subscription, under your
authorization.

**Assuming a layer owns the next one's work.** The boundary matrix in
[ECOSYSTEM.md](ECOSYSTEM.md) settles it, and
[PROJECT_MAP.md](PROJECT_MAP.md) answers "I need to change X — where does it belong?"

---

## Related

| Document | What it gives you |
|---|---|
| [ECOSYSTEM.md](ECOSYSTEM.md) | The canonical ownership model for all seven repositories |
| [PROJECT_MAP.md](PROJECT_MAP.md) | Where a proposed change belongs |
| [GETTING_STARTED.md](GETTING_STARTED.md) | Orientation and the contribution loop |
| [ARCHITECTURE_PRINCIPLES.md](ARCHITECTURE_PRINCIPLES.md) | Why the model is shaped this way |
| [Bootstrap `docs/CLI.md`](https://github.com/atbrydeud/ecosystem-bootstrap/blob/main/docs/CLI.md) | Every `bryde-connect` command in detail |
| [Governance `docs/APPLYING_RULES.md`](https://github.com/atbrydeud/ecosystem-governance/blob/main/docs/APPLYING_RULES.md) | How a rule reaches the thing it governs |
| [Blueprints `docs/MODULE_CONTRACT.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/docs/MODULE_CONTRACT.md) | What every module guarantees a caller |

Nothing in this document grants authority. Where it and a policy differ, the policy wins
and this document is the defect.
