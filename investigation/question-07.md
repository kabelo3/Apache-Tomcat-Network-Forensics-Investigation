# Question 7: Identifying the Malicious Uploaded File

## Objective

Identify the filename of the suspicious file uploaded by the attacker to the Apache Tomcat web server, potentially to establish a reverse shell.

## Tool Used

- Wireshark

## Investigation Methodology

After identifying the credentials used to access the administrative interface, I continued investigating the captured network traffic to identify potential post-authentication activity.

I returned to the relevant packet containing the authentication-related communication and selected **Follow → HTTP Stream** to inspect the HTTP request.

Within the HTTP stream, I examined the `Content-Disposition` header, which contained information about the file being submitted to the web server.

By inspecting the `filename` parameter in this header, I identified the suspicious uploaded file.

## Finding

- **Malicious Filename:** `JXQOZY.war`
- **File Extension:** `.war`
- **File Type:** Java Web Application Archive

## Analysis

WAR files are Java Web Application Archives commonly used to package and deploy web applications on Java application servers such as Apache Tomcat.

The presence of a suspicious WAR file in an administrative upload request raises concerns about potential malicious application deployment.

In this scenario, the file upload was associated with the attacker's attempt to establish a reverse shell, potentially providing remote command execution capabilities.

The filename alone does not confirm successful deployment or execution. Further investigation of server responses and subsequent network traffic would be required to establish whether the payload executed successfully.

## Security Relevance

Malicious web application uploads can be used to compromise application servers and establish remote access.

SOC analysts should investigate suspicious file uploads, inspect deployment activity, correlate server logs and monitor subsequent outbound connections for possible command-and-control communication.

## Evidence

Screenshot: `../screenshots/question-07-war-upload.png`
