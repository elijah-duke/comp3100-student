---
editor_options: 
  markdown: 
    wrap: 72
---

# Engineer's Logbook

*Honourable Guild of Enginewrights — Ex Vapore, Ordo*

Copy this into your week's folder as `logbook.md` and fill it in as you
work. Paste your wax seals where marked — that's how a milestone gets
marked done.

Write it the way you'd explain the week to a classmate who missed it:
plain sentences, no polish. An honest half-answer under "what it means"
— *I got the seal but I'm still fuzzy on why the second run differed* —
beats a confident sentence you don't believe, and it tells me where to
start when you bring it to studio.

**Name:** Elijah Duke

**Week:** 7

**Work Order No.:** 7

## Milestone 1

**What I did:** The four boxes in the order they took their holds:

1.  Northgate (1851-09-26 18:51:07)

2.  Waterside (1851-09-26 18:51:21) [14 seconds later]

3.  Old Quarter (1851-09-26 18:51:35) [14 seconds later]

4.  Kiln Row (1851-09-26 18:51:49) [14 seconds later]

Rule 2 means that the box called most recently has the least to lose
from being released.

**Output or seal:** `~~~ WAX SEAL of the Guild: 8FB23102 ~~~`

**What it means:** We found four boxes that are all on hold. We need to
relase one of them, and the one that is cheapest to release is the one
that was most recently called.

## Milestone 2

**What I did:** Used `gdb` to get information on each thread. Looked at
which box wants which lever.

**Output or seal:** `~~~ WAX SEAL of the Guild: 7ED21DDD ~~~`

**What it means:** Northgate waits for Waterside, Waterside waits for
Old Quarter, Old Quarter waits for Kiln Row, Kiln Row waits for
Northgate

## Milestone 3

**What I did:** I found evidence of the four conditions of deadlock:

1.  Mutual exclusion -- `info thread`: The number changes from run to
    run; the match does not.

2.  Hold and wait -- `print holding`: What each box holds

3.  No preemption -- `pipe thread apply all bt | grep take_lever`:
    Nothing takes a lever back

4.  Circular wait -- `pipe thread apply all bt | grep take_lever`:
    Following box to box closes a ring

**Output or seal:**

`~~~ WAX SEAL of the Guild: FDE452E7 ~~~`

**What it means:** The condition that was broken was No preemption. In
the philosopher problem, it was Circular wait. This time around we are
forcing the younest box to release as a recovery method.

> Fewer or more milestones this week? Copy a block above as needed.

## Reflection

1.  **In a paragraph: why does a deadlock need all four conditions at
    once, and why is breaking any one of them enough? Then take two of
    the table's prevention rows and, for each, describe a program where
    that remedy would be the wrong one to reach for — because it is
    impossible there, or because of what it costs. Use the words mutual
    exclusion, hold and wait, resource preemption and circular wait
    correctly at least once each.**

Deadlock needs all four conditions since mutual exclusion is the reason
why there is waiting in the first place. Hold and wait means the waiters
are blocking something else. No resource preemption means nothing
outside can free a held resource. Circular wait closes the chain, so
there's no thread at the end that is free to finish. Remove one and a
thread can make progress.

2.  **The release worked because somebody outside the ring had the
    authority to tell one box to give way, and a manual that said which.
    Most real programs have nobody like that: when their threads
    deadlock they simply stop, and a person notices eventually. In two
    or three sentences: would you rather build a system that can recover
    from deadlock, or one that cannot get into it at all — and what does
    each choice cost you?**

I would rather design a system that cannot get into a deadlock at all. The cost 
of this would be needing to design a system that is rigid and tightly sealed.

## Sources and help

Anyone or anything that helped you this week — a classmate, a man page,
a Stack Overflow answer, an AI assistant. One line each: who or what,
and what you used it for. This is **not graded and never costs points**;
it is the habit professional engineers keep, and the syllabus asks for
it under *Academic integrity* and *Use of AI tools*.

-   *(example)* Worked through the `fork` ordering with Sam in studio.
-   *(example)* Used an AI assistant to explain what `EAGAIN` means in
    the trace.

*Nothing to report? Write "None" — that's a perfectly normal week.*

## Time spent

Roughly how long this took, start to finish: 3 hours *No
wrong answer — this just helps calibrate future work orders.*
