# AGENTS.md — atbrydeud/.github

This repository is the organization front door and canonical architecture map,
not an implementation layer.

## Agents may

- Improve organization orientation, navigation and shared GitHub templates.
- Keep the profile, ecosystem map, project map and community-health files
  consistent.
- Record known ownership discrepancies and migration follow-ups.

## Agents must not

- Put authoritative policy, infrastructure, provider provisioning, agents,
  workflows, product code or protocols here.
- Duplicate Governance policy or relax it through a shared template.
- Move or delete implementation in another repository as part of a documentation
  cleanup.
- Commit secrets, credentials, private identifiers or confidential data.
- Push conceptual changes directly to `main` or merge without required review.

## Source-of-truth order

- `docs/ECOSYSTEM.md` is canonical for high-level repository ownership.
- `ecosystem-governance` is authoritative for binding policy.
- Repo-local `AGENTS.md` files add implementation-specific instructions.
- If documents conflict, flag the conflict in the pull request and repair the
  lower-authority document; do not silently choose a convenient interpretation.

## Validation

Check internal and absolute links, Markdown/YAML syntax, repository names,
consistent use of **CONNECT → RULE → DEPLOY → EQUIP → OPERATE**, and alignment
between the profile, ecosystem map, project map, support routing and templates.
Review the full diff and open a scoped pull request with no runtime impact.


## Maintaining this file

Keep this file for knowledge useful to almost every future agent session in this project.
Do not repeat what the codebase already shows; point to the authoritative file or command instead.
Prefer rewriting or pruning existing entries over appending new ones.
When updating this file, preserve this bar for all agents and keep entries concise.
