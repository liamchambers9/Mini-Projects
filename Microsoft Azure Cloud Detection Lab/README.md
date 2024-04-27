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

Article Documentation

https://medium.com/@lchambers4383/microsoft-azure-cloud-detection-8bba1f31315
