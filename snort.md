# Snort — Network Intrusion Detection

[Back to overview](README.md)

These notes cover the **Snort** and **Snort Challenge — The Basics** rooms I completed on TryHackMe. They summarize the concepts and exercises, with example commands and rules for revision.

## What I Learned

- The difference between detecting suspicious traffic (IDS) and preventing it (IPS).
- How to inspect live traffic and analyze saved packet captures.
- How rule headers and options control what Snort detects.
- How to inspect alerts and troubleshoot detection rules.

The commands below follow the **Snort 2 environment used in the rooms**.

## Useful Commands

| Command | Purpose |
| --- | --- |
| `snort -V` | Display the installed version. |
| `sudo snort -T -c /etc/snort/snort.conf` | Validate the configuration. |
| `sudo snort -v -i eth0` | Inspect live traffic on the selected interface. |
| `sudo snort -c /etc/snort/snort.conf -r sample.pcap -A console` | Analyze a capture using the configured rules and display alerts. |

Replace the interface and capture filename with the ones in your environment. The configuration must load the rules you want to use.

**Important flags:** `-i` selects an interface, `-r` reads a capture, `-c` selects a configuration, and `-T` validates it.

## Understanding a Rule

This example detects TCP packets traveling to or from port 80:

```text
alert tcp any any <> any 80 (msg:"LAB TCP port 80 traffic"; sid:1000001; rev:1;)
```

| Part | Meaning |
| --- | --- |
| `alert` | Generate an alert when the rule matches. |
| `tcp` | Inspect TCP traffic. |
| `any any` | Any source address and source port. |
| `<>` | Match traffic in either direction. |
| `any 80` | Match port 80 on the other side of the connection. |
| `msg` | The message attached to the alert. |
| `sid` | The rule's identifier. |
| `rev` | The rule's revision number. |

This is a practice rule. Port 80 traffic is not automatically malicious, and matching that port does not prove the traffic is HTTP.

## Exercises Covered

The challenge included:

- Writing rules for traffic involving HTTP and FTP ports.
- Matching FTP response content.
- Detecting image-file signatures and torrent-related content.
- Correcting rule syntax errors.
- Using supplied signatures for MS17-010 and Log4j-related traffic.
- Inspecting generated logs for packet details.

These exercises connected rule-writing with reading the evidence behind an alert.

## My Investigation Workflow

1. Identify the capture and the behavior the rule should detect.
2. Check the protocol, addresses, ports, and direction.
3. Add the relevant rule options.
4. Run Snort against the capture.
5. Inspect matching packets and explain why they triggered.
6. Refine the rule if it matches unrelated traffic or misses the intended activity.

## Main Takeaways

A rule can be syntactically correct and still detect the wrong traffic. Direction, ports, and content matching all affect the result.

An alert shows that traffic matched a rule. I still need to inspect the context before calling it an attack.

## References

- [TryHackMe — Snort](https://tryhackme.com/room/snort)
- [TryHackMe — Snort Challenge: The Basics](https://tryhackme.com/room/snortchallenges1)
- [Supplementary walkthrough — Isiah](https://medium.com/@isiahjohnstone/snort-03c82f965a3d)

These notes were reconstructed after completing the rooms. The examples are for revision; they are not saved outputs from my original lab sessions.
