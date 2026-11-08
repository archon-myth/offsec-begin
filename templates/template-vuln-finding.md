
---
title: Single Vulnerability Finding Card (Annotated + Tailorable)
type: reusable finding-card template
author: S.P.E.A.R.
date: 2026-07-03
purpose: >
  The atomic, reusable unit of a penetration-test report â€” one vulnerability, fully described so a client
  engineer can reproduce, triage, and fix it independently. Teaching-first: every field carries WHY it exists +
  HOW to fill it + a 1-line example, and a fully filled illustrative finding (IDOR) is included at the end.
companion: drop/templates/template-pentest-report.md (the report these cards are assembled into)
scope_note: >
  Reproduction / PoC content is SANITIZED and ILLUSTRATIVE only â€” shape-of-attack, not a live weapon against a
  real target. A finding card documents a vulnerability found under signed authorization; it is never a payload
  library. Any card describing a real system assumes that system was in an authorized RoE scope.
usage: Copy one card block per finding â†’ fill fields â†’ order by severity inside the report's Â§4.
---

# Vulnerability Finding Card â€” Annotated & Tailorable

> **How to read this file.** Each field carries a grey **annotation block** (`> WHY / HOW / EXAMPLE`).
> The annotation is the *teaching layer* â€” read it to understand the field, then delete it in your working copy.
> What remains is the card you fill in. One card = one vulnerability. Part 2 is the blank card to copy; Part 3 is
> a fully filled example so you can see the standard before you write your own.
>
> **The one rule that survives every finding:** a finding an engineer cannot reproduce from your card is a
> finding they will not fix. The card's job is to make the vulnerability *undeniable and actionable* â€” clear
> enough to reproduce, scored so it can be triaged, and paired with a fix so it can be closed.
>
> **Sanitization rule.** Reproduction steps and PoC are illustrative â€” enough to prove and reproduce the issue,
> never a copy-paste weapon aimed at a live third party. Redact real tokens, real PII, and real internal hosts.
> The card proves the vulnerability; it does not arm the reader against someone else.

---

## PART 1 â€” THE ANNOTATED FIELD CATALOG

### Finding ID
> **WHY:** The stable handle every other artifact references â€” the report summary table, the remediation roadmap,
> PRISM's findings RTM, and the retest log all key off this ID. It must be unique and never reused.
> **HOW:** `F-NN` sequential within an engagement (optionally `F-<AREA>-NN` for large scopes, e.g. `F-WEB-01`).
> **EXAMPLE:** `F-01`.

`<F-NN>`

### Title
> **WHY:** A one-line, human-readable label that says *what* is wrong and *where* â€” scannable in a summary table.
> **HOW:** `<Vulnerability class> on/in <asset>`. Specific, not generic. No CVE jargon in the title itself.
> **EXAMPLE:** "Insecure Direct Object Reference (IDOR) on document API."

`<title>`

