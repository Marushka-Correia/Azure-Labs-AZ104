## Today I set up a full Azure Point-to-Site (P2S) VPN, and this one really clicked for me.
## For the first time, I clearly understood how a client machine (like my laptop) securely connects to
## remote servers and resources inside a private network — just like how employees connect to company servers from anywhere.

## 1) Create the Virtual Network
-> Virtual Networks
-> Create a new VNet:
-> Name: P2S-VNet
-> Region: Central India
-> Address space: 10.1.0.0/16
-> Add subnet:
-> Name: VnetSubnet
-> Address: 10.1.1.0/24

## 2) Add a Gateway Subnet
-> Go to P2S-VNet → Subnets
-> Click + Gateway subnet
-> Save

## 3) Create Public IP for VPN Gateway
-> Public IP addresses
-> Create:
-> Name: P2S-VPN-PIP
-> SKU: Standard

## 4) Create the VPN Gateway
-> Virtual network gateways
->Create
-> Name: P2S-VPN-GW
-> Gateway type: VPN
-> VPN type: Route-based
-> SKU: VpnGw1
-> VNet: P2S-VNet
-> Public IP: P2S-VPN-PIP

the deployment took me about 30 minutes to complete.

## 5) Generate a Root Certificate (this step needs to be done on your computer in powershell: run as administrator) -> steps 5 and 6 are chatGPT guided
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-KeyUsageProperty Sign -KeyUsage CertSign

## 6) Export the Root Certificate (.cer)
-> Open Manage user certificates
-> Go to: Personal → Certificates
-> Find P2SRootCert
-> Right-click → All Tasks → Export
## -> Choose:
          No, do not export private key
          Format: Base-64 encoded X.509 (.CER)
          Save as P2SRootCert.cer


## 7) Generate a Client Certificate (required for VPN clients)
Run in PowerShell:
$rootCert = Get-ChildItem -Path "Cert:\CurrentUser\My" | Where-Object {$_.Subject -eq "CN=P2SRootCert"}
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SClientCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $rootCert `
-KeyUsageProperty Sign -KeyUsage DigitalSignature


## 8) Export Client Certificate (.pfx)
-> This one includes private key.
-> Right-click P2SClientCert
-> All Tasks → Export
-> Choose:
-> Yes, export the private key
-> Format: PFX
-> Set a password
-> Save as P2SClientCert.pfx
-> You will import this into the VPN client machine later.

## 9) Upload Root Certificate to Azure
-> Open the VPN Gateway → Point-to-site configuration
-> Click Configure now
->Enter the following:
-> Address Pool (client IP range): 172.16.0.0/24
Tunnel Type
-> OpenVPN (SSL)
-> IKEv2 (optional)
-> Root Certificate
         -> Name: P2SRootCert
         ->Public Certificate Data: open the .cer file → copy all text → paste into portal
         -> Click Save.

## 10) Download VPN Client
-> Go to VPN Gateway → Point-to-site configuration
-> Click Download VPN Client
-> Download Windows 64-bit package
-> Extract ZIP
-> Run installer (VpnClientSetupAmd64.exe)

## 11) Install Client Certificate (.pfx)
-> On your Windows PC:
-> Double-click P2SClientCert.pfx
-> Install in:
             Current User → Personal
             Enter the export password

## 12) Connect to the VPN
-> Open Settings → VPN
-> Select the Azure VPN connection
-> Click Connect



## 13) Test Working
-> Run ipconfig ->  verify VPN adapter
-> I also tried to ping a vm i created for testing from my local pc to the VM. 
-> Check Azure Portal -> VNet -> check in connected devices for your local pc

