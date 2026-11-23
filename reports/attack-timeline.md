# Apache Tomcat Network Forensics: Attack Timeline

## 1. Overview

This report documents the reconstruction of suspicious network activity targeting an Apache Tomcat web server. The investigation was conducted using Wireshark and NetworkMiner to examine packet captures, identify suspicious requests, extract evidence, and understand the attack progression.

## 2. Attack Timeline

| Phase | Activity | Evidence |
|---|---|---|
| 1. Initial Reconnaissance | The attacker at `14.0.0.120` communicated with the target web server. | Wireshark Conversations |
| 2. Service Discovery | The attacker interacted with the Tomcat service running on port `8080`. | HTTP traffic analysis |
| 3. Directory Enumeration | Multiple HTTP requests were sent to discover available directories and resources. Gobuster was identified as the enumeration tool. | HTTP requests and responses |
| 4. Administrative Interface Discovery | The `/manager` directory was accessed and returned an HTTP `200 OK` response. | HTTP stream analysis |
| 5. Credential Access | The credentials `admin:tomcat` were identified in captured network traffic. | NetworkMiner Credentials |
| 6. Malicious File Upload | A file named `JXQOZY.war` was identified in an HTTP upload request. | HTTP Content-Disposition header |
| 7. Command and Control | A reverse-shell command referenced callback destination `14.0.0.120:443`. | TCP stream analysis |

## 3. Key Indicators of Compromise

| Indicator | Value |
|---|---|
| Suspected attacker IP | `14.0.0.120` |
| IP geolocation result | China |
| Target service port | `8080` |
| Enumeration tool | Gobuster |
| Administrative directory | `/manager` |
| Exposed credentials | `admin:tomcat` |
| Uploaded file | `JXQOZY.war` |
| Reverse-shell callback | `14.0.0.120:443` |

## 4. MITRE ATT&CK Relevance

The observed activity is consistent with several MITRE ATT&CK techniques:

- **T1595: Active Scanning**: Network and service discovery activity.
- **T1190: Exploit Public-Facing Application**: Potential exploitation of the exposed web application.
- **T1078: Valid Accounts**: Use or attempted use of identified credentials.
- **T1505.003: Server Software Component: Web Shell**: Relevant to the investigation of the uploaded WAR file, although deployment and execution require further confirmation.
- **T1059: Command and Scripting Interpreter**: Command execution activity indicated by the reverse-shell command.
- **T1059.004: Unix Shell**: Relevant if the observed command used a Unix shell.

These mappings represent investigative relevance, not confirmation that every technique was successfully executed.

## 5. Investigation Tools

- **Wireshark**: Packet inspection, HTTP stream analysis, and network conversation analysis.
- **NetworkMiner**: Credential and network artifact extraction.
- **VirusTotal**: IP intelligence and geolocation lookup.
- **CyberDefenders**: Source of the network forensics challenge and PCAP.

## 6. Important Investigation Notes

- IP geolocation does not establish the attacker's identity or physical location.
- An HTTP `200 OK` response does not independently prove successful administrative authentication.
- Identifying a WAR upload does not prove that the application was deployed or executed.
- A reverse-shell command containing a callback address does not independently prove that a successful shell connection was established.
- Extracted credentials should be treated as sensitive evidence and redacted from publicly shared screenshots where appropriate.

## 7. Conclusion

The packet capture revealed a sequence of suspicious activities involving service discovery, directory enumeration, exposure of administrative credentials, a potentially malicious WAR file upload, and a reverse-shell command.

The investigation demonstrates how network traffic analysis can help security analysts reconstruct attack activity, identify indicators of compromise, and document evidence for further investigation.

**Investigation status:** Completed  
**Tools:** Wireshark, NetworkMiner, VirusTotal  
**Challenge:** CyberDefenders Network Forensics
