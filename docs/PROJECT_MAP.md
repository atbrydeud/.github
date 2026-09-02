# Project Map

**“I need to change X — where does it belong?”**

Start with the organizational stack:

```text
CONNECT → RULE → DEPLOY → EQUIP → OPERATE
```

| Ask | Repository |
|---|---|
| Am I establishing an **account, identity, credential reference or provider connection**? | Bootstrap |
| Am I changing what **must be true**? | Governance |
| Am I defining a **deployable system, infrastructure module or runtime**? | Blueprints |
| Am I creating a **reusable agent, skill, MCP or tool**? | Library |
| Am I changing **how people and agents work**? | Operations |
| Am I changing the **At Bryde Ud ecosystem/Web3 product**? | Platform |
| Am I changing a **protocol definition or contract**? | Protocols |

## Lookup table

| I need to change… | Repository |
|---|---|
| Create/configure a GitHub organization, repositories, teams or GitHub App | `ecosystem-bootstrap` |
| Slack account/workspace connection | `ecosystem-bootstrap` |
| Plane account/workspace connection | `ecosystem-bootstrap` |
| Attio or Xero organization/account connection | `ecosystem-bootstrap` |
| Azure/cloud/hosting provider account foundation | `ecosystem-bootstrap` |
| Supabase account/project foundation | `ecosystem-bootstrap` |
| Daytona account/project foundation | `ecosystem-bootstrap` |
| OpenRouter account/API credential registration | `ecosystem-bootstrap` |
| 1Password vault structure or secret-reference format | `ecosystem-bootstrap` |
| OAuth application, OIDC federation or service-identity registration | `ecosystem-bootstrap` |
| AI authority or autonomy rule | `ecosystem-governance` |
| Human approval or break-glass requirement | `ecosystem-governance` |
| GitHub ruleset requirement | `ecosystem-governance` |
| Security, secrets, audit, data-boundary, RPO/RTO or retention requirement | `ecosystem-governance` |
| Exception and change-control policy | `ecosystem-governance` |
| Azure/AKS, networking, OpenZiti or storage deployment | `ecosystem-blueprints` |
| Postgres, Redis, observability or backup/DR infrastructure | `ecosystem-blueprints` |
| Argo CD/GitOps or CI runner deployment | `ecosystem-blueprints` |
| Eve deployment | `ecosystem-blueprints` |
| TrueForge deployment | `ecosystem-blueprints` |
| AGNTCY deployment | `ecosystem-blueprints` |
| n8n runtime deployment | `ecosystem-blueprints` |
| Reusable agent definition | `ecosystem-library` |
| Reusable skill or prompt/instruction bundle | `ecosystem-library` |
| MCP server or connector, including GitHub/Plane MCP | `ecosystem-library` |
| Reusable tool, wrapper, schema, adapter or eval | `ecosystem-library` |
| Plane project/work-item/state/label configuration | `ecosystem-operations` |
| Engineering, release, incident or infrastructure-request SOP | `ecosystem-operations` |
| Approval flow or Definition of Done | `ecosystem-operations` |
| n8n workflow | `ecosystem-operations` |
| Process deciding when/why an agent runs and who reviews it | `ecosystem-operations` |
| At Bryde Ud dashboard, organization registry or lifecycle API | `ecosystem-platform` |
| Treasury/Web3 product feature or protocol integration | `ecosystem-platform` |
| Protocol specification, smart contract, SDK or shared primitive | `ecosystem-protocols` |
| Intrinsic token/governance mechanics or compatibility rule | `ecosystem-protocols` |
| Organization profile, shared issue/PR template or this architecture map | `.github` |

## Frequently confused boundaries

### Bootstrap vs Blueprints

- Bootstrap establishes an Azure, Supabase or OpenRouter account/project,
  identity, trust and credential reference.
- Blueprints deploys infrastructure or a runtime using that established connection.

### Blueprints vs Library

- Blueprints deploys Eve, TrueForge or AGNTCY.
- Library defines the Engineering Agent, its skills and its MCP tools.

### Blueprints vs Operations

- Blueprints deploys n8n.
- Operations owns the n8n flow and the business process it automates.

### Governance vs every mechanism

- Governance says two human reviewers are required.
- Bootstrap configures the GitHub organization/ruleset mechanism.
- Operations defines the review workflow.
- Neither mechanism may silently redefine the requirement.

### Platform vs Protocols

- Protocols defines canonical protocol behaviour and compatibility.
- Platform integrates and exposes that protocol in the product.

## Cross-layer work

One outcome may require multiple repositories. Split it into linked pull requests
in ownership order, usually Bootstrap → Governance → Blueprints → Library →
Operations, with Platform/Protocols changes linked where relevant. Do not solve
cross-layer work by putting every artifact in one repository.

When an existing artifact is in the wrong repository, preserve it, record the
migration and move it in a separate change unless the move is demonstrably safe
and trivial. See [MIGRATION_FOLLOW_UPS.md](MIGRATION_FOLLOW_UPS.md).

