
Building a Simple Network
Overview
In this project, I designed and configured a simple network for Hackers Poulette, a young startup. The objective was to set up a network that would allow communication between three hosts, connect them to the internet, and ensure the network is easily scalable for future growth.

What
This project involves creating a simple local area network (LAN) with three hosts (PCs) and a Cisco 2960 switch. The hosts are assigned static IPs in the range of 192.168.1.10 - 12 with a subnet mask of 255.255.255.0. Additionally, the router is connected to the internet with a public IP of 203.0.113.2 and a gateway of 203.0.113.1.

# Why
The goal of this network setup is to provide connectivity between the three devices while also connecting them to the internet. The setup should also be easily scalable as the startup grows, allowing for additional devices to be added in the future.

# When
This network was set up as part of a consolidation challenge and completed within one day.

# How
Devices Used:
Cisco 2960 Switch
Cisco 1841 Router
Three Windows 10 PCs
IP Configuration:
PC-Robert: 192.168.1.10, Mask: 255.255.255.0
PC-Camille: 192.168.1.11, Mask: 255.255.255.0
PC-Renaud: 192.168.1.12, Mask: 255.255.255.0
Router Interface 1: 192.168.1.1 (LAN Side)
Router Interface 2: 203.0.113.2 (Internet Side, Gateway 203.0.113.1)
Network Configuration:
Set up IP addresses for each PC.
Connected the PCs to the switch and the switch to the router.
Configured the router for internet access.
Verification:
Used the ping utility to check connectivity between the PCs and to the internet.
# Who
Team: Solo project.
Client: Hackers Poulette, a startup focused on growing its infrastructure.
Deliverables
Cisco Packet Tracer (pka) file with the completed network.
GitHub repository containing the pka file and this README.
Pending Tasks
Publish the pka file on the GitHub repository.
Add a link to the live version with access to the source code.
Share the live version link in the startup's Discord channel.
