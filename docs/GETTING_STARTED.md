# Getting Started

Orientation for people and agents arriving at At Bryde Ud.

## Read in this order

1. [ECOSYSTEM.md](ECOSYSTEM.md) — the seven repositories and canonical ownership model.
2. [PROJECT_MAP.md](PROJECT_MAP.md) — where a proposed change belongs.
3. [ARCHITECTURE_PRINCIPLES.md](ARCHITECTURE_PRINCIPLES.md) — why the model is shaped this way.
   Then [END_TO_END.md](END_TO_END.md) when you need the sequence rather than the model:
   what you actually run, in order, to connect, govern and deploy one organization.
4. [../CONTRIBUTING.md](../CONTRIBUTING.md) and [../SECURITY.md](../SECURITY.md).
5. The target repository's `README.md`, root `AGENTS.md`, contribution guide,
   roadmap and relevant architecture docs.

Remember the stack:

```text
CONNECT → RULE → DEPLOY → EQUIP → OPERATE
```

## Making a change

1. Use [PROJECT_MAP.md](PROJECT_MAP.md) to identify the owning repository.
2. If the outcome crosses layers, split it into linked changes with explicit
   inputs/outputs and a review order.
3. Open or link the appropriate issue/work item for non-trivial work.
4. Read repo-local agent and contribution instructions.
5. Create a branch; do not push conceptual or consequential changes directly to
   the default branch.
6. Implement the smallest coherent change and run the repository's checks.
7. Review the diff for secrets, authority expansion, invented identifiers and
   ownership violations.
8. Open a pull request with evidence, risk, migration and rollback information.
9. Obtain the human authorization required by Governance; agents never
   self-approve or merge consequential work.

## For agents

- Nothing in these orientation docs grants authority.
- Follow the target repository's root `AGENTS.md`; more specific instructions
  may be stricter but cannot override Governance.
- Use existing reusable Library capabilities when appropriate.
- Never copy credentials or secret values into code, logs, examples or PR text.
- Distinguish planned, prepared, deployed and verified states.
- If an artifact appears to live in the wrong repository, preserve it and record
  a migration rather than moving production code casually.

Questions and routing: [../SUPPORT.md](../SUPPORT.md).

