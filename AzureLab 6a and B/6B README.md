### Lab 6B -Deploying a Load Balancer

### this lab was a bit of an intermediate lab as I deployed a load balancer + a public ip + a network security group all together. However the fun part was diagnostics and monitoring after discovering that i was not able to connect to the IP


### Steps
Step 1: Create VNet1
- Name: VNet1
- Address space: 10.1.0.0/16
- Subnet: Subnet1 -> 10.1.1.0/24
- Deploy VM1 into VNet1 -> Subnet1.
- Deploy VM2 into VNet1 -> Subnet1.

Step 2: Create a Public IP
- Setting	       Value
- Name	           LB-PublicIP
- SKU	           Standard
- Assignment	   Static
- Region	      central india 

Step 3: Create Load Balancer
- Type : Public
- Frontend : FrontendConfig -> use LB-PublicIP
- Backend : Create pool WebBackend
Add both (VM1LB, VM2LB) to the pool.

Step 4: Create a Health probe 
- Setting	      Value
- Protocol	      HTTP
- Port	          80
- Path	          /
- Interval	      5 s
- Unhealthy 
threshold	      2

Step 5: Create Load Balancing Rule
- Setting	           Value
- Frontend IP	       FrontendConfig
- Backend Pool	       WebBackend
- Protocol	           TCP
- Port	               80
- Backend Port	       80
- Health Probe	       HTTP-Probe
- Session Persistence  None
- Idle Timeout	       4 min


Step 6: Configure NSG (Inbound Rules)
Priority	Port	Protocol	Action	
100      	80	     TCP	    Allow	
110	        3389	 TCP	    Allow	
200	        *	     Any	    Deny	
 

Step 7: Install IIS 

- Install-WindowsFeature -name Web-Server -IncludeManagementTools
- echo "This is VM1LB" > C:\inetpub\wwwroot\index.html
- netsh advfirewall firewall add rule name="AllowHTTP" dir=in action=allow protocol=TCP localport=80
iisreset 

### same has to be done for Vm2LB 

### from browser 
- http://<LB_Public_IP>

In my lab the connection to public ip was timing out. This was due to the NSG attached to the VMs. it had no rule to allow/block port 80 traffic. I created new rules with priority 100 to allow post 80. Post that i was able to connect to  http://<LB_Public_IP>


### Success Criteria
The web browser loads VM1LB and on multiple refresh shows VM2LB. To make it bit interesting i turned off VM1 and then i was able to see only VM2Lb
