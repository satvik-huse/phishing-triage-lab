# Phishing Report: email-03

| Field | Value |
|---|---|
| Sample | sample-5379.eml (phishing_pot dataset) |
| Date received | Fri, 04 Jul 2025 01:32:50 -0700 |
| Verdict | MALICIOUS: advance-fee / donation scam |
| Impersonates | Dieter Schwarz (Lidl / Schwarz Group owner) |
| Campaign | Same campaign as [email-01](email-01.md) |
| Analyst | Satvik Huse |

## 1. Summary
A Dieter Schwarz donation scam sent from a compromised Pakistan Ministry of Interior mailbox (aodev@interior.gov.pk). The attacker connected from 185.241.208.113, sent through the government's hosting provider, and redirected replies to a Gmail address. Same lure and same Reply-To pattern as email-01, so this is one ongoing campaign that rotates hijacked mailboxes.

## 2. Header analysis
| Header | Value | Finding |
|---|---|---|
| From | aodev@interior.gov.pk | Real government domain, likely compromised account |
| Return-Path | aodev@interior.gov.pk | Matches From |
| Reply-To | dieterschwarz787@gmail.com | MISMATCH: attacker address |
| Subject | [Spam] | Tagged as spam by a mail gateway on the way |
| Origin (first Received hop) | [185.241.208.113] port 62703 | Attacker's machine that logged into the mailbox |
| Relay | host201701.comsatshosting.com, then symantec3.comsats.net.pk [210.56.11.6] | Legitimate Pakistani hosting / mail filter |
| SPF / DKIM / DMARC | pass / none / pass (p=none) | Passed because the real server sent it; p=none means no enforcement |

## 3. Enrichment
| Indicator | Tool | Result |
|---|---|---|
| 185.241.208.113 | VirusTotal | 7/92 vendors flag as malicious |
| interior.gov.pk | urlscan.io / VirusTotal | Clean, 11-year-old domain on Akamai (Frankfurt); site geoblocks foreign visitors |
| 210.56.11.6 | Header review | Legitimate relay (symantec3.comsats.net.pk) |

## 4. IOCs
- dieterschwarz787@gmail.com (attacker reply address, primary IOC)
- 185.241.208.113 (attacker source IP)
- aodev@interior.gov.pk (likely compromised mailbox)

## 5. Campaign link to email-01
| | email-01 | email-03 |
|---|---|---|
| Lure | Dieter Schwarz donation | Dieter Schwarz donation |
| Reply-To | schwarzdieter709@gmail.com | dieterschwarz787@gmail.com |
| Sender | Hijacked Brazilian company mailbox | Hijacked Pakistani government mailbox |
| Auth checks | Passed | Passed |

## 6. Red flags
- Reply-To differs from From and uses free Gmail
- Government ministry address sending a celebrity donation offer
- Gateway tagged the subject [Spam]
- Sent from an external IP flagged by 7 security vendors

## 7. MITRE ATT&CK
- T1566 Phishing
- T1078 Valid Accounts (compromised mailbox used to send)

## 8. Recommended actions
1. Block and quarantine mail with Reply-To dieterschwarz787@gmail.com; hunt for other Gmail Reply-To addresses containing "dieter" or "schwarz"
2. Block 185.241.208.113 at the email gateway / firewall
3. Do not block interior.gov.pk (legitimate); notify their team that aodev@ appears compromised
4. If any user replied, check what they shared

## 9. Lesson learned
The first Received hop shows where the attacker really connected from. Linking IOCs across emails (same lure, similar Reply-To) turns single alerts into a campaign, which is how SOC analysts find the bigger picture.

## Evidence
VirusTotal lookup of sending IP 185.241.208.113, re-checked 2026-10-18: 6/92 vendors flagged it (7/92 at first analysis).

![VirusTotal result for 185.241.208.113](../screenshots/email-03-virustotal.png)
