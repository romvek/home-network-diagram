# 🏠 Home Network Diagram

A fully documented home network built on UniFi, with four isolated VLANs, wired and wireless segments, and IoT isolation. Built as a reference for anyone setting up a prosumer home network with proper segmentation.

> 📁 This is the public portfolio version. Sensitive values (hostnames, subnets, VLAN IDs, firewall ports) have been replaced with placeholders.

---

## 📋 Infrastructure Overview

| Device | Model | Role |
|--------|-------|------|
| ISP modem | Fiber ONT | WAN handoff |
| Router / controller | UniFi Dream Machine SE | Router, firewall, IDS/IPS, UniFi controller |
| Core switch | USW-24-G2 | 24-port PoE+, VLAN trunking |
| Edge switches | 2× USW Flex Mini | 5-port PoE clusters |
| Access points | 2× U7 Pro | Wi-Fi 7, 2.4 / 5 / 6 GHz |

---

## 🌐 VLAN Structure

| VLAN ID | Name | Subnet | Purpose |
|---------|------|--------|---------|
| `VLAN_TRUSTED` | MAIN | `YOUR_TRUSTED_SUBNET` | PCs, phones, laptops, Apple TV, consoles |
| `VLAN_SERVERS` | Servers | `YOUR_SERVER_SUBNET` | NAS and home servers, isolated |
| `VLAN_KIDS` | Time Controlled | `YOUR_KIDS_SUBNET` | Kids devices with scheduled access |
| `VLAN_IOT` | IOT | `YOUR_IOT_SUBNET` | IoT — cameras, smart plugs, WLED, Echos |

---

## 📡 Wi-Fi SSIDs

| SSID | Band | VLAN | Notes |
|------|------|------|-------|
| Trusted 2.4 GHz | 2.4 GHz | Trusted | Legacy + long-range devices |
| Trusted 5 GHz | 5 GHz | Trusted | Primary trusted SSID |
| IoT | 2.4 GHz | IoT | Isolated, no LAN access |
| Kids | 2.4 / 5 GHz | Kids | Schedule enforced via UniFi traffic rules |

---

## 🔌 Wired Devices

| Device | Switch | VLAN |
|--------|--------|------|
| Desktop PC | Flex Mini #1 | Trusted |
| NAS / home server | Flex Mini #1 | Servers |
| Apple TV | Flex Mini #2 | Trusted |
| Gaming console | Flex Mini #2 | Trusted |
| Smart TV | Flex Mini #2 | Trusted |
| Philips Hue bridge | USW-24-G2 | IoT |

---

## 📶 Wireless Devices

| Device | SSID | VLAN |
|--------|------|------|
| Phones | Trusted 5 GHz | Trusted |
| Laptops | Trusted 5 GHz | Trusted |
| Amazon Echo | IoT | IoT |
| WLED LED strips | IoT | IoT |
| IP cameras | IoT | IoT |
| Smart plugs | IoT | IoT |
| Kids devices | Kids | Kids |

---

## 🗂 Repository Structure

```
home-network-diagram/
├── README.md                  # This file
├── diagrams/
│   ├── network.mmd            # Mermaid source — full topology
│   └── vlans.mmd              # Mermaid source — VLAN overview
├── docs/
│   ├── vlan-setup.md          # VLAN configuration guide
│   ├── firewall-rules.md      # Recommended firewall rules
│   └── device-inventory.md    # Device list template
└── assets/
    └── preview.png            # Diagram preview image
```

---

## 🔒 Security Design Principles

- IoT devices are fully isolated — no access to trusted LAN segments
- Servers live on a dedicated VLAN with strict inbound/outbound firewall rules
- Kids VLAN uses UniFi traffic rules for time-based internet blocking
- Hue bridge is wired on the IoT VLAN; bulbs communicate via Zigbee (off-network)
- WLED devices run on ESP chips — isolated on the IoT VLAN by default
- All inter-VLAN routing is explicitly whitelisted; default deny between segments

---

## 🛠 Tools Used

- [UniFi Network Application](https://ui.com/download/unifi) — controller & management
- [Mermaid](https://mermaid.js.org) — diagram source files
- [Mermaid Live Editor](https://mermaid.live) — preview & export

---

## 📄 License

MIT — use this as a template for your own network documentation.
