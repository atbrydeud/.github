# End to End: Connect, Rule, Deploy

How one organization goes from nothing to a running system, using the first three
repositories in the stack.

[ECOSYSTEM.md](ECOSYSTEM.md) says what each layer *owns*. This document says what you
actually *do*, and which command does it. It stops where DEPLOY stops; EQUIP and OPERATE
are named at the end but not covered.

If all you need is TrueForge running today, take the
[shortcut](#shortcut--a-running-trueforge-today) instead. It names everything it skips.

## The shape is not a straight line

The canonical order is `CONNECT → RULE → DEPLOY`, and that order is real: you cannot
govern a tool that is not connected yet. But it describes when each layer *becomes
active*, not a queue in which each finishes before the next begins.

**Bootstrap and Blueprints are sequential. Governance is transversal.**

```text
  BOOTSTRAP
   CONNECT   ─────▶  the organization now exists and is connected
  (finishes)                        │
                                    ▼
            ┌───────────────────────────────────────────────────┐
            │                   GOVERNANCE                      │
            │                      RULE                         │
            │  enforceable from here on, and never "finished"   │
            └───────────────────────────────────────────────────┘
                  │                 │                 │
                  ▼                 ▼                 ▼
             BLUEPRINTS          LIBRARY          OPERATIONS
               DEPLOY             EQUIP             OPERATE
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
| RULE | `ecosystem-governance` | A CLI: `bryde-govern`. Rules are edited as YAML and applied by it | Requirements, and enforcement applied onto connected things |
| DEPLOY | `ecosystem-blueprints` | No CLI. You call its modules and patterns from your own root modules and run `tofu` | Running infrastructure and runtimes |

Bootstrap and Governance each have a CLI, and the two deliberately share a shape:
`status` reads the record with no network and no credential, a second mode goes and looks,
and the third changes things and refuses to run without a person. Learn it once.

Blueprints has none, and that is not an omission — it is a library of modules *and
patterns* you call from your own configuration. The tool you run there is `tofu`, in your
root module, against your subscription.

**Do not conclude that Governance and Blueprints share a toolchain.** They do not.
Blueprints forbids the Terraform CLI in its own repository, and Governance's enforcement
goes through its own CLI and each provider's API rather than through infrastructure-as-code.
Infrastructure-as-code tooling belongs to DEPLOY; when the RULE layer borrows it, the
boundary the model is built on gets blurred at the point it matters most.

---

## Shortcut — a running TrueForge today

The sequence above is the real path. This is not it. This is the fastest honest route to
TrueForge answering on a URL, for when the point is to see it run rather than to stand up
an organization. Every control it drops is named as a skip below, because a skipped control
that reads as a completed step is worse than no step at all.

Chart facts here come from Blueprints'
[`docs/TRUEFORGE_DEPLOYMENT.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/docs/TRUEFORGE_DEPLOYMENT.md),
which verified them against the upstream chart and pinned them to a released commit. Chart
`0.1.6-rc.0` is that commit's chart: its published values file and the pinned commit's are
identical apart from a trailing newline.

**What you end up with:** the stock TrueForge server — API and UI from one image — with the
chart's own bundled Postgres and Redis, on a cluster you can throw away, reached through a
port-forward, with no login of any kind.

### What you need

- `helm` at 3.8 or newer, and `kubectl`. The floor is 3.8 because installing a chart
  straight from an OCI registry — which the install command below does — is only
  supported from Helm 3.8.0.
- A Kubernetes cluster at `>=1.25`, the chart's own `kubeVersion` floor. Any cluster does;
  the commands below stand up a local one with `kind`, which needs Docker.
- Nothing else. No subscription, no credential, no declaration file, no `tofu`.

### The sequence

Create a cluster. Skip this step if you already have one and its context is current.

```bash
kind create cluster --name trueforge-fastpath
```

Install the chart. It is anonymously pullable, and **no values are required**: Postgres and
Redis are bundled subcharts that default to enabled, so the chart is installable as it ships.

