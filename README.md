<!--
  DNS Incident Investigation
  Cybersecurity learning portfolio project
-->

<div align="center">

# 🛡️ DNS Incident Investigation

**Tracing a DNS resolution failure through packet-level evidence**

A hands-on cybersecurity training project exploring DNS, UDP, ICMP, and network troubleshooting with `tcpdump`.

<br/>

![Focus](https://img.shields.io/badge/Focus-Network%20Analysis-2563eb?style=for-the-badge)
![Tool](https://img.shields.io/badge/Tool-tcpdump-0f766e?style=for-the-badge)
![Status](https://img.shields.io/badge/Root%20Cause-Unconfirmed-d97706?style=for-the-badge)

<br/>

[Incident Report](reports/incident-report.md) · [Packet Analysis](evidence/tcpdump-analysis.md)

</div>

---

## 📌 Project Overview

Users reported that they could not access **`www.yummyrecipesforme.com`**. To investigate the issue, a cybersecurity analyst reviewed network traffic using `tcpdump`.

The available log summary showed DNS queries sent over UDP, followed by ICMP error messages reporting that **UDP port 53 was unreachable**.

This project documents the evidence, explains the relevant network protocols, and outlines reasonable next investigative steps—without presenting an unverified cause as fact.

## 🎯 Learning Objectives

- Analyze packet-level observations from a network troubleshooting scenario.
- Explain how **DNS**, **UDP**, and **ICMP** relate to the incident.
- Identify the significance of **port 53**.
- Describe the impact of failed DNS resolution on website access.
- Separate observed evidence from hypotheses and recommended follow-up actions.
- Practice clear, evidence-based incident reporting.

## 🚨 Incident at a Glance

| Field | Observation |
|:--|:--|
| **Reported impact** | Users could not access the website normally |
| **Domain** | `www.yummyrecipesforme.com` |
| **Client IP in scenario** | `192.51.100.15` |
| **DNS server IP in scenario** | `203.0.113.2` |
| **Transport protocol** | UDP |
| **Destination port** | 53 |
| **Error-reporting protocol** | ICMP |
| **Observed error** | `udp port 53 unreachable` |
| **Root cause** | Not confirmed by the available evidence |

### 🔎 What the log tells us

```text
DNS query  ──UDP / port 53──▶  DNS server
                                │
Client ◀────── ICMP error ──────┘
           "udp port 53 unreachable"
```

**In plain language:** the client attempted DNS name resolution, but the observed request failed with a port-unreachable error. Because the domain could not be resolved during those attempts, the browser could not proceed normally to connect to the website by domain name.

> **Protocol distinction:** UDP carries the DNS query, DNS provides name resolution, and ICMP reports the network error.

## 🧭 Investigation Workflow

1. **Identify the user impact** — customers report that the website will not load.
2. **Reproduce the issue** — the analyst encounters the same problem.
3. **Inspect network traffic** — use `tcpdump` to examine the relevant packets.
4. **Interpret the evidence** — DNS queries over UDP are followed by ICMP port-unreachable errors.
5. **Develop hypotheses** — consider DNS service availability and network or firewall configuration.
6. **Recommend validation** — review server status, DNS logs, firewall rules, and additional packet captures.

## 🧰 Tools & Concepts

| Tool / Concept | Role in the investigation |
|:--|:--|
| **tcpdump** | Captures and inspects network packets from the command line |
| **DNS** | Translates a domain name into an IP address |
| **UDP** | Transport protocol used by the DNS queries shown in the scenario |
| **ICMP** | Reports network errors, including port-unreachable conditions |
| **Port 53** | Standard DNS port |
| **Incident reporting** | Documents impact, evidence, hypotheses, and next steps |

### Example command

On a Linux system with `tcpdump` installed, the following command can display traffic involving port 53:

```bash
sudo tcpdump -i any -nn port 53
```

- `-i any` listens across available interfaces on systems that support this option.
- `-nn` keeps addresses and port numbers numeric.
- `port 53` filters for packets where source or destination port is 53.

This is an example command for further investigation; it is **not** a claim that a new packet capture was run for this project.

## 🧩 Possible Causes to Investigate

The evidence is consistent with more than one possibility:

- The DNS service may be unavailable or not listening on UDP port 53.
- A firewall or network security rule may be blocking or rejecting DNS traffic.
- A DNS or network configuration issue may be directing queries to an unavailable service.

**These are hypotheses, not confirmed findings.** The available log summary does not establish a definitive root cause or prove that malicious activity occurred.

### Recommended next steps

- [ ] Check whether the DNS service is running and listening on the expected interface and port.
- [ ] Review firewall and network-device rules affecting DNS traffic.
- [ ] Inspect DNS server logs and collect additional packet evidence.
- [ ] Test whether DNS resolution succeeds after any identified issue is corrected.
- [ ] Document the verified cause and remediation once supported by evidence.

These are recommended follow-up actions, not completed actions claimed by this report.

## 📂 Repository Contents

| File | Description |
|:--|:--|
| [`reports/incident-report.md`](reports/incident-report.md) | Two-part incident report describing the problem and analysis |
| [`evidence/tcpdump-analysis.md`](evidence/tcpdump-analysis.md) | Packet observations, protocol explanations, and investigation limitations |
| `screenshots/` | Placeholder for screenshots you choose to add |

## 💡 Skills Demonstrated

- Network traffic analysis fundamentals
- DNS troubleshooting
- Protocol and port identification
- ICMP error interpretation
- Incident analysis and documentation
- Evidence-based technical communication

## ⚠️ Disclaimer

This repository documents a **simulated cybersecurity training scenario**. The domain, IP addresses, and events are included as scenario evidence. This project does not claim that a real-world attack occurred, that a live system was compromised, or that a production incident was remediated.

---

<div align="center">

**Built as part of a cybersecurity learning journey.**

*Investigate the evidence. Explain the impact. Verify the cause.*

</div>
