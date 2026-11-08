
---
title: Recon & Enumeration Checklist Template (WSTG-informed — Annotated + Tailorable)
type: reusable recon / enumeration methodology checklist
author: S.P.E.A.R.
date: 2026-07-05
purpose: >
  A repeatable, checkbox-driven enumeration methodology for the recon phase of any authorized engagement or
  practice target. OWASP WSTG-informed for the web layer, with passive/active/service phases around it. The
  point is coverage discipline — enumeration is 80% of the box, and the flags you miss are almost always in
  the service you did not enumerate. This is the pre-exploitation companion to the writeup templates.
companion: >
  drop/templates/template-lab-writeup.md (the writeup this feeds — recon findings here become §3 there).
  drop/templates/template-ctf-writeup.md (discrete challenge). drop/templates/template-pentest-report.md
  (professional engagement).
variants: LITE (single-host CTF/box quick sweep) · FULL (multi-host / broad-scope engagement)
usage: >
  Copy this file → delete the annotation call-outs → work the checkboxes top to bottom, filling the findings
  capture area as you go. Do NOT skip a phase because you found something early — the point is coverage, not
  first-blood. Feed the "Notes to next phase" block into your writeup's recon section. Authorized targets only.
---

# Recon & Enumeration Checklist — Annotated & Tailorable

> **How to read this file.** Each phase carries a grey **annotation block** (`> WHY / HOW / EXAMPLE`).
> The annotation is the *teaching layer* — read it to understand why the phase exists, then delete it in your
> working copy. What remains is the checklist you work.
>
> **The one rule that survives every target:** the vulnerability is almost never in the service you scanned —
> it is in the one you *didn't*. Enumeration is 80% of the engagement; the discipline that wins is coverage,
> not cleverness. Work the whole checklist before you choose an attack path. Understand the surface, then fire.
>
> **Scope & legality note.** Recon runs only against authorized targets and only within the agreed scope. For a
> paid engagement this means inside the signed RoE / scope statement — out-of-scope hosts are not touched even
> if reachable. For practice, this means CTF platforms, THM/HTB, and your own lab. Record the scope boundary
> below before the first scan.

---

## 0. Engagement Header & Scope Guard

> **WHY:** Recon that drifts out of scope is the fastest way to break an engagement (or a platform ToS). Pinning
> the scope at the top — before the first packet — is the guard rail. It also timestamps the recon window so
> findings can be aged.
> **HOW:** Fill the header. The in-scope / out-of-scope lines are load-bearing: everything below is bounded by them.
> **EXAMPLE:** `Target: 10.10.11.42 (HTB "Boxname") · Scope: single host, all ports · Out: no other 10.10.11.x`.

| Field | Value |
|---|---|
| Target(s) | `[IP / range / domain]` |
| Engagement / platform | `[HTB / THM / CTF event / client engagement]` |
| In-scope | `[what you MAY touch]` |
| Out-of-scope | `[what you must NOT touch — hosts, actions, DoS, social eng]` |
| Recon window (start) | `[YYYY-MM-DD HH:mm]` |
| Variant | `[LITE single-host / FULL broad-scope]` |

---

## Phase 1 — Passive Recon / OSINT (no packets to target)

> **WHY:** The information you can gather WITHOUT touching the target is free and low-risk — and often reveals
> attack surface (subdomains, tech stack, leaked creds, employee names) that active scanning would take longer
> to find and would announce your presence to get. Passive first is both stealth and efficiency.
> **HOW:** Work the boxes. For a lab/CTF, passive is thin (skip to Phase 2); for a real target with a domain,
> this phase is deep. Record every finding in the capture area — a leaked subdomain now is a foothold later.
> **EXAMPLE:** "crt.sh → `dev.target.com`, `vpn.target.com` surfaced; `staging.` returns a login not on the main site."

- [ ] WHOIS / registrar / IP ownership (`whois`, ARIN/RIPE)
- [ ] DNS records — A/AAAA/MX/TXT/NS (`dig`, `nslookup`)
- [ ] Subdomain enumeration — passive (crt.sh, certificate transparency, `subfinder`, `amass -passive`)
- [ ] Search-engine / Google dorking (`site:`, `filetype:`, `inurl:`)
- [ ] Public code / leak search (GitHub, pastebin, credential dumps)
- [ ] Tech-stack fingerprint from public pages (Wappalyzer, BuiltWith)
- [ ] Employee / email / username OSINT (if in scope — LinkedIn, `theHarvester`)
- [ ] Wayback / historical content (archive.org)

**Findings capture:**

| Finding | Source | Why it matters / next step |
|---|---|---|
| `[…]` | `[…]` | `[…]` |

---

## Phase 2 — Active Scanning (ports / services / versions)

