# How to Enable Ping (ICMP) on Windows VM

By default, Windows Server blocks ICMP (ping). To allow ping:

1. RDP into **VM2**.
2. Open **Windows Defender Firewall with Advanced Security**.
3. Go to **Inbound Rules**.
4. Find rule: **File and Printer Sharing (Echo Request - ICMPv4-In)**.
5. Right-click → **Enable Rule**.

✅ Now VM2 will respond to ping requests from VM1.
