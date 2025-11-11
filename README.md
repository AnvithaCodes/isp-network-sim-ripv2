# ISP Network Simulation using RIP v2

## Objective
Simulate a small ISP-style network connecting multiple branch routers using **RIP version 2** for dynamic routing and a **DHCP server** for automatic IP assignment to LAN clients.

---

## Topology Summary
- 4 routers (R0–R3) in a ring topology (serial links)  
- 4 switches (one per LAN)  
- 4 PCs (one per LAN)  
- 1 DHCP server on R0 LAN  
- Routing protocol: **RIP v2** (dynamic routing)

---

## IP Address Plan

### LAN Networks
| Router | LAN Network      | Interface | IP Address    |
|:-------|:-----------------|:----------|:--------------|
| R0     | 192.168.1.0/24   | Fa0/0     | 192.168.1.1   |
| R1     | 192.168.2.0/24   | Fa0/0     | 192.168.2.1   |
| R2     | 192.168.3.0/24   | Fa0/0     | 192.168.3.1   |
| R3     | 192.168.4.0/24   | Fa0/0     | 192.168.4.1   |

### Serial Links (WAN)
| Link      | Network       | Router A    | Router B    |
|:----------|:--------------|:------------|:------------|
| R0 ↔ R1   | 10.0.0.0/30   | 10.0.0.1    | 10.0.0.2    |
| R1 ↔ R2   | 10.0.0.4/30   | 10.0.0.5    | 10.0.0.6    |
| R2 ↔ R3   | 10.0.0.8/30   | 10.0.0.9    | 10.0.0.10   |
| R3 ↔ R0   | 10.0.0.12/30  | 10.0.0.13   | 10.0.0.14   |

---

## Router Configuration

### Example: R0
```text
enable
configure terminal

interface fastethernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface serial0/3/0
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000
 no shutdown

interface serial0/3/1
 ip address 10.0.0.14 255.255.255.252
 no shutdown

router rip
 version 2
 no auto-summary
 network 192.168.1.0
 network 10.0.0.0

end
write
```

---

## DHCP Configuration (R0)

| Field              | Value           |
|:-------------------|:----------------|
| Server IP          | 192.168.1.2     |
| Default Gateway    | 192.168.1.1     |
| DNS Server         | 8.8.8.8         |
| Start IP Address   | 192.168.1.10    |
| Subnet Mask        | 255.255.255.0   |
| Max Users          | 20              |
| DHCP Service       | ON              |
| Pool Name          | lan1            |

---

## Verification

### Commands
| Command                  | Purpose                                    |
|:-------------------------|:-------------------------------------------|
| `show ip route`          | Verify routing table and RIP-learned routes|
| `show ip protocols`      | Confirm RIP v2 is active                   |
| `show ip interface brief`| Check interface status                     |
| `ping <ip>`              | Test connectivity between LANs             |

---

## Results
- All routers exchanged routes dynamically using RIP v2  
- DHCP server assigned IPs automatically to LAN clients  
- PCs successfully communicated across all LANs with **0% packet loss**

---

## Screenshots

| File                        | Description                              |
|:----------------------------|:-----------------------------------------|
| `topology.png`              | Full network layout                      |
| `show-ip-route.png`         | Routing table output showing RIP routes  |
| `show-ip-protocols-r0.png`  | RIP configuration verification on R0     |
| `show-ip-protocols-r1.png`  | RIP configuration verification on R1     |
| `show-ip-protocols-r2.png`  | RIP configuration verification on R2     |
| `show-ip-protocols-r3.png`  | RIP configuration verification on R3     |
| `pc0-pings.png`             | Successful ping from PC0 to other LANs   |
| `dhcp_pool.png`             | DHCP pool configuration on R0            |
| `dhcp_client.png`           | PC0 showing DHCP-assigned IP             |

---

## How to Open This Project

1. Download or clone this repository
2. Open **Cisco Packet Tracer**
3. Go to **File → Open**
4. Navigate to this folder and open `isp-network-sim-ripv2.pkt`

---

## Files

| Name                  | Description                              |
|:----------------------|:-----------------------------------------|
| `isp-network-sim-ripv2.pkt` | Main Packet Tracer project file    |
| `/router-configs/`    | Router running-configs (R0–R3)           |
| `/screenshots/`       | Topology and test verification images    |
| `README.md`           | Project documentation                    |

---

## About

Demonstrates **dynamic routing** and **automated IP management** in a small ISP level topology using **RIP v2** and **DHCP**

It can be extended with **OSPF**, **BGP** or monitoring servers for scalability.

---

## Author

**Anvitha Bhat A**  
*isp-network-sim-ripv2 | 2025*

---

## License

This project is open source and available under the [MIT License](LICENSE).

---

## Contributing

Contributions, issues, and feature requests are welcome!  
Feel free to check the [issues page](../../issues).

---

## Acknowledgments

- Cisco Packet Tracer for network simulation
- RIP v2 protocol documentation
- ISP network design best practices