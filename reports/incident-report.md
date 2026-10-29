# DNS Incident Investigation Report

## Part 1: Provide a summary of the problem found in the DNS and ICMP traffic log

The network protocol analyzer logs indicate that UDP port 53 is unreachable when attempting to access the website `www.yummyrecipesforme.com`. Port 53 is normally used for DNS services, which translate domain names into IP addresses. The `tcpdump` logs show that DNS requests sent to the DNS server failed and returned ICMP error messages stating “udp port 53 unreachable.” This may indicate a problem with the DNS service or the firewall or network configuration. Further investigation is needed to determine the exact cause of the incident.

## Part 2: Explain your analysis of the data and provide at least one cause of the incident

The incident began at approximately 1:24 p.m., according to the first timestamp shown in the scenario, when customers reported that they could not access `www.yummyrecipesforme.com`. The cybersecurity analyst attempted to access the website and experienced the same issue. The analyst then used the network protocol analyzer tool `tcpdump` to investigate the network traffic. The resulting logs showed that UDP packets sent from the computer at IP address `192.51.100.15` to the DNS server at IP address `203.0.113.2` were followed by ICMP error messages stating “udp port 53 unreachable.” The same delivery error occurred repeatedly. These findings indicate that the DNS requests could not reach a service listening on the destination port, preventing the browser from resolving the website’s domain name. A possible cause is that the DNS service was unavailable or not listening on UDP port 53. Another possibility is that a firewall or network configuration issue was preventing DNS traffic from reaching the service. The security engineers would need to investigate further to confirm the root cause and determine the appropriate corrective actions.

## Evidence-based conclusion

The observed failure involves DNS queries carried over UDP and an ICMP port-unreachable error referring to destination port 53. The available scenario evidence does not confirm whether the underlying cause was a DNS service failure, a firewall rule, a network configuration issue, or malicious activity.