> **WHY:** The map of the live attack surface. Every open port is a door; the version behind it is often the
> difference between "no path" and "known CVE." This phase turns "a host" into "a host running X, Y, Z at
> versions A, B, C" — the raw material for every decision after it.
> **HOW:** Fast full-port sweep first (don't stop at the top 1000 — the flag is often on a high port), then a
> targeted version/script scan on the open ones. Record every open port + service + version in the port table.
> **EXAMPLE:** "Full sweep found 22, 80, 8080, 3306. `-sV` → 8080 = Tomcat 9.0.30 (known deserialization CVEs)."

- [ ] Host discovery / liveness (`nmap -sn`, `ping`, ARP for local)
- [ ] Full TCP port sweep — all 65535 (`nmap -p- --min-rate`, `masscan`)
- [ ] Service + version detection on open ports (`nmap -sV -sC -p <ports>`)
- [ ] UDP scan on common ports (`nmap -sU --top-ports`) — SNMP/DNS/TFTP hide here
- [ ] OS fingerprint (`nmap -O`)
- [ ] Note each version against known-CVE territory (don't exploit yet — just flag)

**Port / service table:**

| Port | Proto | Service | Version | Observation / CVE territory |
|---|---|---|---|---|
| `[…]` | `[tcp/udp]` | `[…]` | `[…]` | `[the one thing that mattered]` |

---

## Phase 3 — Web Enumeration (WSTG-informed) — per web service

> **WHY:** Web is the richest attack surface and the one with the most-structured methodology (OWASP WSTG). For
> every http/https service found in Phase 2, this phase maps the application: its content, its technology, its
> entry points. Skipping directory/vhost enum is the single most common reason a foothold is missed.
> **HOW:** Run per web port. Fingerprint → content discovery (dirs/files) → vhost discovery → entry-point
> mapping (params, forms, APIs) → auth surface. Map to WSTG categories as you go — it makes the coverage gaps
> visible and feeds a professional report cleanly.
> **EXAMPLE:** "gobuster → `/admin` (403), `/backup.zip` (200); vhost fuzz → `dev.target.com` serves a debug
> console; `?debug=` param reflects input → XSS/SSTI test."

- [ ] Server / framework fingerprint (headers, `whatweb`, Wappalyzer) — WSTG-INFO
- [ ] robots.txt / sitemap.xml / security.txt / .well-known
- [ ] Directory & file brute-force (`gobuster`, `feroxbuster`, `ffuf` — with a real wordlist)
- [ ] Virtual-host / subdomain fuzzing (`ffuf -H "Host: FUZZ.target"`)
- [ ] Backup / config / source-leak files (`.git`, `.env`, `.bak`, `~`)
- [ ] Entry-point mapping — params, forms, APIs, hidden fields (Burp spider)
- [ ] Authentication surface — login, register, reset, session/cookie/JWT mechanics — WSTG-ATHN/SESS
- [ ] Input-reflection quick test — where does user input come back? (XSS/SSTI/injection candidates)
- [ ] Client-side review — JS files for endpoints, keys, comments

**Findings capture:**

| URL / endpoint | What I found | WSTG category | Next step |
|---|---|---|---|
| `[…]` | `[…]` | `[e.g. WSTG-ATHN-04]` | `[…]` |

---

## Phase 4 — Service-Specific Enumeration (non-web)

> **WHY:** The non-web services from Phase 2 each have their own enumeration playbook, and each is a potential
> foothold in its own right. SMB shares, an anonymous FTP, an exposed SNMP community, a login-less database —
> these are frequently the intended path, and they are invisible unless you enumerate the service specifically.
> **HOW:** For each non-web open port, run its enumeration playbook. Check the box, record what you found.
> Anonymous/guest/default access is the first thing to test on every service.
> **EXAMPLE:** "SMB `enum4linux` → null session allowed → share `backups$` readable → config with a password."

- [ ] SMB (445/139) — shares, null session, users (`enum4linux-ng`, `smbclient -N`, `crackmapexec`)
- [ ] FTP (21) — anonymous login, writable dirs (`ftp`, banner)
- [ ] SSH (22) — version, auth methods, banner (user-enum only if in scope)
- [ ] SNMP (161/udp) — community strings, MIB walk (`snmpwalk`, `onesixtyone`)
- [ ] LDAP (389/636) — anonymous bind, naming context (`ldapsearch`)
- [ ] Databases (3306/5432/1433/27017) — default creds, no-auth access
- [ ] SMTP (25) — VRFY/EXPN user enum, open relay
- [ ] RPC / NFS (111/2049) — exports (`showmount -e`), rpcinfo
- [ ] Other / custom service — banner-grab and research the protocol

**Findings capture:**

| Service / port | What I enumerated | Finding | Foothold potential |
|---|---|---|---|
| `[…]` | `[tool + method]` | `[…]` | `[low / med / high]` |

---

## Phase 5 — Credential & Attack-Surface Mapping

> **WHY:** Recon's output is not a list of ports — it is a ranked set of attack paths. This phase synthesizes
> everything above into (a) the credentials/secrets you gathered and (b) the prioritized entry points. It turns
> raw enumeration into a decision about where to fire first, which is the whole point of recon.
> **HOW:** Collect every credential/hash/key/username surfaced. Rank the candidate attack paths by likelihood ×
> access-gained. Name the ONE path you'll attempt first and why. This is the bridge from recon to exploitation.
> **EXAMPLE:** "Creds: `admin:backup2023` (from SMB share), `dev` username (OSINT). Top path: Tomcat 9.0.30
> deserialization on :8080 → RCE — highest access, known-exploit. Try before credential spray."

**5.1 Credentials / secrets gathered:**

| Credential / secret | Source | Where it might work |
|---|---|---|
| `[user:pass / hash / key]` | `[…]` | `[…]` |

**5.2 Ranked attack surface:**

| # | Candidate path | Access if it lands | Likelihood | Try order |
|---|---|---|---|---|
| 1 | `[…]` | `[…]` | `[H/M/L]` | `[the first move + why]` |

---

## 6. Notes to Next Phase (hand-off to exploitation / writeup)

> **WHY:** Recon feeds the next step — either your own exploitation or a teammate's. A crisp hand-off (top paths,
> open questions, things to revisit) prevents the classic failure of forgetting a promising lead you scrolled
> past. These notes drop straight into the recon section of your writeup template.
> **HOW:** 3–5 lines. The chosen first path, the backup paths, and the "things I noticed but didn't chase yet."
> **EXAMPLE:** "Primary: Tomcat deser RCE. Backup: SMB creds for lateral. Revisit: the `?debug=` reflection I
> didn't finish testing for SSTI."

- **Primary path to attempt:** `[…]`
- **Backup paths:** `[…]`
- **Leads noticed but not yet chased:** `[…]`
- **Open questions / re-enumerate if stuck:** `[…]`

---

## PART 2 — LITE SKELETON (single-host CTF / box quick sweep)

> **WHY:** For a single-host CTF or an easy box, passive OSINT is thin and this compressed sweep is the correct
> output. Do NOT skip the port sweep's full-range flag or the "did I enumerate EVERY service?" check — that is
> where the missed flag lives.

```markdown
# <Target> — Recon Sweep

**Target / Platform:** <ip / HTB>  ·  **Scope:** <single host, all ports>  ·  **Start:** <YYYY-MM-DD HH:mm>

## Ports (full sweep)
- [ ] nmap -p- --min-rate → <open ports>
- [ ] nmap -sV -sC -p <ports> → <versions>

| Port | Service | Version | Note |
|------|---------|---------|------|

## Web (per http service)
- [ ] fingerprint · [ ] gobuster dirs · [ ] vhost fuzz · [ ] backup/.git · [ ] auth surface · [ ] input reflection

## Services (per non-web port)
- [ ] SMB null/shares · [ ] FTP anon · [ ] SNMP · [ ] DB default creds · [ ] <other>

## Attack surface
- Creds gathered: <…>
- Top path to try first: <… — why>

## Notes to next phase
- Primary: <…> · Backup: <…> · Revisit: <…>
```

---

## PART 3 — WSTG CATEGORY QUICK-MAP (reusable sub-artifact)

> **WHY:** Anchors the web phase (Phase 3) to the OWASP WSTG shared vocabulary so coverage gaps are visible and
> a professional report maps cleanly. A starter map — the categories a recon pass most commonly touches; the
> full guide lives at owasp.org/www-project-web-security-testing-guide.

| WSTG area | Code prefix | Recon-phase focus |
|---|---|---|
| Information Gathering | WSTG-INFO | fingerprint, content discovery, entry points, tech stack |
| Configuration & Deploy | WSTG-CONF | backup files, admin interfaces, HTTP methods, headers |
| Identity Management | WSTG-IDNT | user enumeration, registration, account provisioning |
| Authentication | WSTG-ATHN | login surface, default creds, lockout, credential transport |
| Session Management | WSTG-SESS | cookie/JWT mechanics, session fixation surface |
| Authorization | WSTG-ATHZ | IDOR candidates, path traversal, privilege boundaries |
| Input Validation | WSTG-INPV | reflection points → injection/XSS/SSTI/SSRF candidates |

> **Boundary note.** This checklist documents *authorized recon*. Findings here feed a **lab-writeup**
> (`drop/templates/template-lab-writeup.md`) or **CTF writeup** (`drop/templates/template-ctf-writeup.md`) for
> practice, or a **pentest report** (`drop/templates/template-pentest-report.md`) with **vuln-finding cards**
> (`drop/templates/template-vuln-finding.md`) for a paid engagement. Recon maps the surface; the writeup records
> the exploitation of it — same discipline, next phase.

---

*Template author: S.P.E.A.R. — offensive-execution tradecraft.
Companions: `drop/templates/template-lab-writeup.md`, `drop/templates/template-ctf-writeup.md`,
`drop/templates/template-pentest-report.md`. Feeds: the recon section of every writeup + the Architect's
`A:\offsec-begin` methodology notes. Master lives in Vault `drop/templates/`; filled instances land in the
`offsec-begin` repo.*
