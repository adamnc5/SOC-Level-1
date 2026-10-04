# Network Security Monitoring

[Back to overview](README.md)

This was one of my favorite sections of TryHackMe's SOC Level 1 pathway. I learned how network evidence can reveal discovery, suspicious data transfers, and attempts to intercept communication.

## Rooms Covered

- Network Security Essentials
- Network Discovery Detection
- Data Exfiltration Detection
- Man-in-the-Middle Detection
- IDS Fundamentals

My Snort commands and detection-rule notes are documented separately in [Snort.md](Snort.md).

## 1. Network Security Essentials

Before investigating traffic, I need to understand the network: which systems exist, what they do, and which connections are expected.

Different evidence sources answer different questions:

| Evidence source | What it helps establish |
| --- | --- |
| Firewall logs | Which connections were allowed or blocked. |
| DNS logs | Which domains systems queried and the answers returned. |
| Proxy logs | Web destinations and requests visible to the proxy. |
| Flow records | Who communicated, for how long, and how much data moved. |
| Packet captures | Protocol details and payloads where visible. |
| Endpoint logs | Which process or user may explain the network activity. |

Useful fields include timestamps, source and destination IPs, ports, protocols, connection outcomes, and transferred bytes.

**Key lesson:** A firewall allowing traffic does not make that traffic safe. A blocked attempt also does not prove that a host was compromised.

## 2. Network Discovery Detection

Attackers use discovery to identify reachable systems, open ports, services, and potential weaknesses.

I learned to distinguish two separate questions: **where the scan originates** and **how it moves across targets**.

| Scan category | Meaning |
| --- | --- |
| External | Originates outside the monitored network. |
| Internal | Originates within the monitored network. |
| Horizontal | Probes the same service or port across multiple hosts. |
| Vertical | Probes multiple ports on one host. |

### What I Look For

- One source contacting many destination addresses or ports.
- Repeated connection attempts within a short period.
- ICMP echo requests moving across a range of hosts.
- TCP SYN attempts with few completed connections.
- Repeated UDP probes and related responses.

An internal scan deserves context: it could come from a compromised workstation or an authorized vulnerability scanner.

I compare the source with asset inventories, approved scanning activity, and the surrounding timeline.

**Key lesson:** Scanning behavior identifies something to investigate. It does not establish malicious intent by itself.

## 3. Data Exfiltration Detection

Data exfiltration is the unauthorized transfer of information out of an environment. The room covered how ordinary protocols can become transfer channels.

| Channel | Clues worth investigating |
| --- | --- |
| DNS | Repeated long or encoded-looking subdomains, unusual query volume, or concentration on an unfamiliar domain. |
| FTP | Unexpected uploads, unfamiliar external servers, or transfers inconsistent with the host's role. |
| HTTP | Unusual outbound uploads or repeated requests carrying unexpected data. |
| ICMP | Repeated packets with unusual payload content, sizes, or communication patterns. |

These are indicators, not standalone proof. Legitimate applications can also generate long DNS names, frequent requests, or large uploads.

### How I Would Investigate

1. Identify the source host and transfer destination.
2. Compare the activity with normal behavior.
3. Examine timing, volume, protocol details, and available content.
4. Correlate the traffic with endpoint processes and file activity.
5. Explain what supports an exfiltration assessment and what remains unknown.

**Key lesson:** Data theft does not always produce one large transfer. Small, repeated transmissions can also matter.

## 4. Man-in-the-Middle Detection

A man-in-the-middle attack places an attacker between communicating systems so they can intercept or manipulate traffic.

The room focused on three techniques.

### ARP Spoofing

An attacker sends misleading ARP information to associate another device's IP address—such as the gateway—with the attacker's MAC address.

I look for unexpected changes in IP-to-MAC mappings, competing claims for an address, and suspicious ARP activity.

A mapping change still needs verification: legitimate infrastructure changes and failover can also affect it.

### DNS Spoofing

Manipulated DNS responses can redirect a system toward an unintended destination.

I compare the query, responding server, returned address, and expected resolution. Different answers alone are insufficient because load balancing and CDNs can legitimately return multiple addresses.

### SSL Stripping

An attacker attempts to keep the victim's connection on unencrypted HTTP when HTTPS should be used.

I examine unexpected HTTP activity, redirects, and exposed request content. This does not mean the attacker has broken TLS encryption.

**Key lesson:** Establish what communication should look like, then investigate how the observed traffic differs.

## 5. IDS Fundamentals

An Intrusion Detection System monitors activity and generates alerts for suspicious patterns.

| Concept | Meaning |
| --- | --- |
| Network-based IDS | Monitors traffic visible at its network location. |
| Host-based IDS | Monitors activity on an individual system. |
| Signature-based detection | Looks for known patterns. |
| Anomaly-based detection | Looks for deviations from expected behavior. |

An IDS generally alerts; an IPS can block traffic when deployed and configured for prevention.

Detection also has limits. Sensor placement, encrypted traffic, incomplete logs, and rule quality affect what can be observed.

**Key lesson:** An alert is a starting point. I need to understand its logic and inspect the supporting evidence.

## Useful Wireshark Filters

These are revision examples, not saved commands from my original lab sessions.

| Display filter | Purpose |
| --- | --- |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | Show TCP SYN packets without ACK set. |
| `arp.opcode == 2` | Show ARP replies. |
| `dns` | Focus on traffic dissected as DNS. |
| `icmp` | Focus on IPv4 ICMP traffic. |
| `http.request.method == "POST"` | Show visible HTTP POST requests. |

These filters narrow the capture; they do not automatically identify attacks. Normal connections start with SYN packets, ARP replies are routine, and HTTP POST is widely used legitimately.

Encrypted HTTPS requests are not readable as ordinary HTTP without suitable decryption material.

## My Investigation Checklist

- What triggered the investigation?
- Which hosts and time window are involved?
- Is this behavior expected for the source system?
- Which logs or packets support the suspicion?
- What additional evidence would distinguish an attempt from a successful attack?
- What should be documented or escalated?

## Main Takeaway

Network monitoring is about connecting patterns with context. My goal is to explain who communicated, what happened, why it matters, and how strongly the evidence supports the conclusion.

## References

- [TryHackMe — Network Security Essentials](https://tryhackme.com/room/networksecurityessentials)
- [TryHackMe — Network Discovery Detection](https://tryhackme.com/room/networkdiscoverydetection)
- [TryHackMe — Data Exfiltration Detection](https://tryhackme.com/room/dataexfildetection)
- [TryHackMe — Man-in-the-Middle Detection](https://tryhackme.com/room/mitmdetection)
- [TryHackMe — IDS Fundamentals](https://tryhackme.com/room/idsfundamentals)
  

These notes summarize the rooms I completed. Filters and investigation checklists are revision aids, not saved outputs or findings from my original lab sessions.
