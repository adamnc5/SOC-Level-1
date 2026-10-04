# Cyber Defence Frameworks

[Back to overview](README.md)

Through this section of TryHackMe's SOC Level 1 pathway, I learned how frameworks help analysts organize an investigation, describe attacker behavior, and improve detection.

## Rooms Covered

- Pyramid Of Pain
- Cyber Kill Chain
- Unified Kill Chain
- MITRE
- Summit
- Eviction

## Understanding the Differences

| Framework | Main question it helps answer |
| --- | --- |
| Pyramid of Pain | How difficult would it be for an attacker to adapt to our detection? |
| Cyber Kill Chain | Where does the observed activity fit within an intrusion? |
| Unified Kill Chain | How does an attacker gain access, move through an environment, and achieve objectives? |
| MITRE ATT&CK | Which specific behaviors are being used? |

These frameworks complement each other. They organize the evidence; they do not replace it.

## 1. Pyramid of Pain

The Pyramid of Pain ranks indicators by how difficult they are for an attacker to change when defenders detect or block them.

From the bottom of the pyramid to the top:

| Level | Example | Attacker adaptation |
| --- | --- | --- |
| Hash values | Hash of a malicious file | Modify the file's contents. |
| IP addresses | Address of an attacker-controlled server | Move to another address. |
| Domain names | Domain used for malicious communication | Replace the domain. |
| Network / host artifacts | Distinctive traffic patterns or system changes | Modify parts of the operation. |
| Tools | Software used to perform an attack | Replace or substantially modify tooling. |
| Tactics, techniques, and procedures | The attacker's methods and workflows | Change how the operation is carried out. |

### What I Learned

Blocking a known malicious hash can help immediately, but an altered sample may no longer match it.

Detecting underlying behavior can remain useful even when filenames, hashes, or infrastructure change.

**Key takeaway:** Combine specific indicators with behavioral detection. The lower levels remain useful; they simply tend to be easier to evade.

## 2. Cyber Kill Chain

The Cyber Kill Chain describes seven stages of an intrusion.

| Stage | What happens |
| --- | --- |
| Reconnaissance | Gather information about the target. |
| Weaponization | Prepare a malicious capability for delivery. |
| Delivery | Send or introduce it to the target. |
| Exploitation | Exploit a weakness to enable malicious activity. |
| Installation | Establish a malicious presence on the system. |
| Command and Control | Create a channel for attacker communication or control. |
| Actions on Objectives | Pursue the goal, such as stealing information or disrupting services. |

### How It Helps an Investigation

A suspicious email may relate to delivery. Later execution and outbound communication may reveal additional stages.

This helps me ask what happened before an alert and what evidence to look for afterward.

**Key takeaway:** Interrupting an attack early can prevent later damage, but real incidents do not always appear as a neat sequence in the logs.

## 3. Unified Kill Chain

The Unified Kill Chain expands the attack lifecycle into 18 phases. It combines ideas from other models and gives more attention to activity inside the environment.

Its phases can be understood through three broad goals:

| Goal | Focus |
| --- | --- |
| In | Establish an initial foothold. |
| Through | Expand access and move toward valuable assets. |
| Out | Act on the attacker's objectives. |

This includes behavior such as persistence, discovery, privilege escalation, lateral movement, collection, and exfiltration.

Attackers may repeat phases or move between them as their access and objectives change.

**Key takeaway:** Initial access is only the beginning. An investigation should also consider how access develops and what the attacker ultimately wants.

## 4. MITRE ATT&CK

MITRE ATT&CK is a knowledge base of adversary behavior grounded in observed activity.

### Tactics, Techniques, and Procedures

| Term | Meaning |
| --- | --- |
| Tactic | Why the attacker performs an action: the objective. |
| Technique | How the attacker pursues that objective. |
| Sub-technique | A more specific form of a technique. |
| Procedure | The particular implementation observed in an operation. |

### Example Mappings

These are illustrative examples, not findings from my original labs.

| Observed behavior | Possible ATT&CK mapping |
| --- | --- |
| A targeted phishing email delivers a malicious attachment. | Spearphishing Attachment — T1566.001 |
| An attacker executes commands through PowerShell. | PowerShell — T1059.001 |

The mapping must match the evidence. Simply seeing PowerShell execute does not establish malicious activity.

### Other Resources Covered

- **ATT&CK Navigator:** visualize and annotate techniques, such as those associated with a threat group.
- **CAR:** a repository of defensive analytics.
- **D3FEND:** a knowledge graph of defensive techniques.

**Key takeaway:** ATT&CK provides a shared vocabulary for describing behavior. A technique match alone does not identify the attacker.

## 5. Summit — Applying the Pyramid of Pain

Summit turns the Pyramid of Pain into a practical challenge against a simulated adversary.

The exercise progresses from detecting specific malware and infrastructure toward examining artifacts and behavior as the adversary adapts.

### What the Challenge Reinforced

- A detection can work against one sample and miss its replacement.
- Changing infrastructure can defeat narrowly targeted blocking.
- Host and network observations can support more durable detections.
- Command activity can reveal the method behind an attack.

**Main lesson:** As detection moves beyond easily replaced indicators, the attacker must change more of the operation to avoid it.

## 6. Eviction — Applying MITRE ATT&CK

Eviction uses a fictional E-Corp scenario involving intelligence about APT28. The exercise uses an ATT&CK Navigator layer to examine relevant attacker techniques.

### What the Challenge Reinforced

- Use threat intelligence to decide which behaviors deserve investigation.
- Follow techniques across initial access, execution, persistence, and later activity.
- Connect a technique with the evidence needed to investigate it.
- Treat reported group behavior as a hunting lead, not proof that the group is present.

**Main lesson:** Threat intelligence becomes more useful when it leads to specific investigative questions.

## How I Can Use These Frameworks Together

For an illustrative phishing-led intrusion:

1. Use the **Cyber Kill Chain** to organize the broad sequence.
2. Use **ATT&CK** to describe the specific behaviors supported by evidence.
3. Use the **Unified Kill Chain** to consider internal movement and objectives.
4. Use the **Pyramid of Pain** to assess how easily the attacker could evade proposed detections.

## Main Takeaway

Frameworks help me move from “this looks suspicious” to a clearer explanation of the activity, its place in an attack, and the evidence or detection needed next.

## References

- [TryHackMe — Pyramid Of Pain](https://tryhackme.com/room/pyramidofpainax)
- [TryHackMe — Cyber Kill Chain](https://tryhackme.com/room/cyberkillchainzmt)
- [TryHackMe — Unified Kill Chain](https://tryhackme.com/room/unifiedkillchain)
- [TryHackMe — MITRE](https://tryhackme.com/room/mitre)
- [TryHackMe — Summit](https://tryhackme.com/room/summit)
- [TryHackMe — Eviction](https://tryhackme.com/room/eviction)
- [Unified Kill Chain — Official Website](https://www.unifiedkillchain.com/)
- [MITRE ATT&CK — Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001/)
- [MITRE ATT&CK — PowerShell](https://attack.mitre.org/techniques/T1059/001/)

These notes summarize the rooms I completed. Examples explain the concepts; they are not reconstructed findings or saved outputs from my original lab sessions.
