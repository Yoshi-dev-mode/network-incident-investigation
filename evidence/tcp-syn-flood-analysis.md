# TCP SYN Flood — Log Evidence Analysis

## Purpose and scope

This document analyzes the supplied TCP log workbook, including its color-coded sheet, as a cybersecurity training exercise. It records the visible traffic pattern and explains why it may be consistent with a TCP SYN flood. The workbook alone does not establish the definitive root cause.

## Key observations

| Field | Observation from the workbook | Interpretation |
|:--|:--|:--|
| Source address | `203.0.113.0` appears repeatedly | A recurring source in the observed records; source identity or ownership is not established |
| Destination address | `192.0.2.1` | The destination receiving the repeated connection attempts |
| Source port | `54770` in the repeated pattern | Client-side source port shown in the records |
| Destination port | `443` | HTTPS service port |
| TCP flags | Repeated `[SYN]` packets | Requests to begin TCP connections |
| Server responses | `[SYN, ACK]` responses from port 443 to port 54770 | Server responses consistent with the TCP handshake |
| Other activity | Color-coded log marks suspicious traffic; some normal connections appear to fail during the event | Possible service impact that requires correlation with other evidence |

## How the TCP three-way handshake works

1. **SYN:** The client requests to start a TCP connection.
2. **SYN-ACK:** The server acknowledges the request and signals readiness.
3. **ACK:** The client acknowledges the server response, completing the handshake.

A successfully completed handshake allows the endpoints to proceed with the TCP session. The observed presence of SYN and SYN-ACK packets does not, by itself, prove whether each individual connection completed; the full sequence for each flow must be checked.

## Why this may indicate a SYN flood

A TCP SYN flood is a denial-of-service technique in which a target receives a large number of connection-opening requests, potentially leaving many connections in a half-open state while the server waits for final ACK packets. If this consumes enough connection-tracking or backlog capacity, legitimate clients may experience failed connections or timeouts.

The repeated SYN pattern targeting port 443, together with the workbook's suspicious-activity markings and indications that some normal connections failed, is consistent with a suspected SYN flood. It is a hypothesis to validate, not a conclusive finding.

## What the evidence does not establish

- It does not prove that every SYN request lacked a completing ACK.
- It does not prove that multiple independent source systems were involved; therefore, it does not by itself establish DDoS.
- It does not identify who controlled the source address.
- It does not independently prove that the SYN traffic caused every observed connection failure.
- It does not establish the precise root cause or confirm that mitigation was performed.

## Recommended investigation

- Correlate packet timestamps with monitoring alerts, service availability, and user reports.
- Inspect server TCP statistics, half-open connection counts, backlog limits, resource utilization, and application logs.
- Review firewall, load-balancer, and network-device logs for related evidence.
- Compare traffic sources and rates across a wider time window before classifying the event as distributed.
- If a SYN flood is confirmed, coordinate mitigation with the responsible network administrator and verify both service recovery and legitimate-client connectivity.

## Conclusion

The workbook shows repeated TCP SYN attempts directed at `192.0.2.1:443` from `203.0.113.0:54770`, alongside server SYN-ACK responses. This pattern is consistent with a suspected TCP SYN flood denial-of-service event, but additional packet-level and server-side evidence is required to confirm the mechanism, impact, and root cause.
