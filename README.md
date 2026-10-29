# DNS Incident Investigation

> A cybersecurity training project focused on DNS resolution failure, packet analysis, and network troubleshooting using `tcpdump`.

## Overview

This project documents the investigation of a simulated incident in which users could not access `www.yummyrecipesforme.com`. A packet capture reviewed with `tcpdump` showed DNS queries sent over UDP followed by ICMP errors reporting that UDP port 53 was unreachable.

The goal is to interpret the available evidence, identify the affected protocol and service, and document plausible causes without claiming an unconfirmed root cause.

## Objectives

- Review packet-level evidence from a network troubleshooting scenario.
- Identify the roles of DNS, UDP, and ICMP.
- Interpret the significance of destination port 53.
- Summarize the observed impact on website access.
- Distinguish observed evidence from hypotheses that require further investigation.

## Scenario Summary

Several customers reported that `www.yummyrecipesforme.com` would not load and displayed a “destination port unreachable” error. The analyst reproduced the issue and used `tcpdump` to inspect the traffic.

The log showed DNS requests from client `192.51.100.15` to DNS server `203.0.113.2`, followed by ICMP errors stating:

```text
udp port 53 unreachable
```

The log indicates that DNS resolution failed during the observed attempts. Without resolving the domain name to an IP address, the browser could not proceed normally to connect to the website over HTTPS.

## Key Findings

| Item | Finding |
|---|---|
| Affected transport protocol | UDP |
| Affected service | DNS |
| Destination port | 53 |
| Error-reporting protocol | ICMP |
| Client IP shown in the scenario | `192.51.100.15` |
| DNS server IP shown in the scenario | `203.0.113.2` |
| Observed impact | Domain resolution failed and the website could not be accessed normally |
| Root cause | Not confirmed by the available evidence |

**Protocol distinction:** UDP carried the DNS query; DNS is the name-resolution service; ICMP reported the port-unreachable error.

## Repository Contents

- [Incident report](reports/incident-report.md) — concise report in the requested two-part format.
- [tcpdump analysis](evidence/tcpdump-analysis.md) — explanation of the packet observations and relevant networking concepts.
- `screenshots/` — optional location for sanitized screenshots you capture yourself.

## Investigation and Next Steps

Potential explanations include a DNS service that is unavailable or not listening on UDP port 53, or a firewall/network configuration issue. The log alone does not establish which explanation is correct.

Recommended follow-up checks:

1. Verify that the DNS service is running and listening on the expected interface and port.
2. Review firewall and network-device rules affecting DNS traffic.
3. Inspect DNS server logs and additional packet captures.
4. Confirm whether DNS queries succeed after any identified issue is corrected.
5. Record the confirmed root cause and remediation after evidence supports the conclusion.

These are recommended investigation steps, not actions claimed to have been completed in the training scenario.

## Tools and Concepts

- **tcpdump** — command-line packet capture and inspection tool.
- **DNS** — translates domain names into IP addresses.
- **UDP** — transport protocol used by the DNS queries in this scenario.
- **ICMP** — carries network error messages, including port-unreachable errors.
- **Port 53** — standard DNS port.
- **Incident documentation** — records evidence, impact, hypotheses, and next steps.

Example command for observing port 53 traffic on a Linux system:

```bash
sudo tcpdump -i any -nn port 53
```

The command requires suitable permissions and a supported interface. Actual output depends on the host's network and DNS configuration.

## Skills Demonstrated

- Network traffic analysis
- DNS troubleshooting fundamentals
- Protocol and port identification
- ICMP error interpretation
- Basic incident analysis and reporting
- Evidence-based technical writing

## Disclaimer

This repository documents a simulated cybersecurity training scenario. The IP addresses, domain, and events are presented as scenario evidence; this project does not claim that a real-world attack occurred or that a live incident was remediated.

## Author

**Yoshi-dev-mode** — Cybersecurity learning portfolio
