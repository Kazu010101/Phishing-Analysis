# Phishing Analysis — SOC Tier 1 Portfolio

> A practical reference portfolio covering phishing investigation from initial triage through to defensive response and report writing. Built as a working reference for day-to-day SOC analyst work.

---

## About This Repo

This repository documents my approach to phishing analysis, including the tools, processes, and reasoning I apply when triaging suspicious emails and deciding defensive measures.

I'm currently working toward upskilling myself to land a job as **SOC Analyst Tier 1** and am building hands-on skills in threat detection and incident response.

---

## 📂 Repository Structure

```
📁 phishing-analysis/
│
├── 📄 README.md                        ← You are here
│
├── 📁 reference/
│   ├── phishing_flowchart.html         ← SOC Tier 1 Phishing investigation flowchart
│   ├── phishing_artifact_table.html    ← Artifact collection cheat sheet table
│
└── 📁 sample-reports/
    ├── report_001_credential_harvester.md   ← [Click hyperlink in the table to view the sample report]
    ├── report_002_malicious_attachment.md   ← [In Progress]
    
```

---

## Investigation Flowchart

A visual, step-by-step flowchart covering the full phishing investigation lifecycle — from alert trigger to ticket closure — built as a quick-reference guide for Tier 1 analysts.

**Covers:** Alert triage → Email retrieval → Artifact collection → Analysis → Decision point → Notify recipients → Defensive measures → Report writing

🔗 **[View Flowchart](https://kazu010101.github.io/Phishing-Analysis/phishing_flowchart.html)** *(best viewed via GitHub Pages)*

Screenshot 01
<img width="883" height="847" alt="image" src="https://github.com/user-attachments/assets/4a7f7df3-d5e1-432d-a91b-94c44a58e67e" />

Screenshot 02
<img width="770" height="862" alt="image" src="https://github.com/user-attachments/assets/7b471380-52c5-4141-bb1e-62f4c0004051" />


---

## Artifact Collection Reference Table

A summative reference table mapping every collectible artifact (email headers, URLs, file hashes, etc.) against 11 phishing attack types including BEC, credential harvesters, Office macros, smishing, and vishing.
<img width="1756" height="802" alt="image" src="https://github.com/user-attachments/assets/607f7317-e7a7-4e23-b4fc-5444dcf15c3f" />
<img width="1763" height="252" alt="image" src="https://github.com/user-attachments/assets/1472227b-4f17-460a-be6b-0d8f650ff923" />



| Feature | Detail |
|---|---|
| Attack types covered | 11 across Email, Web, File, and Alt. Channel categories |
| Artifact rows | 29 across email, web, file, and alt-channel groups |
| Cell data | Collection requirement (Required / Conditional / N/A) + recommended tool |

🔗 **[View Artifact Table](https://kazu010101.github.io/Phishing-Analysis/phishing_artifact_table.html)** *(best viewed via GitHub Pages)*

---

## My Learning Notes

Personal learning notes covering the full phishing analysis process, including theory, tooling, and methodology.

| Key Sections | Topics |
|---|---|
| Email Identification | Red flags, attack type classification, social engineering indicators |
| Artifact Collection | Email, web, and file artifact extraction methodology |
| Email Header Analysis | SPF, DKIM, DMARC, hop tracing, Reply-To anomalies |
| URL Analysis | Shortlink expansion, WHOIS, IP reputation, site screenshots |
| File Analysis | Hashing, VirusTotal, sandbox detonation, macro detection |
| Defensive Measures | Gateway blocks, proxy rules, endpoint hash blocks |
| Report Writing | IOC defanging, documentation standards, block action logging |

🔗 **[View Notes](https://github.com/Kazu010101/Phishing-Analysis/blob/main/phishing_analysis_notes.md)**

---

## 🔍 Sample Investigation Reports

Real-world style phishing investigations documented in analyst report format. Each report follows the full investigation workflow: Email Description → artifact collection → analysis findings → defensive actions taken.

<!-- Add a row for each report you include -->

| # | Attack Type | Key Artifacts | Verdict | Report |
|---|---|---|---|---|
| 001 | Credential Harvester | Spoofed sender, fake login URL | ✅ Malicious | [View](https://github.com/Kazu010101/Phishing-Analysis/blob/main/SOC%20Analyst%20Report%20Sample-%20Phishing%20Email%20(fictitious).md) |
| 002 | Malicious Attachment | SHA256 hash, VirusTotal hit | ✅ Malicious | [View](./sample-reports/report_002_malicious_attachment.md) |

> **Note on samples:** All email addresses, domains, IPs, and file hashes in these reports are either fabricated, defanged, or sourced from public threat intelligence feeds. No real victim data is included.

---

## 🛠️ Tools Referenced

| Category | Tools |
|---|---|
| Email Analysis | Thunderbird, Text Editor (raw .eml), PhishTool, MXToolbox |
| URL Analysis | URLScan.io, VirusTotal, URL2PNG, WannaBrowser, PhishTank, URLhaus |
| Domain / IP Intel | whois.domaintools.com, IPVoid |
| File Analysis | VirusTotal, Talos File Reputation, Hybrid Analysis |
| Hashing | PowerShell `Get-FileHash`, Linux `sha256sum` / `md5sum` |
| IOC Formatting | CyberChef (Defang URL / Defang IP operations) |

---

## Skills Demonstrated

- Email header analysis (SPF / DKIM / DMARC interpretation)
- IOC extraction and artifact triage across email, web, and file types
- Threat intelligence tool usage (VirusTotal, URLScan, Hybrid Analysis)
- Phishing attack type classification (BEC, credential harvesting, macro delivery, smishing, vishing)
- Defensive response documentation (gateway blocks, proxy rules, endpoint controls)
- Analyst report writing with defanged IOCs and audit-trail logging

---

## Disclaimer

All content in this repository is for **educational and portfolio purposes only**. Any sample emails, IOCs, domains, or IP addresses referenced in reports are either synthetic, defanged, or drawn from publicly available threat intelligence. Nothing in this repository should be used for malicious purposes.

---

## Contact

If you're a recruiter, hiring manager, or fellow analyst and want to connect:

🔗 **[LinkedIn](https://linkedin.com/in/kazu-suryadikarta-81524a189)**
💼 **[Portfolio Main Page](https://github.com/Kazu010101/Kazu010101/edit/main/README.md)** 

---

*Last updated: [03/05/2026]*
