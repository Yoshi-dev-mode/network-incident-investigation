<!--
  Network Incident Investigation Portfolio
  Cybersecurity learning portfolio project
-->

<div align="center">

# 🛡️ Network Incident Investigation

**Packet-level analysis of network incidents using DNS, TCP, and HTTP evidence**

A hands-on cybersecurity portfolio documenting three network investigation activities through protocol analysis, evidence-based reporting, and practical follow-up recommendations.

<br/>

![Focus](https://img.shields.io/badge/Focus-Network%20Security-2563eb?style=for-the-badge)
![Tools](https://img.shields.io/badge/Analysis-tcpdump%20%7C%20Network%20Logs-0f766e?style=for-the-badge)
![Cases](https://img.shields.io/badge/Cases-3-7c3aed?style=for-the-badge)
![Evidence](https://img.shields.io/badge/Approach-Evidence%20Based-d97706?style=for-the-badge)

[Combined Incident Report](reports/incident-report.md) · [DNS Packet Analysis](evidence/tcpdump-analysis.md) · [TCP SYN Flood Analysis](evidence/tcp-syn-flood-analysis.md) · [SYN Flood Report](reports/tcp-syn-flood-report.md) · [DNS & HTTP Activity Report](reports/dns-http-activity-report.md)

</div>

---

## 📌 Project Overview

This repository brings together **three cybersecurity training activities** that investigate network behavior using packet-level and log evidence.

- **Case 01 — DNS Resolution Failure:** DNS requests over UDP to port 53 are followed by ICMP errors reporting that the destination UDP port is unreachable.
- **Case 02 — Suspected TCP SYN Flood:** A TCP log workbook contains repeated SYN packets targeting port 443, alongside server SYN-ACK responses and indications that some ordinary connections failed during the event.
- **Case 03 — Suspicious DNS and HTTP Activity:** DNS lookups resolve two different domains to different IP addresses, followed by successful TCP handshakes and HTTP GET requests to both websites. The domain change may warrant investigation if it was unexpected, but the excerpt alone does not confirm malicious activity.

The cases are documented separately because their protocols, symptoms, and interpretations differ. Together, they demonstrate a structured approach to observing network behavior, developing hypotheses, documenting limitations, and recommending further investigation.

> **Important:** These are training-scenario findings. The available evidence supports investigation hypotheses, not definitive root causes, confirmed attacker identities, or proof of real-world compromise.

## 🗂️ Incident Overview

| | Case 01: DNS Resolution Failure | Case 02: Suspected TCP SYN Flood | Case 03: Suspicious DNS & HTTP Activity |
|:--|:--|:--|:--|
| **Primary evidence** | Scenario-described `tcpdump` traffic | Supplied TCP log workbook, including a color-coded sheet | Supplied `tcpdump` excerpt |
| **Key protocols** | DNS, UDP, ICMP | TCP | DNS, TCP, HTTP |
| **Ports / indicators** | UDP destination port 53 | TCP destination port 443; repeated `[SYN]` and server `[SYN, ACK]` | DNS A records; HTTP over port 80; `GET /` requests |
| **Observed symptom** | Domain name resolution fails | Repeated connection attempts; some normal connections appear to fail during the event | Machine accesses two different domains at separate times |
| **Working interpretation** | DNS service or network path problem | Pattern is consistent with a TCP SYN flood denial-of-service attempt | Unexpected domain change may be suspicious and merits further review |
| **Confidence / limitation** | Exact cause not confirmed | Attack pattern is suspected; distributed origin and source identity are not established | Maliciousness and user intent are not established by the excerpt |

## 🔎 Case 01 — DNS Resolution Failure

Users reported they could not access `www.yummyrecipesforme.com`. The scenario's `tcpdump` summary showed DNS requests sent from client `192.51.100.15` to DNS server `203.0.113.2` over UDP, followed by ICMP errors stating **`udp port 53 unreachable`**.

```text
Client (192.51.100.15)
        |
        | DNS query over UDP / destination port 53
        v
DNS server (203.0.113.2)
        |
        | ICMP: "udp port 53 unreachable"
        v
Name resolution fails; browser cannot proceed normally
```

**Interpretation:** The client could not successfully resolve the domain during the observed attempts. Possible explanations include an unavailable DNS service, a firewall rule, or a network/DNS configuration issue. The scenario does not establish which explanation is correct or prove malicious activity.

**Detailed evidence:** [DNS packet analysis](evidence/tcpdump-analysis.md) · [Combined incident report](reports/incident-report.md)

## 🚦 Case 02 — Suspected TCP SYN Flood

The supplied TCP log workbook shows repeated SYN packets from `203.0.113.0` using source port `54770` to destination `192.0.2.1` on TCP port `443`. The server also sends SYN-ACK responses. The color-coded workbook marks suspicious traffic, and some normal TCP connections appear to fail during the event.

A TCP connection normally begins with a three-way handshake:

```text
Client                         Server
  | -------- SYN -------------> |
  | <------ SYN, ACK ---------- |
  | -------- ACK -------------> |  Connection established
```

A SYN flood can send many connection-opening requests and leave connections incomplete while the server waits for the final ACK. If enough connection state accumulates, resources may be strained and legitimate connections may time out or fail.

**Interpretation:** The repeated SYN pattern is consistent with a suspected TCP SYN flood denial-of-service event. The log alone does **not** prove that the activity was distributed (DDoS), identify who controlled the source address, or confirm the precise cause of the connection failures.

**Detailed evidence:** [TCP SYN flood analysis](evidence/tcp-syn-flood-analysis.md) · [SYN flood incident report](reports/tcp-syn-flood-report.md)

## 🌐 Case 03 — Suspicious DNS and HTTP Activity

The supplied packet excerpt records DNS queries and HTTP activity involving two domains.

- At **14:18:32**, the machine queried `yummyrecipesforme.com`; DNS returned `203.0.113.22`.
- At **14:18:36**, a TCP three-way handshake completed with `yummyrecipesforme.com` over HTTP port 80. The machine sent `GET / HTTP/1.1`.
- At **14:20:32**, the machine queried `greatrecipesforme.com`; DNS returned `192.0.2.17`.
- At **14:25:29**, another TCP handshake completed with `greatrecipesforme.com` over port 80, followed by `GET / HTTP/1.1`.
- The excerpt mentions additional port 80 traffic, but those omitted packets cannot be independently analyzed here.

**Interpretation:** The machine resolved and connected to two different domains. If the second domain was not intentionally visited, this change is worth investigating. However, the excerpt alone does not establish that either domain is malicious, that the machine was compromised, or that a brute-force attack occurred.

**Recommended next steps:** Preserve the full packet capture; correlate with browser history, endpoint telemetry, DNS logs, and the complete HTTP requests/responses; and validate the domains/IPs using trusted security sources and organizational procedures.

**Detailed report:** [DNS and HTTP activity incident report](reports/dns-http-activity-report.md)

## 🧭 Investigation Workflow

1. **Define the symptom** — determine what users or services cannot do.
2. **Review the available evidence** — identify source/destination addresses, protocols, ports, flags, timestamps, and error messages.
3. **Explain protocol behavior** — connect packet observations to DNS resolution, TCP handshakes, or HTTP requests.
4. **Form a cautious hypothesis** — distinguish patterns consistent with an incident from a confirmed root cause.
5. **Recommend verification** — identify server-side logs, firewall rules, connection statistics, endpoint data, and additional captures needed.
6. **Document the outcome** — record evidence, impact, uncertainty, and follow-up actions.

## 🧰 Tools & Concepts

| Tool / Concept | How it is used |
|:--|:--|
| **tcpdump** | Command-line packet capture and inspection in DNS and HTTP scenarios |
| **DNS / UDP / port 53** | Name-resolution request and transport analysis |
| **ICMP** | Interpretation of the port-unreachable error |
| **TCP flags** | Recognition of SYN, SYN-ACK, and ACK connection-opening traffic |
| **TCP three-way handshake** | Explanation of how a connection is established |
| **HTTP / port 80 / GET** | Analysis of web requests in the DNS and HTTP activity case |
| **DoS vs. DDoS** | Distinguishing a suspected denial-of-service pattern from evidence of multiple distributed sources |
| **Incident reporting** | Separation of observations, hypotheses, impact, limitations, and next steps |

### Example DNS capture command

On a Linux system with `tcpdump` installed:

```bash
sudo tcpdump -i any -nn port 53
```

This is a suggested command for future investigation, not a claim that a new capture was run.

## 🛠️ Recommended Follow-up

### DNS case
- [ ] Confirm whether the DNS service is running and listening on the expected interface and port.
- [ ] Review firewall and network-device rules affecting DNS traffic.
- [ ] Inspect DNS server logs and collect additional packet evidence.
- [ ] Test name resolution after any identified issue is corrected.

### TCP SYN case
- [ ] Correlate packet timestamps with service availability and user reports.
- [ ] Review server TCP connection statistics, backlog/queue metrics, and system resource use.
- [ ] Compare traffic across multiple sources and time windows before deciding whether activity is distributed.
- [ ] Review firewall, load balancer, and server logs for corroborating evidence.
- [ ] Consider appropriate defensive controls with the network administrator, then validate their impact on legitimate traffic.

### DNS and HTTP activity case
- [ ] Determine whether the second domain was expected or user-initiated.
- [ ] Review browser history and endpoint telemetry around the recorded timestamps.
- [ ] Examine the full HTTP request/response data and the omitted port 80 traffic.
- [ ] Correlate DNS logs and validate domain/IP reputation using trusted sources.

These are proposed investigation steps; they are not represented as completed remediation.

## 📂 Repository Structure

| Path | Purpose |
|:--|:--|
| `reports/incident-report.md` | Combined overview of the original DNS and suspected TCP SYN flood cases |
| `reports/tcp-syn-flood-report.md` | Focused written report for the suspected TCP SYN flood |
| `reports/dns-http-activity-report.md` | Report for suspicious DNS resolution and HTTP activity |
| `evidence/tcpdump-analysis.md` | DNS packet observations and protocol analysis for Case 01 |
| `evidence/tcp-syn-flood-analysis.md` | TCP flags, handshake behavior, log observations, and limitations for Case 02 |
| `screenshots/` | Optional screenshots or visual evidence, when available |

## 💡 Skills Demonstrated

- Network protocol and port analysis
- DNS troubleshooting and ICMP interpretation
- TCP handshake analysis
- HTTP request interpretation
- Recognition of traffic patterns consistent with denial-of-service activity
- Evidence-based incident documentation
- Distinguishing observed facts from hypotheses
- Identifying useful next steps for further validation

## ⚠️ Scope and Disclaimer

This repository documents cybersecurity training activities based on supplied scenarios and network logs. It does not claim that these events occurred on a production network, that an attacker was identified, that a DDoS or brute-force attack was confirmed, or that remediation was performed. Conclusions should be revisited if additional packet captures, server logs, or verified incident details become available.

---

<div align="center">

**Investigate the evidence. Explain the impact. Verify the cause.**

*Built as part of a cybersecurity learning portfolio.*

</div>
