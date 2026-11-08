# 🏰 Active Directory Penetration Testing & Attacks

> **Phase:** [phase-f-redteam](../) | **Tags:** `#activedirectory` `#kerberos` `#bloodhound` `#mimikatz`  
> **Cert Target:** CRTO / PNPT / OSCP AD Domain

---

## 📑 AD Kill Chain Checklist
- [ ] 1. AD Enumeration with BloodHound / SharpHound & LDAP
- [ ] 2. LLMNR / NBT-NS Poisoning (`Responder`)
- [ ] 3. AS-REP Roasting (Accounts without pre-authentication)
- [ ] 4. Kerberoasting (Service Principal Names - SPNs)
- [ ] 5. Pass-the-Hash (PtH) & Pass-the-Ticket (PtT)
- [ ] 6. DCSync Attack (`secretsdump.py` / Mimikatz `lsadump::dcsync`)
- [ ] 7. Active Directory Certificate Services (ADCS) Abuses
