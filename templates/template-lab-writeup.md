<!-- SYNCED COPY - master lives in ARCHONOS drop/templates/. Do not edit here; edit master and re-sync. -->

---
title: Lab Writeup Template (CTF / THM / HTB / practice labs â€” Annotated + Tailorable)
type: reusable writeup / lab-log template
author: S.P.E.A.R.
date: 2026-07-03
purpose: >
  Teaching-first writeup template for lab writeups of any kind â€” CTF challenges, TryHackMe rooms,
  Hack The Box machines, and other authorized practice labs.
  Doubles as a learning artifact and a recall feed: every solved box becomes a TTP log mapped to
  MITRE ATT&CK, plus a metacognition record of where you got stuck and how you unblocked.
companion: drop/templates/template-pentest-report.md (the professional-engagement analogue of this lab log)
variants: LITE (quick single-flag room) Â· FULL (multi-stage box, foothold â†’ privesc â†’ root)
usage: >
  Copy this file â†’ pick LITE or FULL â†’ delete the annotation call-outs â†’ fill the fields.
  Keep the "Lessons / TTPs learned" and "Stuck / Unblocked" sections in EVERY writeup â€” they are the
  point, not an afterthought. File finished writeups in your A:\offsec-begin writeups tree.
---

# Lab Writeup (CTF / THM / HTB / practice labs) â€” Annotated & Tailorable

> **How to read this file.** Each section carries a grey **annotation block** (`> WHY / HOW / EXAMPLE`).
> The annotation is the *teaching layer* â€” read it to understand why the section exists, then delete it in
> your working copy. What remains is the writeup you fill in.
>
> **The one rule that survives every box:** a flag you captured but cannot re-explain is a flag you did not
> learn. The writeup is not a trophy log â€” it is how the technique moves from *recognized* to *fluent* (the
> Feynman standard: you know it when you can use it without thinking AND teach it back). Understand before you
> fire; document after you land.
>
> **Legality note.** This template is for *authorized* practice targets only â€” CTF platforms, THM rooms, HTB
> machines, and your own lab. The same discipline scales up to a real engagement, but a real engagement runs
> under a signed RoE (see the pentest-report template). No writeup of an unauthorized target belongs here.

---

## 0. Tailoring Guide â€” LITE vs FULL

> **WHY:** A single-flag beginner room and a two-stage root box do not earn the same document. Ceremony beyond
> the box's need wastes the recall time the writeup is supposed to buy back.
> **HOW:** Pick a variant from the selector, then fill only what that variant demands.
> **EXAMPLE:** A THM Pre-Security walkthrough room â†’ LITE. An HTB box with a web foothold + kernel privesc â†’ FULL.

| Signal | Choose LITE | Choose FULL |
|---|---|---|
| Flags | 1 (or a guided walkthrough) | 2+ (user + root / multi-stage) |
| Privesc | none / trivial | real escalation path |
| Attack surface | one service | multiple services / pivots |
| Learning value | one new technique | a chain worth mapping to ATT&CK |
| Platform | THM guided room, easy CTF | HTB box, hard CTF, exam-style |

| # | Section | LITE | FULL |
|---|---|:--:|:--:|
| 1 | Metadata | âœ” | âœ” |
| 2 | Objective | 1 line | âœ” |
| 3 | Recon & enumeration | âœ” | âœ” |
| 4 | Foothold / initial access | âœ” | âœ” |
| 5 | Privilege escalation | skip | âœ” |
| 6 | Flags captured | âœ” | âœ” |
| 7 | Tools used | âœ” | âœ” |
| 8 | Lessons / TTPs learned (ATT&CK) | âœ” | âœ” |
| 9 | Stuck / Unblocked (metacognition) | âœ” | âœ” |
| 10 | Recall hooks (spaced repetition) | âœ” | âœ” |

> **Scaling rule:** start LITE. The two sections you never cut â€” even on the easiest room â€” are **8 (Lessons/TTPs)**
> and **9 (Stuck/Unblocked)**. Those are the compounding assets; sections 3â€“6 are the disposable narrative.

---

## PART 1 â€” THE ANNOTATED WRITEUP (FULL structure)

### 1. Box / Room Metadata

