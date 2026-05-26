# Device Inventory

Template for documenting all network devices. Fill in your own values — never commit real MAC addresses or IPs to a public repo.

> **Tip:** In UniFi Network, go to **Clients** → select a device → **Fixed IP** to assign static leases. Keep a local copy of the filled-in version in your private repo.

---

## Infrastructure

| Device | Model | IP | Notes |
|--------|-------|----|-------|
| ISP modem | Fiber ONT | — | WAN handoff only |
| Router / controller | UDM SE | YOUR_GATEWAY_IP | Gateway for all VLANs |
| Core switch | USW-24-G2 | YOUR_SWITCH_IP | Managed via UDM SE |
| Access point 1 | U7 Pro | YOUR_AP1_IP | |
| Access point 2 | U7 Pro | YOUR_AP2_IP | |
| Flex Mini #1 | USW Flex Mini | YOUR_FM1_IP | Cluster A |
| Flex Mini #2 | USW Flex Mini | YOUR_FM2_IP | Cluster B |

---

## VLAN_TRUSTED — Default

| Device | Type | IP | Connection | Notes |
|--------|------|----|------------|-------|
| Desktop PC | PC | Static | Wired · Flex Mini #1 | |
| Laptop | Laptop | DHCP | Wi-Fi · Trusted 5 GHz | |
| Phone 1 | Phone | DHCP | Wi-Fi · Trusted 5 GHz | |
| Phone 2 | Phone | DHCP | Wi-Fi · Trusted 5 GHz | |
| Apple TV | Streaming | Static | Wired · Flex Mini #2 | |
| Gaming console | Console | Static | Wired · Flex Mini #2 | |
| Smart TV | TV | Static | Wired · Flex Mini #2 | |

---

## VLAN_SERVERS — Secure Server Net

| Device | Type | IP | Connection | Notes |
|--------|------|----|------------|-------|
| NAS | NAS | Static | Wired · Flex Mini #1 | Assign static IP |

---

## VLAN_KIDS — Time Controlled

| Device | Type | IP | Connection | Notes |
|--------|------|----|------------|-------|
| Kids device 1 | — | DHCP | Wi-Fi · Kids SSID | |
| Kids device 2 | — | DHCP | Wi-Fi · Kids SSID | |

---

## VLAN_IOT — Over The Wall

| Device | Type | IP | Connection | Notes |
|--------|------|----|------------|-------|
| Philips Hue bridge | Smart home hub | Static | Wired · USW-24-G2 | Zigbee coordinator |
| Echo (room 1) | Smart speaker | DHCP | Wi-Fi · IoT SSID | |
| Echo (room 2) | Smart speaker | DHCP | Wi-Fi · IoT SSID | |
| WLED strip 1 | LED controller | Static | Wi-Fi · IoT SSID | Static for HTTP API |
| WLED strip 2 | LED controller | Static | Wi-Fi · IoT SSID | Static for HTTP API |
| IP camera 1 | Camera | Static | Wi-Fi · IoT SSID | |
| IP camera 2 | Camera | Static | Wi-Fi · IoT SSID | |
| Smart plug 1 | Smart plug | DHCP | Wi-Fi · IoT SSID | |
| Smart plug 2 | Smart plug | DHCP | Wi-Fi · IoT SSID | |

---

## Notes

- Assign static IPs (fixed leases) to all infrastructure and servers — firewall rules depend on stable addresses
- WLED and cameras especially benefit from static IPs
- Use UniFi's **Client Alias** feature to name devices in the controller — makes logs much easier to read
- Keep a filled-in copy of this file in your **private** repo only