```bash
helm install trueforge oci://tfy.jfrog.io/tfy-helm/trueforge \
  --version 0.1.6-rc.0 \
  --namespace trueforge --create-namespace \
  --wait --timeout 12m
```

The server container exits and restarts once or twice while Postgres finishes coming up —
the previous container's log ends at `connect ECONNREFUSED <cluster IP>:5432`. That is the
expected startup sequence and it clears itself; `--wait` returns once the pod is genuinely
ready. Helm then prints the release notes, which repeat every warning listed below.

Reach it:

```bash
kubectl --namespace trueforge port-forward svc/trueforge 8790:8790
```

Leave that running, and from a second terminal:

```bash
curl http://localhost:8790/healthz
```

That answers `{"status":"ok","version":"0.2.0-rc.0"}`. Open <http://localhost:8790> for the
UI.

How you tear it down depends on which cluster you used. A throwaway `kind` cluster goes in
one command, and takes the release and its data with it:

```bash
kind delete cluster --name trueforge-fastpath
```

On a cluster you keep, that command does nothing — remove the release itself, and do not
leave it running: anyone who can reach it administers it.

```bash
helm uninstall trueforge --namespace trueforge
kubectl delete pvc --namespace trueforge --selector app.kubernetes.io/instance=trueforge
```

Helm does not remove a subchart's PersistentVolumeClaims, so `helm uninstall` leaves the
bundled Postgres and Redis volumes behind; the second command reclaims that data, matching
only this release's own PVCs. Delete the namespace itself only if you created it for this
install — `--create-namespace` adopts an existing one rather than failing, so it may hold
more than this release.

This sequence was run end to end on 2026-09-04 against `kind` v0.30.0 (Kubernetes v1.34.0)
and `helm` v3.16.3: the release reached `deployed`, all three pods reached `1/1 Running`,
and both `/healthz` and the UI returned HTTP 200.

### What it skips, and what that costs

| Shortcut | What it costs | What the full path does instead |
|---|---|---|
| **No CONNECT, no RULE.** Nothing is declared and nothing is governed. | The deployment exists outside the record. No layer knows about it, no control applies to it, and nothing will notice when it drifts or when you forget it is there. | Steps 1 and 2: a declaration Blueprints reads, and enforcement applied to what was connected. |
| **OIDC is off.** This is the chart's own default, not our choice. | The chart documents this default as a fixed local admin identity: **anyone who can reach the API or the UI is an administrator.** The port-forward is the only thing keeping that to you. Never expose this release. | Blueprints' rules for this chart never generate `enabled: false`; a declaration with no identity provider is refused, `clientSecret` is only ever a secret reference, and `server.publicBaseUrl` is always set. |
| **Port-forward, no ingress, no TLS.** The chart ships no Ingress, Gateway or VirtualService at all. | The URL exists only while `kubectl port-forward` is running, on your machine only, over plain HTTP. Nobody else can reach it. | Blueprints generates an Ingress through the chart's `extraObjects` hook, which still needs an ingress controller running in the cluster. |
| **Bundled Postgres and Redis with the chart's dev defaults.** | Postgres runs on a well-known password published in the chart's own values file, Redis runs with authentication disabled, and both live in the cluster, so the data lasts exactly as long as the cluster does. | An external managed Postgres and Redis over private endpoints, so data outlives the cluster. That path needs modules Blueprints has not built yet, so it is not deliverable today by either route. |
| **The stock image, unbranded.** | TrueForge's own colours. This is not a configuration you forgot to set: branding is applied when the interface is built, and there is no runtime path — no file, no endpoint, no environment variable. | Blueprints generates theme tokens for a branded UI build; someone then runs that build and the release points at the resulting image. |
| **A throwaway local cluster, stood up by hand.** | Nothing here is reproducible from source. It exists on one laptop until `kind delete cluster`. | Step 3: a root module you own, `tofu plan` and `tofu apply` against your subscription, on AKS or Talos. |

