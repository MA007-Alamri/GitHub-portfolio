
## Lab 04: Enterprise LAN Design & Security Assessment

Maram Alamri

Marymount University

IT 520- Enterprise Infrastructure and Networks

Dr. Safiatu Mojidi

March 28, 2026

## Network Architecture & Topology Design

### Physical Layer Configuration (OSI Layer 1)

The network topology was made to represent three different segments of the WBMUD infrastructure. The IT-Router, IT-Switch, and three workstations; IT-Admin-1, IT-Admin-2 and SOC-Workstation are found within IT Operations Network (10.1.10.0/24). The OT-Router, OT-Switch, two SCADA controllers (SCADA-Controller-1 and SCADA-Controller-2) and OT-Web-Server are found in the Operational Technology Network (192.168.50.0/24). Additionally, the Remote-Router, Remote-Switch and Pump- Controller are found in the Remote Pump Station (172.16.5.0/24). The routers are joined with a Core-Switch over the network 10.0.0.0/24 and the Attacker-Node (10.0.0.100) simulated an outside agent of attack. The Copper Straight-Through cabling between the routers and switches was used to make all the connections. Physical cabling, which is Layer 1 functionality supports transmission of data securely to ensure that unauthorized equipment or subjects access data through it.
![Figure 1](https://github.com/user-attachments/assets/51d95a83-001d-4e6f-afdf-6396d10a4719)

### Data Link Layer Configuration (OSI Layer 2)
All switches were configured using VLAN 1 to make them simple with all access ports configured to Switchport mode access without shutdown. The IT-Switch was set to management IP 10.1.10.2, OT-Switch to 192.168.50.2, Remote-Switch to 172.16.5.2 and the Core-Switch was set to have all the ports with VLAN 1. Once the powering of all end devices is done and traffic has been generated with the help of ping tests the MAC address tables were filled in accordingly. Switches that IT-Switch learned were Fa0/2, Fa0/3, Fa0/4, and Gig0/1, which caused the following ‘IT-Admin-1, IT-Admin-2, SOC-Workstation, and IT-Router. OT-Switch identified 4 MAC addresses of the SCADA-Controller-1, SCADA-Controller-2, OT-Web-Server and OT-Router. Remote-Switch had learnt two MAC addresses of Pump-Controller and Remote-Router. MAC addresses are employed at Layer 2 whereby switches employ MAC address tables to forward the packets inside the identical network segment.
![Figure 2](https://github.com/user-attachments/assets/8cec5d1b-cda1-40c5-814a-c4ffaa57d48e)
Figure 2: Switch MAC Address Table

### Network Layer Configuration (OSI Layer 3)
All router interfaces were given IP addresses. IT-Router was configured as a router with 10.1.10.1 on Gig0/0 being connected to IT- Switch and 10.0.0.1 on Gig0/1 being connected to Core- Switch. OT-Switch and OT-Router were set to give 192.168.50.1 to Gig0/0 and 10.0.0.2 to Gig0/1 respectively. Remote-Router had 172.16.5.1 on Gig0/0 which was connected to Remote-Switch and 10.0.0.3 on Gig0/1 which was connected to Core-Switch. The routing was set to the static mode so that the inter-network communication could be allowed. The IT-Router was set up with routes to 192.168.50.0/24 via 10.0.0.2 and routes to 172.16.5.0/24 via 10.0.0.3. OT-Router was set with the 10.1.10.0/24 routes through 10.0.0.1 and 172.16.5.0/24 routes through 10.0.0.3. Remote-Router was set with 10.1.10.0/24 through 10.0.0.1 and 192.168.50.0/24 through 10.0.0.2. Every interface displayed up/up indicating that interfaces were properly connected. IP addressing and routing are Layer 3 functions where IP routers use IP addresses in order to send packets to other networks.
![Figure 3](https://github.com/user-attachments/assets/756b478c-8f19-4b89-a3df-788c671d2cd4)
Figure 3:  IT-Router Interface Configuration
![Figure 4](https://github.com/user-attachments/assets/e000eb7f-8893-46e6-bf6f-9bc2503293a2)
Figure 4: Routing Table Output

## Connectivity Verification
The end devices were configured using static IP addresses. IT-Admin-1 was given 10.1.10.10 with the gateway 10.1.10.1 and IT-Admin-2 was given 10.1.10.11 and SOC-Workstation was given 10.1.10.12. SCADA-Controller-1, SCADA- Controller-2 were assigned to 192.168.50.10, 192.168.50.11 with 192.168.50.1 as the gateway. OT-Web-Server was assigned 192.168.50.20 which shares the same gateway. Pump-Controller was assigned the gateway 172.16.5.10 with the address of 172.16.5.10. Attacker-Node was given 10.0.0.100 and gateway 10.0.0.1. Ping tests run with IT-Admin-1 verified that it could connect with IT-Admin-2 on the same subnet, OT-Web-Server on a different subnet and Pump-Controller on the other side of the WAN link. Every test showed 0% loss of packets together with a reply time of less than 1ms which indicated complete functionality of the network across all segments.
![Figure 5](https://github.com/user-attachments/assets/2b8134d5-90c6-4468-af9a-98f196863401)

Figure 5 : Successful Ping Results

## Protocol Security Analysis (OSI Layers 4-7)
### Transport Layer Security (OSI Layer 4)
OT-Web-Server was set up with the HTTP service on TCP port 80 and through the SOC-Workstation, where a telnet connection managed to obtain the web page. TCP has more security visibility than UDP since its triple state handshake can be examined by firewalls and enables security personnel to monitor each connected request. UDP does not use a handsack protocol and therefore the connection can not be tracked without deep packet inspection. TCP also offers explicit session management which is useful in forensic analysis in case of an incident investigation (Sikos, 2020). The security trade-off lies in the fact that TCP has better visibility, but is susceptible to SYN flood attacks whereas UDP is lesser in overhead and easy to spoof and amplify attacks.

### Application Layer Security (OSI Layer 7)
The OT-Web-Server was set up with port 80 and 443 port to be used with HTTP and HTTPS respectively. A test page was also generated and opened successfully by using both protocols on SOC-Workstation. The HTTPS uses encryption, authentication, and integrity to prevent a man in the middle attack. All data is encrypted with AES-256, and therefore, eavesdropping is avoided. X.509 is also used in authentication so that clients interface with the authentic server. Integrity, which is a message authentication code, is used to monitor tampering of the data that is in transit. None of these protection measures are available in HTTP hence susceptible to attackers to intercept data.
![Figure 6](https://github.com/user-attachments/assets/0ed04287-e5c1-4a98-a542-022b699dc9fc)
Figure 6 : HTTP and HTTPS Browser Connections

All routers were set up with RSA keys and third-party user authentication. Using SOC-Workstation, an SSH connection was made successful to OT-Router. SSH replaced Telnet in corporations since Telnet is used to send all data such as credential in plaintext hence it is exposed to packet sniffing (Jayanti Katariya, 2023). SSH is securely encrypted with a robust cryptography, and credentials and commands cannot be intercepted. SSH also ensures protection by integrity via message authentication codes averting command injection and session hijacking. Additionally, SSH allows authentication using multiple approaches such as key authentication whereas Telnet only allows using password authentication.
![Figure 7](https://github.com/user-attachments/assets/e62eb669-6b45-43f6-8a62-52c2b3c50691)
Figure 7 : SSH Login to Router

## Shellshock Vulnerability Simulation
### Configure Vulnerable LAMP Server
OT-Web-Server was configured with a PHP script test.php which models a Shellshock vulnerable CGI endpoint. In an operational Linux setup where we have Apache CGI, the script would be in the cgi-bin directory, /cgi-bin/test.cgi and would call out Bash. The simulation environment created by Packet Tracer leverages PHP to demonstrate the web server component that is targeted.
![Figure 8](https://github.com/user-attachments/assets/8d3e3657-35ab-44b3-9f78-8055a4fd3745)
Figure 8 : Web Server Configuration

### Exploitation Simulation
Simulation of the Shellshock exploit occurred at the Attacker-Node. In an actual vulnerable system, the User-Agent header that included "() {:; echo shellshock vulnerable" would make Bash execute the injected command and processes the CGI script after doing so. Remote code execution with web server privileges would be confirmed in the output as "Shellshock Vulnerable" preceding the normal CGI response.
![Figure 9](https://github.com/user-attachments/assets/a0d3d569-6311-4853-9976-56c30395a31d)
Figure 9: Shellshock Exploitation

### Attack Analysis & Mitigation
The Shellshock vulnerability takes advantage of a weakness in the way in which Bash processes environment variables that consist of the definition of functions (Ali, 2023). Apache interprets HTTP headers in an HTTP request interested in a CGI application, and turns them into environment variables which it forwards to Bash, which interprets the malicious User-Agent query value of a function and a command and perpetrates the malicious request as local user. The involved components of LAMP stack include Linux (vulnerable Bash), Apache (handling of HTTP requests), and created environment between MySQL and PHP. With the help of remote code execution on OT-Web-Server, an attacker can obtain SCADA access, steal operational data, and move laterally, as well as cause disruption of water treatment. Mitigation would involve patch control (updating Bash), Web Application Firewall rules to identify () { patterns and network isolation of the web server with SCADA. Responsible disclosure and coordinating the distribution of patches was facilitated by the SEI CERT Coordination Center (VU#252743).

  ## Security Incident Response Exercise
### Attack Path Reconstruction
The Shellshock vulnerability provided access to the OT-Web-Server by the attacker as an arbitrary command as the user of www-data. The attacker used the stolen credentials found in the compromised server to open unauthorized sessions of SSH on to the SCADA-Controller-1 and SCADA-Controller-2. Out of these SCADA equipment, the attacker obtained 2.3 GB of operational data that consisted of water treatment parameters. The data that had been exfiltrated was transferred via the OT-Web-Server back to the Attacker-Node. The trace of the attack reveals the value of network segmentation because the web server and SCADA controllers were in the same /24 subnet, which allowed them to move laterally.
![Figure 10](https://github.com/user-attachments/assets/df2719ac-9918-4dac-b826-32e6b329efeb)
Figure10 : Attack Path Diagram

### Root Cause Analysis
Table 1: Root Cause Analysis

| Vulnerability Category | What Went Wrong | Security Control That Was Missing |
|----------------------|----------------|----------------------------------|
| Unpatched Software | OT-Web-Server had an outdated Bash which is vulnerable to Shellshock (CVE-2014-6271) that enabled it to execute remote code execution by creating CGI scripts. | Patch management that includes automatic scanning of vulnerabilities and remediation within 30 days of disclosure. |
| Network Segmentation | OT-Web-Server was unhindered in its access to SCADA controllers operating on the same subnet (/24), which allowed lateral movement after compromise. | Micro-segmentation firewall rules to restrict access to only required protocols and destinations. |
| Access Controls | CGI script was executed with high privileges and had SSH access to SCADA machines, violating the principle of least privilege. | Firewall egress filtering, application allowlisting, and SSH access via jump hosts with MFA. |
| Monitoring & Detection | The 2.3 GB data leakage was not detected until after completion and no alerts were triggered. | SIEM with behavioral analytics, file integrity monitoring, and endpoint detection and response capabilities. |


### Comprehensive Remediation Plan
Containment within 0-24 hours involves applying the ACLs surrounding the OT network, recover the forensic images of the compromised systems, reset all the credentials throughout the OT environment and terminate all active SSHs. The incident response division needs to retain evidence and report to CISA. Short term solutions in 1-7 days involve implementation of Bash patches on all Linux machines, installation of restrictive firewall policies where the web server is only accessible to IT subnet and no access to web server to SCADA devices, enabling extensive logging, and using mod-security with Shellshock detecting rules. The defense-in-depth in combination with IDS/IPS, use of zero trust and micro-segmentation concepts, relocation of OT-Web-Server to a special subnet in the DMZ and the use of least privilege with restrictive CGI scripts are long-term improvements that should be implemented within the 1-3 months period. To achieve monitoring and detection, the application of SIEM using baseline behavioral analytics, setting up of alerts to unusual data transfers, and file integrity monitoring is required (Granadillo et al., 2021). The enhancements to the organization involve enforced patch management policies, quarterly incident response drills, and extensive security awareness education of the OT and IT personnel.

## OSI Model Mapping Summary
| OSI Layer | Lab Activity | Technologies Used | Security Implications |
|----------|-------------|------------------|----------------------|
| Layer 1: Physical | Topology cabling | Copper straight-through cables | Physical security prevents unauthorized device access |
| Layer 2: Data Link | Switch configuration | MAC tables, VLAN 1 | MAC flooding attacks require port security controls |
| Layer 3: Network | IP addressing, static routing | IPv4, routing tables | IP spoofing requires ACL implementation |
| Layer 4: Transport | TCP vs UDP analysis | TCP, UDP | SYN floods require stateful firewalls |
| Layer 5: Session | SSH session establishment | SSH, VTY lines | Session hijacking requires encryption |
| Layer 6: Presentation | HTTPS encryption | TLS/SSL, certificates | Certificate validation prevents MITM attacks |
| Layer 7: Application | HTTP/HTTPS, CGI, Shellshock | HTTP, CGI-Bash | Command injection requires Web Application Firewalls |

## Reflection
This experiment was an informative experience with respect to the direct effect that network architecture has on critical infrastructure security systems. Shellshock vulnerability proved that in case of the lack of proper segmentation, a single system without a patch would result in an entirely compromised OT network. The level of access allowed between the LANs of OT-Web-Server and SCADA controllers allowed movement among the systems, which would have been blocked by effective firewall policies. The 2.3 GB data leak which was not detected indicated that the organizations are unaware of the ongoing breaches without the presence of SIEM with behavioral analytics. Water utilities such as WBMUD are also good targets since interference with them will impact on health and safety of people. The fact that the cybersecurity evaluation of water infrastructure is met with 70% failure rate is a strong indication that there is an urgent need to deploy the principles of zero trust where a micro-segmentation, constant verification, and least privilege would be applied so that in case the web server is hit, the SCADA population controllers would not be affected. Ultimately, this experiment has highlighted the fact that security engineers should think in a holistic manner not only looking at technical controls but also organizational processes to secure critical infrastructure.

## References
Ali, N. (2023, November 8). What is Shellshock vulnerability? What Is Shellshock Vulnerability? https://beaglesecurity.com/blog/vulnerability/shellshock-bash-bug.html
Granadillo, G. G., Zarzosa, S. G., & Diaz, R. (2021). Security Information and Event Management (SIEM): Analysis, Trends, and Usage in Critical Infrastructures. Sensors, 21(14), 4759. https://doi.org/10.3390/s21144759
Jayanti Katariya. (2023, August 16). Telnet vs SSH: Unveiling the Differences in Remote Network Access. Moon Apps. https://www.moonapps.io/blog/telnet-vs-ssh/
Sikos, L. F. (2020). Packet analysis for network forensics: A comprehensive survey. Forensic Science International: Digital Investigation, 32, 200892. https://doi.org/10.1016/j.fsidi.2019.200892


