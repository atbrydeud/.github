# Architecture Principles

The principles behind the ecosystem model. They are deliberately short. Where a
principle needs to become binding, it is expressed as policy in
[`ecosystem-governance`](https://github.com/atbrydeud/ecosystem-governance) —
this document explains the reasoning, it does not enforce it.

---

### 1. AI-first by default
Agents are first-class participants. Repositories, docs and interfaces are
written so that both humans and agents can orient themselves and act. Reusable
intelligence is a shared, versioned capability, not a private script.

### 2. Git is the source of truth
Ecosystem and organization configuration is expressed as code, reviewed as code,
and reconstructible from a repository. A console or SaaS UI may be a convenient
surface; it is never the record.

### 3. Policy as code where practical
Requirements that can be machine-checked should be. Policy that can only exist
as prose is still policy — but prefer the form that can be evaluated in CI.

### 4. Infrastructure as Code
No hand-built environments. Infrastructure is declared, versioned, planned and
applied. Anything created by hand is treated as drift.

### 5. Identity over long-lived credentials
Prefer identity-based access to stored secrets. Long-lived credentials are an
exception that must be justified, scoped and rotated.

### 6. OIDC and workload identity where supported
CI/CD to cloud, and workload to service, should federate rather than carry keys.
Where a provider does not support federation, isolate and rotate.

### 7. Shared standards, isolated organizations
Standards, modules, workflows and protocols are shared upstream. Identity,
state, secrets, data and production authority are isolated per organization.
*Shared code and standards; separate state and authority.*

### 8. Control-plane failure must not cause downstream failure
The At Bryde Ud platform observes, coordinates and provisions. If it is
unavailable, downstream organizations keep running, deploying and recovering.
No downstream production path may depend on upstream availability.

### 9. SaaS is acceptable when it provides leverage
Buy the thing that is not your differentiator. The condition is that its
configuration is reproducible from Git and its loss has a known answer.

### 10. Open source and self-hosting when ownership justifies it
Where portability, sovereignty or ownership of data and capability matter more
than operational convenience, self-host — deliberately, not reflexively.

### 11. Minimal sufficient infrastructure
Build the smallest thing that meets the requirement. Complexity is a permanent
operational cost paid by whoever is on call, including agents.

### 12. Explicit authority boundaries
Every repository, environment and agent has a stated scope of authority. If it
is unclear who may approve or apply a change, that is a defect to fix in
governance, not a judgement call to make at merge time.

### 13. Versioned baselines
Downstream organizations consume named baseline versions, not a moving branch.
Upgrades are explicit, reviewable and reversible, with a stated migration path.

### 14. Drift detection
Declared state is continuously compared against actual state. Drift is surfaced,
attributed and either corrected or converted into an approved exception.

### 15. Downstream environments are independently operable
A downstream organization can be operated, debugged and recovered by its own
team with its own credentials, using documentation it holds. Dependence on
upstream humans is a design failure.

---

## Applying these

| Situation | Expected behaviour |
|---|---|
| A principle conflicts with a deadline | Raise an exception through `ecosystem-governance`, do not silently deviate. |
| A principle conflicts with policy | Policy wins. Fix the principle or the policy — do not leave them in conflict. |
| A principle is unclear in practice | Propose a clarification via PR to this file with a concrete example. |
| A change violates a principle for good reason | Say so explicitly in the pull request, under risk. |