The first three rows are the ones that decide when to stop using this path. **The moment
anything reaches this release other than you through your own port-forward, it needs OIDC
on and a real ingress in front of it** — and at that point you are not taking the shortcut
any more, you are on Step 3 and should read
[`docs/TRUEFORGE_DEPLOYMENT.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/docs/TRUEFORGE_DEPLOYMENT.md)
before going further.

---

## Before you start

- `gh`, authenticated as yourself. Bootstrap's CLI reads GitHub through it and never
  asks for a token of its own.
- `node` and `npm` for the Bootstrap and Governance CLIs.
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

**Repository:** `ecosystem-governance`. **Tool:** `bryde-govern`.

Governance comes second because rules are enforced *onto connected things*. There is no
access control to set on a subscription that does not exist yet. Its position in the
sequence does not limit its authority: every other layer, Bootstrap included, operates
within its requirements.

Governance defines **requirements**, not the mechanism that satisfies them. "Database
traffic must be encrypted" is Governance. The input that turns encryption on is
Blueprints.

You define rules by editing YAML, and apply them with the CLI:

```text
enforcement/controls/control-catalog.yaml      the controls themselves
enforcement/controls/repo-classification.yaml  which controls each class carries
enforcement/controls/risk-levels.yaml          how severity is classified
enforcement/controls/exceptions.yaml           what is exempt, and why
enforcement/companies/<organization>.yaml      which controls apply to whom
enforcement/github/                            GitHub organization enforcement
```

The CLI reads the definitions next to itself: `npx` runs **main's** and cannot see your
local edits, while `node bin/bryde-govern.mjs` from a clone runs **that checkout's**.

```bash
npx github:atbrydeud/ecosystem-governance status  # what main declares, without a checkout.
```

Clone `ecosystem-governance` and install its dependencies once:

```bash
gh repo clone atbrydeud/ecosystem-governance
cd ecosystem-governance && npm ci
```

`plan` and `apply` take the organization to act on. Name it every time. Run `status` and
`plan` from that clone to see the declaration you are about to write; run `apply` only
once the loop below has reviewed, approved and merged it, so it acts on what was approved.

```bash
node bin/bryde-govern.mjs status            # what this checkout declares. No network, no credential.
node bin/bryde-govern.mjs plan ubqty-labs   # go and look at ubqty-labs. Read-only.

git checkout main && git pull               # apply reads the checkout, so make it the merged state
node bin/bryde-govern.mjs apply ubqty-labs  # change ubqty-labs. Refuses without a terminal attached.
```

This is the **GitHub organization login**, not the declaration slug. The slug is derived
from the login by lowercasing it, so it matches whenever the login is already lowercase,
and differs when the login has capitals or when `bryde-connect init --org` named a
different one. The login is what belongs here.

`status` takes no organization: it is offline and reports every declared target, which
costs nothing. `plan` and `apply` act on one, and the argument is not optional in any
useful sense — **an omitted organization means every target, not a sensible default.**
Where a bare `apply` is not refused outright, the target filter passes everything and the
run reaches every organization declared under `enforcement/companies/` rather than the one
you meant. Name the organization and the question never arises.

The login has to match the `organization:` field in a company file, which holds the login
and not the slug. Those files ship with `{{ORG_A}}`-style placeholders, so
`plan ubqty-labs` matches nothing and prints `no targets matched` until `ubqty-labs` is
declared — the placeholder replaced, and its repositories classified against a class from
`repo-classification.yaml`. Declaring it is the first edit of this step, not something
someone else did for you.

**`atbrydeud` is deliberately the last organization enforced, not the first.** A user
cleanup comes before rules land on it, so enforcement is proven on the other organizations
first — `ubqty-labs` is the working target. `atbrydeud` comes under the same controls once
that cleanup is done. This is the standing order of work, not a note waiting to be cleared.

Every rule and target gets exactly one outcome per run — `applied`, `already-conforming`,
`not-yet-applicable`, `not-conforming`, `blocked` or `unknown` — and never none. **`unknown`
is a failure, not a shrug**: a run that could not observe a target says so and exits
non-zero rather than inferring a pass.

The workflows cover the review loop:

| Workflow | Fires on | What it does |
|---|---|---|
| `validate` | pull request | Checks the YAML and formatting |
| `plan-governance` | pull request touching `enforcement/**` | Shows what applying would change |
| `apply-governance` | push to the default branch touching `enforcement/**` | Applies it, with no human in the loop |
| `audit-drift` | schedule, or on demand | Reports where reality has diverged from the rules |

`plan-governance` and `apply-governance` drive the Terraform under `enforcement/terraform/`;
`validate` checks it, and `audit-drift` queries GitHub directly. None of the four runs
`bryde-govern`, so merging a rule does not run the CLI on your organization for you.

`apply-governance` applies on merge. Its job names an environment called `production`,
which carries no protection rules today, so nothing holds the run; required reviewers on
that environment would.

So the loop is: edit YAML → open a PR → read the plan → get the human approval Governance
itself requires → merge → `bryde-govern apply ubqty-labs` puts it onto the connected things.

Read [`docs/APPLYING_RULES.md`](https://github.com/atbrydeud/ecosystem-governance/blob/main/docs/APPLYING_RULES.md)
before your first change. It is normative, and it defines the split between *defining* a
rule and *applying* one — including which layer applies which class of rule, and how
Governance uses a credential without ever holding one.

**A rule with no target is not a failure.** If a control names something Bootstrap has not
connected yet, it has nothing to apply to. The CLI reports that as `not-yet-applicable`,
naming the predicate and the layer that supplies it — expected and recorded, not an error.
Run `status` on a fresh checkout and that is exactly what you will see.

**Output of this step:** requirements that are enforceable, and enforcement applied to
what Bootstrap connected.

---

## Step 3 — DEPLOY: build the systems

**Repository:** `ecosystem-blueprints`. **Tool:** `tofu`.

Blueprints is a library of reusable modules and patterns. **Nothing in it applies
anything.** There is no `apply` in any of its workflows and no environment it could target.
You consume it from your own root module — the one that holds your organization's values
and your state backend — and you run `tofu` there.

> **OpenTofu, not Terraform.** Blueprints requires the `tofu` CLI. Note that
> Governance's own workflows currently invoke `terraform`; the two repositories differ
> here, and if you are moving between them, use each repository's own documented command
> rather than assuming they match.

### Modules and patterns are not the same thing

Blueprints ships both, and the boundary between them is a **layer**, not a size. A module
builds the substrate — resource groups, the network, managed identities, the cluster
itself — everything *below* the Kubernetes API. A pattern is what runs on the cluster, *at
or above* that API. That is the repository's own definition, in
[`patterns/README.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/patterns/README.md) and in the `kind` enum of
[`schemas/catalogue.schema.json`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/schemas/catalogue.schema.json); it is not a
statement about size, and a pattern is held to the same five-file contract a module is.

It is why the foundation modules declare the `azurerm` provider while the patterns declare
only `kubernetes`, and `helm` where a chart is involved. Neither pattern on `main` declares
a cloud provider at all: a baseline only a cloud can satisfy is a baseline the next
substrate cannot have.

It shows up in your own configuration as **two root modules and two states**. The
infrastructure layer takes `subscription_id` and no cluster connection; the cluster layer
takes the cluster connection and configures no Azure provider at all — so a workload
rollback cannot plan a change to a virtual machine. Blueprints'
[`examples/README.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/examples/README.md)
documents that split.

One thing on `main` reads against the rule: `modules/agntcy/*` install Helm charts and
declare `kubernetes` and `helm` rather than a cloud provider. By the definition above they
are cluster-layer work sitting under `modules/`, and
[`examples/README.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/examples/README.md)
places their examples in the cluster layer accordingly. The catalogue's composed
`agntcy-runtime` is a pattern, and it is not built.

### Every runtime starts from the workload baseline

[`patterns/workload-baseline`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/patterns/workload-baseline)
establishes the namespace and its Pod Security Admission level, a service account bound to a
workload identity by annotation, NetworkPolicy that isolates the namespace and names what it
may reach, and container defaults with an optional ceiling. Every runtime in
[`catalogue/systems.yaml`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/catalogue/systems.yaml)
names it in `requires`, which is why it landed first. **One instance is one namespace**, so a
change to one workload's baseline cannot plan a change to another's.

It installs nothing. It is what a runtime lands on.

```hcl
module "baseline" {
  source = "github.com/atbrydeud/ecosystem-blueprints//patterns/workload-baseline?ref=<version>"

  namespace = { name = "agent-runtime" }

  service_accounts = {
    runtime = {
      labels      = { "azure.workload.identity/use" = "true" }
      annotations = { "azure.workload.identity/client-id" = module.identity_workload.client_ids["runtime"] }
    }
  }

  network = { default_deny = ["Ingress", "Egress"] }
}
```

You call it exactly the way you call a module — the same five files, the same contract, the
same CI checks — and you run the same `tofu` commands below, in your cluster root module.

**The NetworkPolicy objects it creates are only a boundary where the CNI enforces them.**
`modules/aks` defaults `network_policy` to Calico, so a cluster built from it enforces;
on Talos the CNI is a cluster decision. The pattern reports what it created and makes no
claim about what is enforced — its README has the probe that answers it.

The second pattern on `main` is
[`patterns/plane-runtime`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/patterns/plane-runtime),
which lands the Plane work-management runtime on top of that baseline. It is covered
[below](#plane-is-the-one-thing-with-two-valid-sources), because Plane is the one system in
this stack that two different layers can legitimately supply.

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
apply to changing the modules and patterns rather than using them:

```bash
tofu fmt -check -recursive .
tofu init -backend=false && tofu validate     # per module, pattern and example directory
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
| 7 | See what would change | RULE | Human or agent | `bryde-govern plan <org>` |
| 8 | Read the plan, get approval | RULE | **Human** | Review the PR |
| 9 | Enforce | RULE | **Human** | `bryde-govern apply <org>` |
| 10 | Compose modules and patterns in your root modules | DEPLOY | Human or agent | Edit HCL |
| 11 | Plan and apply the foundation | DEPLOY | **Human authorization** | `tofu plan` / `tofu apply` |
| 12 | Bootstrap the cluster, if self-managed | DEPLOY | **Human** | `talosctl bootstrap` |
| 13 | Establish the workload baseline | DEPLOY | Human or agent | `tofu apply` in the cluster root module |
| 14 | Deploy the runtimes | DEPLOY | Human or agent | `tofu apply` in the cluster root module |

Steps 4, 5, 8, 9, 11 and 12 need a person. That is deliberate in each case, and each layer
documents why rather than leaving it implicit.

**Steps 6 to 9 are the exception to the numbering.** They are the *first* pass through
governance, not the only one. Every later change — a new control, a new organization
brought under an existing control, a deployment that introduces something to govern —
re-enters at step 6, while steps 10 to 14 continue independently. The drift audit runs on a
schedule and can send you back to step 6 without anything having changed on your side.

The one hard ordering constraint is that **step 6 cannot usefully precede step 1**: a
control naming a provider nobody has connected has nothing to apply to. `bryde-govern
status` reports that as `not-yet-applicable` and names the layer that supplies the missing
precondition — recorded rather than treated as a failure, but not progress either.

---

## Where this stops

DEPLOY leaves you with running systems and nothing running *on* them.

- **EQUIP** — `ecosystem-library` supplies the agents, skills, tools and MCP servers.
- **OPERATE** — `ecosystem-operations` composes those into actual work: Plane
  configuration, SOPs, approval flows, n8n workflows and the human/agent handoffs.

The recurring distinction, in one line: **the n8n runtime is Blueprints; the n8n flow is
Operations.** A generic agent is Library; the process deciding when it runs is Operations.

### It also stops short of things the deploy graph names

[`catalogue/systems.yaml`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/catalogue/systems.yaml)
is a dependency graph, not an inventory — its own header says availability is read from the
repository at run time and never recorded there. So plan against the repository, not the
graph. What is in the repository today is the Azure foundation (`landing-zone`,
`networking`, `identity`, `aks`, `talos`), the AGNTCY component modules, and two
patterns — `workload-baseline` and `plane-runtime`.

Three of the patterns it names are worth calling out, because each is a thing a reader
planning real work plausibly assumes is already there. None has a directory behind it:

| Named in the graph | Would provide | Who asks for it |
|---|---|---|
| `patterns/ingress-controller` | The controller that makes an Ingress object mean something | Every runtime pattern in the graph, `plane-runtime` included |
| `patterns/secrets-from-vault` | The workload-identity path from a pod to an approved secret store | `trueforge-runtime`, `eve-runtime`, `n8n-runtime`, `plane-runtime` |
| `patterns/gitops-argocd` | The delivery substrate that reconciles workload manifests onto the cluster | Nothing yet — it is named, not depended on |

Those two absences reach even the patterns that *have* landed. `plane-runtime` renders an
Ingress and names the Secrets its pods read; it creates neither. An Ingress with no
controller behind it routes nothing, and a pod whose named Secret does not exist sits in
`CreateContainerConfigError` — the pattern's own README says so. Supplying them by hand is
a legitimate first deployment; assuming Blueprints supplies them is not.

`trueforge-runtime` requires the first two, so the governed TrueForge path is not
deliverable today by either route — which is exactly what the
[shortcut](#shortcut--a-running-trueforge-today) is honest about. The same is true of the
`key-vault`, `postgres`, `redis`, `container-registry` and `observability` modules and of
every runtime pattern in the graph except `plane-runtime`.

### Plane is the one thing with two valid sources

Every other system in this stack is supplied by exactly one layer. The work-management
workspace has two sources, and both are legitimate:

- **Registered at CONNECT — the hosted workspace.** Bootstrap's provider catalogue carries
  a `plane` provider under `work-management`, `assisted`, whose human steps are creating the
  workspace, accepting its terms and issuing an administrative API key recorded as a
  reference. Terms acceptance is a legal act by a named person, which is why it is
  `assisted` and why its automation status is `none`.
- **Stood up by DEPLOY — an instance you run.**
  [`patterns/plane-runtime`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/patterns/plane-runtime)
  deploys Plane as a managed Helm release at a pinned chart and application version: seven
  workloads from five images, five HTTP surfaces served on one hostname by path, four data
  services either bundled in the cluster or external, an Ingress, and a documented upgrade
  and rollback path. Every credential it needs arrives as the **name** of a Kubernetes
  Secret; no input takes a value. What was read from Plane's published chart, and when, is
  recorded in
  [`docs/PLANE_DEPLOYMENT.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/docs/PLANE_DEPLOYMENT.md).

**Operations does not care which.** Its business is the configuration *inside* the
workspace — projects, work-item types, states, labels, properties and the workflows that
move a work item between them — applied through Plane Compose against whichever instance the
organization has, and `ecosystem-operations` already holds that structure under `plane/`.
Both sides state the same boundary: Bootstrap's catalogue says configuration-as-code belongs
to Operations, and `plane-runtime`'s README says it deploys the runtime and configures
nothing in it.

Two things are worth knowing before choosing:

- **Plane CE has no chart value for authentication.** Sign-in providers are configured after
  install, through the instance-admin surface at `/god-mode`, and stored in the database.
  That is why `plane-runtime` requires no identity provider and configures none — a
  difference from `trueforge-runtime`, and a finding rather than an oversight.
- **No declaration field distinguishes the two sources yet.** Today the distinction is
  which of the two an organization actually set up. Recording it in the declaration is the
  seam where it belongs, and it is not there.

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
| [Blueprints `docs/MODULE_CONTRACT.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/docs/MODULE_CONTRACT.md) | What every module — and every pattern — guarantees a caller |
| [Blueprints `patterns/README.md`](https://github.com/atbrydeud/ecosystem-blueprints/blob/main/patterns/README.md) | What a pattern is, and which ones are deferred rather than forgotten |

Nothing in this document grants authority. Where it and a policy differ, the policy wins
and this document is the defect.
