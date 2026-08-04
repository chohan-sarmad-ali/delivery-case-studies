# Encoding the integrity claim as a merge gate

*What it takes to make a product's central honesty claim mechanically enforceable — and why
the exemptions matter more than the rules.*

---

## The problem

An analytics product whose entire credibility rests on one narrow claim: it estimates *how
far* something might move, and explicitly refuses to claim *which way*. The distinction is
the product. Remove it and what remains is indistinguishable from every overclaiming tool in
a crowded and disreputable category.

Claims like that decay under ordinary pressure, and not through bad faith. A label gets
shortened for the mobile breakpoint. A landing-page headline gets tightened. Someone writes
a parameter name that reads as directional because it is three characters shorter. Each
change is individually defensible; the aggregate is a product making a claim it spent two
years refusing to make.

Human review does not catch this reliably. Reviewers look at diffs for correctness, and
copy changes look correct. The drift is only visible in aggregate, and by then it has
shipped.

## What was done

A linter runs on every diff. It scans for the specific class of claim the product refuses to
make — direction prediction, win-rate framing, guaranteed outcomes, buy/sell signal framing,
get-rich phrasing — and fails the build when one appears.

Seven patterns, each paired with a human-readable reason. The build failure does not say
*"pattern 4 matched"*; it says *"advertises a win-rate (direction-edge framing)"*. A gate
that reports a regex is a gate people learn to skim.

Marketing copy is linted like code, and it goes through the same pipeline, because the claim
does not care which file it appears in.

## The part that actually matters

The naive version of this tool is worthless, and it fails in a specific, predictable way.

The product's own honest copy is *full* of the banned vocabulary. It says "never a buy/sell
signal." It says "not a guaranteed win." It says "direction is a coin-flip." Every one of
those sentences contains a banned phrase, and every one of them exists precisely because the
product is being honest.

A linter that flags them is worse than no linter. It fires constantly on correct copy, the
team learns the failures are noise, someone adds a skip, and within a fortnight the gate is
decorative. The failure mode that kills a gate is not being evaded — it is being *ignored*.

So the design is mostly exemptions:

**Added lines only.** A diff's removed and context lines are not scanned. You are auditing
what is being introduced, not relitigating what is already there.

**Honest-negation markers.** Any added line carrying a negation or disclaimer marker —
*never*, *not*, *n't*, *no*, *coin-flip*, *calibrat*, *honest*, *refus*, *disclaim*, *warn* —
is exempt. This is the whole trick. "Never a buy/sell signal" carries *never*, so it passes.
"Buy signal" does not, so it fails.

**Scoped paths.** Shipped code and user-facing copy are audited. Tests, documentation,
research scripts and working notes are skipped, because those legitimately discuss the
banned concepts in plain terms — you cannot write a test for the honesty linter without
writing the phrases it bans.

**Markdown excluded.** Same reasoning: the documents that explain the policy necessarily
quote it.

## Honest about what it is

The tool's own docstring describes it as *a heuristic guard, not a proof* — a tripwire that
flags a suspicious addition for a human to review.

That framing was deliberate and it is the difference between a control and a talisman. A
regex cannot determine whether a sentence makes a directional claim; it can determine that a
sentence is worth a second look. Overstating it would have been its own small dishonesty, in
a tool whose entire purpose is enforcing honesty.

It sits alongside a standing rule that automated contributors may only ever *propose*, never
merge. The gate is one layer of a review process, not a replacement for one.

## An aside on false positives

While building an unrelated pre-publication check for this repository, a quick keyword sweep
returned four failures. Every one was a substring collision: *substantial* and *credentials*
matching a three-letter forbidden term, *message* matching a project name. Word boundaries
fixed it in a minute.

That is the same failure mode, encountered live, in a five-line script. It generalises: the
first draft of any content gate cries wolf, and if you ship that draft the gate dies. Budget
more design effort for the exemptions than for the rules.

## What transfers

- **If a claim is load-bearing, make it mechanically checkable.** A value that lives only in
  a founder's head does not survive contact with a deadline.
- **Design the exemptions first.** The rules are the easy part. Whether the gate survives is
  decided entirely by how it behaves on correct input.
- **Scan additions, not the world.** Auditing the whole codebase produces a backlog nobody
  clears. Auditing the diff produces a decision somebody makes today.
- **Say what the control actually is.** "Heuristic tripwire for a human reviewer" invites
  the right amount of trust. "Automated compliance check" invites far too much.
- **Lint the copy.** Marketing text makes claims your users rely on, so it deserves the same
  pipeline as the code.
