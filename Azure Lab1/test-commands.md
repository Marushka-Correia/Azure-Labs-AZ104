# Test Commands for Lab 1 (Windows VMs)

## On your local PC, connect to VM1 using RDP
- Open Remote Desktop Connection
- Enter: VM1 Public IP
- Login with the username/password you set

## Inside VM1 (Command Prompt)

### Ping VM2 by Private IP
```cmd
ping <VM2_Private_IP>
```

### Ping VM2 by Hostname (Azure DNS)
```cmd
ping VM2
```

✅ If you enabled ICMP inbound rule on VM2, both commands should succeed.
