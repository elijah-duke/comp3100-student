# Case Notes

*Honourable Guild of Enginewrights — Ex Vapore, Ordo*

Copy this into your repo root as `case-notes.md` (or wherever your work order says) and keep it running all semester. Add one row every week for anything odd you notice while you work — even if you're not sure it matters yet. Small, plain notes are more useful later than you'd expect.

The skill this builds is noticing; explaining comes later, sometimes weeks later. A half-formed note beats a tidy one you meant to write and never did.

## The ledger

| Week | What I found | Where/how | What I think it means |
|------------------|------------------|------------------|------------------|
| 1 | A denied pension petition after the Grand Analytical Engine replaced the Computing Room. "somebody pins it back up fresh every week." | `cat ~/enginehouse/inbox/noticeboard.txt` | Some system is resetting the file, maybe something that used the `clock` keyword |
| 2 | A ledger annex containing timestamped entries. After each card-reader run, another identical entry is added. | `cat ~/.ledger-annex` | Connected to last week's story, "somebody pins it back up fresh every week." |
| 3 | loom-tender, which is a process running with PPid 1. It has an environment variable, PATRON=E.K. | `/proc/<pid>/environ` | E.K. was the patron who orphaned this process |
| 4 | A dispatcher script that ran burners at courtesy 0 at 15:14. | `ps -o pid,ppid,ni,args` to the parent `crontab -l` | Very similar to previous weeks where a system keeps running itself. |
| 5 | Table IX twice, the two measured rows matched and the computed rows didn't. | `diff ~/enginehouse/ledgers/output-ledger.txt ~/.ledger-annex` | The Engine rounds each number before adding, since my math didn't round til the end |
| 6 | Lever 07 is held by a lock file owned by uid 1849. The roll lists 1849 as "Computing Room corps, pending archive transfer". | `cat lever-07.lock`, `cat lock-record.txt`, and `getent passwd 1849` |  This is relevant to Week 1's denied pension petition. |

Add more rows as the weeks go on. Keep entries short — a sentence or two per column is plenty, and a note that turns out to be nothing costs you nothing.

-   **What I found** — the plain fact. Just what you saw.
-   **Where/how** — the file, command, or tool that showed it to you.
-   **What I think it means** — your own read on it. Guesses are fine; label them as guesses if you're unsure, and "no idea yet" is a perfectly legitimate entry.

## Current suspicions

*Free-write space. What's your running theory? What doesn't add up yet? Revise this section any week — nobody's grading you on being right early, only on citing your own notes later.*

### Week One

I think what is going to be important to discover is what is keeping the noticeboard's message updated, and why is it important that it stays updated? I think the punch card is a clue to figure this out.

### Week Two

Although this week went differently than I initially imagined, we now know how the noticeboard is staying updated. Question I need answering: Why update? Is there any significance behind the `clock` keyword? What does the punchcard do/say?

### Week Three

loom-tender is a process with PPid 1. Furthermore, it has an environment variable, PATRON=E.K.. The first line of the spool is: `TABLE OF PRODUCTS -- computed by hand, entered fair, in ink`. This is similar to the `.ledger-annex` last week. For a parentage to look like this, it was likely orphaned. This could have been done by E.K.. I am still not sure who this is, but I am confident they will be an important figure in upcoming weeks.

### Week Four

This is something timed very intentionally against the exhibition of the Grand Analytical Engine. I think this is protest rather than sabotage and I bet this connects back to E.K.

### Week Five

The rounding problem would make sense considering the consistency of the computations that didn't match. If the system is rounding, then who or what is doing the correct math in the fair copy?

### Week Six

In week one, we read about a denied pension petition due to the Analytical Engine replacing the Computing room. The description, "Computing Room corps, pending archive transfer" seems related, and might be cause for someone to protest or sabotage.

------------------------------------------------------------------------

*Tip: if two weeks' findings seem to point the same direction, say so in a note — connecting your own dots across weeks is exactly the skill this ledger is for.*
