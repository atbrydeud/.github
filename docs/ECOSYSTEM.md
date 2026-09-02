# The At Bryde Ud Ecosystem

This is the canonical high-level ownership model for the seven ecosystem
repositories. Repository-local documents may add detail, but they must not
contradict this document.

## The model

The organizational stack is:

```text
GOVERNANCE      BOOTSTRAP       BLUEPRINTS      LIBRARY         OPERATIONS
   RULE     →     CONNECT    →     DEPLOY    →    EQUIP     →     OPERATE
```

The most important boundary statement is:

> **Governance sets the rules. Bootstrap connects the organization. Blueprints
> deploys its systems. Library equips those systems with reusable intelligence.
> Operations composes those capabilities into actual organizational work.**

Platform and Protocols form the At Bryde Ud Web3 product/protocol domain. They
are not extra steps in the organizational stack:

- **Platform** is the ecosystem/Web3 product and control surface.
- **Protocols** defines the three At Bryde Ud Web3 protocols.

## Ownership model

### 1. ecosystem-governance — RULE

**Question: What must be true?**

The highest policy authority. It owns organizational policy, AI authority and
autonomy rules, security and secrets requirements, GitHub governance intent,
audit and data-boundary requirements, change control, approval requirements,
backup/recovery requirements, exception policy and break-glass policy.

It defines requirements, not the technical or procedural mechanism that
satisfies them.

### 2. ecosystem-bootstrap — CONNECT

**Question: What does an organization need to digitally exist and connect to
the ecosystem?**

It establishes digital foundations: GitHub/cloud/hosting accounts, SaaS tenants
and workspaces, identities, service principals, OAuth/OIDC relationships,
secret-store structures, credential references, provider projects, domains/DNS
relationships and ecosystem registration.

Examples include GitHub and GitHub Apps, Azure/Entra, Slack, Plane, Attio, Xero,
Supabase, Daytona, OpenRouter, 1Password structures and similar organizational
providers. Provisioning may be `automated`, `assisted` or `manual`; legal,
billing, MFA and consent steps must not be bypassed through brittle automation.

Bootstrap owns how credentials are referenced and how services are connected.
Secret values never belong in Git.

### 3. ecosystem-blueprints — DEPLOY

**Question: What systems and runtime capabilities can be deployed into an
organization?**

It owns reusable infrastructure modules and deployment patterns: Azure/AKS,
networking/OpenZiti, Postgres, Redis, storage, observability, backup/DR
infrastructure, Argo CD/GitOps, CI runners, gateways and workload identity.

It also owns deployment of shared runtimes such as n8n, Eve, TrueForge and
AGNTCY. It consumes the accounts, identities and credential references
established by Bootstrap.

### 4. ecosystem-library — EQUIP

**Question: What reusable intelligence and components are available?**

It owns portable agents, skills, MCP servers/connectors, tools, shared
instructions, schemas, evals, adapters, agent templates and reusable automation
components.

Reusability test: if multiple agents, workflows or organizations can reuse the
component without the component itself being an environment, it probably
belongs in Library.

### 5. ecosystem-operations — OPERATE

**Question: How does the organization actually work?**

It owns Plane configuration-as-code, project/work-item structures, states,
templates, Definitions of Done, approval flows, SOPs, n8n workflows,
engineering/release/incident processes, infrastructure-request procedures,
business workflows and human/agent handoffs.

Operations composes Library capabilities running on Blueprint systems, using
Bootstrap connections, within Governance rules.

### 6. ecosystem-platform — WEB3 PRODUCT

This is the actual At Bryde Ud ecosystem/Web3 product, not an infrastructure or
platform-engineering repository. It may contain the dashboard, organization
registry, lifecycle/control capabilities, treasury and Web3 functionality,
approval/governance interfaces, cross-organization visibility, APIs, shared
ecosystem services and protocol integrations.

Downstream organizations must not require the central platform to remain
available for routine operation, deployment or recovery.

### 7. ecosystem-protocols — WEB3 PROTOCOLS

This is the canonical home for the three At Bryde Ud protocol definitions:
specifications, smart contracts, governance/token mechanics, SDKs, shared
primitives, interoperability requirements, reference implementations,
compatibility and versioning.

Platform consumes Protocols; Platform must not become the canonical definition
of them.

## Boundary matrix

| Repository | Owns | Does not own |
|---|---|---|
| Governance | Policy, requirements, authority, controls, exceptions | Accounts, implementations, workflows or product code |
| Bootstrap | Accounts, identities, provider foundations, secret references and connections | General infrastructure/runtime deployments or reusable intelligence |
| Blueprints | Deployable systems, infrastructure, runtimes and technical patterns | Accounts, agent/skill content, business workflows or mandates |
| Library | Reusable agents, skills, MCPs, tools, adapters and evals | Runtime deployment, org-specific process or policy |
| Operations | SOPs, Plane config, n8n flows, approvals and human/agent process | Provider setup, runtime deployment or reusable component implementation |
| Platform | At Bryde Ud ecosystem/Web3 product and protocol integrations | Infrastructure platform or canonical protocol definitions |
| Protocols | Protocol specs, contracts, SDKs and compatibility | Product integration and organizational policy |

## Concrete splits

| Concern | Owner |
|---|---|
| “Production changes require human approval” | Governance |
| Create/configure the GitHub organization | Bootstrap |
| Register OpenRouter and its credential reference | Bootstrap |
| Deploy TrueForge or Eve using that connection | Blueprints |
| Define the Engineering Agent | Library |
| Define the GitHub MCP | Library |
| Define the Plane ticket → agent → PR workflow | Operations |
| Show the result in the ecosystem dashboard | Platform |
| Define protocol semantics used by the product | Protocols |

## Source-of-truth and dependency principles

- Git is canonical for definitions; consoles and SaaS UIs are operational surfaces.
- Secret values stay in approved stores; Git contains references and metadata only.
- Prefer identity/OIDC over long-lived credentials.
- Version reusable baselines and detect drift.
- Keep direct repository dependencies minimal and use explicit contracts.
- Bootstrap is the natural connector/assembler, not a monolithic runtime owner.
- Shared standards do not imply shared access: each downstream organization owns
  its identity, state, secrets, data, infrastructure and production authority.
- Pragmatic SaaS is acceptable; open source/self-hosting is preferred where
  sovereignty, ownership or economics justify its operational cost.

## Composition flow

```text
Plane work item
  → Operations process
  → reusable agent and MCP from Library
  → executes on runtime from Blueprints
  → uses connection from Bootstrap
  → obeys authority from Governance
  → may surface status in Platform
```

For a practical routing table, see [PROJECT_MAP.md](PROJECT_MAP.md). For the
reasoning behind the model, see
[ARCHITECTURE_PRINCIPLES.md](ARCHITECTURE_PRINCIPLES.md).

