# Fixed scope, and a handover with a hole in it

*A client platform delivered to production, the handover artifact that worked, and the one
that was never written.*

---

*Client, sector and product details are omitted deliberately. What transfers is the delivery
shape, not the domain.*

---

## The engagement

A fixed-scope client web platform: marketing surface, a partner registration and sign-in
flow, and a bilingual in-product assistant. Built over roughly five weeks, deployed to
production, handed over.

Fixed scope on a client engagement is mostly a documentation problem rather than a
negotiation one. The scope is agreed once, early, when everyone is agreeable, and then the
only thing standing between that agreement and month three is whether it was written down
precisely enough to be checked.

## The handover artifact that worked

The delivery included a deployment and CI/CD document, and reading it back it does three
things right.

**It is written in the second person.** It says *"create an empty repository, then:"* and
gives the commands. It walks through connecting the hosting provider, step by step, with the
specific buttons named. This sounds trivial. It is the difference between a document written
to record what the author did and a document written so somebody else can do it — and almost
all handover documents are the first kind while claiming to be the second.

**It states what CI enforces, not just that CI exists.** Install, lint, typecheck, build, on
every push and every pull request. And explicitly: *a red check blocks the merge.* The
enforcement, not the intention. A reader knows immediately whether a failing check is
advisory or terminal, which is the first thing anyone actually needs to know.

**It states the security posture as a fact about the system.** *No deploy secrets live in
the source host; environment variables are managed in the hosting provider.* One sentence,
and it answers the question the next person would otherwise have to discover by searching
for credentials that are not there. Stating where secrets are **not** is as useful as stating
where they are.

It also carried a recommendation the engagement did not itself implement — branch protection
on the main branch — written as a recommendation rather than presented as done. That
distinction is worth preserving. A handover that quietly lists aspirations among completed
work is worse than one that lists fewer things.

## The hole

The repository's main README was never written. It is the unmodified scaffold the framework
generates — *"This is a Next.js project bootstrapped with create-next-app"*, the default
getting-started instructions, links to the framework's own tutorials.

That is a real gap and it is worth naming rather than tidying away.

The deployment document answers *how do I ship this*. Nothing answers *what is this, what
were the constraints, what is deliberately absent, and what would I do next*. Those are
different questions asked by different people at different times, and the second set is
asked long after the engagement ends, usually by somebody with no way to reach the author.

The failure is characteristic and worth being precise about. The operational document got
written because it was needed *immediately* — the site could not go live without it. The
orientation document was never forced by a deadline, so it never happened. Handover quality
tracks urgency, not importance, unless something makes it otherwise.

The fix is not more discipline. It is making the orientation document a blocking item on the
release checklist, the way the deployment steps effectively were.

## What transfers

- **Write the handover in the second person.** If it reads as a record of what you did, it
  is not a handover. If it reads as instructions to a stranger, it is.
- **State what CI enforces, not that CI exists.** "A red check blocks the merge" is
  information. "We have CI" is not.
- **Say where the secrets are not.** It saves the next person a search that ends in
  uncertainty.
- **Distinguish recommendations from completed work**, explicitly, in the document.
- **A handover is only as good as its weakest document.** A thorough deployment runbook
  alongside a framework-default README is not a clean handover; it is a well-documented
  deploy attached to an undocumented project.
- **Make the orientation document a release blocker,** because nothing else will force it.
  The operational document gets written under deadline pressure. The one explaining *why*
  never does.
