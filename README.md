# Delivery case studies

Four delivery problems, and what was actually done about them.

Each describes the **approach**, not the internals — the systems are private, the mechanics
transfer. Every claim traces to a real artifact: a spec, a CI job, a committed document.
Anything that could not be evidenced was cut rather than softened.

---

| # | Case study | The problem | The mechanism |
|---|---|---|---|
| 1 | [The binding spec hierarchy](01-binding-spec-hierarchy.md) | Ten specs written over months will disagree, and nobody knows which wins | Declared precedence, plus invariants a linter enforces |
| 2 | [Encoding the integrity claim as a merge gate](02-linting-the-claim.md) | A product's central honesty claim decays under ordinary pressure | A CI linter on every diff — where the exemptions matter more than the rules |
| 3 | [Deciding what not to build](03-deciding-what-not-to-build.md) | Nine reasonable "could it also…"s kill a schedule | Written refusals, each with its reason attached |
| 4 | [Fixed scope, and a handover with a hole in it](04-fixed-scope-clean-handover.md) | Handover quality tracks urgency, not importance | What the deployment runbook got right, and the document that was never written |

---

## A note on the fourth one

It describes a gap in my own work — a thorough deployment runbook shipped alongside a
framework-default README. It stays in because a set of case studies where everything went
well is not a set of case studies, it is a brochure. The interesting part of that engagement
is the failure mode, and it generalises: the document that gets written is the one a
deadline forces, and orientation documents are never forced.

## Related

- **delivery-spec-kit** — the templates these practices distil into
- **delivery-gates** — several of these controls as runnable CI gates
