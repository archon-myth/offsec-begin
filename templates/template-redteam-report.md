<!-- SYNCED COPY - master lives in ARCHONOS drop/templates/. Do not edit here; edit master and re-sync. -->

---
title: Red-Team / Adversary-Emulation Engagement Report Template (Annotated + Tailorable)
type: reusable red-team engagement-report template
author: S.P.E.A.R.
date: 2026-07-05
purpose: >
  Adversary-emulation / red-team engagement report — organized around the ATT&CK kill-chain and the
  objective ("crown jewel") rather than a flat findings list. Teaching-first: every section carries WHY it
  exists + HOW to fill it + a 1-line example, so it doubles as SAGE learning material AND a working
  deliverable. Distinct from the pentest report (breadth of vulns) — this narrates ONE end-to-end attack
  path against a defended target, mapped to MITRE ATT&CK, with the blue-team detection story alongside.
companion: >
  drop/templates/template-pentest-report.md (breadth-of-findings sibling — use that for vuln-coverage work) ·
  drop/templates/template-vuln-finding.md (atomic finding card, reused for the specific exploited weaknesses) ·
  drop/templates/template-recon-checklist.md (the recon phase this consumes) ·
  drop/templates/template-requirements-offsec.md (PRISM — the RoE/scope + threat model this report consumes)
scope_note: >
  This is the EXECUTION / REPORTING side of an authorized adversary-emulation engagement. It does NOT define
  scope, objective, or authorization — that is PRISM's RoE/SOW template. A signed RoE with an explicit
  objective and ROE constraints (permitted TTPs, no-go actions, deconfliction contacts) is a hard
  precondition assumed already in place. This report presents results; it never grants permission.
variants: LITE (single objective / short assumed-breach) · FULL (multi-phase full-scope emulation)
usage: >
  Copy this file → pick LITE or FULL → delete the grey annotation call-outs → fill the fields → attach
  finding cards for the specific weaknesses exploited. Master lives in ARCHONOS drop/templates/; filled
  instances land in the offsec-begin repo. Authorized engagements only.
---

# Red-Team / Adversary-Emulation Engagement Report — Annotated & Tailorable

> **How to read this file.** Each section carries a grey **annotation block** (`> WHY / HOW / EXAMPLE`).
> The annotation is the *teaching layer* — read it, then delete it in your working copy.
>
> **The one rule that survives every engagement:** a red-team report is a **narrative of a path**, not a
> catalog of holes. The pentest report answers "how many ways in?"; the red-team report answers "could a
> determined adversary reach the crown jewel, how, and would we have seen them?" Tell the story from initial
> access to objective, map every step to ATT&CK, and pair each attacker action with the defender's chance to
> catch it. If the blue-team detection column is empty, the engagement is half-done.
>
> **Authorization boundary.** Everything here was performed under a signed RoE with an explicit objective and
> permitted-TTP constraints (see `drop/templates/template-requirements-offsec.md`). This report *presents*
> the emulation; it is not the authorization and never substitutes for it. No action outside the agreed ROE
> is reported as performed.

---

## 0. Tailoring Guide — LITE vs FULL

> **WHY:** An assumed-breach test against one objective and a full-scope multi-phase emulation do not earn
> the same report. Ceremony beyond the engagement's need buries the attack narrative.
> **HOW:** Pick a variant, fill only what it demands. The attack narrative (§4) and the detection/response
> story (§5) are never optional at any size.
> **EXAMPLE:** An assumed-breach "reach the domain admin from this workstation" → LITE. A full external →
> foothold → domain dominance → exfil objective → FULL.

| Signal | Choose LITE | Choose FULL |
|---|---|---|
| Scope | one objective / assumed-breach start | full external-to-objective, multiple phases |
| Start position | given a foothold | must gain initial access first |
| Blue-team interaction | detection notes only | full purple-team / detection-and-response timeline |
| Duration | days | weeks / multi-phase campaign |

| # | Section | LITE | FULL |
|---|---|:--:|:--:|
| 1 | Executive summary + objective verdict | short | ✔ |
| 2 | Scope, objective & ROE | ✔ | ✔ |
| 3 | Adversary profile & emulation plan | brief | ✔ |
| 4 | Attack narrative (kill-chain) | ✔ | ✔ |
| 5 | Detection & response analysis (blue-team story) | ✔ | ✔ |
| 6 | ATT&CK coverage matrix | ✔ | ✔ |
| 7 | Exploited weaknesses (finding cards) | ✔ | ✔ |
| 8 | Strategic recommendations | ✔ | ✔ |
| 9 | Appendices (IOCs, tooling, timeline) | skip | ✔ |

