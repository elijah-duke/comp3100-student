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

**Name:** Elijah Duke **Week:** 12 **Work Order No.:** 11

## Milestone 1

**What I did:**

-   `ls -l /proc/self/fd`
-   listed the shell's own open file descriptors.
-   Ran `ls` on a missing path two ways:
    -   `ls /no-such-fitting 2>&1 > order.txt`
    -   `ls /no-such-fitting > both.txt 2>&1`
-   Checked each file's size with `wc -c`.
-   Ran `yes | head -1` and printed `${PIPESTATUS[*]}`.

**Output or seal:** `~~~ WAX SEAL of the Guild: 5B79A45C ~~~`

**What it means:**

-   **FDs 0/1/2** (stdin, stdout, stderr) all point at the terminal
    `/dev/pts/0`; FD 3 is the directory `ls` opened to read the list.
-   **Order of redirections matters** (applied left to right):
    -   `2>&1` then `>` → stderr stays on the terminal, `order.txt` is
        **0 bytes**.
    -   `>` then `2>&1` → stderr follows stdout into the file,
        `both.txt` is **64 bytes**. (`&>` is shorthand for this.)
-   **`PIPESTATUS: 141 0`** — `head` exited 0 after one line; `yes` kept
    writing, got SIGPIPE, and died with 128 + 13 = **141**.

## Milestone 2

**What I did:**

-   Created a FIFO `tube` and read it with `cat` — once with no writer
    (killed by `timeout`), once while Terminal Two wrote to it.
-   Held the FIFO open from Terminal Two with `exec 3<> tube`, then
    inspected it with `fuser -v` and `lsof`; closed it with `exec 3<&-`.
-   Ran two readers on one pipe:
    `(cat tube > a.txt &); (cat tube > b.txt &)`, then fed 20 lines in.
-   Compared against `tee`: `cat tube | tee copy.txt > heard.txt`, then
    fed 3 lines in.

**Output or seal:** `~~~ WAX SEAL of the Guild: F8D8DAE5 ~~~`

**What it means:**

-   **A FIFO blocks until both ends are open.** With no writer, `cat`
    hung and `timeout` killed it → `rc=124`; it unblocked only once
    Terminal Two opened and wrote the tube.
-   **`fuser`/`lsof` show who holds it open** — Terminal Two's shell
    (PID 1772), FD `3u` (read+write), type `FIFO`.
-   **Two readers on one pipe split the data** — the 20 lines were
    divided (e.g. `a=10 b=10`); each line goes to only one reader, but
    the split always sums to 20.
-   **`tee` copies to every output** — both `copy.txt` and `heard.txt`
    got all 3 lines. That's the key contrast: `tee` duplicates, rival
    readers take turns.

## Milestone 3

**What I did:**

-   Listed the post with `ls -lan ~/enginehouse/post/` to see owners and
    fitting types.
-   Sent a capsule: `echo "TEST CAPSULE from $USER" > outbound`, caught
    it at `exchange-line` with `cat`, and checked the drop box with
    `tail -n 1`.
-   Inspected the copier with `ps -C tube-tee` and
    `/proc/$PID/cmdline` + `status`.
-   Tried to read its open files: `ls -l /proc/$PID/fd`,
    `lsof -p "$PID"`, `lsof exchange-line`.

**Output or seal:** `~~~ WAX SEAL of the Guild: 7AFD0197 ~~~`

**What it means:**

-   **Two owners on the post:** uid 1000 (you) holds `exchange-line` and
    `post/`; uid 1849 holds the three fittings given to the copier —
    `.drop-box`, `.listening-tube`, and the `outbound` symlink into the
    tube.
-   **The line works:** writing `outbound` (= `.listening-tube`) made
    the copier send one copy out `exchange-line` and write another into
    the drop box. `rc=124` is just `timeout` ending `cat` — the FIFO
    stayed open, not a failure.
-   **The copier is a daemon:** runs as uid 1849, reads
    `.listening-tube`, writes `exchange-line` + `outbound-copy.txt`.
    `State: S` = sleeping for the next capsule; `PPid: 1` = reparented
    to systemd, so no shell or schedule owns it.
