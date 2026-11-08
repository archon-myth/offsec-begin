<!-- SYNCED COPY - master lives in ARCHONOS drop/templates/. Do not edit here; edit master and re-sync. -->

---
title: CTF Challenge Writeup Template (per-challenge, category-tagged — Annotated + Tailorable)
type: reusable CTF-challenge writeup template
author: S.P.E.A.R.
date: 2026-07-05
purpose: >
  Teaching-first writeup template for a SINGLE CTF challenge — web / pwn / crypto / rev / forensics / misc.
  This is the challenge-shaped sibling of the full-box lab writeup: one challenge, one category, one flag.
  Doubles as a learning artifact and a recall feed — the point is not the flag, it is the technique the flag
  taught and the failed paths you walked to reach it.
companion: >
  drop/templates/template-lab-writeup.md (the multi-stage full-box analogue — foothold → privesc → root).
  Use lab-writeup for a THM/HTB machine; use THIS for a discrete CTF challenge scored by points.
variants: single — one challenge per file; batch an event by filing one writeup per solved challenge
usage: >
  Copy this file → delete the annotation call-outs → fill the fields. Keep sections 4 (Approach & Attempts,
  including the FAILED paths), 5 (Flag), and 6 (Key Takeaway) in EVERY writeup — the failed paths are the
  learning, not noise. File finished writeups in your A:\offsec-begin writeups tree, grouped by event/category.
  Authorized CTF targets only.
---

# CTF Challenge Writeup — Annotated & Tailorable

> **How to read this file.** Each section carries a grey **annotation block** (`> WHY / HOW / EXAMPLE`).
> The annotation is the *teaching layer* — read it to understand why the section exists, then delete it in
> your working copy. What remains is the writeup you fill in.
>
> **The one rule that survives every challenge:** a flag you captured but cannot re-derive is a flag you did
> not learn. The writeup is not a scoreboard receipt — it is how the technique moves from *recognized* to
> *fluent* (the Feynman standard: you can use it without thinking AND teach it back). The failed attempts are
> not clutter to trim — they are the map of the dead ends you will recognize faster next time.
>
> **Scope note.** This template is for *authorized* CTF challenges only — public/college/vendor CTF events and
> your own practice sets. A discrete, points-scored challenge belongs here; a multi-stage box with a
> foothold→privesc chain belongs in the lab-writeup template.

---

## 0. Challenge-vs-Box — am I using the right template?

> **WHY:** A CTF challenge and a full box are different shapes. A challenge is usually one artifact (a binary, a
> web endpoint, a cipher, a capture file) yielding one flag; a box is a host you enumerate, foothold, and
> escalate. Using the box template on a challenge buries the one technique under machine ceremony.
> **HOW:** If it is scored by points, tagged with a single category, and yields `flag{...}` from one artifact →
> THIS template. If it is a host with services, a foothold, and privilege escalation → lab-writeup.
> **EXAMPLE:** "picoCTF `Mod 26` (crypto, 10 pts) → CTF challenge. HTB `Blue` (SMB → EternalBlue → SYSTEM) → box."

| Signal | This template (CTF challenge) | Lab-writeup (full box) |
|---|---|---|
| Scoring | points, per-challenge | flags: user + root |
| Artifact | one (binary / endpoint / file / cipher) | a host with multiple services |
| Category | single (web/pwn/crypto/rev/forensics/misc) | multi-stage chain |
| Win | submit `flag{...}` | user.txt + root.txt |

---

## 1. Challenge Metadata

> **WHY:** Metadata makes a writeup searchable and lets you group by event, category, and technique when you
> build a recall set. The category + technique tags are the join key to your spaced-repetition schedule and
> your per-category coverage map (which categories you keep solving vs which you avoid).
> **HOW:** Fill the table. Tags = category + technique + primitive, not just the challenge name. Be honest about
> whether you solved it solo, on a hint, or read the writeup after — it changes the recall weight.
> **EXAMPLE:** `Cookie Monster | web | 200 | picoCTF 2026 | Medium | jwt, none-alg, auth-bypass | solo`.

| Field | Value |
|---|---|
| Challenge name | `[…]` |
| Category | `[web / pwn / crypto / rev / forensics / misc]` |
| Points / difficulty | `[… pts / Easy·Medium·Hard]` |
| Event | `[CTF event name + year, or "practice set"]` |
| Date solved | `[YYYY-MM-DD]` |
| Time spent | `[hh:mm]` |
| Tags | `[category, technique, primitive — e.g. web, jwt, none-alg]` |
| Solved | `[solo / hint / writeup-after — be honest, it sets the recall weight]` |

## 2. Challenge Prompt & Provided Artifacts

