- Useful Link: https://www.mandiant.com/resources/blog/evasive-attacker-leverages-solarwinds-supply-chain-compromises-with-sunburst-backdoor
- It injected a malicious DLL(`SolarWinds.Orion.Core.BusinessLayer.dll`) into the build process of their Orion product
- The malware masquerades its network traffic as the Orion Improvement Program(OIP) protocol and stores reconnaissance results within  legitimate plugin configuration files.
- It was displayed in the SolarWinds website as a standard `Windows Installer Patch` file.
- The malware will attempt to resolve a subdomain to mask the malicious traffic. The DNS response will return a CNAME that points to a `Command and Control` domain.

![[Pasted image 20230618140719.png]]
