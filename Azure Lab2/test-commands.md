# Test Commands used in Lab 2

## From your PC
1. RDP into VM1 using its public IP.

## Inside VM1
### Try RDP into VM2 (has to work)
- Open Remote Desktop Connection
- Enter VM2 private IP

### Try ping VM2 (has to fail)
```cmd
ping <VM2_Private_IP>
```

### Optional
Test-NetConnection <VM2_Private_IP> -Port 3389
```
