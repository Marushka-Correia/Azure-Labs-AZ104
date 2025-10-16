# Test Commands for Lab 2 (Windows VMs)

## From your PC
1. RDP into VM1 using its public IP.

## commands to open port 1433 to VM1
# Open port 1433 in Windows Firewall
netsh advfirewall firewall add rule name="Open1433" dir=in action=allow protocol=TCP localport=1433

# Start a TCP listener
$listener = [System.Net.Sockets.TcpListener]1433
$listener.Start()

## Inside VM1
### open windows powershell and type the command 
Test-NetConnection <vm2 private ip> -Port 1433
 ### should work


### Try ping VM2 (should fail)
```cmd
ping <VM2_Private_IP>
```

### try Remote desktop connection from VM1 to VM2 (should fail)