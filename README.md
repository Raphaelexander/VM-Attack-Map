# VM-Attack-Map
This is a lab I ran using Microsoft Azure's virtual machines and workspaces to create a honeypot machine for threat actors. I created a SIEM tool in Azure using the Event Viewer and KQL to track the IPs of that unsuccessfully logged into the machine. This was a very insightful lab about where many threat actors can be located, as well as how many attempts they make to break into a machine.

Here are the steps I took to conduct this lab:
 -
1. Create a honey pot VM in Microsoft Azure:

    ![image](https://github.com/user-attachments/assets/7cba5e9e-0707-45d6-a3ba-82f1529b0a03)

I created a new VM and named it something official like "CORP-NET-EAST-2". I used the US-East-2 server so this seemed appropriate. I set the password to an appropiate 15 character length, and used a username called LabTest1. I then changed the inbound network rules to allow any inbound traffic on any port on any protocol. I also logged into the virtual machine and turned off Windows Firewall completely.

2. Test the VM Event Viewer:

   ![image](https://github.com/user-attachments/assets/d4109820-cb74-4ea5-93ca-c921e00c3db6)

I logged out of the VM, just to attempt to log in with a username called "Employee". After failing 3 times, I logged into the VM successfull and pulled up the Event Viewer. I noted that my three attempts back to back resulted in an EventID 4625 Audit Failure. The Event Viewer then gave me detailed information of the "threat actor" (myself), including username I attempted to sign in with, date, time, and even my public IP. I left the VM running and moved on to the next step.

3. Log Analytic Forwarding using KQL:

   ![image](https://github.com/user-attachments/assets/a2493b5f-592f-4046-9665-7909bb709733)

I created a log analytics workspace in Azure and named it LAW-SOC-LAB, then created a Sentinel instance and connected it to my log analytics. This allowed me to create a SIEM tool to track events, and query the system using KQL to clean up the data for further analysis. In the screenshot above, I was able to query all of the security events with EventID 4625, and only look at the data I wanted to see, such as what account name the threat actors tried to sign in with, the name of my VM they were trying to get into, the eventID, and the most important part, the IP address associated with the machine that was attempted to log in.
