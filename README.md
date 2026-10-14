# Phishing Triage Lab
Hands-on investigation of real-world phishing emails from the public
phishing_pot dataset, done like an L1 SOC analyst.

## What I do for each email
- Header analysis (From, Reply-To, Return-Path, SPF/DKIM/DMARC)
- Sender IP reputation (AbuseIPDB)
- URL analysis (urlscan.io, VirusTotal)
- Attachment hashing without opening it (CyberChef, VirusTotal)
- Verdict, IOCs, MITRE ATT&CK mapping, recommended actions

## Reports
| # | Type | Verdict | Report |
|---|---|---|---|
| 01 | Advance-fee scam from compromised mailbox | Malicious | [email-01](reports/email-01.md) |
| 02 | Cybercrime-tools spam sent from hacked website | Malicious spam (low) | [email-02](reports/email-02.md) |
| 03 | Advance-fee scam from hijacked government mailbox (same campaign as 01) | Malicious | [email-03](reports/email-03.md) |

## Tools
Ubuntu, grep, AbuseIPDB, urlscan.io, VirusTotal, CyberChef, MXToolbox

## Coming next
Python tool (phish_triage.py) to automate IOC extraction and enrichment.
