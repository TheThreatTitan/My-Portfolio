## Walkthrough
We set up a virtual environment to simulate a typical enterprise network and implemented several key security measures to showcase their effectiveness. These measures include Windows Defender Firewall, BitLocker Encryption, Role-Based Access Control (RBAC), and Privileged Access Management (PAM). The detailed process of installation is as follows:

**1. Set Up a Virtual Environment**
   **a. Install a Hypervisor:**
-Choose a Hypervisor: Select VMware Workstation or [VirtualBox](https://www.virtualbox.org/wiki/Downloads), based on your system compatibility and requirements.
-Install the Hypervisor: Follow the installation guide provided by the hypervisor’s official documentation.
-Configure Settings: Allocate sufficient resources (CPU, RAM, Disk Space) to your VMs based on the intended workload.
   **b. Create a Windows Server VM:**
-Download Windows Server ISO: Obtain the ISO file for the version of Windows Server you plan to use.
-Create VM: Set up a new virtual machine in your hypervisor with the appropriate settings (e.g., disk size, network adapter).
-Install Windows Server: Boot the VM from the ISO and follow the installation prompts.

**2. Implement Active Directory and Domain Services**
**a. Install and Configure AD DS:**
Install AD DS Role:
 o	Open Server Manager.
 o	Add Roles and Features.
 o	Select Active Directory Domain Services and complete the installation.

•	Promote to Domain Controller:
o	Open Server Manager.
o	Follow the wizard to promote the server to a Domain Controller.
o	Set up the domain name (e.g., TechForAll) and other domain settings.

•	Create User Accounts:
o	Open Active Directory Users and Computers.
o	Create user accounts, groups, and organizational units (OUs) as needed.

**3. Implement Encryption**
**a. Setup group policy to use BitLocker without TPM**
o	Open Server Manager
o	Group Policy Management 
o	Add new Group Policy Object (BitLocker Policy) 
o	Follow the wizard to enable “Require additional authentication at startup”.
o	Link to the organizational unit 
o	Open the windows command prompt and enter the command “gpupdate /force” then restart the server

**b. Install and Configure BitLocker Drive Encryption:**
o	Open Server Manager.
o	Add Roles and Features.
o	Select BitLocker Drive Encryption and complete the installation.

**c. Enable BitLocker:**
o	Open Control Panel, go to System and Security, select BitLocker Drive Encryption.
o	Turn on BitLocker and follow the prompts to encrypt the server’s disk.

**Rationale:** Even if an attacker gains physical access to our machine, encrypted disks prevent them from reading the data without the proper decryption keys. It also helps maintain the confidentiality and integrity of the data stored on the disks.
image
image

**4. Implement Endpoint Protection**
**Install Endpoint Protection Solution:**
•	**For Windows Defender:**
o	Open Windows Security from the Start menu.
o	Ensure that Defender Antivirus is enabled and configure settings for real-time protection, cloud-delivered protection, and automatic sample submission.
•	**For Other Solutions:**
o	Install and configure third-party endpoint protection solutions according to their documentation.

**Rationale**: Endpoint protection helps to prevent unauthorized access to internal services and reduces the potential pathways an attacker can use to infiltrate or move within the network.

**5. Implement Role Based Access Control**
**a. Create Security Groups**
o	Under each department OU, create a security group
o	Add necessary members of the department to the group
**b. Create a shared FOLDER**
o	Under Files and Storage Services, create a new shared folder 
o	Enable access-based enumeration
o	Follow the wizard to customize permissions

**Rationale**: RBAC Assigns permissions based on roles rather than individual users, simplifying the management of user rights and ensuring consistent access policies. It also helps reduce the risk of misconfigured permissions and ensures that access is granted based on job roles.
image
image
**6. Implement Privileged Access Management (PAM)**
a. Enable Privileged Access Management
•	In the Windows PowerShell ISE, enter the command to display the privileged access management feature
• The first line of command displays the privileged access management
feature.
• The second line of command is to enable privileged access management.
We add the domain we want to target in this command. Note that this action is irreversible so once it is enabled it cannot be disabled.
• Now we want to add a user to a group for a specific period of time.
• The third line of command, we create a variable and set the time span.
• The fourth line we add the group members alongside the time to live
(variable assigned in line three).
• The fifth line; we get the group member. This displays all the information, and shows that the user is automatically removed based on the time to live.
•	Enter the command to enable the feature and add the domain you want to target.
image
b. Add user to a privileged group for a specific period
•	Enter the command to create a variable and set the period
•	Add the group members alongside the time to live assigned earlier

**Rationale**: This is extremely useful, especially in organizations where people might need certain access but not necessarily need to be granted permanently. This will reduce the possibilities of APTs, as it is easier to track users that have/had access and at the exact period they have/had it for.

**7. Configured firewall:** The firewall will filter the inbound and outbound traffic, block
unauthorized access to the network and permit secure communication.
• Firewall was automatically installed so we had to manage it.
• Configured the inbound and outbound rules under windows defender with
advanced security.
*Note:* New rules could be created based on the level of traffic

# Recommendations
Here are some protection and prevention tips against APTs for individuals as well as organizations
•	**Focus on solutions that address the malware risk**: Ninety-three percent of respondents say malware was the source of the attack so focus on solutions to address such risks.
•	**Pay more attention to targeted attacks**: They require more attention than opportunistic attacks. Respondents report that opportunistic attacks are less frequent and easier to prevent than targeted attacks. 
•	**Be very vigilant**: actively analyzing logs and performing tests of security measures can inform IT teams of the potential presence of APTs, allowing them to deal with these threats immediately. 
•	**Keep systems and apps updated**: APTs exploit vulnerabilities of devices and systems for many of their tactics. Developers regularly release patches and fixes to ensure that critical vulnerabilities are addressed.
