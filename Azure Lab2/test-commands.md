# Test Commands for Lab 2 (Windows VMs)

## From your PC
1. RDP into VM1 using its public IP.

## Inside VM1
### Try RDP into VM2 (should work)
- Open Remote Desktop Connection
- Enter VM2 private IP

### Try ping VM2 (should fail)
```cmd
ping <VM2_Private_IP>
```

### (Optional) Test specific port with PowerShell
```powershell
Test-NetConnection <VM2_Private_IP> -Port 3389
```
