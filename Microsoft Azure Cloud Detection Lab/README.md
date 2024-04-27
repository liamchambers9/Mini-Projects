In this lab I created a Resource Group to manage related resources and deploying a Windows 10 Virtual Machine within it. Using best practices in security I was able to add Just-In-Time access for added security. Microsoft Defender for the cloud was also enabled. A Log Analytics Workspace is established for log monitoring, and Microsoft Sentinel is set up to analyze data. Data connectors for Windows Security Events are configured, and users connect to the VM via RDP to analyze event logs. I used Kusto Query Language to query event logs. Local security policy was configured to log object access events, and a simulated malicious scheduled task is created for testing. An alert rule is written in Microsoft Sentinel to detect the fake event, demonstrating incident detection and response capabilities.



As a student studying information assurance and cybersecurity, I find it imperative to start getting experience in Security Information and Event Management (SIEM). This is due to the fact that SIEM is a critical tool in the detection, analysis, and response to security incidents. This article is intended to document my experience of building out a Microsoft Azure Cloud Detection Lab. Before we get started this lab was explained by CyberWox Academy and was similarly followed along by me. If you are just getting started with your journey in Cybersecurity and interested in building incredible relationships with a great community I strongly encourage you to give them a visit.

This lab will cover the following content:

Create an Azure Resource Group
Deploy a Windows 10 VM (Virtual Machine)
Implement Network and Virtual machine Security Best Practices (Just-in-time access)
Leverage Data Connectors for ingesting data into Microsoft sentinel (Cloud Native SIEM) for evaluation
Gain Insight into Windows Security Event Logs
Fine-tune Windows Security Policies for Enhanced Protection
Adjust Windows Security Policies
Harness Kusto Query Language (KQL) for Log Exploration
Create Custom Analytic Rules to Identify Microsoft Security Incidents



