**Report Created**: Fri, 1st May 2026 04:05:13 pm by Kazu
**Escalated to**: Bob - On Duty SOC Tier 2 
# Email Description

Email is posing as XYZ Bank with very well styled logo, background colors, and branding materials, asking user to click a link in order to update login details within the next 24 hours, or user's account will be permanently closed. The email is categorised as`Credential Harvester`. 

# Artefacts Collected (defanged)

- **Sender:** customer_support@xyz[.]com[.]au
- **Subject Line**: "You won't be able to login - RESPOND NOW!"
- **Recipient(s)**: Jane.Smith@abc.co.au 
- **Sending Server IP**: 209.85.167.42
- **Reverse DNS**: mail-lf1-f42.google.com
- **Reply-To**: mysecretemail@gmail.com (*if available*)
- **Date and Time**: Fri, 1st May 2026 03:54:11 pm
- **Hyperlinked URL**: hxxps://www.igotyoue.com/creds/2026/j41da1/xyzbank.php?

# Artefact Analysis

**WHOIS Analysis** – Performing a WHOIS search shows that the domain was registered 7 days ago, with JojoMyMan as the domain registrar.  Also, Reverse DNS shows that the email originates from Gmail, indicating that the email is spoofed. 


**VirusTotal Reputation** – Searching for the full URL and the root domain on VT shows that it is currently not being flagged as malicious, indicating the domain is new and thus, has not been analysed by security engines.

**URL2PNG Analysis –** Using URL2PNG to view the URL link showed that the site was hosting a fake XYZ bank login portal, used to steal any credentials that are entered. Looking at the root domain “hxxps://www.igotyoue.com” shows that the site has attempted to counterfeit the xyz bank homepage visually, a high indicator that the domain is used for malicious operations.

# Suggested Defensive Measures

Although the sender address managed to spoof `customer_support@xyz[.]com[.]au`, Reverse DNS analysis reveals that the IP `209.85.167.42` originates from Gmail, not XYZ Bank. Thus, it is not recommended to block the sending server IP because it originates from Gmail, as doing so could block legitimate emails, which could negatively affect the business. We also cannot block `customer_support@xyz[.]com[.]au` because it is a legitimate email that our business have interest with. 

- Defensive Measure #1 Proposed: `Block Subject Line on the Email Gateway` 

	**Rationale**: The subject line contains key word "RESPOND NOW!" which is a distinct characteristic of social engineering attack in creating a sense of urgency to the victim. Furthermore, XYZ Bank or any financial institution never request credential reset via email link, in a such an urgent manner. 

- Defensive Measure #2 Proposed:`Domain Block "igotyoue[.]com" on the Web Proxy` 

	**Rationale**: The URL used for the credential harvester uses a malicious domain "igotyoue[.]com", which was created solely for malicious purpose. Also, since we have no business interest with this domain whatsoever, it is therefore appropriate to completely block the entire domain to mitigate risk caused by user from visiting the malicious link. 

- Defensive Measure #3 Proposed: `Block Reply-To Address "mysecretemail@gmail.com" on the Email Gateway` 

	**Rationale**:  Blocking the actual Reply-To email address `mysecretemail@gmail.com` would stop more malicious email sent by this sender. 

There are no negative implications to the business by implementing all defensive measures proposed. 
