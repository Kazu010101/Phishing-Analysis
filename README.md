# Phishing Analysis — SOC Tier 1 Portfolio

> A practical reference portfolio covering phishing investigation from initial triage through to defensive response and report writing. Built as a working reference for day-to-day SOC analyst work.

---

## About This Repo

This repository documents my approach to phishing analysis, including the tools, processes, and reasoning I apply when triaging suspicious emails.

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
│   └── notes/
│       └── phishing_analysis_notes.md  ← Full investigation notes
│
└── 📁 sample-reports/
    ├── report_001_credential_harvester.md   ← [Add your report]
    ├── report_002_malicious_attachment.md   ← [Add your report]
    └── report_003_bec.md                    ← [Add your report]
```

---

## Investigation Flowchart

A visual, step-by-step flowchart covering the full phishing investigation lifecycle — from alert trigger to ticket closure — built as a quick-reference guide for Tier 1 analysts.

**Covers:** Alert triage → Email retrieval → Artifact collection → Analysis → Decision point → Notify recipients → Defensive measures → Report writing

🔗 **[View Flowchart](./reference/phishing_flowchart.html)** *(best viewed via GitHub Pages)*

<!-- Optional: Add a screenshot of the flowchart here once hosted -->
<!-- ![Flowchart Preview](./assets/flowchart_preview.png) -->

---

## 📋 Artifact Collection Reference Table

An interactive reference table mapping every collectible artifact (email headers, URLs, file hashes, etc.) against 11 phishing attack types including BEC, credential harvesters, Office macros, smishing, and vishing.

| Feature | Detail |
|---|---|
| Attack types covered | 11 across Email, Web, File, and Alt. Channel categories |
| Artifact rows | 29 across email, web, file, and alt-channel groups |
| Cell data | Collection requirement (Required / Conditional / N/A) + recommended tool |

🔗 **[View Artifact Table](https://github.com/Kazu010101/Phishing-Analysis/blob/main/phishing_artifact_table.html)** *(best viewed via GitHub Pages)*

---

## 📝 Investigation Notes

Personal reference notes covering the full phishing analysis process, including theory, tooling, and methodology.

| Section | Topics |
|---|---|
| Email Identification | Red flags, attack type classification, social engineering indicators |
| Artifact Collection | Email, web, and file artifact extraction methodology |
| Email Header Analysis | SPF, DKIM, DMARC, hop tracing, Reply-To anomalies |
| URL Analysis | Shortlink expansion, WHOIS, IP reputation, site screenshots |
| File Analysis | Hashing, VirusTotal, sandbox detonation, macro detection |
| Defensive Measures | Gateway blocks, proxy rules, endpoint hash blocks |
| Report Writing | IOC defanging, documentation standards, block action logging |

🔗 **[View Notes](./reference/notes/phishing_analysis_notes.md)**

---

## 🔍 Sample Investigation Reports

Real-world style phishing investigations documented in analyst report format. Each report follows the full investigation workflow: artifact collection → analysis findings → defensive actions taken → IOC summary.

<!-- Add a row for each report you include -->

| # | Attack Type | Key Artifacts | Verdict | Report |
|---|---|---|---|---|
| 001 | Credential Harvester | Spoofed sender, fake login URL | ✅ Malicious | [View](./sample-reports/report_001_credential_harvester.md) |
| 002 | Malicious Attachment | SHA256 hash, VirusTotal hit | ✅ Malicious | [View](./sample-reports/report_002_malicious_attachment.md) |
| 003 | BEC | Reply-To mismatch, wire transfer request | ✅ Malicious | [View](./sample-reports/report_003_bec.md) |
| 004 | *(Add yours)* | — | — | — |

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

🔗 **[LinkedIn](https://linkedin.com/in/yourprofile)**
💼 **[Portfolio / Personal Site](https://yoursite.com)** *(if applicable)*

---

*Last updated: [Month Year]*
