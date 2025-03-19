<h1>Wireshark</h1>

<h2>Description</h2>

<p>Monitoring network traffic is essential for detecting anomalies that could indicate security threats. Wireshark, a network protocol analyzer, enables deep packet inspection to identify issues such as unencrypted credentials, improper protocol configurations, and unauthorized data transmissions.</p>

<p>Using a virtual machine provided by my school, I conducted Wireshark analysis within the lab environment to examine real-time network traffic. This analysis uncovered several security risks, including plaintext password transmission over FTP, improper IGMP handling leading to potential denial-of-service conditions, and unencrypted LDAP authentication. Based on these findings, I recommended implementing encrypted communication protocols, refining multicast traffic management, and enforcing secure authentication methods to enhance network security.</p>

<h2>Wireshark Anomalies</h2>

<h3>Weak Password Transmission Over FTP</h3>

<p align="center">
<img src="https://i.imgur.com/7bmiqYU.png" height="80%" width="80%" alt="Wireshark"/>
<br />

<p>In packet range 213819 to 213821, a weak password transmission was observed during an FTP exchange. The client sent the command PASS 3.55.1, and the server responded with 230 Logged on, indicating successful authentication. This vulnerability is identified in CVE-1999-0612, which describes the risks associated with plaintext FTP authentication.</p>

<h3>Improper IGMP Handling Leading to Potential Denial of Service</h3>

<p align="center">
<img src="https://i.imgur.com/EwxuupM.png" height="80%" width="80%" alt="Wireshark"/>
<br />

<p>In packet ranges 214730 to 214742 and 219142 to 219149, multiple IGMPv3 Membership Reports were observed, where the host 10.16.80.243 repeatedly joins and leaves multicast groups 224.0.0.251 and 224.0.0.22. This behavior relates to CVE-2017-1000410, where improper handling of IGMP traffic could lead to a denial of service.</p>

<h3>Unencrypted LDAP Simple Bind Transmission</h3>

<p align="center">
<img src="https://i.imgur.com/NoAQaXq.png" height="80%" width="80%" alt="Wireshark"/>
<br />

<p>In packet range 151078 to 151086, a simple bind request using LDAP was captured, where credentials were transmitted in plaintext without encryption. This issue is aligned with CVE-2017-8563, where Microsoft disabled simple bind in Windows Server and Active Directory over unencrypted channels due to the high-security risks involved.</p>

<h2>Implications of each Wireshark Anomaly</h2>

<h3>Weak Password Transmission Over FTP</h3>

<p>Since FTP transmits data, including passwords, in plaintext, this transmission is vulnerable to interception. A malicious actor could use packet-sniffing tools to capture these credentials, making the exchange susceptible to man-in-the-middle attacks. FTP transmits sensitive credentials like the USER and PASS commands without encryption, allowing attackers to intercept and exploit the data using tools such as Wireshark.</p>

<h3>Improper IGMP Handling Leading to Potential Denial of Service</h3>

<p>The rapid joining and leaving of groups can lead to unnecessary network overhead, potentially indicating issues with multicast group management or inefficient handling by the network. If left unresolved, such behavior can cause instability in the multicast configuration or network equipment. The repeated joining and leaving of multicast groups can exhaust resources on network devices, resulting in outages or severe performance degradation.</p>

<h3>Unencrypted LDAP Simple Bind Transmission</h3>

<p>Simple Bind sends sensitive information such as usernames and passwords without using encryption mechanisms like TLS or SSL, making it highly vulnerable to interception. Without encryption, attackers can capture the transmitted credentials and potentially gain unauthorized access to the directory service. Simple Bind requests in this capture suggest similar risks, where sensitive credentials are exposed during transmission without encryption, potentially leading to credential leakage.</p>

<h2>Recommended Solutions</h2>

<h3>Weak Password Transmission Over FTP</h3>

<p>Enforcing strong password policies is important to address the weak password transmission observed during the FTP exchange (packet range 213819 to 213821). This involves requiring complex passwords that are regularly updated (NIST, 2017). Implementing encrypted protocols like SFTP or FTPS ensures that credentials are not transmitted in plaintext, protecting them from interception (IETF, 2006). Monitoring network traffic using intrusion detection systems helps detect unsecured credential transmissions and prompts immediate remediation (Scarfone & Mell, 2007).</p>

