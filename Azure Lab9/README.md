### Lab 9 - Private EndPoints

### In this lab, I tried a concept I came across while preparing for my AZ104 exam, that is: private endpoints and Private DNS zones.


### Steps
Step 1:Create Virtual Network and Subnet
- Go to Virtual Networks → + Create
- Name: VNet-PrivateAccess
- Address space: 10.1.0.0/16
- Add subnet:
  - Name: AppSubnet
  - Address range: 10.1.1.0/24
  - Review + Create

Step 2: Create a Virtual Machine
- Go to Virtual Machines → + Create
- Name: VM-PrivateAccess
- Region: Central India
- Network:
  - VNet: VNet-PrivateAccess
  - Subnet: AppSubnet
  - Public IP: None
  - Administrator: azureuser
  - Review + Create

Step 3: Create a Storage Account
- Search Storage Accounts → + Create
- Name: privatestorage<unique>
- Region: Central India
- Networking Tab:
- Connectivity method: Private endpoint
- Public network access: Disabled
- Review + Create

Step 4: Create the Private Endpoint
- In the storage account → Networking → Private Endpoint connections → + Private endpoint
- Name: pe-storage
- Resource: privatestorage<unique>
- Target sub-resource: blob
- Networking Tab:
  - VNet: VNet-PrivateAccess
  - Subnet: AppSubnet
  - Enable Private DNS integration

Azure automatically creates:
A Private Endpoint NIC
A Private DNS Zone: privatelink.blob.core.windows.net
A link between that DNS zone and your VNet

Step 6: Verify DNS Resolution
- From inside your VM (via Bastion):
   -> nslookup privatestorage<unique>.blob.core.windows.net


 output:
       Name: privatestorage<unique>.privatelink.blob.core.windows.net
       Address: 10.1.1.5

  -> From your local computer (outside Azure), open: https://privatestorage<unique>.blob.core.windows.net/
 
 output:
       You should get Access Denied or DNS not found — confirming no public exposure.




### Success Criteria


DNS lookup inside VNet returns Private IP
Storage access from VM works internally.


This allows secure communication between the VM and storage without exposure to public using azures backbone for communication


