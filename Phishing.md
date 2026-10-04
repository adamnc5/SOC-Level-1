# Phishing Analysis — Investigating Suspicious Emails

[Back to overview](README.md)

Phishing analysis is one of the core skills I studied in TryHackMe's SOC Level 1 pathway. These notes cover inspecting suspicious emails, investigating their artifacts, and connecting the findings to a wider incident.

## Rooms Covered

| Room / Scenario | Focus |
| --- | --- |
| Introduction to Phishing | Getting started with phishing alert triage in the SOC Simulator. |
| Phishing Analysis Fundamentals | Email structure, delivery, headers, and message source. |
| Phishing Emails in Action | Recognizing techniques used in phishing messages. |
| Phishing Analysis Tools | Collecting and investigating email artifacts. |
| Phishing Prevention | Email authentication and defensive controls. |
| The Greenholt Phish | Investigating a suspicious email and attachment. |
| Snapped Phish-ing Line | Connecting emails and URLs to a phishing campaign. |
| Phishing Unfolding | Investigating an unfolding attack through SOC alerts. |

## 1. Understanding the Email

I learned to inspect both the displayed message and its raw source. The source can reveal information hidden behind formatting, images, or misleading link text.

Email delivery also matters: SMTP transfers messages, while IMAP and POP3 provide ways to retrieve them.

| Evidence | What I check |
| --- | --- |
| From | Does the actual address match the claimed sender? |
| Reply-To | Would a reply go somewhere unexpected? |
| To / Subject / Date | Who was targeted, with what message, and when? |
| Received headers | What route does the message appear to have taken? |
| Authentication-Results | What did the receiving system report about authentication? |
| HTML body | Where do links actually point? |
| Attachment metadata | What filename, content type, and encoding are declared? |

Header fields can be forged. I use trusted receiving-system evidence and supporting context rather than accepting every field at face value.

## 2. Recognizing Phishing Techniques

The example messages showed how attackers encourage a user to act:

- **Impersonation:** copying a familiar brand or sender identity.
- **Urgency:** claiming an account will close or a payment needs immediate attention.
- **Link manipulation:** displaying reassuring text while pointing elsewhere.
- **Redirection:** using shortened links or several intermediate destinations.
- **Credential harvesting:** directing the recipient to a fake login page.
- **Attachment lures:** disguising a harmful file as an invoice or document.
- **Tracking pixels:** loading remote content that can reveal message interaction.

A message does not need poor spelling to be phishing. I focus on what it asks the recipient to do and whether the technical evidence supports its claims.

## 3. Investigating Links and Attachments

My approach is to extract useful artifacts and preserve their connection to the original email.

For links, I examine the actual hostname, path, and any redirects. A familiar brand name somewhere in a URL does not establish ownership.

For attachments, I check the file's actual type and calculate a hash before researching it. A displayed filename or declared MIME type is not enough to establish what the file contains.

Illustrative Linux commands:

```bash
file attachment.bin
sha256sum attachment.bin
```

These identify the file type and calculate its SHA-256 hash without executing it.

Tools covered across the lessons and supporting research include:

| Tool | Purpose |
| --- | --- |
| Thunderbird / message source | Inspect the original email and raw content. |
| Header analyzers, such as MXToolbox | Make header information easier to examine. |
| CyberChef | Decode content such as Base64. |
| VirusTotal | Add reputation and analysis context to artifacts. |
| PhishTool | Bring email evidence together for investigation. |
| Malware sandboxes | Observe file behavior in a controlled environment. |

Base64 is an encoding method, not proof of malware. Similarly, a clean reputation result does not prove an artifact is harmless.

Suspicious content belongs in an isolated analysis environment. Private messages or attachments should not be submitted to public analysis services without authorization.

## 4. Understanding Email Protection

| Control | What it does |
| --- | --- |
| SPF | Checks whether the sending server is authorized for the envelope-sender domain. |
| DKIM | Uses a domain's cryptographic signature to verify signed message content. |
| DMARC | Requires an aligned SPF or DKIM pass with the visible From domain; also supports policy and reporting. |
| S/MIME | Supports certificate-based message signing and encryption. |

DMARC does not require both SPF and DKIM to pass: one passing, aligned mechanism can be sufficient.

Authentication helps assess the sending identity. It does not guarantee safe content: attackers can use their own authenticated domains or compromised accounts.

Other protections include attachment analysis, URL filtering, user reporting, and awareness training.

## 5. Applying the Skills in Challenges

### The Greenholt Phish

This challenge brings together email-source inspection, sender and domain checks, and attachment analysis.

The main lesson is to connect several pieces of evidence. The message's story, its sender information, and its attachment should be assessed together.

### Snapped Phish-ing Line

This challenge expands the investigation from individual emails to related URLs and phishing infrastructure.

The main lesson is to look for connections between messages. Shared artifacts can help reveal a campaign and identify its scope.

### Introduction to Phishing and Phishing Unfolding

The simulator scenarios connect email analysis with SOC work: reviewing alerts, assessing evidence, and documenting decisions.

Phishing Unfolding adds the wider attack context. Investigating the email is only part of the job; related endpoint and network activity can show what happened afterward.

## 6. A Workflow I Can Reuse

1. Preserve the original message and record its recipients and timestamps.
2. Inspect sender details, headers, and authentication results.
3. Examine the body, actual link destinations, and attachments.
4. Enrich relevant artifacts using appropriate analysis tools.
5. Search available logs for related messages, clicks, downloads, or execution.
6. Document the assessment, supporting evidence, affected entities, and next steps.

I distinguish **delivery**, **interaction**, and **compromise**. Receiving a phishing email does not prove that someone clicked it or that a device was compromised.

## Main Takeaway

A strong phishing assessment explains why a message is suspicious, what evidence supports that conclusion, and what impact can actually be established.

## References

### Official rooms

- [Phishing Analysis Fundamentals](https://tryhackme.com/room/phishingemails1tryoe)
- [Phishing Emails in Action](https://tryhackme.com/room/phishingemails2rytmuv)
- [Phishing Analysis Tools](https://tryhackme.com/room/phishingemails3tryoe)
- [Phishing Prevention](https://tryhackme.com/room/phishingemails4gkxh)
- [The Greenholt Phish](https://tryhackme.com/room/phishingemails5fgjlzxc)
- [Snapped Phish-ing Line](https://tryhackme.com/room/snappedphishingline)
- [Introduction to Phishing — Simulator](https://tryhackme.com/soc-sim/scenarios?scenario=introduction-to-phishing)
- [Phishing Unfolding — Simulator](https://tryhackme.com/soc-sim/scenarios?scenario=phishing-unfolding-v2)


These notes were reconstructed after completing the rooms using official material and supporting walkthroughs. Commands are revision examples; no personal lab outputs or simulator scores are claimed.
