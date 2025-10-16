Lab 3 - In this lab i practised associating an application security group to the VMs, by doing this, instead of specifying IP addresses for the Network security groups(NSG), i could directly associate all resources connected with that ASG.
Goal
Use Application Security Groups (ASGs) to control inbound traffic so that:

VM1 (Web tier) can connect to VM2 (DB tier) on SQL port 1433.

All other traffic is blocked.

Steps
Step 1: Create Application Security Groups
Create ASG-Web and associate it with VM1’s NIC.
Create ASG-DB and associate it with VM2’s NIC.
Step 2: Update Network Security Group (NSG)
Go to NSG-Subnet2 (already attached to Subnet2 where VM2 resides).
Add inbound rule:
Name: Allow-Web-to-DB
Priority: 100
Source: ASG-Web
Destination: ASG-DB
Port: 1433
Protocol: TCP
Action: Allow
Ensure a Deny-All rule exists:
Priority: 200
Source: Any
Destination: Any
Port: *
Protocol: Any
Action: Deny
Step 3: Test Connectivity
RDP into VM1 using its public IP.
From VM1, test connectivity to VM2 on port 1433:
Success Criteria
VM1 (ASG-Web) can only reach VM2 (ASG-DB) on port 1433.
All other traffic (RDP, Ping, etc.) is blocked by the NSG.
Initially the connection to port 1433 will fail since there is no resource on VM2 connected to that port. It is mandatory to understand that NSG only control the traffic to ports and do not open ports.
it is mandatory to open the port on VM2 inorder to connect from VM1.