> **WHY:** The win condition and the raw material. Recording the exact prompt and what you were given keeps the
> writeup honest about what the challenge actually asked versus what you assumed, and lets future-you re-attempt
> from the same starting line.
> **HOW:** Quote the prompt verbatim. List every provided artifact (file + type/hash), the endpoint/URL, and
> any hint given. Note the flag format if specified.
> **EXAMPLE:** "Prompt: 'The admin cookie holds the key.' Artifact: none. Endpoint: http://chal.ctf:8080. Flag
> format: `picoCTF{...}`."

- **Prompt (verbatim):** `[…]`
- **Provided artifacts:** `[file name + type/size/hash, or "none"]`
- **Endpoint / URL:** `[… or n/a]`
- **Flag format:** `[e.g. flag{...} / picoCTF{...}]`

## 3. Recon & Observation

> **WHY:** Before firing, understand the artifact. For a CTF challenge this is the "what am I actually looking
> at?" step — file type, protections, structure, the one anomaly that hints at the intended vulnerability class.
> Recording the *observation that mattered* trains the instinct that makes the next challenge in this category
> faster.
> **HOW:** Category-appropriate first look — `file`/`strings`/`checksec` for pwn·rev; request inspection /
> source view / cookie decode for web; frequency analysis / format ID for crypto; `binwalk`/`exiftool`/`file`
> for forensics. Record the command AND the observation that redirected you toward the vuln class.
> **EXAMPLE:** "`checksec` → NX enabled, no PIE, no canary → ret2 territory, not a stack-smash-shellcode chal.
> Pivot to ROP." / "JWT decoded → `alg:HS256`, but server accepts unsigned — test `alg:none`."

**3.1 First look (command / method run):**
```
[category-appropriate recon command(s) — file, checksec, strings, request inspection, format ID, binwalk, …]
```

**3.2 Observations that mattered:**

| What I observed | Why it pointed at the vuln class |
|---|---|
| `[e.g. no stack canary]` | `[e.g. overflow → control flow, no canary bypass needed]` |
| `[…]` | `[…]` |

## 4. Approach & Attempts (including the paths that failed)

> **WHY:** THIS is the section that makes the writeup an asset instead of a scoreboard. The failed attempts are
> the map of dead ends — recording them turns wasted time into a pattern you recognize instantly next time, and
> directly addresses the Architect's core friction ("not knowing what I don't know") by naming each dead end.
> Most people delete their failures; that deletes the learning.
> **HOW:** One row per attempt, in order, INCLUDING the ones that failed. For each: the hypothesis, what you
> tried, the result, and what it told you. The winning attempt is just the last row — the failures above it are
> the point.
> **EXAMPLE:** `1 | SQLi in login | ' OR 1=1-- | WAF blocked "OR" | not injection — pivot to auth logic` ·
> `2 | JWT alg confusion | set alg:none, strip sig | accepted → admin | WIN`.

| # | Hypothesis | What I tried | Result | What it told me |
|---|---|---|---|---|
| 1 | `[…]` | `[…]` | `[failed — how]` | `[the redirect it gave me]` |
| 2 | `[…]` | `[…]` | `[failed / partial]` | `[…]` |
| 3 | `[the winning idea]` | `[…]` | `[flag]` | `[why THIS worked]` |

> **Dead-end habit:** over many challenges, the failed rows accumulate into your personal "things I try first
> that usually don't work in category X" list. That list is a real skill — it's what separates the 40-minute
> solve from the 8-minute one.

## 5. Exploitation → Flag

> **WHY:** The clean, reproducible path from artifact to flag — the winning attempt written up so future-you (or
> a teammate) can re-derive it end to end. Capture the *primitive/mechanism*, not just the payload string,
> because the mechanism transfers to the next challenge; the exact payload rarely does.
> **HOW:** State the vulnerability/primitive, the minimal working exploit (sanitized script or step sequence),
> and the captured flag. Keep it to the load-bearing steps — the narrative of getting there lives in §4.
> **EXAMPLE:** "Primitive: JWT `alg:none` signature bypass. Steps: decode → set `alg:none` → clear signature →
> re-encode → replace cookie → request /admin. Flag: `picoCTF{n0_alg_n0_trust_...}`."

- **Vulnerability / primitive:** `[the exploitable mechanism — the transferable part]`
- **Working exploit (sanitized):**
```
[minimal script / step sequence that reproduces the solve — illustrative, load-bearing steps only]
```
- **Flag captured:** `[flag{...}]`

## 6. Key Takeaway / Technique Learned

