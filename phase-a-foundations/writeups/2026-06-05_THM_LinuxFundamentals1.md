# TryHackMe — Linux Fundamentals Part 1

**Date:** 2026-06-05
**Platform:** TryHackMe
**Target:** Linux Fundamentals Part 1
**Phase:** A — Core Foundations
**Status:** Complete

---

## Objective

Build Linux CLI fluency. This room covers navigation, file operations, searching, and basic permissions — the substrate everything in Phase A rests on.

---

## Walkthrough

### Task 1 — Introduction
- Accessed in-browser terminal. No setup required.

### Task 2 — Running Your First Commands
```bash
echo "Hello World"   # print to stdout
whoami               # current user
```

### Task 3 — Interacting with the Filesystem
```bash
ls          # list directory
ls -la      # all files including hidden, long format
cd /home    # change directory
pwd         # print working directory
cat file    # print file contents
```

### Task 4 — Searching for Files
```bash
find / -name passwords.txt 2>/dev/null   # find by name, suppress errors
grep "THM" access.log                    # search inside file
grep -r "keyword" /directory/            # recursive search
```

---

## Key Takeaways

- `find` + `2>/dev/null` is essential — suppresses permission errors on restricted dirs
- `grep -r` is the recon equivalent of `find` for file content
- Long format `ls -la` reveals hidden files (dotfiles) — critical for CTF enumeration

---

## Questions / Gaps

- [ ] How does `grep` handle binary files? (check `--text` flag)
- [ ] Difference between `>` and `>>` for output redirection

---

## Tags

`phase-a` `linux` `tryhackme` `cli` `filesystem`