-   **You hit the permission wall:** as uid 1000 you can't read another
    user's `/proc/1445/fd`, so `ls`/`lsof` both got "Permission denied"
    (`lsof exchange-line` → `rc=1`, nothing to report). Confirming its
    open files needs root.

## Milestone 4

**What I did:**

-   Counted capsules two ways: `grep -c '^CAPSULE ' "$DROP"` and
    `grep -c 'CAPSULE' "$DROP"`.
-   Tallied by date:
    `grep '^CAPSULE ' "$DROP" | cut -d' ' -f4-7 | uniq -c`.
-   Tallied by sender:
    `grep '^CAPSULE ' "$DROP" | cut -d' ' -f9- | sed 's/ TO .*//' | sort | uniq -c | sort -rn`.
-   Pulled the sender-less capsule's body:
    `grep -A 8 '^CAPSULE 04 ' "$DROP"`.

**Output or seal:** `~~~ WAX SEAL of the Guild: 304DF696 ~~~`

**What it means:**

-   **Anchoring matters:** `^CAPSULE` counts only header lines → 12;
    unanchored `CAPSULE` also catches one in-body mention → 13. The gap
    is that single extra mention.
-   **Date tally:** 5 + 2 + 2 + 3 = 12. `uniq -c` works without `sort`
    only because the capsules are already in date order.
-   **Sender tally:** trim from `TO` on to leave the sender, then rank.
    Exhibition Office leads with 3; `FROM (none given)` ×1 is the
    capsule with no real sender.
-   **`grep -A 8`** prints a header plus the 8 following lines, so it
    pulls capsule 04's whole body. The earlier `NN` run matched nothing
    — it was a literal placeholder, not a real header.

> Fewer or more milestones this week? Copy a block above as needed.

## Reflection

1.  **In a paragraph: explain to someone who has never used a terminal
    why ls /no-such-fitting 2\>&1 \> order.txt left the complaint on the
    screen and ls /no-such-fitting \> both.txt 2\>&1 did not, and what
    happens to yes in yes \| head -1. Use file descriptor, redirection,
    pipe and buffer correctly at least once each. zyBooks Ch 9.2
    (optional).**

Each program has numbered output channels called file descriptors: 1 is
normal output, 2 is errors, both wired to your screen by default. A
redirection like \> order.txt rewires one of them, and the shell applies
them left to right. In ls /no-such-fitting 2\>&1 \> order.txt, the 2\>&1
first aims errors at wherever 1 points now — still the screen — and only
then does \> order.txt move 1 to the file, so the error stays on screen
and the file is empty. Reversed, \> both.txt 2\>&1 sends 1 into the file
first, so 2\>&1 then copies that same file into 2 and both land in
both.txt. In yes \| head -1, the pipe is a one-way channel feeding yes’s
output into head’s input via a buffer; head reads one line and quits,
and when yes next writes to a pipe with no reader, the system kills it
with a broken-pipe signal (exit 141 = 128 + 13).

2.  **In two or three sentences: you could read the copier's command
    line but not its table of open files. Is that the right place for
    the Engine to draw the wall between one account and another? What
    would you want a shared machine to show everyone about a process,
    and what should it keep to the process's owner? (There is no answer
    key.)**

A shared machine should make a process’s broad facts public — name,
owner, state, existence — and keep its file descriptors, memory, and
contents readable only to the owner and root. The caveat is that a
command line can itself leak a secret passed as an argument, so the real
lesson is to keep secrets off the command line.

## Sources and help

Anyone or anything that helped you this week — a classmate, a man page,
a Stack Overflow answer, an AI assistant. One line each: who or what,
and what you used it for. This is **not graded and never costs points**;
it is the habit professional engineers keep, and the syllabus asks for
it under *Academic integrity* and *Use of AI tools*.

-   Used Claude to format my responses nicely into markdown

*Nothing to report? Write "None" — that's a perfectly normal week.*

## Time spent

Roughly how long this took, start to finish: 3 hours *No
wrong answer — this just helps calibrate future work orders.*
