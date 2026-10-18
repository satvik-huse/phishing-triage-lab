# Phishing Report: email-01

| Field | Value |
|---|---|
| Sample | sample-7712.eml (phishing_pot dataset) |
| Date received | 10 Feb 2026 10:17:24 -0800 |
| Verdict | MALICIOUS: advance-fee / donation scam |
| Impersonates | Dieter Schwarz (Lidl / Schwarz Group owner) |
| Analyst | Satvik Huse |

## 1. Summary
An unsolicited email claiming to be from billionaire Dieter Schwarz was sent from a genuine Brazilian company mailbox that appears to be compromised. Replies are redirected to an attacker-controlled Gmail address. No links or attachments: the goal is to start a conversation and later request fees or personal data.

## 2. Header analysis
| Header | Value | Finding |
|---|---|---|
| From | edmilson.oliveira@minc.ind.br | Legitimate domain |
| Return-Path | edmilson.oliveira@minc.ind.br | Matches From |
| Reply-To | schwarzdieter709@gmail.com | MISMATCH: attacker address |
| Subject | AW: Mein Name ist Dieter Schwarz. | Fake "AW:" (German "Re:") reply prefix |
| SPF / DKIM / DMARC | pass / pass / pass | Sent from the domain's real server |
| Sending server | webmail.minc.omid.com.br [45.189.95.57] | Company webmail |

## 3. Enrichment
| Indicator | Tool | Result |
|---|---|---|
| 45.189.95.57 | AbuseIPDB | No abuse reports |
| minc.ind.br | urlscan.io | Real 6-year-old business site, Hostinger, Sao Paulo, Brazil |
| URLs | grep | None found |
| Attachments | grep | None found |

## 4. IOCs
- schwarzdieter709@gmail.com (attacker reply address, primary IOC)
- edmilson.oliveira@minc.ind.br (likely compromised mailbox)
- 45.189.95.57 (legitimate but abused mail server)

## 5. Red flags
- Reply-To differs from From and uses free Gmail
- Unknown sender claiming to be a famous billionaire
- Fake reply subject to suggest an existing conversation
- German subject sent from a Brazilian domain

## 6. MITRE ATT&CK
- T1566 Phishing

## 7. Recommended actions
1. Block and quarantine mail with Reply-To schwarzdieter709@gmail.com across all mailboxes
2. Do not block minc.ind.br (legitimate business); watchlist 45.189.95.57
3. Report as phishing; notify minc.ind.br that the account appears compromised
4. If any user replied, check what they shared and alert them

## 8. Lesson learned
SPF, DKIM and DMARC passing only proves which server sent the email, not that the sender can be trusted. Compromised legitimate accounts pass every check, so the Reply-To and the content decide the verdict.

## Evidence
VirusTotal lookup of sending IP 45.189.95.57, re-checked 2026-10-18: 0/92 vendors flagged it.

![VirusTotal result for 45.189.95.57](../screenshots/email-01-virustotal.png)
