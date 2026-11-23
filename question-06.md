# Question 6: Identifying the Attacker's Login Credentials

## Objective

Identify the username and password successfully used by the attacker to access the Apache Tomcat administrative interface.

## Tool Used

- NetworkMiner

## Investigation Methodology

After identifying the `/manager` administrative directory, I continued the investigation by examining the captured network traffic for authentication-related information.

I opened the PCAP file in NetworkMiner and navigated to the **Credentials** tab.

NetworkMiner's credential extraction functionality allowed me to examine authentication information identified within the captured traffic.

By reviewing the extracted credentials, I identified the username and password associated with the administrative login.

## Finding

**Credentials:**

`admin:tomcat`

- **Username:** `admin`
- **Password:** `tomcat`

## Security Relevance

Weak or default credentials can expose administrative interfaces to unauthorized access.

The identified credentials highlight the risks associated with using default passwords and failing to enforce strong authentication policies.

SOC analysts should investigate credential-related activity alongside authentication logs, HTTP sessions and subsequent administrative requests to determine whether unauthorized access occurred.

## Evidence

Screenshot: `../screenshots/question-06-networkminer-credentials.png`

The screenshot should show NetworkMiner's Credentials tab with the identified authentication information. Redact credentials if sharing the evidence publicly.
