# Lab 2 - in this lab I practised creating a Vnet and 2 subnets, added a vm in each subnet and later configured a NSG to restrict the ICMP ie ping command from VM1 to VM2 and only allow Remote desktop connection.


I created a Network Security Groups (NSGs) to control inbound traffic to a subnet so that VM2 only allows RDP  and blocks all other traffic.

---

## Steps

### Step 1: Create Network Security Group (NSG)
- Name: NSG-Subnet2
- Resource Group: RG-Beginner
- Region: central India

### Step 2: Associate NSG with Subnet2
- Go to NSG-Subnet2 > Subnets > Associate
- Select VNet1, Subnet2 (where VM2 is deployed)

### Step 3: Add Inbound Rules
- Allow RDP:
  - Priority: 100
  - Source: Any
  - Destination: Any
  - Port: 3389
  - Protocol: TCP
  - Action: Allow

- Deny All:
  - Priority: 200
  - Source: Any
  - Destination: Any
  - Port: *
  - Protocol: Any
  - Action: Deny

### Step 4: Test Connectivity
1. RDP into VM1 using its public IP.
2. From VM1, try to RDP into VM2 using VM2's private IP -> connects successfully.
3. From VM1, try pinging VM2 by private IP or hostname → stimes out IE fails.

---

## Success Criteria
- VM1 can RDP into VM2.
- Ping and other traffic are blocked by the NSG.
