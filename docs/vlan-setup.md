# VLAN Setup Guide

Configuration reference for all four VLANs on the UniFi Dream Machine SE.

Replace all `YOUR_*` placeholders with your actual values before use.

---

## VLAN_TRUSTED — Default

The primary trusted network. All managed personal devices live here.

**Devices:** Desktop PC, phones, laptops, Apple TV, gaming console, smart TV

**UniFi settings:**
- Network name: `Default`
- VLAN ID: `VLAN_TRUSTED`
- Subnet: `YOUR_TRUSTED_SUBNET`
- DHCP range: `YOUR_TRUSTED_SUBNET.100 – .254`
- DNS: Use gateway (UDM SE) or set custom (e.g. `1.1.1.1`)

---

## VLAN_SERVERS — Secure Server Net

Isolated network for NAS and home servers. Tightly controlled inbound/outbound rules.

**Devices:** NAS / home server

**UniFi settings:**
- Network name: `Servers`
- VLAN ID: `VLAN_SERVERS`
- Subnet: `YOUR_SERVER_SUBNET`
- DHCP range: `YOUR_SERVER_SUBNET.100 – .200`

**Recommended firewall rules:**
- Allow VLAN_TRUSTED → VLAN_SERVERS on specific ports only (file sharing, web UI)
- Block VLAN_SERVERS → all other VLANs
- See [`firewall-rules.md`](firewall-rules.md) for full rule set

---

## VLAN_KIDS — Time Controlled

Kids devices with scheduled internet and LAN access via UniFi traffic rules.

**Devices:** Kids phones, tablets, laptops

**UniFi settings:**
- Network name: `Time Controlled`
- VLAN ID: `VLAN_KIDS`
- Subnet: `YOUR_KIDS_SUBNET`
- SSID: dedicated kids SSID on both 2.4 and 5 GHz

**Traffic rules:**
1. In UniFi Network → Profiles → create a time schedule
2. Create a Traffic Rule: block all traffic from VLAN_KIDS outside schedule
3. Optionally enable content filtering via UniFi's built-in categories

---

## VLAN_IOT — Over The Wall

Fully isolated IoT network. Devices reach the internet but cannot initiate connections to trusted VLANs.

**Devices:** Amazon Echos, WLED strips, IP cameras, smart plugs, Philips Hue bridge

**UniFi settings:**
- Network name: `IOT` (or your preferred name)
- VLAN ID: `VLAN_IOT`
- Subnet: `YOUR_IOT_SUBNET`
- SSID: dedicated IoT SSID (2.4 GHz preferred for IoT devices)

**Recommended firewall rules:**
- Block VLAN_IOT → VLAN_TRUSTED
- Block VLAN_IOT → VLAN_SERVERS
- Allow VLAN_IOT → WAN (internet)
- Allow established/related return traffic
- See [`firewall-rules.md`](firewall-rules.md) for full rule set

**Note on Philips Hue:**
The Hue bridge is wired on VLAN_IOT. Bulbs communicate over Zigbee — they never touch the IP network. The bridge only needs outbound internet to sync with Hue cloud.

**Note on WLED:**
WLED runs on ESP32/ESP8266 chips. Each strip connects via Wi-Fi on VLAN_IOT. Assign static IPs so the HTTP API address is stable. If using Home Assistant on VLAN_TRUSTED, add a firewall rule allowing VLAN_TRUSTED → WLED IPs on port 80.
