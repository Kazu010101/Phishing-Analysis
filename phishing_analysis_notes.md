# Phishing Analysis Notes

Personal study notes covering the full phishing analysis process, from understanding how email works to writing the final investigation report.

---

## Table of Contents

1. [How Email Works](#1-how-email-works)
2. [Anatomy of an Email](#2-anatomy-of-an-email)
3. [What is Phishing](#3-what-is-phishing)
4. [Types of Phishing Attacks](#4-types-of-phishing-attacks)
5. [Tactics and Techniques](#5-tactics-and-techniques)
6. [Collecting Artifacts](#6-collecting-artifacts)
7. [Analyzing Artifacts](#7-analyzing-artifacts)
8. [Taking Defensive Measures](#8-taking-defensive-measures)
9. [Writing the Investigation Report](#9-writing-the-investigation-report)

---

## 1. How Email Works

### Email Address Structure

An email address has two parts separated by `@`:

- **Mailbox** (also called localpart or username) -- e.g. `contact`
- **Domain** -- e.g. `securityblue.team`

So `contact@securityblue.team` means the mailbox is `contact`, hosted on the domain `securityblue.team`.

### Protocols

**SMTP (Simple Mail Transfer Protocol)**
- Used to send and relay email between servers
- Default port 25, but the modern standard is port 587 with TLS encryption
- When you send an email, your mail client hands it to your organization's SMTP server, which figures out where the recipient's server is (via DNS) and delivers it

**POP3 (Post Office Protocol v3)**
- Downloads emails from the server to your local device, then deletes them from the server
- Emails can only be accessed from the device that downloaded them

**IMAP (Internet Mail Access Protocol)**
- Emails stay on the server and are read remotely
- You can check the same mailbox from multiple devices
- More common than POP3 for this reason

### How a Sent Email Travels

1. Sender writes email in their client
2. Email goes to sender's outbound SMTP server
3. SMTP server queries DNS to find the recipient domain's server IP
4. Email travels across the internet, possibly through multiple SMTP relay servers
5. Reaches the recipient's SMTP server
6. Moved to a POP3 or IMAP server so the recipient can access it via their client

### Webmail vs Email Clients

**Webmail** (Gmail, Outlook.com, Yahoo) -- accessed via browser, works from any device with internet
**Email clients** (Thunderbird, Outlook app) -- downloads emails locally, works offline

---

## 2. Anatomy of an Email

Every email has two parts: a **header** and a **body**.

### Header

The header contains metadata about the message -- who sent it, who it goes to, when, and what servers handled it along the way. It begins with the `From` line and gets updated each time it passes through a server.

**Core header fields:**

| Field | What it tells you |
|---|---|
| `From` | Sending address (can be spoofed) |
| `To` | Recipient address |
| `Date` | When the email was sent |
| `Subject` | Subject line |
| `Reply-To` | Where replies go (can differ from From) |
| `Message-ID` | Unique ID for this email |
| `Received` | Hop-by-hop server trail |
| `X-Sender-IP` | IP of the originating server |

**Important note:** Header fields can be edited by anyone. There is no built-in verification that the `From` address is real. This is exactly how spoofing works.

**X-Headers** are custom headers added by software. They start with `X-`. For example, spam filters often add `X-Spam-Status: YES`.

### Body

The visible content of the email. Can be:

- Plain text
- HTML-styled with images, buttons, branding
- Base64-encoded (common for HTML emails -- use CyberChef to decode)

**Useful HTML tags to know when reading raw email source:**

- `<a href="url">text</a>` -- hyperlink (the URL is inside `href`, not the visible text)
- `<table>` -- used for layout and spacing
- `<b>`, `<i>`, `<u>` -- bold, italic, underline

To find a hidden URL in a phishing email: open the `.eml` in a text editor and look for `<a href=`. The `href` value is the real destination, which might be completely different from the link text displayed to the user.

---

## 3. What is Phishing

Phishing is the act of sending a malicious email to trick someone into doing something they would not normally do -- entering credentials, downloading malware, transferring money, or giving out information. It targets humans, not systems.

Phishing is the most common route of compromise. It is cheap for attackers, hard to fully prevent, and only needs to work once. Key stats to know:

- 90% of data breaches in 2019 were linked to phishing
- Average cost of a breach: $3.86 million
- Phishing attempts grew 65% from 2018 to 2019
- 1.5 million new phishing sites are created every month

**Related terms:**

- **Vishing** -- phishing via phone call (voice)
- **Smishing** -- phishing via SMS/text message

---

## 4. Types of Phishing Attacks

### Recon Emails

Used to confirm that a mailbox is active. The attacker is not trying to do damage yet, just gathering information before a real attack.

Three types:
- **Spam recon** -- sends a message with random body text and waits to see if it bounces. No bounce = active mailbox
- **Social engineering recon** -- tries to get a human reply by posing as someone the target knows
- **Tracking pixel recon** -- embeds a tiny 1x1 image from an attacker-controlled server. When the email is opened, the client loads the image and sends back the target's IP address, device type, email client, and timestamp

Recon emails usually pass through email gateways because they do not contain malicious links or files.

### Credential Harvester

The most common type. A branded email impersonating a legitimate company (Amazon, Microsoft, DHL, PayPal, etc.) tells the recipient to click a link. The link goes to a fake login page that looks real. Any credentials entered are captured by the attacker.

Signs to look for:
- The URL is not the real company's domain
- Display name looks legitimate but the actual email address is not
- Urgency language ("Your account will be suspended")
- Login page asks for credentials when none should be needed

Example real-world URL: `hxxps://amazonupdates.sytes[.]net/ap/signin?` -- looks Amazon-related due to the subdomain, but the root domain is not amazon.com.

### Malicious Attachment

Email convinces the target to open an attached file, which is either malware or a document that downloads malware.

**Three attachment categories:**
1. Social engineering files (forms, invoices) -- not inherently malicious, used to extract information
2. Lure documents (PDFs, Word docs) -- contain a hyperlink to a malicious site, not malicious themselves
3. Malicious files -- typically Office documents with macros that run code when enabled

**Office Macros** are the main delivery method here. Macros are disabled by default in modern Office. The attacker has to convince the user to click "Enable Content". The document usually shows a fake warning like "this document was created in an older version, please enable content to view it." Once enabled, the macro runs and usually connects to a remote server to download malware.

Watch for double extensions like `HMRC-Tax-Announcement-README.pdf.exe` -- the real extension is `.exe` but it looks like a PDF at a glance.

### Spam / Junk Email

Not always malicious, but unsolicited. Examples: newsletters, marketing, cryptocurrency schemes. Should not be confused with malspam (malicious spam), which is phishing sent at scale.

Even spam can be used for reconnaissance -- if the user clicks an "unsubscribe" link it confirms the address is active.

### Spear Phishing

Targeted phishing based on research about the victim. The attacker gathers information from LinkedIn, Facebook, company websites, and other OSINT sources to craft an email specific to one person, making it much more convincing.

Example: attacker finds a target on LinkedIn, notes their company and role, then sends an email referencing a real colleague by name and including a relevant document.

### Whaling

Spear phishing targeting senior executives (CEO, CFO, COO). These individuals often have access to sensitive data and financial systems, making them high-value targets. These emails are sent in very low volumes and are highly polished, making them hard to detect.

### Business Email Compromise (BEC)

High-impact attack targeting organizations that transfer money to vendors or suppliers. The attacker either compromises a real email account or spoofs one, then:

- Sends fake invoices to vendors directing payment to a different bank account
- Poses as the CEO and pressures finance staff to transfer funds urgently
- Requests employee data (tax forms, payroll info) by impersonating HR
- Replies to old email threads with a malicious URL (zombie phishing)

BEC attacks caused $1.77 billion in losses in the USA in 2019 alone.

The key artifact to look for in BEC: the `Reply-To` address is different from the `From` address. The `From` looks like a legitimate executive, but any reply goes to an attacker-controlled inbox.

### Vishing

Phishing over voice call. The attacker calls and impersonates someone (IT support, a bank, a government agency) to extract information or credentials. Relies heavily on social engineering -- urgency, authority, and manipulation.

Targets are usually employees with access to sensitive systems or financial accounts.

Defense: user awareness training, internal verification codes, separation of duties for financial processes.

### Smishing

Phishing over SMS. Often sent in bulk to many numbers. Usually goes after personal or payment card information.

Example: a text that looks like it is from PayPal with a link to `PayPal.verification-procedure.com` -- looks legitimate at a glance but the domain is not paypal.com.

---

## 5. Tactics and Techniques

### Sender Spoofing

SMTP does not verify the `From` field. Anyone can set the `From` address to anything they want. This means an attacker can send an email that appears to come from `support@amazon.com` without owning that address.

To detect spoofing: check the `X-Sender-IP` header and perform a reverse DNS lookup. If the sending server belongs to Gmail but the `From` says `@dicksonunited.co.uk`, the address has been spoofed.

**Reply-To spoofing**: if the attacker spoofs `contact@amazon.com`, any replies would go to Amazon (not the attacker). To get replies, the attacker adds a `Reply-To` with their own address, e.g. `hacktheplanet@gmail.com`. The recipient sees the From as Amazon but replies go to the attacker.

### Typosquatting

Registering a domain that looks like a real one by slightly misspelling it. Examples for `securityblue.team`:

- `securltyblue.team` -- lowercase L replacing the letter I
- `securitybllue.team` -- extra L in blue
- `securtyblue.team` -- missing I in security

These are used for both the email sending address and the credential harvester domain. The attacker can register mailboxes on their typosquatted domain and send emails from it.

### Homoglyphs

A more advanced version of typosquatting. Some Unicode characters from different alphabets look identical to Latin characters. For example, the Cyrillic `О` looks exactly like the Latin `O`. Domains using these characters are called internationalized domain names (IDN).

Users cannot spot this visually. Security tools that visit the link to check it are more effective than user training for this type of attack.

### Subdomain Impersonation

Using a real-looking subdomain to make a fake URL appear legitimate. For example:

`dhl-faileddelivery.shaneppalkkbc.com`

The subdomain is `dhl-faileddelivery` but the root domain is `shaneppalkkbc.com`. At a quick glance the DHL part stands out first, and the malicious domain gets overlooked. Always check the root domain, not just the subdomain.

### URL Shortening

Services like Bitly create a short redirect URL (e.g. `bit.ly/2vyvczQ`) that hides the real destination. Used to bypass email filters that block known malicious URLs.

To safely expand a shortened URL without clicking it:
- Use WannaBrowser (https://wannabrowser.net) -- simulates a browser request and shows the redirect chain and final URL
- For Bitly specifically: add a `+` to the end of the URL in a browser (e.g. `bit.ly/2vyvczQ+`) to see stats and the destination

Always record the shortened URL as an artifact, then expand it before analysis.

### HTML Styling

Legitimate emails use HTML to apply branding, logos, buttons, and color. Attackers copy this exact styling to make credential harvester emails look identical to the real thing. Sometimes the fake looks better than a poorly formatted legitimate email.

When analyzing a phishing email, view it in a text editor to see the raw HTML. The visible button text might say "Verify Account" but the `href` in the anchor tag goes somewhere completely different.

### Use of Legitimate Services

Attackers use trusted platforms to avoid detection:

- **Email delivery**: Gmail, Outlook.com -- organizations rarely block these domains
- **File hosting**: Google Drive, Dropbox, OneDrive -- trusted domains, less likely to be flagged
- **Email marketing**: MailGun, MailChimp -- legitimate sending infrastructure, hard to block

---

## 6. Collecting Artifacts

Always analyze phishing emails on a VM or a dedicated "dirty" system. Never on a personal or work machine.

There are three categories of artifacts: email, web, and file.

### Email Artifacts

Collect from the email client first, then open the raw `.eml` in a text editor (Sublime Text works well) for the rest. Use `CTRL+F` to search for fields by name.

| Artifact | Where to find it | Why it matters |
|---|---|---|
| Sending Address | Email client or `From:` in text editor | Who sent it (or appears to have) |
| Sender Domain | Part of the sending address | Is this a spoofed or malicious domain? |
| Reply-To | Text editor, search "reply" | Where replies actually go -- key BEC indicator |
| Sending Server IP | Text editor, search "X-Sender-IP" | Reveals the real sending server, catches spoofing |
| Reverse DNS of IP | whois.domaintools.com | Hostname of the sending server |
| Subject Line | Email client | Search/block term for the email gateway |
| Date and Time | Email client or `Date:` header | Timeline of the attack |
| Recipient(s) | Email client or `To:` header | Who else received this? |

### Web Artifacts

| Artifact | How to get it |
|---|---|
| Full URL | Right-click hyperlink in email client and copy link, or find `<a href=` in text editor |
| Root domain | The domain part of the URL, excluding subdomains and path |
| Shortened URL | Copy as-is -- record the original AND the expanded destination |

### File Artifacts

| Artifact | How to get it |
|---|---|
| Filename + extension | Email client attachment view, or search `filename=` in text editor |
| MD5 hash | PowerShell: `Get-FileHash -Algorithm MD5 .\filename` |
| SHA256 hash | PowerShell: `Get-FileHash .\filename` (default is SHA256) or Linux: `sha256sum filename` |

SHA256 is the preferred hashing standard. MD5 and SHA1 have known collision weaknesses and should not be relied on for blocking or intelligence sharing.

---

## 7. Analyzing Artifacts

Collecting artifacts just gives you raw data. Analysis is where you determine if they are actually malicious.

Important note: if something comes back clean on a reputation tool, that does not mean it is safe. Treat suspicious artifacts as guilty until proven innocent. New and targeted attacks will not have been seen by vendors before.

### Email Header Analysis

**SPF (Sender Policy Framework):** Checks whether the sending server IP is authorised to send email on behalf of the claimed domain. A `FAIL` result means the IP is not authorised -- strong indicator of spoofing.

**DKIM (DomainKeys Identified Mail):** A cryptographic signature added to the email by the sending server. Verifies the email has not been tampered with in transit and was sent by the claimed domain. A `FAIL` means the signature is invalid or missing.

**DMARC (Domain-based Message Authentication, Reporting, and Conformance):** Policy that specifies what to do when SPF or DKIM fails (none, quarantine, or reject). Records the result in the email header.

Tool: MXToolbox -- paste the email header and it checks SPF, DKIM, and DMARC.

A note on compromised domains: if an attacker has taken over a legitimate domain's mailbox, SPF and DKIM may still pass because the sending server is authorised. Authentication results alone do not prove an email is safe.

### URL Analysis

**Step 1 -- Expand:** If the URL is shortened, use WannaBrowser to expand it first. Never submit a shortened URL to reputation tools without expanding it first.

**Step 2 -- Visualise:** Use URL2PNG or URLScan.io to take a screenshot of the page without visiting it. This lets you see if it is a fake login page, a malware download page, etc.

**Step 3 -- Reputation check:** Submit to VirusTotal (URL tab) and URLScan.io. Check PhishTank and URLhaus as well for phishing-specific databases.

**Step 4 -- Domain/IP intel:** 
- WHOIS: whois.domaintools.com -- check registration date (very recently registered = suspicious), registrant info, and country
- IP reputation: IPVoid -- checks the hosting IP against blacklists

Signs a domain is malicious:
- Registered very recently (days or weeks ago)
- Privacy-protected registration hiding the owner
- Subdomain impersonation of a known brand
- No legitimate business associated with the domain

### File Analysis

**Step 1 -- Hash lookup:** Submit the MD5 or SHA256 hash to VirusTotal and Talos File Reputation. If it has been seen before and flagged, you will get vendor detection results.

**Step 2 -- File upload:** If the hash is unknown, upload the file to VirusTotal. This runs it through ~70 antivirus engines.

**Step 3 -- Sandbox detonation:** Submit to Hybrid Analysis or an enterprise sandbox. The file is actually run in a safe virtual environment. The report shows what happened: network connections made, files created or modified, processes spawned, C2 domains contacted. This is how you find out what a macro actually does.

**For Office documents specifically:** Open in a VM and check for the "Enable Content" prompt. If it appears, macros are present. Do not click Enable Content unless you are in an isolated environment.

---

## 8. Taking Defensive Measures

Once an email is confirmed malicious, take action to prevent further harm. Every action must be documented with: block type, value blocked, date and time, and analyst name.

Always consider business impact before blocking. Blocking a legitimate domain or IP will cause collateral damage.

### Blocking Email Artifacts (Email Gateway)

| What to block | When it is appropriate |
|---|---|
| Sending address | When the address is uniquely malicious and not used by a legitimate organization |
| Sender domain | Only when the entire domain is malicious (not a legitimate brand being spoofed) |
| Sending server IP | When the IP belongs to a dedicated malicious server -- do not block if it is a shared provider like Gmail |
| Subject line | When the subject is highly specific and unlikely to appear in legitimate mail |

Example decision: an email spoofing `contact@dhl.com` was sent from a Gmail IP. Blocking the `contact@dhl.com` address would block legitimate DHL emails. Blocking Gmail's IP would block all Gmail. The right call was to block the specific subject line, which was unusual enough that it would not match any legitimate emails.

### Blocking Web Artifacts (Web Proxy / Firewall)

Block the malicious URL or domain on the web proxy. This prevents users from connecting to the page even if they click the link.

If the domain was created purely for malicious use (no legitimate content), block the root domain. If it is a compromised legitimate domain, be more careful -- blocking the domain may cause collateral damage.

### Blocking File Artifacts (Endpoint / EDR / AV)

Block the file hash (SHA256 preferred) on the endpoint protection platform. This prevents the file from executing on any device in the environment.

If machines have already been infected, escalate to the incident response team.

### Notifying Recipients

Once you identify who received the email (via email gateway logs), notify them via BCC. The notification should include:

- The date and time the phishing email was sent
- The subject line of the phishing email (so they can find it easily)
- Clear instructions: delete it, or forward it to the security mailbox
- Contact details if they are unsure

Ask if anyone has already interacted with the email. If they have, that becomes a separate incident.

### Preventative Measures (for context)

**Email gateway rules:** Blocking by sender, domain, IP, subject line, or attachment type.

**Attachment sandboxing:** Detonates attached files in a virtual environment before delivering the email. Can identify macro-based malware that looks benign to static scanners.

**Security awareness training:** Users should be trained to spot: unknown senders, urgency language, grammar errors, suspicious links and attachments. Simulated phishing exercises (GoPhish, Sophos Phish Threat) test this regularly.

---

## 9. Writing the Investigation Report

The report is the written record of everything you found and everything you did. It creates an audit trail.

### Structure

A good investigation report section covers:

1. Summary of the attack (what type of phishing, what the attacker was after)
2. Full IOC list (defanged -- see below)
3. Analysis findings for each artifact
4. List of recipients notified and their response
5. Every defensive action taken with justification
6. Timestamps and analyst name on each action

### What Good Report Writing Looks Like

For each defensive action, the report should explain:
- What you found and why it is malicious
- Why the specific block type was chosen
- Why there is no negative business impact (or what the impact is and why it is acceptable)
- The action taken in a standard format

Example format for a block action:

> Subject Line Block (Email Gateway) "Failed Delivery DHL RESPOND NOW -- URGENT!!" on 22nd December at 12:03 PM by Jane Smith.

That one line tells you: what was blocked, where, the exact value, when, and who did it.

### Bad practice example vs good practice

**Too vague:** "The email was malicious so I blocked the domain."

**Better:** "The URL used within the email resolves to the domain shaneppalkkbc[.]com, which has no legitimate business purpose and was created purely for malicious use. There is no reason for employees to visit this domain. Blocking it on the web proxy will prevent users from accessing any content hosted there. Domain Block (Web Proxy) shaneppalkkbc[.]com on 22nd December at 12:07 PM by Jane Smith."

### Defanging IOCs

Before including any URLs or IP addresses in a report, defang them. This prevents someone from accidentally clicking a live malicious link.

Rules:
- Replace `.` with `[.]` in IP addresses and domains
- Replace `http` with `hxxp` in URLs

Examples:

| Original | Defanged |
|---|---|
| `8.8.8.8` | `8[.]8[.]8[.]8` |
| `https://evil.com/malware` | `hxxps://evil[.]com/malware` |
| `192.168.1.1` | `192[.]168[.]1[.]1` |

Use CyberChef for batch defanging:
- Recipe: "Defang IP Addresses" for IPs
- Recipe: "Defang URL" for URLs

---

## Quick Reference: Tools

| Task | Tool |
|---|---|
| View raw email source | Sublime Text, any text editor |
| View email in client | Mozilla Thunderbird |
| Decode Base64 body | CyberChef (From Base64 recipe) |
| Expand shortened URL | WannaBrowser (wannabrowser.net) |
| Screenshot a URL | URL2PNG, URLScan.io |
| URL reputation | VirusTotal (URL tab), URLScan.io |
| Phishing database | PhishTank, URLhaus |
| WHOIS / reverse DNS | whois.domaintools.com |
| IP reputation | IPVoid |
| Email header analysis | MXToolbox |
| File hash lookup | VirusTotal, Talos File Reputation |
| File sandbox | Hybrid Analysis |
| Hash a file (Windows) | PowerShell: `Get-FileHash .\file` (SHA256) or `Get-FileHash -Algorithm MD5 .\file` |
| Hash a file (Linux) | `sha256sum filename`, `md5sum filename` |
| Defang IOCs | CyberChef (Defang IP / Defang URL) |

---

## Glossary

| Term | Meaning |
|---|---|
| IOC | Indicator of Compromise -- a piece of evidence that an attack occurred or is occurring |
| Artifact | A specific piece of data collected during an investigation (email address, URL, hash, etc.) |
| File hash | A unique fixed-length string generated from a file's contents. Used for identification and reputation checks |
| Recon | Reconnaissance -- gathering information about a target before attacking |
| Credential harvester | A fake login page that captures credentials entered by victims |
| Vishing | Voice phishing -- phishing via phone call |
| Smishing | SMS phishing -- phishing via text message |
| BEC | Business Email Compromise -- impersonating executives or vendors to steal money or data |
| SPF | Sender Policy Framework -- verifies the sending server is authorised for the domain |
| DKIM | DomainKeys Identified Mail -- cryptographic signature verifying email integrity |
| DMARC | Domain-based Message Authentication, Reporting and Conformance -- policy for handling SPF/DKIM failures |
| Defanging | Making IOCs safe to include in reports by breaking URLs and IPs so they cannot be clicked |
| Typosquatting | Registering a domain that looks like a real one with minor misspellings |
| Homoglyph | A character from one script (e.g. Cyrillic) that looks identical to a character from another (e.g. Latin) |
| Malspam | Malicious spam -- phishing distributed at scale |
| Zombie phishing | Replying to legitimate old email threads with malicious content from a compromised account |
| Whaling | Spear phishing specifically targeting C-level executives |
