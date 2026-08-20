# Least privilege as an import contract

An agent with tool access has the blast radius of a badly scoped IAM role. The standard
answer is a policy engine: intercept each action at call time, check it against policy.
That works while the engine is consulted. The failure mode is the path that never asks.

This system enforces the constraint earlier, in the import graph, where a path that never
asks cannot compile into existence.

## The pattern

**One door.** All outbound traffic originates from one package, `egress`. Thirteen other
top-level modules in the application. None of them can make an outbound call, because none
of them may import the machinery that does.

**The door is guarded.** Egress decisions run through a single `prepare_egress` function:
allow-list, vault gate, secret and PII scan, and a manifest row recording what left and why.
High-stakes actions sit behind a mandatory human approval. The agent drafts; a human
releases.

**A linter keeps it single.** Four contracts in `.importlinter`, run on every push. The
encrypted store importable from exactly one module. The browser engine from exactly one.
Provider SDKs importable nowhere, because completions queue to a worker instead of calling
out. And the retrieval path may never import egress at all: the evidence lane has zero
network, by contract rather than by convention.

A violation fails CI before review. Nobody has to remember the rule. The build remembers.

## The part worth reading: the day it failed

The import contract polices *how* a call is made. It says nothing about *whether it was
authorised*. That gap is not theoretical. A pull request shipped an LLM job queue as a
second egress path with no policy check and no manifest row, and every existing gate was
green on it. A human reading the diff caught it. Nothing else did.

The fix was not a stricter policy. It was a new kind of gate: static tests that parse every
module's source and fail if any module constructs an egress request without running it
through `prepare_egress`, or writes to the manifest from outside the egress package. Static
matters: a path that is merely reachable fails, even if no test ever executes it.

So the boundary is layered. The import contract binds what can be imported. The static gate
binds what can be called. The human gate binds what actually leaves.

## Where the boundary holds, and where it does not

- **An import contract binds your import graph, nothing else.** `subprocess`, `eval`, or a
  raw socket walk past it unless named explicitly. That is what the static gate exists to
  narrow.
- **The tool has edges, and we hit one.** import-linter cannot forbid subpackages of an
  external namespace, so one provider SDK could not be named in the contract. It is covered
  by a test that reads the source directly. When the tool ran out, the invariant did not.
- **An agent that can edit the contract edits the boundary.** The contract is a file in the
  repository. Protection is procedural: the agent only proposes, a human merges, and a
  separate gate fails any pull request that changes watched code without a recorded
  decision. A contract edit therefore needs a human merge and a written reason, in the
  diff, in daylight.
- **Human approval is a bottleneck by design.** A stalled action beats an unsupervised one
  for high-stakes egress. Wrong trade for high-volume, low-stakes calls. Pick per lane.

## The result

The agent operates at full capability inside the boundary and has none outside it. Not
because a policy said no at call time. Because the code that would do it cannot exist on
main.

## Related

- [The binding spec hierarchy](01-binding-spec-hierarchy.md) — the contract layer these
  invariants live in, and the precedence rule that makes it authoritative
- **delivery-gates** — the watched-files gate that makes editing the contract a recorded
  decision
