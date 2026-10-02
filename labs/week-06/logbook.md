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

**Week:** 6

**Work Order No.:** 6

## Milestone 1

**What I did:** Looked at and ran `starter/interlocking.c`. I added a
mutex (`frame_guard`) and a condition variable (`lever_free`). I added a
locked while loop around `pthread_cond_wait`, and unlocked it after
claiming the line.

**Output or seal:** `~~~ WAX SEAL of the Guild: 883642FC ~~~`

**What it means:** The mutex fixed the error. Both processes 
can't both `clear`. The condition variable fixed the spinning. The idle 
looks dropped dramatically.

## Milestone 2

**What I did:** Looked at and ran `starter/sorting-floor.c`. I added 
`#include <semaphore.h>` and two semaphores, `room` (starting at 8) and 
`cards_waiting` (starting at 0). I also added a mutex `chute_guard` around the 
shared indices and a `high_water` counter to measure the deepest the chute ever 
got. Each press now does `sem_wait(&room)`, then locks, tips the card, unlocks 
and `sem_post(&cards_waiting)`.

**Output or seal:** `~~~ WAX SEAL of the Guild: 2DA46117 ~~~`

**What it means:** `room` counts empty slots (starts at 8), and cards_waiting 
counts full ones (starts at 0). Semaphores enforce the size limit, but they 
don't protect `tipped_in` and `taken_out`. Those are shared counters. The mutex 
is what prevents data loss.

## Milestone 3

**What I did:** Looked at and ran `tarter/ledger-hall.c`. Six different readers 
check if the page adds up. I added `#define _GNU_SOURCE` and declared 
`pthread_rwlock_t page_lock`. I wrapped the reader's copy-and-sum in 
`rdlock`/`unlock`, and wrapped all five of the clerk's writes in a single 
`wrlock`/`unlock`.

**Output or seal:** `~~~ WAX SEAL of the Guild: 85DAAF22 ~~~`

**What it means:** Every value was correct when written, but the reader saw two 
different moments, so the page as a whole is false. A plain mutex would give 0 
torn pages too, but it makes the six readers take turns even though reading 
never conflicts with reading. All five writes go inside one write-lock because 
the page is only correct after all five are done writing.

## Milestone 4

**What I did:** Looked at and ran `starter/philosophers.c`. Each 
philosopher takes their left fork, then reaches for their right one. The table 
stalled with 0 of 15 meals. I changed `dine()` so every philosopher takes the 
lower-numbered fork first (first/second instead of left/right). I reran it and 
all 15 meals were eaten.

**Output or seal:** `~~~ WAX SEAL of the Guild: F4BE746C ~~~`

**What it means:** Aurelia holds fork 0 and waits for fork 1 (held by Bramwell),
Bramwell holds fork 1 and waits for fork 2 (held by Cordelia), Cordelia holds 
for fork 2 and waits for fork 3 (held by Desmond), Desmond holds fork 3 and 
waits for fork 4 (held by Eustace), Eustace holds fork 4 and waits for fork 0 
(held by Aurelia). To fix the deadlock, each philosopher reaches for a 
lower-numberedfork, therefore whoever holds fork 0 will not reach for fork 4 and 
cause deadlock.

## Milestone 5

**What I did:** Listed `~/enginehouse/interlocking/` with `ls -ln` to see the 
owners as numbers, and checked the lock's owner and mode with `stat -c`. I read 
`lever-07.lock` and `lock-record.txt` with `cat`, then looked up the holding uid 
on the house roll with `getent passwd 1849`.

**Output or seal:** `~~~ WAX SEAL of the Guild: 2A5CA4EF ~~~`

**What it means:** The lock's uid is 1849 with the description of: Computing 
Room corps, pending archive transfer.

> Fewer or more milestones this week? Copy a block above as needed.

## Reflection

1.  **You used four different tools this week: a mutex, a condition variable, 
two semaphores, and a reader/writer lock. In a paragraph each for any three of 
them, say what the tool does that the others cannot, and name a situation where 
reaching for the wrong one would produce a program that is correct but useless. 
Use the words critical section, bounded buffer and starvation correctly at least
once each. zyBooks Ch 4.2–4.5.**

A mutex provides mutual exclusion. It allows only one thread at a time to enter 
a critical section. Unlike a condition variable, semaphore, or reader/writer 
lock, a mutex is specifically designed for protecting shared data where exactly 
one thread should have access at a time. For example, using a mutex to protect a
shared counter is appropriate. However, using a mutex to implement a bounded 
buffer would produce a program that could be correct but useless if the producer
simply locks the mutex while waiting for the buffer to have space, the consumer 
may need the same mutex to remove an item, causing the producer to block the 
thread that could make progress.

A condition variable allows a thread to sleep until some condition involving 
shared state becomes true, and it is normally used together with a mutex. What 
makes it different is that it provides a way for threads to efficiently wait for
a state change rather than repeatedly checking a condition. For example, a 
consumer in a bounded buffer can wait on a condition variable until the buffer 
is no longer empty. Reaching for a condition variable when you really need 
mutual exclusion, such as protecting a critical section, would produce a program
that is correct but useless. A condition variable by itself does not prevent 
multiple threads from simultaneously modifying shared data.

A reader/writer lock allows multiple threads to read shared data simultaneously 
while ensuring that a writer has exclusive access. This is something a simple 
mutex cannot provide, because a mutex would allow only one reader at a time. A 
reader/writer lock is useful for a data structure that is read frequently but 
modified infrequently. However, using one when exclusive access is always 
required would produce a correct but unnecessarily restrictive program, because 
the additional reader/writer machinery provides no benefit. Depending on the 
lock's policy, excessive preference for readers or writers can also lead to 
starvation, where the other type of thread waits indefinitely.

2.  **The long table stalls because every philosopher follows the same 
reasonable rule at the same time. Nobody is greedy and nobody is wrong. In two 
or three sentences: what does that tell you about testing? Specifically — your 
fixed table ran fifteen meals in under a second, three times out of three. What 
would you need to see before you were willing to tell the Guild that a piece of 
concurrent machinery is safe, given that "I ran it and it worked" is the same 
sentence a student says about a program that stalls once a fortnight? (There is
no answer key. I am asking what standard you hold yourself to.)**

I would need to know that all of the edge cases are passed. In this case, an
edge case would be a situation that could cause a deadlock.

## Sources and help

Anyone or anything that helped you this week — a classmate, a man page,
a Stack Overflow answer, an AI assistant. One line each: who or what,
and what you used it for. This is **not graded and never costs points**;
it is the habit professional engineers keep, and the syllabus asks for
it under *Academic integrity* and *Use of AI tools*.

- Used ChatGPT to better understand mutex, condition variables, and
reader/writer locks

*Nothing to report? Write "None" — that's a perfectly normal week.*

## Time spent

Roughly how long this took, start to finish: 4 hours *No
wrong answer — this just helps calibrate future work orders.*
