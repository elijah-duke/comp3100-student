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

**Name:** Elijah

**Week:** 4

**Work Order No.:** 4

## Milestone 1

**What I did:** I ran two looms, north and south, both with the hihest
courtesy of 19. In a second terminal I watched them in `top` and
recorded their rates.

**Output or seal:** `~~~ WAX SEAL of the Guild: E9891F11 ~~~`

**What it means:** Since each process was using close to 100% CPU, there
is little cost to being courteous when there isn't very much
competition.

## Milestone 2

**What I did:** I ran the drill and the looms slow down. I followed a
PID to its parent and found `~/enginehouse/machinery/amendment-314.sh`.
I added it to the list in `crontab`. Then, I reniced the process to the
most courteous, 19.

**Output or seal:** `~~~ WAX SEAL of the Guild: D3EB0E13 ~~~`

**What it means:** When I ran the drill, the looms dropped dramatically
in rates. In `top` I found processes running with courtesy 0. I followed
a PID up to its PPID which led to /bin/sh /home/.../amendment-314.sh.
This was a dispatcher that ran every day at 15:14. To fix this, I
reniced the running burners to 19. None of the processes were killed and
the looms climbed back to a normal rate. This is because at equal
courtesy everyone has equal opportunity to use the engine.

## Milestone 3

**What I did:** I inspected my shell's own scheduling class. Then, I ran
a reference loom at courtesy 0 and compared the two rates.

**Output or seal:** `~~~ WAX SEAL of the Guild: E0EF418A ~~~`

**What it means:** `SCHED_FIFO` takes priority over other jobs on no
matter its nice valuem which is why renice on it changes nothing while
it holds that class. FIFO can cause starvation since it never yields.

> Fewer or more milestones this week? Copy a block above as needed.

## Reflection

1.  **You now hold three levers: courtesy (nice/renice), the real-time
    class (SCHED_FIFO), and the governor (CPUQuota). The Guild wants the
    3 o'clock Demonstration protected from any future queue-jumper. In a
    paragraph: which lever do you pull, on which jobs, and what does
    each alternative cost or risk? (There is more than one defensible
    answer; costs are the point.) zyBooks 3.1–3.3.**

I'd reach for the governor. The 15:14 amendment showed courtesy's
weakness: a job that sets its own courtesy to 0 gets priority, so renice
only ever catches a queue-jumper after it's too late. A FIFO job never
yields on its own schedule, it ignores nice entirely, and if I put the
Demonstration's own process there and it ever hangs or loops, it can
cause starvation, with no courtesy system left to appeal to. A CPUQuota,
doesn't need to know courtesy. It enforces a ceiling on the system so
that no future amendment-314, whatever nice value it picks, can take
more than its share. The costs of this are background jobs under that
quota might need more than its allowed, and it requires setup.

2.  **During the drill your courtesy-19 loom fell to a handful of lines
    per second — on a floor with many engines it may even have reported
    a flat 0, its share having rounded below a single card — and the
    moment the burners matched its courtesy it climbed two orders of
    magnitude. In two or three sentences: what was the scheduler still
    promising the loom at the bottom of the queue (a reported zero is
    not the same as never being run), and when is courtesy 19 the right
    setting for a job you love?**

The scheduler still promised the loom a turn eventually, it just prioritized 
jobs that were less courteous. Courtesy 19 is the right setting for a job you
love when you know it needs to finish eventually but not urgently, and other
systems don't depend on it. So, when the less-courteous jobs finish, it gets
a turn.

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
