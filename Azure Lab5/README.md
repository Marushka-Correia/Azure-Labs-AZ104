### Lab 5 - Hub-and-spoke Network Architecture
### This lab was a lot of fun since I tried out something new. The hub and spoke network which basically is a central Vnet (hub) connected to multiple Vnets(spokes) using peering. there is no peering created between any of the spokes unless necessary. This Network architecture keeps configuration central and no messy unnecessary peering between unwanted Vnets


### Create 3 VNets with different IP address ranges.

### Deploy one VM in each VNet.

### Configure Peering between Vnet1 <-> Vnet2 and Vnet1 <-> Vnet3 so the VMs can talk privately.

### Test connectivity using ping and RDP.

### Steps
Step 1: Create VNet1
- Name: VNet1
- Address space: 10.1.0.0/16
- Subnet: Subnet1 -> 10.1.1.0/24
- Deploy VM1 into VNet1 -> Subnet1.

Step 2: Create VNet2
- Name: VNet2
- Address space: 10.2.0.0/16
- Subnet: Subnet2 -> 10.2.1.0/24
- Deploy VM2 into VNet2 -> Subnet2.

Step 3: Configure VNet Peering
- From VNet1 → Peerings -> + Add:
- Name: VNet1toVNet2
- Remote VNet: VNet2
- Allow virtual network access: Enabled
- From VNet2 → Peerings -> + Add:
- Name: VNet2toVNet1
- Remote VNet: VNet1
- Allow virtual network access: Enabled

### configure similar peering between Vnet1 and Vnet3


as discussed in previous labs ICMP is blocked by the VMs firewall hence ICMP needs to be enabled to test connectivity using ping.
However Remote desktop connection still works

### Success Criteria

VM1 can reach VM2 using its private IP.
VM1 can reach VM3 using its private IP.
Ping works after enabling ICMP.

Vm2 cannot connect to Vm3


Communication flows directly over Azure’s backbone, not the internet.


### note: it is mandatory to peer both Vnets. It should also be noted that peering only routes traffic and does not enable/disable the host firewall. This lab taught me that connectivity depends on routing, NSG, and host firewalls all together
