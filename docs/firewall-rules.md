# Firewall Rules

Recommended UniFi firewall rules for a four-VLAN home network. Apply in UniFi Network under **Settings → Firewall & Security → LAN Rules**.

All rules use **LAN In** (traffic entering the UDM SE from a LAN segment).

---

## Rule order matters — UniFi processes top to bottom

1. Allow established/related (always first)
2. Specific allow rules
3. Block inter-VLAN rules
4. Default allow (built-in)

---

## Baseline rules (all VLANs)

| Priority | Action | Source | Destination | Protocol | Notes |
|----------|--------|--------|-------------|----------|-------|
| 1 | Allow | Any | Any | All | Established / related sessions |
| 2 | Allow | Any | Gateway | TCP/UDP 53 | DNS to gateway |
| 3 | Allow | Any | Gateway | UDP 67–68 | DHCP |

---

## VLAN_SERVERS — Secure Server Net

| Priority | Action | Source | Destination | Protocol | Notes |
|----------|--------|--------|-------------|----------|-------|
| 10 | Allow | VLAN_TRUSTED | VLAN_SERVERS | TCP YOUR_FILE_SHARE_PORT | File sharing |
| 11 | Allow | VLAN_TRUSTED | VLAN_SERVERS | TCP YOUR_NAS_UI_PORT | NAS web UI |
| 20 | Block | VLAN_SERVERS | RFC1918 | All | Prevent server reaching other LANs |

---

## VLAN_KIDS — Time Controlled

| Priority | Action | Source | Destination | Protocol | Notes |
|----------|--------|--------|-------------|----------|-------|
| 30 | Block | VLAN_KIDS | VLAN_TRUSTED | All | No access to trusted devices |
| 31 | Block | VLAN_KIDS | VLAN_SERVERS | All | No access to servers |
| 32 | Block | VLAN_KIDS | VLAN_IOT | All | No access to IoT segment |

> Time-based blocking is handled via **Traffic Rules** in UniFi Network → Traffic Management. Create a rule that blocks internet access for VLAN_KIDS outside your defined schedule.

---

## VLAN_IOT — Over The Wall

| Priority | Action | Source | Destination | Protocol | Notes |
|----------|--------|--------|-------------|----------|-------|
| 40 | Allow | VLAN_TRUSTED | VLAN_IOT | TCP YOUR_WLED_PORT | WLED HTTP API (trusted → IoT only) |
| 41 | Allow | VLAN_TRUSTED | VLAN_IOT | TCP YOUR_HUE_PORT | Hue bridge API |
| 50 | Block | VLAN_IOT | VLAN_TRUSTED | All | IoT cannot reach trusted devices |
| 51 | Block | VLAN_IOT | VLAN_SERVERS | All | IoT cannot reach servers |
| 52 | Block | VLAN_IOT | VLAN_KIDS | All | IoT cannot reach kids VLAN |

---

## mDNS / Bonjour across VLANs

For AirPlay, Chromecast, or other discovery protocols across VLANs:

- Go to **Settings → Networks → [network] → Advanced → mDNS**
- Enable on VLAN_TRUSTED and any VLAN that needs device discovery
- Alternatively use Avahi on the UDM SE for finer-grained control

---

## Notes

- Enabling **"Block inter-VLAN routing"** on IoT/Kids networks in UniFi is a quick start — these explicit rules make the intent auditable and easier to extend
- Review firewall logs under **Insights → Traffic** regularly to catch unexpected cross-VLAN attempts
- If adding Home Assistant, place it on VLAN_TRUSTED and whitelist specific IPs/ports to each IoT device it needs to control
