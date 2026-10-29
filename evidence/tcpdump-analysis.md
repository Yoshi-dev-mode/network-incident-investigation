# tcpdump Evidence Analysis

## Purpose

This document explains the packet observations described in the training scenario. It is based on the scenario's written description of the `tcpdump` log; it does not reproduce a complete raw packet capture.

## Observed Traffic

| Field | Scenario evidence | Interpretation |
|---|---|---|
| Timestamp | First event at `13:24:32.192571` | Approximately 1:24 p.m. |
| Client/source IP | `192.51.100.15` | The computer sending the DNS query |
| DNS server/destination IP | `203.0.113.2` | The DNS server being queried |
| Query transport | UDP | Carries the DNS request in this scenario |
| Destination port | 53 | Standard DNS port |
| Error protocol | ICMP | Reports the delivery error |
| Error text | `udp port 53 unreachable` | No service was reachable/listening at the indicated destination port, as reported by the error |
| Repetition | Error reported on additional attempts | The failure was repeated during the observed attempts |

## Packet Flow

1. The browser needs the IP address for `www.yummyrecipesforme.com`.
2. It sends a DNS query to the DNS server using UDP, targeting port 53.
3. The client receives an ICMP error indicating that UDP port 53 is unreachable.
4. The error repeats on subsequent attempts.
5. Because the name was not successfully resolved, the browser cannot proceed normally to connect to the website by its domain name.

## Protocols: What Is Affected?

- **UDP — transport protocol used for the DNS queries.** The attempted UDP communication to destination port 53 fails.
- **DNS — affected service.** The client cannot successfully complete name resolution during the observed attempts.
- **ICMP — error-reporting protocol.** It communicates the port-unreachable condition; it is not the DNS service itself.
- **HTTPS — intended web protocol.** The scenario indicates the browser would use the resolved IP address to request the webpage. The evidence described here points to a failure before the normal HTTPS connection can proceed.

## Possible Causes (Not Confirmed)

The observed error is consistent with several possibilities, including:

- The DNS service is stopped or is not listening on UDP port 53.
- A firewall or network security rule is rejecting or preventing DNS traffic.
- A DNS server or network configuration problem is directing queries to an unavailable service.

The described log does not prove that an attack occurred. Additional checks of DNS service status, server logs, firewall rules, and other packet captures would be needed to identify the root cause.

## Example Capture Command

On a Linux system with `tcpdump` installed, this command displays traffic involving port 53:

```bash
sudo tcpdump -i any -nn port 53
```

- `-i any` listens on available interfaces on systems that support this option.
- `-nn` prevents address and port-name resolution, so numeric values are displayed.
- `port 53` filters for traffic where source or destination port is 53.

This is a general example command. It is not evidence that a new packet capture was run for this project.

## Limitations

The scenario provides a summary of the log, not the complete raw capture and server-side logs. Accordingly, this analysis identifies the affected service and observed error but does not establish the definitive root cause or remediation.
