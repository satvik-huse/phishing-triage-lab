# Phishing Triage Lab
Hands-on investigation of real-world phishing emails from the public [phishing_pot](https://github.com/rf-peixoto/phishing_pot) dataset, worked the way an L1 SOC analyst would triage a reported email.

## Method
1. **Headers**: From, Reply-To, Return-Path, Received chain, SPF / DKIM / DMARC
2. **Enrichment**: sender IP and domain reputation (VirusTotal, AbuseIPDB, urlscan.io)
3. **Links and attachments**: extracted with grep and checked by reputation only, never opened
4. **Verdict**: severity, IOCs with actions, red flags, MITRE ATT&CK mapping, response steps
5. **Evidence**: VirusTotal screenshot in every report

## Reports
| # | Email | Verdict | Severity | MITRE | Report |
|---|---|---|---|---|---|
| 01 | Dieter Schwarz donation scam from a likely compromised company mailbox | Malicious | Medium | T1566, T1078 | [Report 01](reports/email-01.md) |
| 02 | Hacking-tools spam sent by a script on a hacked website | Malicious spam | Low | T1584.004 | [Report 02](reports/email-02.md) |
| 03 | Dieter Schwarz scam via a likely compromised government mailbox (campaign with 01) | Malicious | Medium | T1566, T1078 | [Report 03](reports/email-03.md) |
| 04 | Black-market ad selling shells, cPanels, SMTPs and RDPs | Malicious spam | Low | T1650 | [Report 04](reports/email-04.md) |

## Key findings so far
- **SPF / DMARC "pass" is not trust**: reports 01 and 03 passed authentication because real mailboxes were abused.
- **Reply-To is the giveaway**: both scam emails send replies to free Gmail addresses.
- **Campaign linking**: same lure and Reply-To pattern across two countries' mailboxes.
- **Clean reputation is a snapshot**: 3 of 4 sending IPs score 0/92 today despite clearly malicious content.

## Safety
All samples were inspected as text in an isolated Ubuntu VM. No links were clicked and no attachments were opened.

## Tools
Ubuntu, grep, VirusTotal, AbuseIPDB, urlscan.io, CyberChef, MXToolbox

## Coming next
- Reports 05 to 10
- `phish_triage.py`: Python tool to automate IOC extraction and enrichment
