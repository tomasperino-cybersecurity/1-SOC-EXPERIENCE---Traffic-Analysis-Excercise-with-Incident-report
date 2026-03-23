🕵️‍♂️ # 1-SOC-EXPERIENCE---Traffic-Analysis-Excercise-with-Incident-report

🌎 Background
As dynamic go-getter at a Security Operations Center (SOC), you check the Security Information and Event Management (SIEM) system and find several signature hits for NetSupport Manager RAT from 45.131.214[.]85 over TCP port 443. The activity started on 2026-02-28 at 19:55 UTC.

Using this information, you quickly retrieve a packet capture (pcap) of the traffic from the internal IP address that triggered these alerts. It's all on you now! You're expected to write up an incident report, so someone can track down the infected computer and put a stop to this nonsense!

Malicious activity related to NetSupport Manager RAT was detected from the external IP address 45.131.214.85 over TCP port 443. Network traffic analysis identified a compromised internal host within the 10.2.28.0/24 network segment.

The program used to analyze the packets was Wireshark 🦈​

🌎​ Environment Details
- Lan Range: 10.2.28.0/24
- Domain: easyas123.tech
- Domain Controller: 10.2.28.2
- Gateway: 10.2.28.1

​🔎​ Findings:
- Infected IP Address: 10.2.28.88
- Infected MAC Address: 00:19:d1:b2:4d:ad
- Attacker IP Address: 45.131.214.85
- Infected Hostname: DESKTOP-TEYQ2NR
- Infected Windows Account: brolf
- Infected Windows User: Becka Rolf

​⚠️​ Activity: Persistent communication over TCP 443 (TCP ACK Packets)(Retransmissions)(Persistent Connection Pattern)
​⚠️​ Type: Possible Command & Control (C2)
This behavior is consistent with Remote Access Trojan

🛜​ Network Behavior:
Continuous communication observed between the internal host and the external IP.
HTTP traffic was observed prior to the malicious communication, however, the initial infection vector could not be confirmed.

🧐​ Process & Filters:
ip.addr == 45.131.214.85
- With this filter I was able to identify the Persistent and Repetitive Communication from IP 45.131.214.85 to the internal IP 10.2.28.88 and its MAC address

<img width="903" height="453" alt="image" src="https://github.com/user-attachments/assets/fcd0c441-90fe-4644-9947-89ee961737eb" />

<img width="551" height="129" alt="image" src="https://github.com/user-attachments/assets/c3016487-3385-4409-ad40-751e707654aa" />

nbns
- This filter allows me to identify the Hostname (DESKTOP-TEYQ2NR) that corresponds to the IP address 10.2.28.88

<img width="862" height="125" alt="image" src="https://github.com/user-attachments/assets/26759eb6-0b26-4b06-827a-016b263ceebb" />

kerberos.CNameString
- This filter allows me to identify the Windows user account name (brolf). The employee's last name is "Rolf".

<img width="664" height="124" alt="image" src="https://github.com/user-attachments/assets/103ff43f-9469-49d5-a987-5dcd58b2578c" />

<img width="313" height="163" alt="image" src="https://github.com/user-attachments/assets/84a4de7d-a449-4b6b-a912-ce04548ee9f8" />

With this information, I filter for packages that contain "Rolf"

<img width="943" height="80" alt="image" src="https://github.com/user-attachments/assets/247e5779-620a-4f54-8185-74997a3fe084" />

<img width="472" height="314" alt="image" src="https://github.com/user-attachments/assets/c12c2572-8655-46d9-87ed-29fc6bfbdc5b" />

😎​ Conclusion:
The host 10.2.28.88 (DESKTOP-TEYQ2NR) shows behavior consistent with a NetSupport RAT infection, based on persistent communication with known external infrastructure.
The initial infection vector and associated user could not be definitively determined.

🛡️​​ Recommendations as a Cybersecurity Professional:
1) Containment
- Isolate the infected host.
- Block communication with the malicious IP.

2) Investigation
- Perform full antivirus/EDR scan.
- Check for persistence mechanisms.

3) Credential Security
- Reset user credentials.

4) Network Security
- Monitor for similar traffic patterns.
- Implement detection rules.

5) Hardening
- Restrict unnecessary HTTP traffic (Only HTTP servers used by the company will be enabled.)
- Apply outbound filtering policies.