> **WHY:** Metadata makes a writeup searchable and lets you group by platform, difficulty, and tag when you
> build a recall set later. The tags are the join key between this writeup and your spaced-repetition schedule.
> **HOW:** Fill the table. Tags = technique + service + ATT&CK-ish label, not just the box name.
> **EXAMPLE:** `Blue | TryHackBox | Easy | 2026-07-03 | Windows | smb, eternalblue, ms17-010, initial-access`.

| Field | Value |
|---|---|
| Name | `<box / room name>` |
| Platform | `TryHackMe / HackTheBox / PicoCTF / â€¦` |
| URL | `<link>` |
| Difficulty | `Easy / Medium / Hard / Insane` |
| OS | `Linux / Windows / other` |
| Date completed | `YYYY-MM-DD` |
| Time spent | `<hh:mm>` |
| Tags | `<service>, <technique>, <ATT&CK label>, â€¦` |
| Completed with help? | `solo / hint / walkthrough â€” be honest, it changes the recall weight` |

### 2. Objective

> **WHY:** States the win condition before the narrative â€” the "user requirement" of the box. Keeps the writeup
> honest about what you were actually trying to achieve versus what you stumbled into.
> **HOW:** One or two lines. Name the flags and the intended path if known.
> **EXAMPLE:** "Capture user.txt and root.txt. Intended path: exploit a vulnerable web upload â†’ shell â†’ SUID privesc."

- Win condition: `<flags to capture / goal>`
- Intended path (if stated / suspected): `<one line>`

### 3. Recon & Enumeration

> **WHY:** Enumeration is 80% of the box. This section is where you train the instinct to map attack surface
> *before* firing â€” the "understand before execute" discipline. Record the commands verbatim so future-you can
> re-run them, and record the *reasoning* (why this port mattered), not just the output.
> **HOW:** Port scan first, then service-by-service enumeration. For each open port: service, version, and the
> one observation that mattered. Keep raw output trimmed to the load-bearing lines.
> **EXAMPLE:** "nmap â†’ 80/tcp Apache 2.4.49 â†’ version is CVE-2021-41773 territory â†’ pivot recon to path traversal."

**3.1 Host discovery / port scan**
```bash
# Command run (fill in your actual invocation):
nmap -sC -sV -oN nmap-initial.txt <target-ip>
```

**3.2 Open ports & services**

| Port | Proto | Service | Version | Observation that mattered |
|---|---|---|---|---|
| 22 | tcp | ssh | `<ver>` | `<e.g. key-only, note for later>` |
| 80 | tcp | http | `<ver>` | `<e.g. version â†’ known CVE>` |

**3.3 Service enumeration notes**
> Per interesting service: what you enumerated (dirs, shares, versions, default creds), the tool, and the finding.
- `<service>`: `<tool + command>` â†’ `<finding>`

### 4. Foothold / Initial Access

> **WHY:** The transition from "outside" to "inside" â€” the first real compromise. Capture the *mechanism*, not
> just the exploit name, because the mechanism is what transfers to the next box; the specific CVE rarely does.
> **HOW:** Vulnerability â†’ how you confirmed it â†’ how you exploited it â†’ what access you landed. Include the
> sanitized command/payload shape (illustrative, not a copy-paste weapon).
> **EXAMPLE:** "LFI in `?page=` confirmed by reading /etc/passwd â†’ log-poisoning via User-Agent â†’ PHP RCE â†’ www-data shell."

- **Vulnerability:** `<what was wrong + where>`
- **Confirmation:** `<how you proved it before exploiting â€” the mental model>`
- **Exploitation (sanitized):** `<command / payload shape, illustrative>`
- **Access gained:** `<user / service account + shell type>`
- **Stabilization:** `<e.g. upgrade to a TTY: python3 -c 'pty.spawn("/bin/bash")'>`

### 5. Privilege Escalation (FULL only)

> **WHY:** Privesc is a distinct skill from foothold and deserves its own record. This is usually where the
> highest-value TTP of the box lives. Log the enumeration step that *revealed* the path â€” that's the repeatable
> skill; the exploit is downstream.
> **HOW:** Enumeration (what you ran: LinPEAS / WinPEAS / manual) â†’ the misconfiguration or vuln found â†’ the
> escalation technique â†’ proof of elevated access.
> **EXAMPLE:** "LinPEAS flagged a SUID `find` â†’ `find . -exec /bin/sh -p \; -quit` â†’ root shell â†’ read root.txt."

