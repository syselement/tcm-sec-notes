# AD - Case Studies

## Case study #1

> - [AD Case Study #1 - You Spent How Much on Security? - TCM Security](https://tcm-sec.com/pentest-tales-001-you-spent-how-much-on-security/)

This case study details a **penetration test** conducted on a **well-funded U.S. hospital** with a strong security infrastructure, including **IDS/IPS, CyberArk PAM, Symantec Endpoint Security, and proper patch management**. Despite these defenses, testers found **critical security gaps** that could be exploited.

**Key Findings:**

- **SMB Relay Attack Exposure:**
  - LLMNR/NBNS poisoning was **mitigated**, but **SMB relay attacks** were still possible
  - Attackers could use **NTLM relaying** to escalate privileges
- **Privilege Escalation Risks:**
  - Misconfigurations and local users easy-reused hashes/passwords allowed **privilege escalation**, leading to potential **Domain Admin access**
  - Weak **Active Directory hardening** left **high-value targets exposed**
- **Security Investment ≠ Full Protection:**
  - Even with expensive security solutions, **configuration weaknesses** left the network vulnerable
  - **Lateral movement & persistence techniques** were viable due to **improper segmentation** and **overprivileged accounts**

**Key Takeaways:**

- **Network segmentation & NTLM hardening** are critical
- **Regular security assessments** are needed despite high investment in security tools
- **Least privilege enforcement** should be a priority to prevent escalation

This case study highlights how **misconfigurations and overlooked weaknesses** can lead to **serious security risks**, even in well-funded environments.

---

## Case study #2

> - [AD Case Study #2 - #Pentest Tales #002: Digging Deep - TCM Security](https://tcm-sec.com/pentest-tales-002-digging-deep)

This case study outlines a **penetration test** on a **well-funded U.S. hospital** with solid security measures, including **LLMNR/IPv6 disabled, SMB Signing enforced, IDS/IPS, and patched systems**. Despite these controls, the assessment revealed **critical security gaps** that could be exploited.

**Key Findings:**

- **Default Credentials on Development Apps:**
  - A **development environment application** was found using **default credentials**, granting unauthorized access.
  - Attackers could **leverage this access** to extract **sensitive information**.
- **Local Administrator Password Reuse:**
  - The **same local admin password** was used across multiple machines.
  - Once a **single system** was compromised, **lateral movement** became trivial.
- **WDigest Enabled on Legacy Systems:**
  - Older systems had **WDigest enabled**, storing **plaintext credentials** in memory.
  - Attackers could extract **Domain Admin credentials** using tools like **Mimikatz**.
- **Overprivileged Service Accounts:**
  - Service accounts had **Domain Admin** privileges unnecessarily.
  - Compromising one of these accounts **led to full domain compromise**.

**Key Takeaways:**

- **Enforce unique local admin passwords** across endpoints (**LAPS**).
- **Disable WDigest** on all systems to prevent plaintext credential exposure.
- **Restrict service account privileges** to the **minimum necessary**.
- **Regular security assessments** are necessary, even with strong security investments.

This case highlights how **misconfigurations and weak credential management** can undermine otherwise strong defenses, making **lateral movement and domain compromise easy** for attackers.

---

