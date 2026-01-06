# Lab 1: Basic Recon & Scope Validation

## Objective
Identify ownership, infrastructure, and exposed surface area.

---

## Steps
- WHOIS lookup  
- DNS / subdomain discovery  
- Identify technology stack  

---

## Artifacts
- Recon summary (5 bullets)

---

## Deliverable
Short recon report

---

## Mini Report Template

### Finding
Running `nslookup` returns publicly visible DNS and name server information.

---

### Risk Impact
Low — information disclosure consistent with standard DNS behavior.

---

### Evidence
Passive DNS and certificate transparency enumeration.

---

### Recommendation
No immediate remediation required. Ensure test environments remain clearly segmented and monitored.

---

## Capture

**Organization / Registrar:**  
Gandi SAS  

**Country:**  
France  

**Name servers:**  
<img width="499" height="198" alt="Name server enumeration" src="https://github.com/user-attachments/assets/0b8a1351-70d0-446a-9367-cd8c26f0f520" />

**Registration date (age):**  
<img width="594" height="224" alt="Registration date" src="https://github.com/user-attachments/assets/94fafdd1-ca32-40cb-aea7-e278f6bd352e" />

---

## Observations

**Who appears to own it?**  
Invicti Security Limited  

**Is it cloud-hosted?**  
Yes — AWS and Google  

**Is anything intentionally hidden?**  
Contact information is redacted for privacy.

---

## Certificate Transparency (crt.sh)

Gained experience using **crt.sh**, which shows domains and subdomains that have TLS/SSL certificates.  
This often reveals hidden or forgotten subdomains **without touching the target**.

<img width="828" height="416" alt="crt.sh results" src="https://github.com/user-attachments/assets/89360bd7-371c-4a07-b11b-6b4f07f5e91f" />

Certificate transparency log analysis using `crt.sh` (Identity LIKE search) returned **no results** for `vulnweb.com` subdomains.

This suggests:
- No publicly logged TLS certificates exist for subdomains under this domain, **or**
- Certificates do not expose subdomain identities  

This limits CT-based visibility but does **not** imply absence of subdomains.

---

## Additional Passive Enumeration

An additional scan was performed using:  
https://dnsdumpster.com/

---

## Subdomain Analysis (Name-Based Inference)

| Subdomain | Likely Purpose |
|---------|---------------|
| testphp | PHP test application |
| testasp | ASP test application |
| testaspnet | ASP.NET test application |
| testhtml5 | Front-end test application |
| antivirus1 | Security-related service |
| virus / viruswall | Malware simulation |
| tetphp | Likely typo or legacy system 🔥 |
| www.test.php | Poor naming hygiene 🔥 |

🔥 These entries represent **real-world red flags** in non-test environments.

---

## Subdomains Identified
- Multiple test and demonstration subdomains discovered (PHP, ASP, ASP.NET, HTML5)
- Presence of security-themed subdomains (`antivirus`, `virus`, `viruswall`)
- Naming inconsistencies and likely legacy or typo-based subdomains
- Broad exposed surface area across multiple technologies

---

## Recon Summary – vulnweb.com
- Hosts multiple test and demonstration subdomains across varied technologies
- Subdomain naming suggests intentionally vulnerable applications for testing
- Security-themed subdomains indicate malware or defensive simulation services
- Inconsistent naming may represent legacy or unmanaged assets
- Broad exposed surface area identified through passive DNS-based enumeration

---

# 📄 Short Recon Report (Final Deliverable)

**Target:** vulnweb.com  
**Recon Type:** Passive Enumeration  

### Summary
Passive reconnaissance identified numerous subdomains under `vulnweb.com`, including multiple technology-specific test applications and security-themed services. Subdomain naming conventions suggest the environment is intentionally designed for vulnerability testing. The presence of inconsistent and typo-based subdomains indicates a wide and potentially unmanaged attack surface, which would warrant further scoped enumeration in a real engagement.

**Risk Level:** Low (Test Environment)
