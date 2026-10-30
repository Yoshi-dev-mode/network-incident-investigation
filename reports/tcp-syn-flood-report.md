# TCP SYN Flood Incident Report

## Part 1: Summary of the problem

The supplied TCP log workbook shows repeated TCP SYN packets from source address `203.0.113.0`, using source port `54770`, to destination address `192.0.2.1` on TCP port `443`. The server also sends SYN-ACK responses. The color-coded workbook marks suspicious activity, and some ordinary TCP connections appear to fail during the event. These observations are consistent with a suspected TCP SYN flood denial-of-service event, although the workbook alone does not confirm the root cause.

## Part 2: Analysis and a possible cause

TCP normally establishes a connection through a three-way handshake: the client sends a SYN packet, the server responds with SYN-ACK, and the client sends the final ACK. In a SYN flood, a target may receive repeated connection-opening requests while some connections remain incomplete as the server waits for the final acknowledgment. A sufficiently high volume of such requests can consume connection-tracking or backlog resources and may prevent legitimate users from connecting reliably.

The repeated SYN packets targeting port 443 provide a reason to investigate a possible SYN flood. However, the presence of SYN and SYN-ACK packets does not prove that every handshake was incomplete, and the log does not establish that the traffic caused every failed connection. It also does not prove a distributed denial-of-service (DDoS) attack or identify who controlled the source address. Further investigation should correlate the packet records with server TCP statistics, resource utilization, monitoring alerts, firewall or load-balancer logs, and user reports.

## Evidence-based conclusion

The observed pattern is consistent with a suspected TCP SYN flood, but the available workbook is insufficient to confirm the definitive root cause. The report treats the attack classification as a hypothesis requiring additional evidence, not as a verified conclusion. No mitigation or remediation is claimed as completed.