<h3>Improper IGMP Handling Leading to Potential Denial of Service</h3>

<p>To mitigate risks associated with the observed IGMPv3 Membership Reports (packet ranges 214730 to 214742 and 219142 to 219149), updating the firmware and software of all network devices to the latest versions is recommended. Manufacturers often release updates to fix known vulnerabilities and improve IGMP handling (Cisco Systems, 2018). Implementing IGMP snooping on switches helps manage multicast traffic efficiently and reduces unnecessary network overhead (IEEE, 2011). Monitoring and limiting IGMP traffic using network monitoring tools prevents potential denial-of-service attacks by controlling the flow of IGMP messages (Juniper Networks, 2019).</p>

<h3>Unencrypted LDAP Simple Bind Transmission</h3>

<p>For the Simple Bind requests observed in the LDAP capture (packet range 151078 to 151086), enforcing the use of LDAP over SSL/TLS (LDAPS) is essential. This encrypts data in transit and prevents interception of credentials (Microsoft, 2017). Configuring the directory service to reject Simple Bind requests that are not over an encrypted channel ensures that clients use secure methods for authentication. Training users and administrators on the importance of secure protocols and proper configuration reduces the likelihood of implementing insecure practices (SANS Institute, 2018). Conducting regular security audits helps detect and remediate insecure configurations promptly (ISO/IEC, 2013).</p>

<h2>References</h2>

- Cisco Systems. (2018). Cisco IOS and IOS XE Software IGMPv3 Denial of Service Vulnerability. Retrieved from [https://tools.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20180905-igmp](https://tools.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20180905-igmp)
- IEEE. (2011). IEEE Standard for Local and Metropolitan Area Networks—Bridges and Bridged Networks. Retrieved from [https://standards.ieee.org/standard/802_1Q-2011.html](https://standards.ieee.org/standard/802_1Q-2011.html)
- International Organization for Standardization. (2013). ISO/IEC 27002:2013 Information Technology – Security Techniques – Code of Practice for Information Security Controls. Retrieved from [https://www.iso.org/standard/54533.html](https://www.iso.org/standard/54533.html)
- Internet Engineering Task Force. (2006). SSH Transport Layer Protocol (RFC 4253). Retrieved from [https://tools.ietf.org/html/rfc4253](https://tools.ietf.org/html/rfc4253)
- Juniper Networks. (2019). Understanding IGMP Snooping and Filtering. Retrieved from [https://www.juniper.net/documentation/en_US/junos/topics/concept/igmp-snooping-filtering-overview.html](https://www.juniper.net/documentation/en_US/junos/topics/concept/igmp-snooping-filtering-overview.html)
- Microsoft. (2017). LDAP Data Signing and LDAP Channel Binding. Retrieved from [https://docs.microsoft.com/windows-server/security/kerberos/ldap-signed-communications](https://docs.microsoft.com/windows-server/security/kerberos/ldap-signed-communications)
- National Vulnerability Database. (1999). CVE-1999-0612. Retrieved from [https://nvd.nist.gov/vuln/detail/CVE-1999-0612](https://nvd.nist.gov/vuln/detail/CVE-1999-0612)
- National Vulnerability Database. (2018). CVE-2017-1000410. Retrieved from [https://nvd.nist.gov/vuln/detail/CVE-2017-1000410](https://nvd.nist.gov/vuln/detail/CVE-2017-1000410)
- SANS Institute. (2018). Security Awareness Planning Kit. Retrieved from [https://www.sans.org/security-awareness-training/resources/security-awareness-planning-kit](https://www.sans.org/security-awareness-training/resources/security-awareness-planning-kit)
- Scarfone, K., & Mell, P. (2007). Guide to Intrusion Detection and Prevention Systems (IDPS) (NIST SP 800-94). Retrieved from [https://doi.org/10.6028/NIST.SP.800-94](https://doi.org/10.6028/NIST.SP.800-94)
