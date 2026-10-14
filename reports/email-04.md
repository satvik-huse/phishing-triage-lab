# Phishing Report: email-04

| Field | Value |
|---|---|
| Sample | sample-1247.eml (phishing_pot dataset) |
| Date received | Sat, 05 Nov 2022 10:43:42 +0000 |
| Verdict | MALICIOUS SPAM: cybercrime marketplace advertising (low severity) |
| Impersonates | None: display name "Omgo" |
| Related | Same theme and period as [email-02](email-02.md) |
| Analyst | Satvik Huse |

## 1. Summary
Spam advertising a "black market" selling stolen access: web shells, hacked cPanel hosting accounts, hijacked SMTP mail servers and compromised RDP (remote desktop) logins. Sent from kipa-group.com through a third-party mail gateway (getway.biz). This is how attackers buy footholds for later attacks, so it is useful threat intelligence even though it targets no one directly.

## 2. Header analysis
| Header | Value | Finding |
|---|---|---|
| From | Omgo <info@kipa-group.com> | Display name unrelated to the domain |
| Return-Path | info@kipa-group.com | Matches From |
| Reply-To | (none) | Contact is in the body |
| Subject | Best Black Market [shells,cpanels,smtps,rdps,..etc] | Openly sells criminal access |
| Sending server | mailgw5.getway.biz [185.231.59.226] | Third-party mail gateway |
| SPF / DKIM / DMARC | pass / none / none | No DKIM signature, no DMARC policy |

## 3. Enrichment
| Indicator | Tool | Result |
|---|---|---|
| 185.231.59.226 | VirusTotal | 0 vendors flag as malicious |
| kipa-group.com | VirusTotal / urlscan.io | Clean |

## 4. IOCs
- info@kipa-group.com (sender)
- 185.231.59.226 / mailgw5.getway.biz (sending gateway)

## 5. What is being sold
| Item | Meaning | Risk to a company |
|---|---|---|
| Shells | Web shells on hacked websites | Attacker control of a web server |
| cPanels | Hosting control-panel logins | Website defacement, phishing pages |
| SMTPs | Hijacked mail servers | Sending spam and phishing as you |
| RDPs | Remote desktop logins | Direct entry point for ransomware |

## 6. Red flags
- Subject openly advertises a "black market"
- Display name "Omgo" has no link to kipa-group.com
- No DKIM and no DMARC policy
- Unsolicited bulk mail

## 7. MITRE ATT&CK
- T1650 Acquire Access (the marketplace sells initial access)

## 8. Recommended actions
1. Delete and block the sender
2. Block any shop URLs or contact handles in the body
3. Threat intel: check that company RDP is not exposed to the internet and that SMTP/cPanel accounts use MFA
4. Low priority: no lure targeting users

## 9. Lesson learned
Not every malicious email targets the reader. Spam advertising criminal access shows what attackers trade, and why exposed RDP and weak hosting passwords get sold.
