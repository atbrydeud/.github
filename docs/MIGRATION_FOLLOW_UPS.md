# Ecosystem Migration Follow-ups

The ownership model is canonical now; existing implementation is not assumed to
already match it. This register records known discrepancies for separate,
reviewed migration. It does not authorize moving or deleting production code.

## Known discrepancies

| Existing content | Current location | Target decision |
|---|---|---|
| GitHub organization/repository/team/ruleset Terraform and apply scripts | `ecosystem-governance/enforcement/` | Classify as Bootstrap provider/account application; retain Governance as the source of requirements |
| Organization drift, backup and template-sync scripts/workflows | `ecosystem-governance/enforcement/` | Split mechanism between Bootstrap/Blueprints and response process into Operations |
| Restore and other operational runbooks | `ecosystem-governance/policies/operations/` | Move procedures to Operations; keep RPO/RTO/control requirements in Governance |
| TrueForge ownership statement | `ecosystem-library/ROADMAP.md` | Runtime deployment belongs to Blueprints; reusable agents/skills/MCPs belong to Library |
| Existing Web3 product/protocol repositories | multiple repositories named in Platform/Protocols roadmaps | Inventory contents and decide migrate, integrate, archive or retain before moving anything |

## Migration rules

1. Inventory behaviour, consumers, secrets, state and release history first.
2. Identify the new owner and explicit interface to adjacent layers.
3. Preserve history or provide a traceable replacement.
4. Migrate in small pull requests with compatibility and rollback.
5. Deprecate old entry points before deletion.
6. Confirm downstream organizations remain independently operable.

## Recommended order

This order follows migration logic — inventory before you move, the destination
contract before the move lands — and is deliberately independent of the layer
order.

1. Governance enforcement inventory and classification
2. Bootstrap provider catalogue and secret-reference contract
3. Blueprints runtime deployment contracts
4. Library artifact/versioning contract
5. Operations workflow and runbook migration
6. Platform/Protocols Web3 repository consolidation decision