> **Scaling rule:** the two sections that never shrink are **4 (Attack narrative)** and **5 (Detection &
> response)**. Trim adversary profile and appendices for LITE; never trim the path or the blue-team story.

---

## PART 1 — THE ANNOTATED REPORT (FULL structure)

### 1. Executive Summary + Objective Verdict

> **WHY:** The one section leadership reads. It must answer the question the engagement was bought to answer:
> *was the objective reached, how far did we get, and would we have known?* Business language — no TTP codes.
> **HOW:** State the objective, the verdict (ACHIEVED / PARTIAL / NOT ACHIEVED), the single most important
> path fact, and the single most important detection gap. Quantify in business consequence, not exploit
> mechanics.
> **EXAMPLE:** "Objective (exfiltrate the customer PII store) was ACHIEVED in 3 days from a single phished
> workstation, undetected until exfiltration. The organization can be fully compromised from one careless
> click; the detection gap is lateral movement, not initial access."

| Field | Value |
|---|---|
| Objective (crown jewel) | `[what a real adversary would be after]` |
| **Objective verdict** | `ACHIEVED / PARTIAL / NOT ACHIEVED` |
| Path in one line | `[initial access → … → objective, plain language]` |
| Time to objective | `[from start to crown jewel]` |
| Detected? | `[when, by what — or "undetected until <stage>"]` |
| Single biggest defensive gap | `[the one thing to fix first]` |

### 2. Scope, Objective & ROE

> **WHY:** Defines the authorized game — the objective, the permitted TTPs, the no-go actions, and the
> deconfliction path. This section does not re-derive scope; it references the signed RoE and restates the
> boundaries a reader needs to interpret the narrative.
> **HOW:** Link the RoE/SOW artifact. Restate objective, start position, permitted/forbidden TTPs,
> deconfliction contacts, and the assessment window. Do not reproduce the full RoE — cite it.
> **EXAMPLE:** "Authorized per RoE signed 2026-07-01. Objective: reach `PII-DB01`. Assumed-breach start:
> `WS-2044`. Forbidden: DoS, ransomware simulation, prod data destruction. Deconfliction: SOC lead on call."

| Field | Value |
|---|---|
| Authorization ref (RoE/SOW) | `[link to PRISM RoE artifact — do not reproduce]` |
| Authorizing party | `[name / title]` |
| Objective (crown jewel) | `[the defined goal]` |
| Start position | `[external / assumed-breach host / phishing-only]` |
| Permitted TTPs | `[the agreed technique envelope]` |
| Forbidden actions | `[DoS, destruction, real ransomware, out-of-scope hosts]` |
| Deconfliction / blue-aware? | `[double-blind / SOC-aware purple / contacts]` |
| Assessment window | `[dates / timezone]` |

> **Boundary note:** the RoE, objective definition, and threat model are **PRISM's** artifacts
> (`drop/templates/template-requirements-offsec.md`). This report consumes them; it does not re-specify them.

### 3. Adversary Profile & Emulation Plan

> **WHY:** Red teaming emulates a *specific threat*, not "a hacker." Naming the adversary being emulated
> (a named APT, a ransomware crew, an insider) makes the TTP selection defensible and the results mappable
> to the threat intel the client actually cares about.
> **HOW:** Name the emulated actor / archetype, cite the threat-intel basis, and list the ATT&CK techniques
> you planned to exercise (planned ≠ achieved — §6 records what actually landed).
> **EXAMPLE:** "Emulated FIN-style financially-motivated intrusion: phishing initial access (T1566),
> credential dumping (T1003), lateral movement via SMB (T1021.002), exfil over web (T1567)."

- **Adversary emulated:** `[named APT / archetype / insider]`
- **Threat-intel basis:** `[why this actor is relevant to the client]`
- **Planned technique set:** `[the ATT&CK techniques the plan intended to exercise]`

### 4. Attack Narrative (the kill-chain)

> **WHY:** The heart of the report — the end-to-end story of how the objective was reached. Told as an
> ordered chain of stages so a defender can walk the exact path and see every branch point. This is what
> makes a red-team report actionable in a way a flat vuln list never is.
> **HOW:** One sub-entry per kill-chain stage, in order. Each stage records: what was done, the ATT&CK
> technique ID, the evidence/artifact, and — critically — whether the blue team could have seen it. Map to
> the kill-chain phases below; skip phases that did not occur.
> **EXAMPLE stage:** "**Initial Access (T1566.001):** spearphish with a macro-enabled doc to 3 users; 1
> executed → Cobalt Strike beacon on `WS-2044`. Detection opportunity: EDR flagged the macro spawn but no
> alert was triaged."

