# Network Map

This renders automatically as a diagram on GitHub — no image file needed,
it's just text.

```mermaid
graph TD
    ISP["Comcast ISP Modem<br/>10.0.0.1"] --> X1["SonicWall - X1 WAN<br/>10.0.0.201"]
    X1 --> SW["SonicWall Firewall"]
    SW --> X0["X0 - LAN<br/>192.168.10.0/24"]
    SW --> X2["X2 - LAN<br/>192.168.20.0/24"]
    X0 --> NH["Netgear Nighthawk<br/>WiFi Extender (gaming room)"]
    NH -."WiFi<br/>(double-NAT if in Router Mode)".-> KALI["Kali Attack Laptop"]
    X2 --> SWITCH["Network Switch"]
    SWITCH --> DESKTOP["Lab Desktop<br/>VirtualBox + Splunk"]
    SWITCH --> GAMEDESK["Gaming Desktop"]
    DESKTOP --> VM["Windows Server VM - Victim<br/>192.168.20.60"]
```

## Notes

- **X0** (`192.168.10.0/24`) and **X2** (`192.168.20.0/24`) are separate
  LAN zones on the SonicWall.
- The Nighthawk, when left in Router Mode, created a hidden third network
  and NAT'd Kali's real address — this is the double-NAT issue documented
  in the README's "What I've learned" section.
- The lab desktop hosts the Windows Server victim VM internally; Splunk
  runs directly on the desktop, not inside a VM.
