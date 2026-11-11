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

**Name:** Elijah Duke **Week:** 9 **Work Order No.:** 8

## Milestone 1

**What I did:**

| Symbol        | Region | Why                                                 |
|------------------------|------------------------|------------------------|
| `main`        | text   | Executable code; lowest address (`…f331c9`).        |
| `tally_kept`  | data   | Initialized global; just above code (`…f36010`).    |
| `spare_slips` | bss    | Uninitialized global; right after data (`…f36040`). |
| `slip`        | heap   | `malloc`'d at runtime; above bss (`a0b2e2a0`).      |
| `on_hand`     | stack  | Local; high `0x7ffe…` address.                      |

**Output or seal:** `~~~ WAX SEAL of the Guild: 8F513B1B ~~~`

**What it means:** Code and globals are fixed into the file (text, data, bss), 
while things created as the program runs (heap allocations, local variables) are
not. That’s why size can report text, data, and bss but not the heap or stack. 
Those are built at runtime and the file on disk won’t know how big they’ll get.

## Milestone 2

**What I did:**

| \# | Heading | First `.c` line | Block |
|------------------|------------------|------------------|------------------|
| 1–4 | Invalid write of size 1 | `enter:38` | blocks size 7/8/15/11, alloc'd at `:33` |
| 5 | Invalid read of size 4 | `main:67` | size 24, free'd at `strike:48`, alloc'd at `enter:27` |
| 6 | Invalid read of size 1 | `main:71` | size 15, alloc'd at `:33` |
| 7 | definitely lost (rec 6/6) | `enter:27` | 102 B (24 direct + 78 indirect), 1 block |

**Three faults:** 1. **Invalid write of size 1** → line **38** (buffer
one byte short; report 6 is its after-effect at line 71). 2. **Invalid
read of size 4** (use-after-free) → line **67**. 3. **definitely lost**
(leak) → line **27**.

**Output or seal:** `~~~ WAX SEAL of the Guild: 1A6E8F38 ~~~`

**What it means:** `valgrind` caught three bugs in the clerk: a one-byte-short 
buffer that strcpy overran, a slip read after it was already freed, and slips 
that were never freed before exit. 

## Milestone 3

**What I did:** Built and ran `store-siphon`.

```         
store-siphon: 52 blocks drawn from the Store; none returned. 
store-siphon: the cap is reached. Resting.
```

**Output or seal:** `~~~ WAX SEAL of the Guild: 95E9B758 ~~~`

**What it means:** The siphon outputted that it drew 52 blocks and returned 
none. It's cap was 48. 

## Milestone 4

**What I did:** Read comments and a leaked block
```
1:  /* store-siphon -- draws from the Store and keeps what it draws.  -- E.K. */
6:  /* Never more than this. The Engine shall not want for a block on my account.  -- E.K. */
13: /* The draft is kept in cipher on paper; the Store keeps the fair copy.  -- E.K. */
61: /* A fair copy is never written over. A new block for every hand.  -- E.K. */
```

**Output or seal:** `~~~ WAX SEAL of the Guild: E3B07045 ~~~`

**What it means:** Every comment is signed `-- E.K.`, and fair_copy mallocs a 
fresh block for each line it’s given and never frees it, so at rest those leaked
blocks still hold readable text that x/s reads straight back out.

> Fewer or more milestones this week? Copy a block above as needed.

## Reflection

1.  **In a paragraph: the routine leaks every block it draws, and the Engine is 
none the worse. Explain why a leak in a program that runs for a second and exits
costs almost nothing, why a leak in a program that runs for months can bring a 
machine down, and what the cap changes. Use the words heap, leak, definitely 
lost and indirectly lost correctly at least once each. zyBooks Ch 6.**

The siphon draws block after block onto the heap and frees none, yet the Engine 
is fine: the program runs a second and exits, and on exit the OS reclaims the 
whole address space at once. So, every block definitely lost and indirectly lost
is handed back regardless. In a program that runs for months, nothing reclaims 
those blocks while it keeps running, so the leak grows the heap without bound 
until memory is exhausted and the machine is dragged into swap or the process 
killed. The cap changes only the rate, not the fate: SIPHON_CAP bounds how much 
one run can draw, but a program that loops or restarts still 
bleeds the heap dry over time.

2.  **The clerk printed the right ledger every time you ran it, and was wrong 
three ways. In two or three sentences: is a program that always prints the right
answer a correct program? What would you want to see before you said so? 
(There is no answer key. I am asking what you would defend in a code review.)**

No. A right answer only proves the output was correct on the runs you watched, 
not that the program is. Bugs happen to not touch the numbers, and with longer. 
Before calling it correct I’d want valgrind to confirm it's clean.

## Sources and help

Anyone or anything that helped you this week — a classmate, a man page,
a Stack Overflow answer, an AI assistant. One line each: who or what,
and what you used it for. This is **not graded and never costs points**;
it is the habit professional engineers keep, and the syllabus asks for
it under *Academic integrity* and *Use of AI tools*.

**Used Claude to generate markdown tables**

*Nothing to report? Write "None" — that's a perfectly normal week.*

## Time spent

Roughly how long this took, start to finish: 2 hours *No wrong answer —
this just helps calibrate future work orders.*
