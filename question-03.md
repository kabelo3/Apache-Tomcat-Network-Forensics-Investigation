# Question 3: Identifying the Web Server Admin Panel

## Objective
Identify which open port provides access to the Apache Tomcat web server's administrative interface.

## Tools Used
- Wireshark

## Investigation Methodology

After identifying the suspicious source IP address (`14.0.0.120`), I continued investigating the network traffic to determine which services were exposed on the target web server.

I applied the following Wireshark display filter:

```wireshark
ip.addr == 14.0.0.120 && http
```

### Filter Explanation

- `ip.addr == 14.0.0.120`: Displays packets where the identified attacker IP appears as either the source or destination address.
- `&&`: Combines the filtering conditions.
- `http`: Restricts the results to traffic identified as HTTP.

This filter helped isolate HTTP communications involving the suspicious IP address, reducing unrelated traffic and making the investigation more focused.

I then selected a relevant HTTP packet and used **Follow → HTTP Stream** to reconstruct and inspect the application-layer communication between the attacker and the web server.

By examining the HTTP stream and the associated network endpoints, I identified the port used to access the web server's administrative interface.

## Finding

- **Identified Port:** `8080`
- **Protocol:** HTTP
- **Service:** Apache Tomcat administrative interface

## Security Relevance

Port 8080 is commonly used by Apache Tomcat for HTTP services and administrative interfaces.

Exposed administrative interfaces can present a security risk if they are accessible to unauthorized users or lack appropriate authentication and access controls.

Identifying exposed services helps SOC analysts understand the attack surface and investigate potential unauthorized access or exploitation.

## Evidence

- Wireshark display filter showing HTTP traffic involving the suspicious IP.
- Follow HTTP Stream inspection showing the administrative interface communication.
