# Deciding what not to build

*A written doctrine as scope defence, and why each refusal needs a reason attached.*

---

## The problem

An early-stage statistical product with a wide adjacent solution space and a single
developer. The failure mode for a project like this is not building the wrong thing — it is
building nine reasonable things instead of one good one.

The pressure does not arrive as a bad idea. It arrives as a good one, in month three, in the
form *"could it also…"*. Every instance is defensible in isolation. The tenth is what kills
the schedule, and by then the first nine are load-bearing.

Scope defence built on memory does not work. Six months on you no longer recall whether an
exclusion was a deliberate refusal or something that simply never came up, and the two are
indistinguishable from the outside.

## What was done

Ten numbered non-negotiables, committed early, in a single document. Each one states a
refusal and — critically — the reason for it.

A representative example: the product will never issue anything a user can present to a
third party as an endorsement. It sells instruments, not credentials. The reason is
attached: five named companies that built businesses on selling verification, all of them
gone. Verification survives as a feature of another business, not as the business.

Another: no setting, pricing tier, or experiment may weaken a statistical threshold. The
same bar applies to a free user and the highest-paying one. Written as a law rather than a
default, because a default is something a growth experiment is entitled to test.

A third: a specific combination of commercial mechanics is refused *by construction*, with a
regulatory precedent cited by name. Not "we should be careful about this" — refused, with
the case that makes it a real risk rather than a vague one.

The public-facing surface carries a compressed version. Three lines under a heading that
says what the product **is not**, high on the front page, before any description of what it
does.

## Why the reasons matter more than the refusals

A refusal without a reason survives exactly one confident challenge.

*"We don't issue certificates"* invites the obvious reply: **why not, users are asking for
them.** There is no answer in the room, and the person arguing for the feature is
enthusiastic while the person defending the line is working from memory.

*"We don't issue certificates, because five companies built that business and all five are
gone"* is a different conversation. The challenger now has to argue those five cases were
different, which is a real argument they may even win — and if they win it, the refusal
*should* be revisited. The reason makes the doctrine both defensible and falsifiable, which
is what separates a principle from a preference.

It also means the document survives its author. Someone reading it in a year gets the
argument, not just the conclusion, and can tell whether the reasoning still holds.

## The honest part

The document's opening line states that it is *CI-enforced where a linter can reach it, and
human-enforced where it cannot.*

That sentence does more work than it appears to. Several of the laws concern vocabulary in
user-facing copy — a machine can check those, and one does. Others concern product
judgement: whether a verdict page buries bad news, whether a framing bends toward retention.
No linter reaches those, and claiming otherwise would be theatre.

Being explicit about which controls are automated and which depend on a person is the
difference between a governance document and a compliance costume. It also tells you exactly
where the risk sits: the human-enforced laws are the ones that quietly erode, so they are
the ones worth re-reading before a release.

The same instinct shows up in the project's founding decision record, which contains a
section for choices *deliberately not made* — deployment target left open, nothing in the
design assuming a host. Recording a non-decision as a decision stops the next person
reading a gap as an oversight and filling it.

## What it cost

Almost nothing to write, and it was written before there was much to defend — which is the
only time it is easy. Writing exclusions after the pressure arrives means writing them
against a specific person's specific proposal, which makes it a conflict rather than a
policy.

The ongoing cost is honesty about drift. A doctrine nobody re-reads becomes decoration. The
mitigation is small: the compressed version sits on the front page where it is seen
constantly, and the full version is referenced from the contribution path.

## What transfers

- **Write the refusals down early**, while they are cheap and abstract, not late and
  personal.
- **Attach a reason to each one.** A refusal with a reason can be challenged on its merits,
  which is what keeps it honest. A refusal without one gets overturned by whoever is most
  confident in the room.
- **Ground reasons in precedent where you can.** "Five companies died doing this" ends a
  debate that "I don't think we should" merely starts.
- **Say which controls are automated and which are not.** Pretending a judgement call is
  mechanically enforced is worse than admitting it depends on a person paying attention.
- **Record deliberate non-decisions.** An unexplained gap gets filled by the next person who
  assumes it was an oversight.
