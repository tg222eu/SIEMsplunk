# SIEMAzureSentinel

The purpose of the lab is to great a Azure cloud environment using the build in Sentinal lab. Once setup we will have an exposed VM that logs traffic what an potentian attacker does aka SIEM. The guide will be covered from a complete beginner level of Azure cloud.

First create a virtual machine (Search virtual machines and click on the service), which purpose is to be exposed, or a honey pot, then click create -> Virtual machine.

Make sure the subscription is selected on "Pay-as-you-go", as it will only use up the payment of the free resources provided when the account was created

Create a new resource group, we will be using all the lab resources in this group. We will call it "Honeypotlab"

Name the virtual machine "Honeypot-vm", leave the image and size as default. Image: Windows 10 pro Version 20H2 - Gen1, Size: Standard_D2s_v3 - 2 vcpus, 8 Gig memory

Create a username and password for the VM. Tick the "Licensing" and hit Next:DISK

Leave everything default in DISK and go straight to Networking

In networking we will change the "NIC network security group", its kind of a firewall. Hit Advanced and "Configure network security group" will appear. Create a new one
The purpose of the security group is to allow all traffic to pass. Remove the default rule and hit "Add an inbound rule"
In Desitination port range put a "*" mark, Protocal: ANY, Action: Allow, Priority: 100, and name the rule. Our case "DANGER_ANY_IN", Then hit okay.

Final step is to hit "Review + create"
