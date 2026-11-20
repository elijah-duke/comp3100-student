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

**Name:** Elijah Duke **Week:** 13 **Work Order No.:** 12

## Milestone 1

**What I did:**

Ran `id` → `uid=1000(ubuntu) gid=1000(ubuntu) groups=…,27(sudo),…`, then
walked my passwd line field by field
(`getent passwd "$USER" | tr ':' '\n' | cat -n`): 1 `ubuntu`, 2 `x`, 3
`1000`, 4 `1000`, 5 `Ubuntu`, 6 `/home/ubuntu`, 7 `/bin/bash`. Cycled
`note.txt` through three states — `chmod 600` → `-rw-------`,
`chmod g+r` → `-rw-r-----` (640), `chmod u-r` → owner read stripped so
`cat` returned *Permission denied*.

**Output or seal:** `~~~ WAX SEAL of the Guild: F3647AD8 ~~~`

**What it means:**

My account is a normal uid-1000 user whose only privileged footing is
membership in `sudo`, and the passwd line is just seven colon-fields
(the `x` being a placeholder for the real hash in `/etc/shadow`). The
`chmod` runs show the three permission triads are independent — removing
owner read locks *me* out even though I own the file.

## Milestone 2

**What I did:**

Listed the stock setuid tools — `-rwsr-xr-x root root /bin/su`,
`…/usr/bin/passwd`, `…/usr/bin/sudo` — and confirmed `sudo id` →
`uid=0(root) gid=0(root)`. Set the bit on my own script (`-rwxr-xr-x` →
`-rwsr-xr-x` after `chmod u+s`), but `./probe.sh` still printed `1000`;
`find "$HOME" -perm -4000` surfaced only `.spare-key`, which `ls -lan`
shows owned by `1849 100` as `-rwsr-xr-x`, and running it printed its
one line while `id -u` stayed `1000`.

**Output or seal:** `~~~ WAX SEAL of the Guild: B3F1D4C2 ~~~`

**What it means:**

The setuid bit only elevates when the *file's owner* is privileged and
the kernel honors it — which it does for compiled root-owned binaries
but **not** for scripts, so my `probe.sh` and the uid-1849 `.spare-key`
both run with my own identity regardless of the `s`.

## Milestone 3

**What I did:**

Tallied the log by field — keys (top: `47 key=apprentice-master`,
`17 key=1849`), doors (`45 BOARD-ROOM` …), and events (`249 PRESENT`,
`6 SESSION-OPEN`, `6 SESSION-CLOSE`). Pulled the 17 `key=1849` lines in
order (first `1851-07-29`, last `1851-10-30`), found the log spans
`1850-01-06` → `1851-10-31`, and swept the whole bench with
`find / -xdev -perm -4000 -type f` → `14` files.

**Output or seal:** `~~~ WAX SEAL of the Guild: 4411D53D ~~~`

**What it means:**

Cut/sort/uniq turns a flat log into frequency tables, and `key=1849` —
my own key — shifts partway through from bare `PRESENT` marks to paired
`SESSION-OPEN`/`SESSION-CLOSE`. The bench carries 14 legitimate setuid
binaries (su, sudo, passwd, mount, etc.), the normal system set plus the
planted `.spare-key`.

## Milestone 4

**What I did:**

Built `open-days.txt` (6 `SESSION-OPEN` dates) and `visit-days.txt` (32
visitor-ledger dates), then `comm` showed all six openings also appear
as visit days (`comm -12` returned the six `1851-09-16 … 10-30`,
`comm -23` nothing). Interleaving IN/OUT/SEEN marks and scanning with
awk, all `6` openings fell inside an open visit (first 11 sorted lines
run `09-16 IN / SEEN / OUT`, `09-18 …`, `09-23 …`, `10-14 IN / SEEN`);
the most-frequent key, `apprentice-master` at `47` lines, landed inside
a visit `0` times.

**Output or seal:** `~~~ WAX SEAL of the Guild: 318D62BA ~~~`

**What it means:**

Joining the Warden's log against the porters' book confirms every one of
my key's sessions is backed by a physical visit, while the busiest key's
47 appearances never coincide with one — its marks (e.g. the `09-16`
SEEN at 09:32, before that day's 16:48 IN) sit outside any logged
presence, the signature of a key used without a body in the room.

> Fewer or more milestones this week? Copy a block above as needed.

## Reflection

1.  **(Prompt 1 from the work order):**

Every process on a Linux system runs as some numbered user — its uid —
and the file /etc/shadow, which holds everyone’s passwords, is owned by
root (uid 0) with a mode so tight that an ordinary user can’t even read
it. So how does passwd let you change your own password, a line inside
that very file? The answer is the setuid bit, a flag in the program’s
mode that says “run this as the file’s owner, not as whoever launched
it.” Because the passwd binary is owned by root and carries that bit,
when you run it the kernel gives the process an effective uid of 0 — the
identity it actually checks permissions against — even though your real
uid is unchanged. That borrowed root authority, lasting only for the
length of the program, is what lets it write a file you yourself may not
read. Now put the same setuid bit on a shell script and nothing happens:
a script isn’t executed directly but handed to an interpreter (/bin/sh),
and the kernel deliberately refuses to raise the effective uid when
launching an interpreter that way, because a script is trivially easy to
tamper with mid-run. The mark is there, but the kernel ignores it — so
the script stays at your own uid and can touch only what you already
could.

2.  **(Prompt 2 from the work order):**

You’d have to bind the key to a person at the moment it’s shown — a PIN
typed at the door, a signature in the porters’ book, or a biometric like
a fingerprint or a photograph of whoever presents it — so the log
records a hand, not just a key. The cost is privacy and friction: every
entry now tracks named individuals and can be subpoenaed, correlated, or
leaked, and people have to stop and authenticate (and can be locked out
when the reader fails or the finger doesn’t match). You trade the
anonymity and convenience of a shared, pocketable token for
accountability you can actually attribute.

## Sources and help

Anyone or anything that helped you this week — a classmate, a man page,
a Stack Overflow answer, an AI assistant. One line each: who or what,
and what you used it for. This is **not graded and never costs points**;
it is the habit professional engineers keep, and the syllabus asks for
it under *Academic integrity* and *Use of AI tools*.

-   Used Claude to format my writing into .md

*Nothing to report? Write "None" — that's a perfectly normal week.*

## Time spent

Roughly how long this took, start to finish: 3 hours *No
wrong answer — this just helps calibrate future work orders.*
