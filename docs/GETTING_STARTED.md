# Getting Started

Orientation for people and agents arriving at At Bryde Ud.

---

## Read in this order

1. [ECOSYSTEM.md](ECOSYSTEM.md) — what the ecosystem is and how it is divided. *(~10 min)*
2. [PROJECT_MAP.md](PROJECT_MAP.md) — which repository owns which kind of change.
3. [ARCHITECTURE_PRINCIPLES.md](ARCHITECTURE_PRINCIPLES.md) — the reasoning behind the model.
4. [../CONTRIBUTING.md](../CONTRIBUTING.md) — the org-wide contribution baseline.
5. The `README.md` and `AGENTS.md` of the repository you are actually working in — repo-local instructions always win.

---

## Making your first change

1. **Find the owner.** Use [PROJECT_MAP.md](PROJECT_MAP.md). Changing the wrong
   repo is the most common and most expensive mistake here.
2. **Open a work item or issue** in that repository before non-trivial work.
   Use the governance-change template if you are changing what *must be true*.
3. **Read the repo-local instructions.** `AGENTS.md`, `CONTRIBUTING.md` and any
   `docs/` in that repository are more specific than anything here and take
   precedence.
4. **Branch and open a pull request.** No direct pushes to protected branches.
5. **Fill in the pull request template honestly** — especially risk, security
   impact and AI assistance.
6. **Get review from the owning project.** Cross-project changes need agreement
   from every project they touch, in the order given by *policy before mechanism*.

---

## For agents

This repository is written to be usable as orientation context.

- **Authority:** nothing in this repository grants authority. What an agent may
  do, and under what review, is defined in
  [`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance).
- **Capabilities:** shared skills, tools, MCP servers and agent templates live in
  [`ecosystem-library`](https://github.com/atbrydeud/ecosystem-library). Prefer a
  shared capability over a bespoke one.
- **Precedence:** repo-local `AGENTS.md` > this repository's documents > general
  convention. Where they conflict, follow the more specific and say so.
- **Accountability:** an AI-assisted change is still owned by the human or team
  that submits it. Disclose AI assistance in the pull request.
- **Boundaries:** do not implement policy, infrastructure, protocol or platform
  code in this repository. See [PROJECT_MAP.md](PROJECT_MAP.md).

---

## Where to ask

See [SUPPORT.md](../SUPPORT.md). Short version: ask in the repository that owns
the concern, not here.
