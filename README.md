# SIEM Splunk

The purpose of the lab is to collect Windows event logs to Splunk of brute force attack on RDP. Once setup we will have an exposed VM that logs traffic what an potentianl attacker does aka SIEM. The guide will be covered from a complete beginner level.


<h2>Creating cloud machine</h2>

First we need a virtual machine. It could be a virtual machine created on your own computer, just make sure it within a isolated network. For this lab we will be using Microsoft Azure Cloud. 

Head to Azure cloud to create a virtual machine
(Search virtual machines and click on the service), which purpose is to be exposed, or a honey pot, then click create -> Azure virtual machine.
Create a new resource group, we will be using all the lab resources in this group. We will call it "Honeypotlab"
Name the virtual machine "Honeypot-vm", leave the image and size as default. Image: Windows 11 pro Version 22H2 - Gen1, Size: Standard_D2s_v3 - 2 vcpus, 8 Gig memory. Make sure you use US region if you are using free account as its the only allowed region.
Create a username and password for the VM. Tick the "Licensing" and hit Next:DISK
Leave everything default in DISK and go straight to Networking
In networking we will change the "NIC network security group", its kind of a firewall. Hit Advanced and "Configure network security group" will appear. Create a new one
The purpose of the security group is to allow all traffic to pass. Remove the default rule and hit "Add an inbound rule"
In Desitination port range put a "*" mark, Protocal: ANY, Action: Allow, Priority: 100, and name the rule. Our case "DANGER_ANY_IN", Then hit okay.
Final step is to hit "Review + create" This will take some time to create. Meanwhile we can create log analytics workspaces

<h2>Install Splunk server + forwarder</h2>

Now we will be installing Splunk on our own computer. Download Splunk Enterprise from www.splunk.com. Splunk Enterprise has a free trial version that allows 500mb per day, and thats plenty for our case. You will need to create an account for this.
During installation you will be prompt to enter username and password. This credential is for logging in on the webinterface of splunk. When finished installation, login on http://127.0.0.1:8000 or the IP address which the splunk server is on.

Now we need to setup a receiving port. In Splunk, go to Settings, then forwarding and recieving, and under Recieve data hit +add new. Enter the default port 9997 and hit save

![alt text](https://github.com/tg222eu/SIEMsplunk/blob/main/SplunkForwarderInstaller.JPG)

Now login to the cloud computer, download and install Splunk Universal Forwarder. Make sure you do the "Customize Option". Hit next until you get to the options what to logs. Select all boxes under Windows Event Logs and During the installation you will need to enter the IP address of your splunkserver, if its on the same computer then enter 127.0.0.1 and port 9997 as you configured.

![alt text](https://github.com/tg222eu/SIEMsplunk/blob/main/datasummary.JPG)

Now the machine should be sending logs to the server. Make sure by loggin into the server, press search&report, press "Data Summary" and your host should appear with some atleast system generated logs. If logs are generated then let the machine run for as long as you wish.

<h2>Result</h2>

Here is the result of running the machine for 7 days. 

![alt text](https://github.com/tg222eu/SIEMsplunk/blob/main/map.JPG)

![alt text](https://github.com/tg222eu/SIEMsplunk/blob/main/account.JPG)
