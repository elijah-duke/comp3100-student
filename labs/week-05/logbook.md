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

**Week:** 5

**Work Order No.:** 5

## Milestone 1

**What I did:** I read through `starter/twin-looms.c`. Then, I ran it
and compared the number of entries in the ledger. It was different on
every run.

**Output or seal:** `~~~ WAX SEAL of the Guild: AB2DDCB5 ~~~`

**What it means:** The first two lines are trustworthy, since each loom
has its own counter. The ledger is not trustworthy since both looms are
trying to write to it.

## Milestone 2

**What I did:** I opened the c file for the looms and added code to
implement a lock (mutex) around the critical section `total`

**Output or seal:** `~~~ WAX SEAL of the Guild: 8D7A745E ~~~`

**What it means:** With a mutex, only one thread is able to access the
locked critical section. A read-write-modify will finish before the next
thread can read. `l->woven++` got to stay outside since only one thread
touches it. The cost of adding a guard is time. A guard will be slower,
but it is worth being correct.

## Milestone 3

**What I did:** I read through `~/enginehouse/ledgers/output-ledger.txt`
and `~/.ledger-annex` and compared the difference between each file with
`diff`.

**Output or seal:** `~~~ WAX SEAL of the Guild: 70DAB71D ~~~`

**What it means:** The computations are not equal between the two.
Furthermore, `awk` will chop off any trailing zeroes. While the number
itself doesn't change, what gets stored in the ledger would.

> Fewer or more milestones this week? Copy a block above as needed.

## Reflection

1.  **Your guarded build printed the same total five times out of five —
    400000 on the default count, or whatever your two A1 figures add up
    to if you raised it. In a paragraph: does five exact runs prove the
    race is gone? Say what the unguarded runs would have looked like if
    you had been unlucky enough to see only exact ones — and then say
    what would count as proof, given that "I ran it and it worked" is
    the same sentence a student says about a program with a live data
    race in it. Use the words critical section and data race correctly
    at least once each. zyBooks Ch 4.1.**

Five exact runs would prove that the race is gone. The likelihood of the
unguarded runs looking like that are very slim. To have the same number
of entries would be unlikely, but for all of them to print the correct
amount is borderline impossible. This is because in a data race two
different threads are trying to write to the same peice of memory, which
is called a critical section. The chance of each thread writing while
the other one isn't is very slim, especiallt for so many entries.

2.  **You have just told the Guild that two figures in a filed ledger do
    not match a second document, and that your own arithmetic makes a
    third opinion. In two or three sentences: what would you need before
    you were willing to tell the Board that a figure in a ledger is
    wrong? Name the evidence you would want, and — harder — name
    something that would still not be enough. (There is no answer key. I
    am asking what standard you hold yourself to.)**

I am not sure what else I would need in order to prove this wrong, I've noticed 
a pattern, but that doesn't necessarily make it false. The evidence I have that
my math matches the fair copy is not enough evidence I don't think.

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

Roughly how long this took, start to finish: \_\_\_\_\_\_\_ hours *No
wrong answer — this just helps calibrate future work orders.*
