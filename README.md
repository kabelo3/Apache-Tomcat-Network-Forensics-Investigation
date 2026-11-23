# Apache Tomcat Network Forensics Investigation

![Category](https://img.shields.io/badge/Category-Network%20Forensics-blue)
![Platform](https://img.shields.io/badge/Platform-CyberDefenders-red)
![Tools](https://img.shields.io/badge/Tools-Wireshark%20%7C%20NetworkMiner-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 1. Project Overview

This project documents a hands-on Blue Team network forensics investigation completed on the CyberDefenders platform.

The investigation involved analyzing a captured network traffic file (PCAP) to identify suspicious activity targeting an Apache Tomcat web server within a company's intranet.

Using Wireshark and NetworkMiner, I examined network communications, investigated suspicious HTTP requests, identified exposed administrative resources, extracted authentication information, and analyzed evidence of malicious file uploads and reverse shell activity.

The objective was to reconstruct the attack progression, identify indicators of compromise (IOCs), and understand how network forensic analysis can support security operations and incident response.

## 2. Investigation Scenario

The Security Operations Center (SOC) team identified suspicious activity targeting an internal Apache Tomcat web server.

A network traffic capture was provided for forensic examination. Initial observations indicated requests across multiple ports, suggesting possible reconnaissance and port-scanning activity.

The investigation focused on determining:

- The source of the suspicious network activity.
- The geographical information associated with the source IP.
- Exposed web services and administrative interfaces.
- Tools used during directory and file enumeration.
- Credentials exposed during authentication.
- Evidence of suspicious file uploads.
- Commands associated with potential remote access.
- The destination of the reverse shell callback.

## 3. Objectives

The primary objectives were to:

1. Identify the suspicious source IP address.
2. Investigate the geographical information associated with the IP.
3. Identify the port hosting the administrative interface.
4. Determine which enumeration tool was used.
5. Identify the administrative directory discovered.
6. Extract credentials from the captured network traffic.
7. Identify the suspicious uploaded file.
8. Determine the reverse shell callback destination.

## 4. Tools and Technologies

| Tool | Purpose |
|---|---|
| Wireshark | PCAP analysis, packet inspection, HTTP filtering and TCP stream reconstruction. |
| NetworkMiner | Network forensic analysis and credential extraction. |
| VirusTotal | IP address intelligence and geographical enrichment. |
| CyberDefenders | Blue Team practical investigation environment. |

### Wireshark Display Filter

The following display filter was used to isolate HTTP traffic involving the identified suspicious IP address:

```wireshark
ip.addr == 14.0.0.120 && http
```

**Filter breakdown:**

- `ip.addr` matches packets where the specified IP is either the source or destination.
- `14.0.0.120` specifies the suspicious IP address.
- `&&` combines the filtering conditions.
- `http` restricts the results to traffic identified as HTTP.

This reduced unrelated traffic and helped focus the investigation on communications involving the suspected attacker.

## 5. Investigation Methodology

The investigation followed a structured network forensic approach.

### Phase 1: Network Traffic Analysis

The PCAP file was opened in Wireshark.

Using **Statistics → Conversations**, I examined IPv4 communications to identify suspicious communicating hosts and determine the source IP associated with the requests targeting the web server.

### Phase 2: IP Address Intelligence

The identified IP address was investigated using VirusTotal to obtain additional threat intelligence and geographical information.

### Phase 3: Service and Port Investigation

Wireshark display filters were used to isolate HTTP communications involving the suspicious IP.

Following relevant HTTP streams helped identify the administrative web service operating on port 8080.

### Phase 4: Directory Enumeration Analysis

Further packet inspection revealed repeated HTTP requests targeting different directories and files.

Multiple HTTP 404 responses suggested automated directory enumeration.

By following the HTTP stream, I identified Gobuster as the enumeration tool.

### Phase 5: Administrative Interface Discovery

Further HTTP stream analysis revealed a request targeting the `/manager` directory.

The corresponding server response contained an HTTP 200 OK status code, indicating that the requested resource was successfully processed.

### Phase 6: Credential Investigation

NetworkMiner was used to examine the captured traffic.

Under the **Credentials** tab, I identified authentication information associated with the administrative interface.

### Phase 7: Malicious File Upload Analysis

Returning to Wireshark, I followed the relevant HTTP stream and inspected the `Content-Disposition` header.

The filename parameter revealed a suspicious WAR file submitted to the server.

### Phase 8: Reverse Shell Investigation

I examined the subsequent TCP stream to investigate post-upload activity.

Commands such as `whoami` and `pwd` were observed, alongside a reverse shell command.

The callback destination was identified from the reverse shell command.

## 6. Key Investigation Findings

The following findings were established during the investigation:

| # | Investigation | Finding |
|---|---|---|
| 01 | Source IP address | `14.0.0.120` |
| 02 | Reported IP geolocation | China |
| 03 | Administrative service port | `8080` |
| 04 | Directory enumeration tool | Gobuster |
| 05 | Discovered administrative directory | `/manager` |
| 06 | Extracted credentials | `admin:tomcat` |
| 07 | Suspicious uploaded file | `JXQOZY.war` |
| 08 | Reverse shell callback | `14.0.0.120:443` |

## 7. Attack Progression

Based on the evidence identified in the PCAP, the following attack progression was reconstructed:

1. **Reconnaissance:** Suspicious requests originated from `14.0.0.120`, targeting multiple ports on the web server.

2. **Service Discovery:** The attacker investigated exposed services and identified the administrative web interface operating on port `8080`.

3. **Directory Enumeration:** Gobuster was identified as the tool used to enumerate web directories and files.

4. **Administrative Interface Discovery:** The attacker discovered the `/manager` directory, which returned an HTTP 200 OK response.

5. **Credential Access:** NetworkMiner revealed the credentials `admin:tomcat` in the captured network traffic.

6. **Malicious File Upload:** A suspicious WAR file named `JXQOZY.war` was identified in the HTTP Content-Disposition header.

7. **Command Execution:** Subsequent TCP stream analysis revealed commands including `whoami` and `pwd`, alongside a reverse shell command.

8. **Command and Control:** The reverse shell command referenced a callback destination of `14.0.0.120:443`.

## 8. MITRE ATT&CK Relevance

The observed activity relates to several MITRE ATT&CK tactics covered by the challenge.

| Tactic | Observed activity |
|---|---|
| Reconnaissance | Port scanning and service discovery. |
| Execution | Commands and potential malicious application execution. |
| Persistence | Suspicious WAR upload and potential backdoor establishment. |
| Privilege Escalation | Relevant to investigating the attack, but escalation was not independently established by the documented evidence. |
| Credential Access | Extraction of administrative credentials. |
| Discovery | Directory enumeration and system commands. |
| Command and Control | Reverse shell callback command targeting port 443. |

These mappings describe the investigative relevance of the observed behaviour. They do not establish that every associated attack technique was successfully executed.

## 9. Security Observations and Lessons Learned

This investigation highlighted several important security considerations:

- **Exposed administrative interfaces:** Administrative endpoints should be restricted to trusted management networks.
- **Weak credentials:** Default or easily guessed credentials create opportunities for unauthorized access.
- **Directory enumeration:** Repeated requests to different web paths can reveal automated reconnaissance.
- **Suspicious file uploads:** Unexpected WAR file uploads should trigger investigation and file integrity checks.
- **Outbound connections:** Unexpected outbound connections from application servers may indicate reverse shells or command-and-control activity.
- **Network visibility:** PCAP analysis can reveal suspicious activity that may otherwise be missed through conventional monitoring.

The exercise also reinforced the importance of correlating network evidence across multiple packets and streams rather than relying on isolated events.

## 10. Repository Structure

```text
Apache-Tomcat-Network-Forensics-Investigation/
│
├── README.md
│
├── investigation/
│   ├── question-01.md
│   ├── question-02.md
│   ├── question-03.md
│   ├── question-04.md
│   ├── question-05.md
│   ├── question-06.md
│   ├── question-07.md
│   └── question-08.md
│
├── screenshots/
│   ├── question-01-conversations.png
│   ├── question-02-virustotal.png
│   ├── question-03-filter.png
│   ├── question-03-http-stream.png
│   ├── question-04-http-requests.png
│   ├── question-04-gobuster-stream.png
│   ├── question-05-manager-request.png
│   ├── question-05-http-200.png
│   ├── question-06-networkminer-credentials.png
│   ├── question-07-war-upload.png
│   └── question-08-reverse-shell.png
│
└── reports/
    └── attack-timeline.md
```

## 11. Disclaimer

This investigation was conducted as part of a controlled Blue Team training challenge on CyberDefenders.

All findings are based on the supplied PCAP file and associated investigation activities. The credentials, IP addresses and filenames documented here are included strictly for educational and forensic analysis purposes.

IP geolocation indicates the country associated with the IP intelligence record and does not independently establish the physical location or identity of an attacker.

The presence of a successful HTTP response or a reverse shell command does not, by itself, establish successful authentication, payload execution or an established reverse shell session.

## 12. References

- [CyberDefenders - Blue Team Challenges](https://cyberdefenders.org/blueteam-ctf-challenges/)
- [Wireshark](https://www.wireshark.org/)
- [NetworkMiner](https://www.netresec.com/?page=NetworkMiner)
- [VirusTotal](https://www.virustotal.com/)
- [MITRE ATT&CK](https://attack.mitre.org/)

---

**Project completed:** November 2026

**Focus Areas:** Network Forensics | PCAP Analysis | Threat Intelligence | Web Server Investigation | Incident Response | Blue Team Operations
