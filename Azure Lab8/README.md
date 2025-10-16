### Lab 8 - Using routing tables and a firewall

### In this Lab I tried to deploy a Firewall to prevent/allow the VM from accessing certain applications.

### Steps
Step 1: Create VNet
VNet	Address Space	Subnets
Hub-VNet	10.0.0.0/16	AzureFirewallSubnet (10.0.1.0/24)
Spoke1-Web	10.1.0.0/16	Spoke1-Subnet (10.1.1.0/24)
Spoke2-App	10.2.0.0/16	Spoke2-Subnet (10.2.1.0/24)

### The subnet name must be of type AzureFirewallSubnet for the firewall to be created.



Step 2: Create the firewall
## Configuration:
- Name: Hub-Firewall
- Region: Central India
- VNet: Hub-VNet
- Subnet: AzureFirewallSubnet
- Public IP: Create new -> HubFirewall-PIP
- Firewall Policy: Create new -> Policy-FW
- SKU: Standard or Premium

Step 3: Create Peering
From	        To	            Peering Settings
Hub-VNet -> Spoke1-Web      	Allow traffic both ways + allow forwarded traffic	
Hub-VNet -> Spoke2-App	        Same as above

Step 4: Create Route Table (UDR)
- Search Route Tables -> Create
- Name: RT-Firewall
- Region: Central India
- Add Route:
- Name: DefaultToFirewall
- Address Prefix: 0.0.0.0/0
- Next Hop Type: Virtual appliance
- Next Hop IP: Firewall private IP (e.g., 10.0.1.4)
- Associate this route table to both:
-> Spoke1-Subnet
-> Spoke2-Subnet


Step 5: Add Firewall Rules
-> Network Rule
## Field	           Value
- Name	           Allow-Web-Traffic
- Source	       10.1.0.0/16, 10.2.0.0/16
- Destination	    *
- Protocol	       TCP
- Port         	   80
- Action	       Allow

Step 6: Deploy Test VMs
##  VM	     VNet     	Subnet	    Public IP
VM-Web	Spoke1-Web	Spoke1-Subnet	None
VM-App	Spoke2-App	Spoke2-Subnet	None

Step 7: Test Connectivity
- RDP/SSH into VM-Web using Bastion (no public IP needed).

open powershell and type the following commands:

- Test-NetConnection www.bing.com -Port 80
- Test-NetConnection www.facebook.com -Port 80

### Success Criteria
Bing = allowed
Facebook = Blocked




### a firewall helps to filter out traffic passing in and out of a Vnet. The routing table helps to navigate the traffic through the firewall.

### the best example of a firewall + routing table would be: consider the VMS as cars leaving the city onto a highway(internet), however, the routing table forces the VM to go through a checkpoint(next hop to Firewall). providing extra security for the data moving in and out of the Vnet

