# Contributing to At Bryde Ud

This is the organization-wide contribution baseline. A repository may add
stricter local requirements, but it cannot relax authoritative Governance policy.

Start with [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md) and remember:

```text
CONNECT → RULE → DEPLOY → EQUIP → OPERATE
```

## Contribution baseline

1. **Change the owning repository.** Use
   [docs/PROJECT_MAP.md](docs/PROJECT_MAP.md) before implementation.
2. **Split cross-layer work.** Use linked pull requests and explicit contracts;
   do not place policy, connections, systems, intelligence and workflows together.
3. **Use a branch and pull request** for meaningful changes. Do not push
   conceptual or consequential work directly to the default branch.
4. **Link the issue or work item** for non-trivial work where one exists.
5. **Read repo-local instructions:** `README.md`, root `AGENTS.md`, contribution
   guide, roadmap and relevant architecture documents.
6. **Test what changed** and include evidence matching the repository's CI.
7. **Review the complete diff** for secrets, sensitive identifiers, unexpected
   permissions, generated output and ownership violations.
8. **Describe risk, migration, operational impact and rollback** honestly.
9. **Obtain human review and authorization** required by Governance. Agents do
   not approve or merge their own consequential work.
10. **Do not bypass controls.** Request a time-boxed exception through
    [GOVERNANCE.md](GOVERNANCE.md) when a legitimate need conflicts with policy.

## AI-assisted contributions

AI assistance is welcome within the authority model in
[`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance).
The accountable contributor must review the diff, tests and tool actions.

- Disclose material AI assistance in the pull request.
- Never grant an agent authority the operator does not hold.
- Use reusable, versioned capabilities from `ecosystem-library` where practical.
- Do not put secrets or credentials into prompts, logs, fixtures or PR text.
- Distinguish a draft or plan from an applied, deployed or verified result.

## Security-sensitive work

Do not disclose vulnerabilities in a public issue or pull request. Follow
[SECURITY.md](SECURITY.md).

Questions: [SUPPORT.md](SUPPORT.md).

