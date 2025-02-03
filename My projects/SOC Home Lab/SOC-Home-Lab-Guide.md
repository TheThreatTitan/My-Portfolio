# How to Build a SOC Home Lab with Elastic Stack
This is a project and a guide on setting up a home SOC lab, particularly using Elastic Stack Security Information and Event Management. I generated security events on a Kali VM, set up an agent to forward the data to the SIEM, and query and analyze the logs in the SIEM.

**Prerequisite**
1.	VirtualBox or VMware
2.	Basic knowledge of Linux and virtualization software.

**Overview of the tasks**
•	Set up a free Elastic account and install Kali VM
•	Configure the Elastic Agent on the Linux VM to collect the logs and forward it to the SIEM.
•	Generate security events on the Kali VM.
•	Query to find the security events in the Elastic SIEM.
•	Create a Dashboard to visualize security events.
•	Create alerts for security events.

**Step 1: Set up your Elastic Stack Agent in Kali**
Please note: These instructions are written with the assumption that you have the Kali VM set up and the instructions using Elastic Stack are as accurate of 31st January 2025.
In the context of Elastic SIEM, an agent is used to collect and forward security-related events from your endpoints to your Elastic SIEM instance.

1.	Sign up for a free trial to use Elastic Cloud (https://cloud.elastic.co/registration)
2.	Follow the Wizard and select Elastic for Security Instance image
3.	Log in to your Elastic Security instance and select Fleets from the asset menu.
4.	Select Add agent.
5.	Make sure agent is enrolled in the fleet.
6.	Copy the command into your kali and wait for it to download. image You need root privileges for this installation or use an account with sudo privileges.
8.	When installation is complete, you’ll see the agent in your fleet and its agent policy.

**Step 2: Set up log collection**
To set up the agent to collect logs from your Kali VM and forward them to your Elastic SIEM instance, follow these steps:
1.	Navigate to the Integrations page by: clicking on the management settings at the bottom of your side menu, then selecting “Integrations”.
2.	Select “Elastic Defend” and click on it to open the integration page. add elastic defend
3.	Name the integration and add description so another person who has to work with it.
   ![image](https://github.com/TheThreatTitan/My-Portfolio/blob/main/My%20projects/images/ES5.jpg?raw=true)
5.	Select existing policy and use the agent policy associated with your machine
6.	Your setup is complete

**Step 3: Generating security events**
To view how the agent works, you can generate some security-related events on Kali. To do this you can use Nmap for port scanning, authentication errors, and hash cracking. You can run the following commands:
1.	Sudo nmap -sT -O <your ip address>, sudo nmap -sS -A -Pn <your ip address> image

**Step 4: Querying for Security Events in Elastic SIEM**
1.	In the left menu, click on explore, then Hosts under the explore options. Here you’ll see logs from all your Hosts. 
2.	In the search bar, enter a search query to filter the logs. For example, “nmap_scan” or “nmap” . It might take sometime for the events to load.
3.	The results will be displayed in a table below. You can click the expand button to view more details and the expand button to view even more details. Image

**Step 5: Create a Dashboard to Visualize the Events**
Dashboards helps to visually analyze the logs and identify patterns or anomalies. For this project, you can create a dashboard that shows the count of security events over time.
1.	Click on Dashboards on the left menu.
2.	Click on create new dashboard on the top right to create a new dashboard.
3.	Click on create visualization
4.	Select your visualization type depending on your preference. For this example, “Bar chart” and “stacked” is used. Image
5.	In the metrics section, select count in the vertical field type and timestamp in the horizontal field
6.	Click on save button to save the visualization. image

**Step 6: Create an Alert**
Alerts are created based on predefined rules, and can be configured to trigger specific actions when certain conditions are met. For this project, I created an alert that triggers during nmap scans.
1.	Click alerts on the left menu, and then manage rules on the right alerts dashboards.
2.	This will take you to the rule’s menu. Click on the create new rule button
3.	Under the “Define rule” section, select the “Custom query” option.
4.	Under custom query, set the condition for the rule. For example use the query “event.action: “nmap_scan”” to detect Nmap scan events, then click on continue.
5.	Under the “About rule” section, give your rule a name and description. 
6.	Select the severity level and click continue. Image
7.	In the “Actions” section, select the action you want to take. You can choose to send a message by slack, teams, or mail e.t.c image
8.	Click on create and enable rule to create the alert.

Conclusion
If you followed me on this project, you must have successfully set up a home lab using Elastic SIEM and a Kali virtual machine.
