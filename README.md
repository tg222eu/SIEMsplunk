# SIEMAzureSentinel

The purpose of the lab is to collect Windows event logs of brute force attack on RDP. Once setup we will have an exposed VM that logs traffic what an potentianl attacker does aka SIEM. The guide will be covered from a complete beginner level.

First we need a virtual machine. It could be a virtual machine created on your own computer, just make sure it within a isolated network. For this lab we will be using Microsoft Azure Cloud. 

(Search virtual machines and click on the service), which purpose is to be exposed, or a honey pot, then click create -> Azure virtual machine.

Create a new resource group, we will be using all the lab resources in this group. We will call it "Honeypotlab"

Name the virtual machine "Honeypot-vm", leave the image and size as default. Image: Windows 11 pro Version 22H2 - Gen1, Size: Standard_D2s_v3 - 2 vcpus, 8 Gig memory. Make sure you use US region if you are using free account as its the only allowed region.

Create a username and password for the VM. Tick the "Licensing" and hit Next:DISK

Leave everything default in DISK and go straight to Networking

In networking we will change the "NIC network security group", its kind of a firewall. Hit Advanced and "Configure network security group" will appear. Create a new one
The purpose of the security group is to allow all traffic to pass. Remove the default rule and hit "Add an inbound rule"
In Desitination port range put a "*" mark, Protocal: ANY, Action: Allow, Priority: 100, and name the rule. Our case "DANGER_ANY_IN", Then hit okay.

Final step is to hit "Review + create" This will take some time to create. Meanwhile we can create log analytics workspaces

We will be ingesting windows events log from the virtual machine we just created with our own custom log that contains geographical information so we can see where the attackers are coming from. 
Next go to log analytics workspaces and hit "create log analytics workspace". In the resource group, select the resource group we created. In our case "Honeypotlab" and give it a name, example "law-honeypot" and select your region.

Next go to Security Center. Click oin pricing & Settings, then click on the "law-honeypot" we just made and turn on "Azure defender on", leave servers ON but turn OFF the SQL servers on machines as we dont need it.
Then head to Data collection on the left panel and press "All events", then hit save.

Go back to log analytics workspace and connect it to the "Honeypot-VM". Click "law-honeypot" workspace, go to virtual machines and select the "honeypot-vm" then "Connect"

Now we will create Azure Sentinel. This is our SIEM that we will use to visualize the attack data. Head to Azure sentinel, hit create and then add "law-honeypot"

Once the virtual machine is finished creating, head to Virtual machine, click on the machine and then copy the public IP address that has been created for the machine. Open up Remote Desktop, paste the IP-address and connect. Then click on more choices, use a different account and login with the username and password you provided. 

Open windows firewall (wf.msc), go to windows defender firewall properties, in Domain- private and public profile turn of the firewall state then hit apply.

We also want an API  that convert the IP address from the windows security logs to get long/lat location, which we then can add to a map and track where the attackers are from. This simply stores a seperate log which we can use later. The free version will let you log 1000 request per day. You need to register in https://ipgeolocation.io mainly to get an API key which you have to paste in a powershell scrip on your VM

https://ipgeolocation.io

<b>Creating a custom log<b>

Go to Log analytics, click on law-honeypot1, select custom logs and add new custom logs. Here you need to upload a custom log. The custom log can be found in https://ipgeolocation.io

In recoird delimiter, check that everything is okay

In collection path enter the windows path that the logs that will be stored in the honeypot VM

Details name the custom log example FAILED_RDP_WITH_GEO