My Topology: Microsoft Azure Cloud Security & Detection 
![image](https://github.com/liamchambers9/My-Projects/assets/101218893/36ead783-dd1b-42ec-8c04-a895eaec65c4)


Acknowledgments
A huge thank you to the original content creator Charles Quansah @CyberWox Academy. Please make sure to make a visit

The Resources used in this article came from the following:


https://cyberwoxacademy.com/azure-cloud-detection-lab-project/?source=post_page-----8bba1f31315--------------------------------


https://youtu.be/Ttcv75dV-00?si=fXOOntnqkdXpIOu5



Part 1: Creating a free account with Azure
In order to get started in this lab exercise you must make an Azure account. An email address and credit card will be needed for the creation of this account, but otherwise, the process is free.




Part 2: Create a Resource Group
Once your Azure account has been created you will find yourself at the Azure Portal, from here your first task will be to generate a Resource Group. A resource group in Azure is a container for related resources in a solution. You are now able to include either all the resources for the solution or only select resources in a group for easy deployment, management, deletion, or updates. This is what makes a resource group a fundamental component of resource management in Azure.

At the Microsoft Azure Portal search for “Resource Groups” in the search bar

Select “Create”

Fill in the required fields:

Subscription: Select your Microsoft Azure Subscription
Resource: Group: Give your Resource Group a Name
Region: Select a region (I recommend leaving it on default)
Then select Review + Create


![image](https://github.com/liamchambers9/My-Projects/assets/101218893/a4dd0517-41ce-4cdf-b91b-54dd8b76a6e1)

On the Review + Create screen your Resource group will create a Validation Check. Once it has passed this, select Create to finish creating the Resource Group.



Part 3: Create and Deploy a Windows 10 Virtual Machine
At the upper left corner of the screen click the Portal Menu

Select “Create a resource”

Select “Virtual machine”

On the Create a virtual machine page fill in the following:

Subscription: This likely will stay default assuming you only have 1 subscription
Resource Group: Select the Resource Group you’ve created from Part 2.
Virtual Machine Name: Give your VM a Name
Region: Select a region: (I recommend leaving it on default)
Availability Options: Leave Default
Availability Zone: Leave Default
Security Type: Leave Default
Image: Windows 10 Pro, version 22H2 — x64 Gen2 (free services eligible). (At the time of documentation 22H2 is the option, but this will change as Microsoft Releases newer patches of Windows 10)
VM architecture: Default
Run with Azure Spot Discount: Leave unchecked
Size: Standard D2s v3 2 VCPUs 8GiB Memory (Default)
Username: Create a username to use to log into this virtual machine
Password: Create a password to use to log into this virtual machine
Public inbound ports: Allow Selected Ports (Default)
Select inbound ports: RDP 3389 (Default)
Licensing: Check the Box
Select Review + Create

*Note if you wish to review all the remaining tabs please feel free to do so, but otherwise they can stay on the default settings.


![image](https://github.com/liamchambers9/My-Projects/assets/101218893/bb8f66f5-a9b9-42c3-bb0c-861e8cb66409)

On the Review + Create Screen wait for the Validation to pass and then select Create.

Your Virtual Machine is now being created. This process will likely take just a few moments to complete.

Part 4: Network Security
Taking a look at the virtual machine we’ve just created, we can see valuable information such as the Resource group, Public IP address (which will be important when we connect to the virtual machine), and other key specifications about the machine.


![image](https://github.com/liamchambers9/My-Projects/assets/101218893/fa86c994-504b-453d-8663-be4c6a7f6e86)

If we click on the “Networking Tab” we will be able to see the Inbound port rules. Looking at the first rule RDP for port 3389 we can see that this connection will allow any RDP port into the virtual machine, this can be quite dangerous. With this virtual machine being out on the internet it makes this VM an attractive candidate for someone to scan and obtain the IP address leading to various attacks such as password spraying and brute-force types of attacks.

In order to mitigate this vulnerability we will be using Microsoft Defender for Cloud

From the search bar type Microsoft Defender for Cloud

Select your Subscription

Select Upgrade

![image](https://github.com/liamchambers9/My-Projects/assets/101218893/b299bc78-d4c5-4a25-b38f-3d7e91c7d8dd)


Once Upgrade is clicked a notification should appear indicating that the deployment of Microsoft Defender for Cloud has been Deployed Successfully (The notification may take a minute).

Next, you will arrive at the Microsoft Defender for Cloud | Getting Started screen. Select Enable All Plans.

Select save


![image](https://github.com/liamchambers9/My-Projects/assets/101218893/3dfa4b3a-1201-42e7-af5d-23f22ebc95af)


Part 5: Using Just-In-Time Access
Let's checkout the coverage that Microsoft Defender for Cloud gives us.

From the sidebar scroll to the Cloud Security section

Select “workload protections”

Workload protections give us a glance at our overall security posture. This allows us to gain a bird's-eye-view of what installs are covered, what installs are not covered, and compliance that is being met.

![image](https://github.com/liamchambers9/My-Projects/assets/101218893/0a52ea3f-a5c5-4419-9203-d9cb69f8b09e)




Okay, so we have a bird's-eye-view of our virtual machine’s security. Now we have to make sure that this virtual machine can enable authorized users only when it’s needed, Microsoft Defender for Cloud helps us with that by allowing just-in-time (JIT) access for Azure VMs. Just-in-time permits access only when it’s required, through designated ports, and for a restricted duration which helps prevent vulnerabilities associated with permanent firewall rules.

Now enable just-in-time (JIT) access to our virtual machine

Select the “Search Bar” and search for virtual machines

Select the virtual machine you’ve created

Select “Connect”

Select Configure for this port

Select “Configure”

![image](https://github.com/liamchambers9/My-Projects/assets/101218893/4a17c985-66f4-4634-979c-f7e6c7845048)

Shortly after a notification should come on the screen saying “Just-in-time policy successfully configured”.

Now If we come back to the networking tab on our virtual machine the inbound port rules look a little different than before.

Notice that “Microsoft Defender For Cloud” is now listed at port 3389. Now due to rule priority, RDP traffic is allowed for a certain amount of time only from the IP of your computer via port 3389.


![image](https://github.com/liamchambers9/My-Projects/assets/101218893/1c77bedb-79e8-40d8-844a-c90150d4de0a)



https://medium.com/@lchambers4383/microsoft-azure-cloud-detection-8bba1f31315
