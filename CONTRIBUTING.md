# Contributing to At Bryde Ud

The organization-wide contribution baseline. It applies to every At Bryde Ud
repository unless that repository is stricter — **repo-local rules always win
where they are stricter**, never where they are looser.

New here? Start with [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md).

---

## Baseline

1. **Change the right repository.** Use [docs/PROJECT_MAP.md](docs/PROJECT_MAP.md)
   before you start. Cross-cutting work is several changes in a stated order,
   not one change in the wrong place.
2. **Meaningful changes go through a pull request.** No direct pushes to
   protected branches, by humans or agents.
3. **Reference the issue or work item** the change belongs to, where one applies.
   Non-trivial work should have one before the pull request.
4. **Follow repo-local instructions.** `AGENTS.md`, `CONTRIBUTING.md` and
   `docs/` in the target repository are more specific than this file and take
   precedence.
5. **Test what you change**, and show it. The pull request template asks for
   evidence — plans, test output, screenshots, dry runs.
6. **Get review.** Every change needs review by someone accountable for the
   affected area. Review is a check on intent and risk, not just syntax.
7. **AI-assisted contributions are welcome and expected.** Disclose the
   assistance in the pull request.
8. **You remain accountable for what you submit** — including anything an agent
   produced on your behalf. Read the diff before you request review.
9. **Never expose secrets.** Not in code, issues, pull requests, logs, test
   fixtures or screenshots. See [SECURITY.md](SECURITY.md).
10. **Do not bypass controls.** If a rule blocks legitimate work, request an
    exception through [GOVERNANCE.md](GOVERNANCE.md) instead of working around it.

---

## Pull requests

- Keep them scoped to one concern; a reviewable diff beats a complete one.
- Write the description for a reader who was not in the conversation.
- Fill in the template honestly — risk, security impact, operational impact and
  rollback matter more than a tidy summary.
- Link the governance change if the pull request implements one.
- Green CI is the floor, not the goal.

## Commits

- Present tense, imperative, explaining *why* where it is not obvious.
- Reference the issue or work item where one exists.

## AI-assisted contributions

Allowed by default, subject to the AI policy in
[`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance) and
the tool permission profiles in
[`ecosystem-library`](https://github.com/atbrydeud/ecosystem-library).

- Disclose AI assistance in the pull request.
- Agents do not get authority a human does not have; review requirements are
  identical.
- Never give an agent long-lived credentials or a secret it does not need.
- Prefer shared, versioned capabilities from `ecosystem-library` over bespoke
  automation.

## Security-sensitive changes

Do not open a public issue or pull request describing a vulnerability. Follow
[SECURITY.md](SECURITY.md).

## Questions

[SUPPORT.md](SUPPORT.md) — ask in the repository that owns the concern.
