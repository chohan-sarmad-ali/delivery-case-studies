# The binding spec hierarchy

*Contract-first delivery, where conflicts resolve by lookup rather than by meeting.*

---

## The problem

A desktop application with ten numbered specification documents, written over several
months, covering everything from data contracts to the browser-automation subsystem.

Specs written months apart disagree. This is not a failure of care — it is arithmetic.
Spec 03 makes a reasonable assumption about how the storage layer behaves. Spec 07, written
six weeks later against a storage layer that has since changed, makes a different reasonable
assumption. Both authors were right at the time of writing. The documents are now in
conflict and nobody knows it.

The conflict surfaces during implementation, usually as a question in the form *"which of
these is actually true?"* — and the default resolution is a meeting. On a solo project the
meeting is with yourself, which is worse, because there is no second person to notice you
resolved it differently last Tuesday.

## What was done

Two of the ten specs were designated the **binding contract layer**. The remaining eight
are subsystem specs. The rule is stated in the repository's front page, in the first
paragraph anyone reads:

> Read `00` and `09` first — they are the **binding** contract layer. They override the
> subsystem specs `01`–`08` on conflict.

That is the entire mechanism. One sentence.

Its effect is that a conflict is no longer a question. It is a lookup. When spec 03 and
spec 00 disagree, spec 00 wins and spec 03 is a defect — someone opens a change against it,
and the resolution is recorded rather than remembered.

Three properties matter more than they look:

**Precedence is declared, not inferred.** A reader does not have to reconstruct which
document is more authoritative from its tone, its length, or how recently it was touched.

**The contract layer is small.** Two documents out of ten. If the binding layer is large,
everything is binding, and precedence stops discriminating between anything.

**The code is not an authority.** Where the code disagrees with a binding spec, the code is
wrong. That sounds obvious and is routinely violated — the usual move is to quietly amend
the document to match what was built, which converts the spec from a contract into a
changelog.

## The second-order effect

The contract layer contains a short list of architecture invariants. Things like: the
encryption binding is imported in exactly one module; the browser driver in exactly one; all
outbound network calls originate from exactly one; and one specific read path never touches
the network layer at all.

Those are rules a document can state and a linter can check. They were wired into
`import-linter`, which runs on every push. A violation fails the build.

This is the part worth generalising. The precedence rule made the contract layer
*authoritative*. Making its invariants mechanically checkable made it *executable*. Nobody
has to remember the rule, nobody has to spot the violation in review, and the rule cannot
quietly decay while everyone assumes someone else is watching.

The distinction between those two states — a document everyone agrees is authoritative, and
a document a machine enforces — is most of the difference between architecture that holds
and architecture that erodes.

## What it cost

Writing the precedence rule took about a minute. Keeping the contract layer small takes
ongoing discipline: there is a constant, reasonable pull to promote a subsystem spec into
the binding layer because it feels important. Resisting that is the actual work.

The invariants were harder. Expressing "the encryption binding is imported in exactly one
module" as a lint rule required the module boundaries to be real rather than aspirational,
which forced two refactors that would otherwise have been deferred indefinitely. That cost
was worth paying and would have been much higher a year later.

## What transfers

- **Declare precedence in writing, before you need it.** The cost is one sentence. The cost
  of omitting it is one meeting per conflict, forever, with inconsistent outcomes.
- **Keep the binding layer small.** Precedence only discriminates if most things are not
  binding.
- **State that the code is not the authority.** Otherwise the specification silently becomes
  documentation of whatever was built.
- **Any invariant a linter could check, a linter should check.** A rule living only in a
  document is a rule that will be broken by someone who never read it — and on a project
  with a high volume of machine-generated change, that someone is not always a person.
