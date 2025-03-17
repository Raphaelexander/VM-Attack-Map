# VM-Attack-Map
This is a lab I ran using Microsoft Azure's virtual machines and workspaces to create a honeypot machine for threat actors. I created a SIEM tool in Azure using the Event Viewer and KQL to track the IPs of that unsuccessfully logged into the machine. This was a very insightful lab about where many threat actors can be located, as well as how many attempts they make to break into a machine.

![image](https://github.com/user-attachments/assets/56b101ee-5e35-41de-8cb2-71d9bb079685)


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

I created a log analytics workspace in Azure and named it LAW-SOC-LAB, then created a Sentinel instance and connected it to my log analytics. This allowed me to create a SIEM tool to track events, and query the system using KQL to clean up the data for further analysis. In the screenshot above, I was able to query all of the security events with EventID 4625, and only look at the data I wanted to see, such as what account name the threat actors tried to sign in with, the name of my VM they were trying to get into, the eventID, and the most important part, the IP address associated with the machine that was attempted to log in. This gave me a lot of information to work with, too many logs to count. So I decided to refine the data further and move onto sept 4.

4. Log Enrichment and Specific Location Data:

   ![image](https://github.com/user-attachments/assets/8ada9f94-8646-4243-9837-dabcb826bbdc)

With the help of Josh Madakor (https://www.youtube.com/@JoshMadakor) on YouTube, I was able to import a large datasheet he made that housed over 55,000 different IP addresses and their general locations around the world. Within Microsoft Sentinel I created a workbook that lets me create graphs, spreadsheets, and other visual data representations of any input of the VM that I was running. After I configured the backend location data, I was able to import it with KQL. It was a further refinement from step 3, but this time I created columns that matched the IP address with the nearest city, the country the actor's IP was in, and the latitude and longitude. This within itself is fascinating to see. There are patterns of countries already emerging, as well as a couple outliers that could lead to further analysis.

5. The Attack Map:

   ![image](https://github.com/user-attachments/assets/bb59c8a2-609b-4b63-bb27-502fb2a978b0)

Josh Makador also provided this JSON for the lab, which almost seamlessly read the data from the VM and displayed where threat actors's IPs originated. Many factors can affect the IP address of a machine, so this map is not necessarily accurate. If someone is scouring the internet to access a computer with very low security, they're most likely using a VPN or IP spoof themselves to avoid legal consequences. What was the most insightful from this data was the sheer amount of attempts the attackers were able to make on my machine with no repercussions. Most of the attacks from Poland and Belguim came from a single IP address. So these one or two individuals found my honeypot machine and were able to brute force password attempts over 16,000 times before they eventually got bored and moved onto their next victim. 

## Conclusion
Overall, this lab was very insightful into how and where threat actors operate. It also taught me the power of just a 15 character password. I ran this VM only for about 3 hours, and this many attempts were made on my machine. It is frightening to see how vulnerable we are on the internet on any given moment. This is why it is so important to practice standard security measures such as unique passwords, MFA, and the use of IDSs. I also got some valuable hands on experience with my own SIEM tool, KQL for data cleaning, and Microsoft Azure. These are valuable skills for SOC analysts in order to help protect network infrastructure.

For the future of this lab, I plan on further refining the data I collected. I also want to cross reference the location of attacks as well as how many different IPs those attacks came from. I am also curious how attackers would act if they got into my system. What would they go for? Would they look for private files or try to get further into my network? I plan on creating more honeypot files within my VM to find what they are most attracted to