| Stage (kill-chain) | Action taken | ATT&CK ID | Evidence | Detection opportunity |
|---|---|---|---|---|
| Initial Access | `[…]` | `[Txxxx]` | `[artifact / screenshot ref]` | `[seen? by what? triaged?]` |
| Execution | `[…]` | `[Txxxx]` | `[…]` | `[…]` |
| Persistence | `[…]` | `[Txxxx]` | `[…]` | `[…]` |
| Privilege Escalation | `[…]` | `[Txxxx]` | `[…]` | `[…]` |
| Defense Evasion | `[…]` | `[Txxxx]` | `[…]` | `[…]` |
| Credential Access | `[…]` | `[Txxxx]` | `[…]` | `[…]` |
| Discovery | `[…]` | `[Txxxx]` | `[…]` | `[…]` |
| Lateral Movement | `[…]` | `[Txxxx]` | `[…]` | `[…]` |
| Collection | `[…]` | `[Txxxx]` | `[…]` | `[…]` |
| C2 | `[…]` | `[Txxxx]` | `[…]` | `[…]` |
| Exfiltration / Impact (objective) | `[…]` | `[Txxxx]` | `[…]` | `[…]` |

> **Narrative discipline:** every stage is reproducible from the evidence, within ROE, and paired with a
> detection read. A stage with no "detection opportunity" entry is an incomplete stage — the blue-team value
> lives in that column.

### 5. Detection & Response Analysis (the blue-team story)

> **WHY:** The unique deliverable of a red-team engagement. The point is not only "could we get in" but
> "would the defenders have caught it, and how fast." This section is what turns an offensive result into a
> defensive improvement — and it is what a pentest report does not have.
> **HOW:** Build the detection timeline: attacker action → was it logged? alerted? triaged? responded to?
> Compute the gaps (dwell time, missed alerts). Name what the SOC saw, ignored, and never had visibility into.
> **EXAMPLE:** "Detected at exfil (day 3), 68h dwell time. EDR logged the initial macro but the alert was
> auto-closed. Lateral movement over SMB produced no telemetry — no visibility, not a missed alert."

| Attacker action | Logged? | Alerted? | Triaged? | Gap type |
|---|---|---|---|---|
| `[stage]` | `[Y/N]` | `[Y/N]` | `[Y/N]` | `[no visibility / missed alert / slow response]` |

- **Dwell time (start → first detection):** `[…]`
- **Missed detection opportunities:** `[alerts that fired but were not actioned]`
- **Visibility gaps:** `[stages with no telemetry at all — the harder problem]`

### 6. ATT&CK Coverage Matrix

> **WHY:** A single-glance map of which techniques were exercised and which were detected, in the shared
> ATT&CK vocabulary the client's detection engineering is organized around. Turns the narrative into a
> heat-map the blue team can prioritize from.
> **HOW:** One row per technique actually exercised (not planned). Mark tactic, technique ID/name, outcome
> (succeeded / blocked), and detection (detected / missed / no-visibility).
> **EXAMPLE row:** `Lateral Movement | T1021.002 SMB/Admin Shares | Succeeded | No visibility`.

| Tactic | Technique (ID + name) | Outcome | Detection |
|---|---|---|---|
| `[tactic]` | `[Txxxx — name]` | `Succeeded / Blocked` | `Detected / Missed / No-visibility` |

### 7. Exploited Weaknesses (finding cards)

> **WHY:** The narrative shows the path; this section captures the specific fixable weaknesses that made each
> step possible, as atomic, trackable cards. This is the overlap with the pentest report — reuse the same
> finding-card format so weaknesses are triaged and retested consistently.
> **HOW:** One **finding card per exploited weakness**, using `drop/templates/template-vuln-finding.md`.
> Keep IDs consistent with PRISM's findings RTM. Summary table first, then the cards.
> **EXAMPLE summary row:** `F-01 | Unconstrained delegation on SRV-DC01 | High | T1558 | Open`.

| ID | Weakness | Severity | ATT&CK | Status |
|---|---|---|---|---|
| F-01 | `[…]` | `[…]` | `[Txxxx]` | Open |

> Insert one finding card per weakness here (atomic unit defined in the vuln-finding template).

### 8. Strategic Recommendations (prioritized)

> **WHY:** Red-team findings are systemic, not point-fixes — the value is in the *detection and architecture*
> improvements, not just patching one host. Prioritize by risk-reduction across the whole kill-chain: closing
> one lateral-movement path often matters more than patching ten vulns.
> **HOW:** One row per recommendation, ordered by defensive leverage. Split into detection improvements
> (close visibility gaps) and hardening (close attack paths). Reference the stages/findings each closes.
> **EXAMPLE:** "P1 — Deploy SMB/lateral-movement telemetry (closes the §5 visibility gap that hid 3 stages)."