- **Enumeration:** `<tool / manual checks run>`
- **Weakness found:** `<SUID / sudo rule / cron / kernel / cred reuse / â€¦>`
- **Escalation (sanitized):** `<technique + illustrative command>`
- **Proof:** `<id output / hostname / flag path>`

### 6. Flags Captured

> **WHY:** The receipt. Also a completeness check â€” did you actually finish, or leave root on the table?
> **HOW:** One row per flag. Note where it lived (path) as a recall cue.
> **EXAMPLE:** `user.txt | /home/bob/user.txt | after www-data â†’ bob via reused password`.

| Flag | Location | Obtained via |
|---|---|---|
| `user.txt` | `<path>` | `<step>` |
| `root.txt` | `<path>` | `<step>` |

### 7. Tools Used

> **WHY:** Builds your personal tool fluency map over time and speeds the next box's setup.
> **HOW:** Tool + what you used it for on THIS box (not a generic description).
> **EXAMPLE:** `gobuster â€” dir brute-force found /admin; ffuf â€” parameter fuzz found ?debug=`.

| Tool | Used for (on this box) |
|---|---|
| `nmap` | `<enumeration>` |
| `<tool>` | `<purpose>` |

### 8. Lessons / TTPs Learned  âŸ¶ MITRE ATT&CK

> **WHY:** This is the section that makes the writeup an *asset* instead of a diary. Every technique you used
> maps to a MITRE ATT&CK tactic/technique â€” mapping it (a) forces you to name what you actually did in the
> field's shared vocabulary, and (b) lets you see coverage gaps across all your boxes over time. This is also
> the section SAGE pulls from to build Socratic recall questions, so write each row as if teaching it back.
> **HOW:** One row per distinct technique. Map to the ATT&CK tactic + technique ID. "What to remember" = the
> transferable heuristic, in your own words â€” the thing that makes the NEXT box faster.
> **EXAMPLE:** `Log poisoning â†’ RCE | Initial Access / Execution | T1190 Exploit Public-Facing App | "If there's LFI + a log you can write to (User-Agent), you often have RCE."`

| Technique used | ATT&CK tactic | ATT&CK technique (ID) | What to remember next time (your words) |
|---|---|---|---|
| `<e.g. SMB enum>` | `Reconnaissance` | `T1046 Network Service Discovery` | `<transferable heuristic>` |
| `<e.g. SUID abuse>` | `Privilege Escalation` | `T1548.001 Setuid/Setgid` | `<transferable heuristic>` |

> **Coverage habit:** over many writeups, tally which ATT&CK tactics you keep hitting (recon, foothold) versus
> which you rarely touch (persistence, lateral movement, exfil). The thin columns are your next study targets â€”
> the writeup corpus becomes a self-diagnosing curriculum.

### 9. Where I Got Stuck / How I Unblocked  (metacognition)

> **WHY:** This is the highest-leverage section for a learner and the one most people skip. The *content* of a
> box fades; the *pattern of how you get unstuck* is a skill you carry to every future target. Recording it
> turns friction into a reusable playbook and â€” per the OffSec mindset profile â€” directly addresses the
> Architect's core friction point ("not knowing what I don't know") by converting each unknown into a named,
> filed step.
> **HOW:** For each wall you hit: what stopped you, how long, what actually broke it open, and â€” critically â€”
> what you'd check EARLIER next time. Be honest about hints/walkthroughs; a box you needed help on is a higher
> recall priority, not a mark of shame.
> **EXAMPLE:** "Stuck 40 min on privesc â€” kept trying kernel exploits. Unblocked by re-reading LinPEAS: a
> writable cron I'd scrolled past. Next time: read the WHOLE enum output before choosing a path."

| # | Where I got stuck | ~Time lost | What unblocked it | What I'll check earlier next time |
|---|---|---|---|---|
| 1 | `<the wall>` | `<mm>` | `<the key insight / resource>` | `<the earlier check>` |

> **Unblock strategies I used (self-audit):** stepped away & returned Â· re-read enumeration output Â· searched a
> writeup/hint Â· pattern-matched a previous box Â· brute-forced through. Note which one worked â€” over time you
> learn your own most-reliable unstick move.

### 10. Recall Hooks (spaced repetition feed)

