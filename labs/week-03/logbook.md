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

**Week:** 3

**Work Order No.:** 3

## Milestone 1

**What I did:** For the first TODO, I used `fork` to make a copy of the
desk. For the second TODO, I added `execvp(argv[1], &argv[1])` so that
the child becomes the command. For the third TODO, I added
`waitpid(child, &status, 0)` for the parent to wait for the child to
finish.

**Output or seal:** `~~~ WAX SEAL of the Guild: BA055E04 ~~~`

**What it means:** This task demonstrated the
`fork() -> execvp() -> waitpid()` flow of commands.

## Milestone 2

**What I did:** I read `zombie-maker` and ran it in the background. I
captured its `Z` line.

**Output or seal:** `~~~ WAX SEAL of the Guild: BBDB3293 ~~~`

**What it means:** The Z line meant the child had already exited but was
still in the table because its parent hadn't called wait yet.
`/proc/14536/status` added commandline evidence of this being the case.
My Task 1 pantograph never makes one because it runs `waitpid`.

## Milestone 3

**What I did:** Found my shell's pid and saved it. I looked through its
parentage and found out it is an orphan.

**Output or seal:** `~~~ WAX SEAL of the Guild: CC62BF35 ~~~`

**What it means:** I found the job by comparing PPIDs. Its parentage
says it's an orphan, not a zombie. Its real parent exited, so the kernel
handed it up to PID 1. The drill turned up a `PATRON=E.K`.

> Fewer or more milestones this week? Copy a block above as needed.

## Reflection

1.  **fork() copies a process; execvp() replaces the program inside one.
    Most languages you have used offer a single "run this command" call
    instead. In a paragraph: what does splitting the job into two steps
    let a shell do between them that a single call would not? (You built
    the seam yourself in Task 1 — everything your shell does with
    redirection and pipes happens in that gap.)**

Splitting the job into two steps allows a shell to manipulate the child
before execution. It is in this gap that the environment of this child
changes. Its input and output pipes will redirect. In other languages,
this wouldn't be an option. It would instead hand it off to a shell that
is able to manipulate environment variables.

2.  **A zombie has finished but has not been collected; an orphan is
    still running but its parent is gone. You met one of each this week.
    In two or three sentences: which resources does each one hold, who
    is responsible for clearing each, and why is the zombie the one that
    can bring a machine down?**

A zombie just holds a pid and status in the kernel's table, and its
parent must call `waitpid` to clear it. An orphan holds resources like
any other process, and it cleans itslef up after being reparented and
finishing. The zombie can bring a machine down since it occupies a table
entry until its parent calls `waitpid`.

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

Roughly how long this took, start to finish: 3 hours *No wrong answer —
this just helps calibrate future work orders.*