### Severity + CVSS v3.1 vector + score
> **WHY:** Turns "this feels bad" into a defensible, auditable rating. The **vector string** is mandatory â€” the
> number alone is not reproducible, but the vector shows exactly how you reached it and lets the client recompute.
> **HOW:** Build the v3.1 base vector (metrics below), compute the base score, map to a severity band (see the
> report template's severity table). State all three: band, vector, score.
> **EXAMPLE:** "High â€” `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N` â€” 6.5" (adjust to your actual metrics).

| Field | Value |
|---|---|
| Severity band | `Critical / High / Medium / Low / Informational` |
| CVSS v3.1 vector | `CVSS:3.1/AV:_/AC:_/PR:_/UI:_/S:_/C:_/I:_/A:_` |
| CVSS base score | `<0.0â€“10.0>` |

> **CVSS v3.1 base metrics cheat-sheet (fill the vector from these):**
> - **AV** Attack Vector: `N`etwork / `A`djacent / `L`ocal / `P`hysical
> - **AC** Attack Complexity: `L`ow / `H`igh
> - **PR** Privileges Required: `N`one / `L`ow / `H`igh
> - **UI** User Interaction: `N`one / `R`equired
> - **S** Scope: `U`nchanged / `C`hanged
> - **C / I / A** Confidentiality / Integrity / Availability impact: `H`igh / `L`ow / `N`one

### Affected asset / endpoint
> **WHY:** Pinpoints *where* to fix. A vulnerability class without a concrete location is not actionable.
> **HOW:** URL/endpoint, host/IP, parameter, service+version, and the account/role context if relevant.
> **EXAMPLE:** "`GET /api/docs/{id}` on `app.example.com`; parameter `id`; any authenticated low-priv user."

| Field | Value |
|---|---|
| Endpoint / component | `<URL / path / function>` |
| Host / service | `<host / IP / service + version>` |
| Parameter / input | `<the injectable / manipulable input>` |
| Access context | `<unauth / authenticated role required>` |

### Category (OWASP / CWE)
> **WHY:** Classifies the finding into the shared taxonomy so the client can map it to their own standards and so
> systemic patterns surface across findings. CWE is the precise root-cause class; OWASP Top 10 is the business
> grouping.
> **HOW:** Give the CWE ID + name and the OWASP Top 10 (2021) category.
> **EXAMPLE:** "CWE-639 Authorization Bypass Through User-Controlled Key; OWASP A01:2021 Broken Access Control."

| Field | Value |
|---|---|
| CWE | `CWE-___ <name>` |
| OWASP Top 10 (2021) | `A__:2021 <category>` |

### Description
> **WHY:** The technical explanation of the flaw and its root cause â€” what the mechanism is, not just its name.
> This is where you demonstrate you *understand* the vulnerability, which is what makes the fix advice credible.
> **HOW:** 2â€“4 sentences: what the flaw is, why it exists (root cause), and what an attacker can do with it.
> **EXAMPLE:** "The document API returns any record by numeric ID without checking ownership. Authorization is
> enforced only in the UI, not server-side, so any authenticated user can read another tenant's documents."

`<description>`

### Reproduction steps / Proof of Concept (SANITIZED)
> **WHY:** The undeniable evidence â€” how the client's own engineer confirms the issue exists. Reproducibility is
> what separates a finding from an opinion. Keep it illustrative: enough to reproduce, redacted of real secrets.
> **HOW:** Numbered steps from a known state to the observable proof. Show the request/response *shape* with real
> tokens, PII, and internal hostnames redacted. Attach evidence references (screenshot/log IDs), not raw dumps.
> **EXAMPLE:** "1. Log in as user A. 2. Request `GET /api/docs/1002` (a doc owned by user B). 3. Observe B's
> document returned with HTTP 200 instead of 403."

```
1. <precondition / starting state>
2. <action â€” sanitized request shape>
   e.g.  GET /api/docs/1002  Authorization: Bearer <REDACTED-user-A-token>
3. <observable result that proves the flaw>
   e.g.  HTTP/1.1 200 OK  â†’ returns user B's document (expected: 403 Forbidden)
Evidence: <screenshot ref / log entry ID â€” no raw PII in the report body>
```

> **Sanitization checklist:** real credentials â†’ `<REDACTED>`; real PII in responses â†’ masked; internal hostnames
> â†’ generic; destructive steps â†’ described, not executed. The card proves the bug; it does not weaponize it.

### Impact (business + technical)
> **WHY:** Two readers, two languages. The executive needs the business consequence; the engineer needs the
> technical reach. Splitting them means neither has to translate the other's section.
> **HOW:** One line business impact (what the organization loses) + one line technical impact (what the attacker
> gains). Tie business impact to data/compliance/availability/reputation.
> **EXAMPLE (business):** "Any customer's confidential documents are exposed â€” a data-breach and PDPA/GDPR
> reporting event." **(technical):** "Full read access to all tenants' document records via ID enumeration."

| Lens | Impact |
|---|---|
| Business | `<what the org loses â€” data / money / compliance / reputation / uptime>` |
| Technical | `<what the attacker gains â€” access / data / control>` |

### Likelihood
> **WHY:** Impact alone does not equal risk â€” a catastrophic bug behind three unlikely preconditions ranks below
> a trivially exploitable moderate one. Likelihood tempers severity into real-world priority.
> **HOW:** Rate `High / Medium / Low` with a one-line justification: exploit complexity, preconditions, whether a
> public exploit exists, and how exposed the asset is (internet-facing vs internal).
> **EXAMPLE:** "High â€” no special tooling, single authenticated account, IDs are sequential and enumerable."

`<High / Medium / Low>` â€” `<one-line justification>`

### Recommended remediation
> **WHY:** The finding's reason to exist â€” an issue reported without a fix is half a deliverable. This is what the
> client actually implements and what the retest verifies.
> **HOW:** State the concrete fix (the control, not just "fix the bug"), plus any defense-in-depth follow-ups.
> Prefer the *need* ("enforce server-side object-level authorization") over prescribing one library.
> **EXAMPLE:** "Enforce server-side object-level authorization on every document fetch: verify the requesting
> user owns/has grant to the record before returning it. Defense-in-depth: use non-sequential IDs (UUIDs)."

- **Primary fix:** `<the control that closes the root cause>`
- **Defense-in-depth:** `<secondary hardening>`
- **Maps to security requirement:** `<SR-id from PRISM's threat model, if one exists>`

### References
> **WHY:** Grounds the finding in authoritative sources so the client can verify and learn, and so the technique
> is traceable to real-world adversary behavior (ATT&CK).
> **HOW:** Link the CWE, any relevant CVE, the OWASP WSTG test ID, and the MITRE ATT&CK technique.
> **EXAMPLE:** "CWE-639; OWASP WSTG-ATHZ-04; MITRE ATT&CK T1190 Exploit Public-Facing Application."

| Type | Reference |
|---|---|
| CWE | `CWE-___` |
| CVE (if applicable) | `CVE-____-_____` |
| OWASP WSTG test | `WSTG-____-__` |
| MITRE ATT&CK | `T____ <technique>` |

### Status / Retest
> **WHY:** Tracks the finding through its lifecycle to closure â€” and closure means *verified*, not *claimed*. This
> is the field that feeds PRISM's findings RTM and prevents an unretested "fixed" from becoming an orphan.
> **HOW:** Status = `Open / Remediating / Retest-Pending / Closed / Risk-Accepted`. On retest, record verdict +
> date + evidence. Only PASS-with-evidence closes a finding.
> **EXAMPLE:** "Closed â€” retest PASS 2026-07-20; cross-tenant probe now returns 403; no regression to legit access."

| Field | Value |
|---|---|
| Status | `Open / Remediating / Retest-Pending / Closed / Risk-Accepted` |
| Retest verdict | `PASS / FAIL / PARTIAL / â€”` |
| Retest date | `<date>` |
| Retest evidence | `<what confirmed the fix held>` |

---

## PART 2 â€” BLANK CARD (copy one block per finding)

```markdown
### [F-NN] <Title>

| | |
|---|---|
| **Severity** | <band> â€” CVSS:3.1/AV:_/AC:_/PR:_/UI:_/S:_/C:_/I:_/A:_ (<score>) |
| **Affected asset** | <endpoint / host / parameter / access context> |
| **Category** | CWE-___ <name> Â· OWASP A__:2021 <category> |
| **Likelihood** | <High/Med/Low> â€” <justification> |
| **Status** | <Open/Remediating/Retest-Pending/Closed> |

**Description.** <what the flaw is + root cause + what it enables>

**Reproduction (sanitized).**
```
1. <precondition>
2. <sanitized request/action shape â€” REDACT real tokens/PII>
3. <observable proof>
Evidence: <ref>
```

**Impact.** Business: <consequence>. Technical: <attacker gain>.

**Remediation.** Primary: <control>. Defense-in-depth: <hardening>. Maps to: <SR-id>.

**References.** CWE-___ Â· CVE-____-_____ Â· WSTG-____-__ Â· MITRE ATT&CK T____.

**Retest.** <verdict> <date> â€” <evidence>.
```

---

## PART 3 â€” FILLED EXAMPLE (illustrative â€” IDOR)

> **WHY:** A worked example is the fastest way to internalize the standard â€” this is the card a graded report
> would accept. The target below is fictional; the values are illustrative and sanitized. Use it as the bar for
> your own cards, not as a real vulnerability record.

### [F-01] Insecure Direct Object Reference (IDOR) on document API

| | |
|---|---|
| **Severity** | High â€” CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N (6.5) |
| **Affected asset** | `GET /api/docs/{id}` on `app.example.com`; param `id`; any authenticated low-priv user |
| **Category** | CWE-639 Authorization Bypass Through User-Controlled Key Â· OWASP A01:2021 Broken Access Control |
| **Likelihood** | High â€” single authenticated account, no special tooling, IDs are sequential and enumerable |
| **Status** | Open |

**Description.** The document API returns any record by its numeric `id` without verifying that the requesting
user owns or has been granted access to it. Authorization is enforced only in the front-end UI, not server-side.
As a result, any authenticated user can retrieve other tenants' documents by changing the `id` value â€” and
because IDs are sequential, the entire document store can be enumerated.

**Reproduction (sanitized).**
```
1. Authenticate as low-privilege user A; note A owns document id 1001.
2. Request a document A does not own:
   GET /api/docs/1002  HTTP/1.1
   Host: app.example.com
   Authorization: Bearer <REDACTED-user-A-token>
3. Server responds 200 OK and returns user B's document (expected: 403 Forbidden).
   Incrementing/decrementing id enumerates further records across tenants.
Evidence: screenshot EV-01 (response body PII masked), proxy log entry #4471.
```

**Impact.** Business: any customer's confidential documents are exposed â€” a data-breach event with PDPA/GDPR
reporting obligations and direct reputational/customer-trust damage. Technical: full read access to all tenants'
document records via trivial ID enumeration; no privilege escalation or tooling required.

**Remediation.** Primary: enforce server-side object-level authorization on every document fetch â€” verify the
authenticated principal owns or holds an explicit grant to the requested record before returning it; return 403
otherwise. Defense-in-depth: replace sequential integer IDs with non-guessable identifiers (UUIDv4); add rate-
limiting + alerting on sequential-ID access patterns to detect enumeration. Maps to: SR-AUTHZ-01 (object-level
authorization) from the engagement threat model.

**References.** CWE-639 Â· OWASP WSTG-ATHZ-04 (Testing for IDOR) Â· OWASP A01:2021 Â· MITRE ATT&CK T1190
(Exploit Public-Facing Application).

**Retest.** â€” (pending) â€” on fix, confirm cross-tenant `id` probe returns 403 and legitimate own-document
access still returns 200 (no regression), then set Status = Closed with PASS + date + evidence.

---

*Template author: S.P.E.A.R. â€” offensive-execution finding unit.
Assembled into: `drop/templates/template-pentest-report.md` (Â§4 Findings).
Tracked to closure via PRISM's findings RTM: `drop/templates/template-requirements-offsec.md` (Â§6).*