| Priority | Recommendation | Type | Closes (stage / finding) | Effort |
|---|---|---|---|---|
| P1 | `[action]` | `detection / hardening` | `[stage 8, F-01]` | `S/M/L` |

> **Sequencing note:** lead with the recommendation that closes the biggest *visibility* gap — an undetected
> path is worse than a detected one. Hardening that removes an attack path outranks patching a single host.

### 9. Appendices (FULL only)

> **WHY:** Park reference detail for the engineer reproducing or hunting against the engagement.
> **HOW:** Add an appendix only when a reader will consult it.

- **A. IOC list** — hashes, C2 domains/IPs, artifact names (for the blue team to hunt/purge).
- **B. Full engagement timeline** — timestamped action log (attacker-side), for correlation with defender logs.
- **C. Tooling inventory** — tool, version, purpose (reproducibility).
- **D. Evidence handling** — capture/storage/encryption/destruction per RoE; chain of custody.
- **E. Deconfliction log** — every blue-team notification / pause / safety call during the engagement.

---

## PART 2 — LITE SKELETON (single objective / assumed-breach)

> **WHY:** For an assumed-breach test against one objective this is the *entire* correct report. Do not
> inflate it — but never drop the attack narrative or the detection story.

```markdown
# <Client> — Red-Team Report (Lite)
**Objective:** <crown jewel> · **Start:** <assumed-breach host> · **Window:** <dates> · **Auth ref:** <RoE link>

## 1. Objective Verdict
- Verdict: <ACHIEVED / PARTIAL / NOT ACHIEVED> · Time to objective: <…> · Detected: <when / undetected>
- Biggest defensive gap: <one line>

## 2. Scope & ROE
- Objective: <…> · Start: <…> · Forbidden: <…> · Authorized by <name> per <RoE ref>.

## 3. Attack Narrative (kill-chain)
| Stage | Action | ATT&CK | Detection opportunity |
|-------|--------|--------|----------------------|
| Initial access | … | Txxxx | seen? triaged? |
| … | … | … | … |
| Objective | … | Txxxx | … |

## 4. Detection & Response
- Dwell time: <…> · Missed alerts: <…> · Visibility gaps: <…>

## 5. Exploited Weaknesses
| ID | Weakness | Severity | ATT&CK | Status |
|----|----------|----------|--------|--------|
| F-01 | … | … | Txxxx | Open |
<insert finding cards>

## 6. Strategic Recommendations
| Priority | Recommendation | Type | Closes |
|----------|----------------|------|--------|
| P1 | … | detection | stage/finding |
```

---

## PART 3 — KILL-CHAIN ↔ ATT&CK QUICK-MAP (reusable sub-artifact)

> **WHY:** Anchors the attack narrative (§4) to the ATT&CK tactic vocabulary so coverage gaps are visible and
> the report maps cleanly to the client's detection engineering. A starter map — the full matrix lives at
> attack.mitre.org.

| Kill-chain stage | ATT&CK tactic | Typical techniques (examples) |
|---|---|---|
| Initial Access | TA0001 | T1566 Phishing · T1190 Exploit public-facing app |
| Execution | TA0002 | T1059 Command/scripting · T1204 User execution |
| Persistence | TA0003 | T1547 Boot/logon autostart · T1053 Scheduled task |
| Privilege Escalation | TA0004 | T1548 Abuse elevation · T1068 Exploit for privesc |
| Defense Evasion | TA0005 | T1070 Indicator removal · T1027 Obfuscation |
| Credential Access | TA0006 | T1003 OS credential dumping · T1558 Kerberos abuse |
| Discovery | TA0007 | T1087 Account discovery · T1018 Remote system discovery |
| Lateral Movement | TA0008 | T1021 Remote services · T1550 Alt auth material |
| Collection | TA0009 | T1005 Data from local system · T1074 Data staged |
| Command & Control | TA0011 | T1071 App-layer protocol · T1573 Encrypted channel |
| Exfiltration | TA0010 | T1567 Exfil over web service · T1041 Exfil over C2 |
| Impact | TA0040 | T1486 Data encrypted · T1485 Data destruction |

> **Boundary note.** This report documents an *authorized* adversary emulation. It consumes scope + objective
> from PRISM (`template-requirements-offsec.md`), recon from `template-recon-checklist.md`, and reuses
> `template-vuln-finding.md` cards for exploited weaknesses. Where the work is breadth-of-vulnerability rather
> than a single objective path, use `template-pentest-report.md` instead.

---

*Template author: S.P.E.A.R. — adversary-emulation reporting.
Consumes scope + objective from PRISM (`template-requirements-offsec.md`); recon from
`template-recon-checklist.md`; reuses finding cards from `template-vuln-finding.md`.
Master lives in ARCHONOS `drop/templates/`; filled instances land in the `offsec-begin` repo.*