> **WHY:** A writeup you never revisit decays like any unused skill (the OffSec profile is explicit: retention
> is tied to usage frequency). These hooks are the seeds SAGE/your review schedule turns into recall prompts.
> **HOW:** Write 2â€“4 questions this box should be able to answer for you in a month, plus a one-line "if I see X,
> I do Y" reflex. Phrase questions so the answer is the technique, not the box name.
> **EXAMPLE Q:** "You have LFI and can control a log file's contents â€” what's the escalation and why?" â†’ log poisoning â†’ RCE.

- Q1: `<question whose answer is a transferable technique>`
- Q2: `<question>`
- Reflex: "If I see `<signal>`, I try `<technique>` because `<reason>`."
- Next review: `<date â€” feed to spaced-repetition schedule>`

---

## PART 2 â€” LITE SKELETON (copy-paste for a quick room)

> **WHY:** For a single-flag walkthrough room this is the *entire* correct output. Do not inflate it into a FULL
> writeup â€” but do NOT drop the Lessons and Stuck rows, even here. Those two lines are why you kept the room.

```markdown
# <Room> â€” Lite Writeup

**Platform / Difficulty / Date:** <THM / Easy / YYYY-MM-DD>  Â·  **Tags:** <service, technique>

## Objective
- <win condition, 1 line>

## Recon
- nmap â†’ <open ports + the one that mattered>
- enum â†’ <tool â†’ finding>

## Foothold
- Vuln: <what> â†’ Exploited (sanitized): <payload shape> â†’ Access: <user/shell>

## Flag(s)
- <flag> @ <path>

## Lessons / TTPs (ATT&CK)
- <technique> â†’ <ATT&CK tactic / Txxxx> â†’ remember: <heuristic in my words>

## Stuck / Unblocked
- Stuck on <x> (~<mm>) â†’ unblocked by <y> â†’ next time check <z> earlier.

## Recall
- Q: <question whose answer is the technique>  Â·  Next review: <date>
```

---

## PART 3 â€” MITRE ATT&CK QUICK-MAP (reusable sub-artifact)

> **WHY:** Removes the friction of "which tactic was that?" so you actually fill Section 8 every time instead of
> skipping it. This is a starter map of the tactics a THM/HTB box most commonly exercises â€” extend it as you go.
> **HOW:** Find what you did, grab the tactic + technique ID, drop it into the Section 8 table. Confirm IDs at
> attack.mitre.org when a box does something off this list.

| What you did on the box | ATT&CK tactic | Common technique (ID) |
|---|---|---|
| Port/service scan, version detection | Reconnaissance / Discovery | T1046 Network Service Discovery |
| Directory / vhost brute-force, param fuzz | Reconnaissance | T1595 Active Scanning |
| Default / guessed / reused creds | Initial Access / Credential Access | T1078 Valid Accounts Â· T1110 Brute Force |
| Exploit a public-facing web app / service | Initial Access | T1190 Exploit Public-Facing Application |
| Web shell / reverse shell upload | Execution / Persistence | T1505.003 Web Shell |
| Command execution via injection | Execution | T1059 Command & Scripting Interpreter |
| SUID / sudo / cron misconfig abuse | Privilege Escalation | T1548 Abuse Elevation Control Mechanism |
| Kernel / service exploit for root | Privilege Escalation | T1068 Exploitation for Privilege Escalation |
| Dump / reuse creds, hashes, keys | Credential Access | T1003 OS Credential Dumping Â· T1552 Unsecured Credentials |
| Enumerate users, groups, files, network | Discovery | T1087 Account Discovery Â· T1083 File and Directory Discovery |
| Pivot to another host / user | Lateral Movement | T1021 Remote Services |
| Read the flag / stage data out | Collection / Exfiltration | T1005 Data from Local System |

> **Boundary note.** This template documents *authorized practice*. When the same skill graduates to a paid,
> scoped engagement, the write-once artifact becomes the professional **pentest report**
> (`drop/templates/template-pentest-report.md`), each finding formalized as a
> **vuln-finding card** (`drop/templates/template-vuln-finding.md`) with a CVSS score and a client-facing
> remediation. A lab writeup teaches you; a report gets someone paid â€” same rigor, different reader.

---

*Template author: S.P.E.A.R. â€” offensive-execution tradecraft.
Companion (professional side): `drop/templates/template-pentest-report.md`.
Feeds: the Architect's `A:\offsec-begin` writeups tree + spaced-repetition recall schedule.*

