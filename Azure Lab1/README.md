# Lab 1 - in this lab I built a simple Virtual network with 2 subnets and deployed a Virtual machine in each subnet. This lab helped me learn connectivity inside a Vnet 

Create a Virtual Network with two subnets and deploy two **Windows VMs**. Verify private connectivity using IP and DNS (VM names).

---

## Steps

### Step 1: Create Resource Group
- Name: RG-Beginner

### Step 2: Create Virtual Network (VNet1)
- Address space: 10.0.0.0/16
- Subnets:
  - Subnet1: 10.0.1.0/24
  - Subnet2: 10.0.2.0/24

### Step 3: Deploy Windows VMs
- VM1 → Subnet1
- VM2 → Subnet2
- Image: Windows Server 2022 Datacenter
- Administrator username/password -> i added azureUser1
- Allow inbound port: RDP (3389)

### Step 4: Verify Connectivity
1. From your PC → connect to VM1 using RDP and its **Public IP**.
2. Inside VM1 → open Command Prompt.
3. Get VM2's private IP from Azure Portal.
4. Ping VM2's private IP:
  - ping <VM2_Private_IP>
  
5. Ping VM2 by hostname (Azure DNS):
   - ping VM2
   
Note: Windows blocks ping by default. You must enable ICMP Echo Request on VM2 or Turn off windows firewall(not recommended) for VM2 and VM2

---

## results
- VM1 connects via RDP.
- VM1 can ping VM2 using its private IP and VM name after enabling ICMP inbound rule on VM2.
