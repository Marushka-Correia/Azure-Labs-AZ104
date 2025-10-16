### Lab 7 - Connecting to a VM using Azure Bastion

### In this Lab I tried to connect to my Virtual Machine via Azure Bastion and not the usual way by using public IP of the VM.

### Steps
Step 1: Create VNet1
- Name: VNet1
- Address space: 10.1.0.0/16
- Subnet: Subnet1 -> 10.1.1.0/24



Step 2: Create a virtual Machine
- Go to Virtual Machines -> Create
- Name: BastionVM
- Region: same as VNet
- Networking tab:
- VNet: VNet-Bastion
- Subnet: VMSubnet
- Public IP: None
- Review + Create.

Step 3: Go to VM and Connect 
-> connect via Bastion -> (enables connection to VM)
-> try by selecting connect -> should fail.

### Success Criteria
- You can connect to the VM through Bastion without any public IP.
- Direct RDP/SSH access from the internet fails.


### Azure Bation is a a prefered method of connecting to the VM since there is no need of Public IPs. Bation connects to the VM via the azure backbone as mentioned in the AZ-104 documentation


