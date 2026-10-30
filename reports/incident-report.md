# Network Incident Investigation Report

This report combines two separate cybersecurity training activities. The cases are analyzed independently because they involve different protocols and symptoms.

## Case 01: DNS Resolution Failure

### Summary

The scenario describes users being unable to access `www.yummyrecipesforme.com`. The `tcpdump` log summary shows DNS requests sent over UDP from client `192.51.100.15` to DNS server `203.0.113.2`, followed by ICMP error messages stating “udp port 53 unreachable.” Port 53 is commonly used by DNS. Since name resolution failed during the observed attempts, the browser could not proceed normally to access the website by domain name.

### Analysis and possible causes

The observed traffic indicates a DNS resolution failure associated with an ICMP port-unreachable error. Possible explanations include the DNS service being unavailable or not listening on UDP port 53, a firewall or network security rule rejecting the traffic, or an incorrect DNS/network configuration. The scenario summary does not confirm which cause applies, and it does not establish that the event was malicious.

### Recommended follow-up

Check DNS service status and listening interfaces, review relevant firewall and network rules, inspect DNS server logs, and collect additional packet captures. Confirm resolution after any verified issue is corrected. These are recommendations, not actions confirmed as completed.

## Case 02: Suspected TCP SYN Flood

### Summary

The supplied TCP log workbook shows repeated TCP SYN packets from `203.0.113.0` using source port `54770` to `192.0.2.1` on destination port `443`. Server SYN-ACK responses are also present, and the color-coded workbook identifies suspicious traffic. Some normal TCP connections appear to fail during the event. This pattern is consistent with a suspected TCP SYN flood denial-of-service event.

### Analysis and possible causes

TCP connection establishment normally uses a three-way handshake: the client sends SYN, the server replies with SYN-ACK, and the client completes the handshake with ACK. A SYN flood can create many connection-opening attempts without completing all handshakes. The server may have to retain state while waiting for final acknowledgments; if connection resources become strained, legitimate users may experience delays, failed connections, or timeouts.

The repeated SYN pattern supports further investigation for a SYN flood, but the available workbook alone does not prove that every handshake was incomplete or that the observed failures were caused solely by this traffic. It also does not establish a distributed denial-of-service (DDoS) attack, identify the person or system controlling the source address, or independently confirm the root cause.

### Recommended follow-up

Correlate the traffic with service monitoring and user reports; inspect server TCP connection statistics, connection backlog, CPU and memory use, and relevant firewall or load-balancer logs. Compare source addresses and traffic across multiple time windows before assessing whether the activity is distributed. Work with the network administrator to evaluate appropriate defensive controls and verify that legitimate traffic is restored.

## Combined conclusion

The DNS case shows a name-resolution failure associated with UDP port 53 and an ICMP port-unreachable error. The TCP case shows repeated connection-opening SYN packets targeting port 443, a pattern consistent with a suspected SYN flood. These are different network symptoms and should not be treated as one confirmed incident or as evidence of a common cause. Both analyses remain provisional until corroborated by fuller packet captures, server-side logs, and service-health data.

## Evidence limitations

This report is based on the provided training scenario summary and TCP log workbook. It does not claim that a live production incident occurred, that an attacker was identified, that DDoS was confirmed, or that remediation was performed.
