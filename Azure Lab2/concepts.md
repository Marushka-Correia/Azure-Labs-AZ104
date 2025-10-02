# Key Concepts from Lab 2 (NSGs)

- NSGs are Azure's built-in network firewalls at subnet or NIC level.
- They allow or deny traffic based on rules with priority numbers.
- Lower priority number = higher precedence.
- Destination port (3389) matters because VM2 listens on that port.
- Source port is left as '*' since the client (VM1) uses random ephemeral ports.
- NSG vs Windows Firewall:
  - NSG = network-level filtering (before traffic reaches the VM)
  - Windows Firewall = host-level filtering (inside the VM OS)
- Best practice: use both NSG + Windows Firewall for layered security.