> **WHY:** The compounding asset. One challenge = one transferable technique; naming it in your own words is
> what moves it from "I solved it once" to "I own this primitive." This is the section SAGE pulls from to build
> recall questions — write it as if teaching it back.
> **HOW:** State the technique in the field's shared vocabulary, then the transferable heuristic in YOUR words —
> the "if I see X, I try Y because Z" reflex. Add 1–2 recall questions whose answer is the technique, not the
> challenge name, and a next-review date for the spaced-repetition schedule.
> **EXAMPLE:** "Technique: JWT algorithm-confusion (`alg:none`). Heuristic: if a JWT app trusts the token's own
> `alg` header, try stripping the signature with `alg:none` before anything fancier. Q: 'A JWT app lets the
> token pick its own algorithm — what's the first bypass to try and why?'"

- **Technique (shared vocabulary):** `[named technique / primitive class]`
- **Transferable heuristic (my words):** "If I see `[signal]`, I try `[technique]` because `[reason]`."
- **Recall Q1:** `[question whose answer is the technique, not the challenge name]`
- **Recall Q2:** `[optional second question]`
- **Next review:** `[date — feed to spaced-repetition schedule]`

## 7. Tools Used

> **WHY:** Builds your personal tool-fluency map over time and speeds the next challenge's setup — over an event
> you see which tools you reach for per category.
> **HOW:** Tool + what you used it for on THIS challenge (not a generic description).
> **EXAMPLE:** `jwt_tool — forged the alg:none token; Burp Repeater — replayed the cookie swap`.

| Tool | Used for (on this challenge) |
|---|---|
| `[…]` | `[…]` |

---

## PART 2 — LITE SKELETON (copy-paste for a quick challenge)

> **WHY:** For an easy challenge this is the *entire* correct output. Do not inflate it — but never drop the
> Attempts (including failures) and Key Takeaway rows. Those two are why you kept the writeup.

```markdown
# <Challenge> — CTF Writeup

**Category / Points / Event:** <web / 200 / picoCTF 2026>  ·  **Date:** <YYYY-MM-DD>  ·  **Tags:** <technique, primitive>  ·  **Solved:** <solo/hint>

## Prompt
- <win condition + flag format, 1 line>

## Recon
- <first look command> → <the observation that mattered>

## Attempts (incl. failures)
1. <hypothesis> → <tried> → <failed: why> → <redirect>
2. <winning idea> → <tried> → flag

## Exploitation → Flag
- Primitive: <mechanism> → Steps (sanitized): <…> → Flag: <flag{...}>

## Key Takeaway
- Technique: <named> — "If I see <signal>, I try <technique> because <reason>."
- Recall Q: <question whose answer is the technique>  ·  Next review: <date>

## Tools
- <tool> — <use>
```

---

## PART 3 — CATEGORY FIRST-LOOK QUICK-MAP (reusable sub-artifact)

> **WHY:** Removes the "where do I even start for a rev challenge?" friction so you actually run recon (§3)
> every time. A starter map of the first move per category — extend it as you build fluency.
> **HOW:** Find the category, run the first-look moves, let the observation redirect you. Not exhaustive — the
> opening move, not the whole solve.

| Category | First-look moves | Common primitive to test |
|---|---|---|
| web | view source · decode cookies/JWT · inspect requests (Burp) · check robots.txt / params | injection · auth/JWT confusion · IDOR · SSRF · deserialization |
| pwn | `file` · `checksec` · `strings` · run it · disassemble | buffer overflow · format string · ret2libc / ROP · use-after-free |
| crypto | identify scheme · frequency analysis · check key/nonce reuse · small-exponent | classical cipher · weak RSA · ECB/CBC flaw · nonce reuse |
| rev | `file` · `strings` · `ltrace`/`strace` · decompile (Ghidra) | hardcoded check · XOR/obfuscation · license/keygen logic |
| forensics | `file` · `binwalk` · `exiftool` · `strings` · packet/memory inspect | embedded data · steganography · pcap extraction · memory artifact |
| misc | read prompt literally · check encoding · scripting/logic · esolang / QR / audio | encoding chains · scripted brute · out-of-band puzzle |

> **Boundary note.** This template documents *authorized CTF practice*. The same skills, applied to a scoped
> paid engagement, graduate to the professional **pentest report** (`drop/templates/template-pentest-report.md`)
> with each issue formalized as a **vuln-finding card** (`drop/templates/template-vuln-finding.md`). A multi-stage
> practice box uses the **lab-writeup** (`drop/templates/template-lab-writeup.md`). A CTF challenge teaches a
> primitive; a report gets someone paid — same rigor, different reader.

---

*Template author: S.P.E.A.R. — offensive-execution tradecraft.
Companions: `drop/templates/template-lab-writeup.md` (full box), `drop/templates/template-pentest-report.md`
(professional engagement). Feeds: the Architect's `A:\offsec-begin` writeups tree + spaced-repetition recall
schedule. Master lives in ARCHONOS `drop/templates/`; filled instances land in the `offsec-begin` repo.*
