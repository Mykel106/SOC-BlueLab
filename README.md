#BlueLab

This is a Home lab project to practice and prove to my self I can build the skills to be a SOC Analyst: I will be attacking my own system, investigating what happened in log files Using Splunk and explaining it clearly to for myself and for someone who is non-technical. 

Why this project exists

To help practice for A SOC Analyst role, I do not want this to just be detection work i want it to act as a go between for end users and a SOC Team, breaking down complex log data and SOC concepts into guidance a non-technical person can actually use.
This will be built around 2 skills at once:
  1. The technical side - Running attacks, capturing telemetry, and finding it in Splunk
  2. The communication side - writing up every incident like I am explaining it to a partner who has no security background, not just logging findings for myself.

The Plan:

- set up a home lab: old desktop running VM and Splunk, with one Windows VM as the "victim"
- Attack that VM from my Kali Laptop, using real attacker techniques (Beginner)
- Use Splunk to find and investigate what each attack looked link in the logs
- Write up every attack as a short incident report, includes 5 Whys, in a clear plain language, also in technical language.

Hopeful End Result

A working lab plus a folder of documented "Incidents". Each one showing an attack technique, the raw evidence in Splunk, and a clear write up explaining it. Showing a portfolio that i can actually do the technical work and communicate it. 


**What I have Learned:(Running Log - Most recent at the top)**

- Diagnosed an Access Denied error using the forwarder's own log file. So I looked online and someone said read the actual log files from Splunk and I checked it out and found out that data was just not reaching Splunk even though the network path was confirmed open. Reading the actual log, splunkd.log, showed "errorCode=5" when the forwarder tried to subscribe to the Sysmon event log channel there was a permission problem. I looked it up and a reddit commit said that it is because I made a virtual account when installing the Splunk Universal Forwarder. Virtual accounts do not have the access needed to actually read the specific log. I fixed this by switching the service to run as Local System.(will include a Screen shot). What this shows me is this is a real least-privilege/access-control issue.   

- Ping is not reliable connectivity test sometimes. I did not know that Windows will also automatically block IMCP(ping) be default even on working connections. I used Test-NetConnection -Port 9997 instead, this tests the actual port that i setup with Splunk from what I read it is a bit more accurate.  

- Windows Firewall automatically block unsolicited Inbound connections by default. I had to manually create an Inbound rule (first time ever) to open TCP port 9997 before the desktop would accept forwarded data from the VM. 

- Splunk does not create a inputs.conf file automatically. I had to create this by hand with using there help function and research and A LOT of reddit. Also did not know that Window will automatically make files .txt not Conf unless you switch "Save as type" to All Files.  

- Learned that Virtual Box VM default to NAT networking, this isolates them from the reset of the home network. I had to switch to Bridged Adapter mode so the VM could reach Splunk on the host and would later be reachable by Kali(my Attack Box) too. 
