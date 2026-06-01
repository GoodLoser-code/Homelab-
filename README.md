# Homelab

A self-hosted homelab built on Proxmox, running Docker-based services and a dedicated Splunk SIEM for security monitoring.

---

## Network Diagram

```mermaid
graph TD
    Internet([🌐 Internet])
    Gateway[Xfinity Gateway\n192.168.x.x]
    DarwinPC[DarwinPC\nWindows Desktop\nMain Machine]
    HP[HP Elite Mini PC\nProxmox Hypervisor]
    VM1[Ubuntu Server VM 1\nDocker Host]
    VM2[Ubuntu Server VM 2\nSplunk SIEM]
    Portainer[Portainer\nDocker UI]
    Navidrome[Navidrome\nMusic Server]
    Pihole[Pi-hole\nDNS / Ad Blocker]
    Tailscale([Tailscale Network\n100.x.x.x])
    Phone([📱 iPhone\nSubstreamer])

    Internet --> Gateway
    Gateway --> DarwinPC
    Gateway --> HP
    HP --> VM1
    HP --> VM2
    VM1 --> Portainer
    VM1 --> Navidrome
    VM1 --> Pihole
    VM2 --> Splunk

    DarwinPC -- SSH --> VM1
    DarwinPC -- SSH --> VM2
    DarwinPC -- DNS queries --> Pihole
    DarwinPC -- logs --> VM2
    VM1 -- logs --> VM2

    VM1 -. Tailscale .-> Tailscale
    Phone -. Tailscale .-> Tailscale
    Phone -- Substreamer stream --> Navidrome
```

---

## Services

| # | Service | Host | Purpose |
|---|---------|------|---------|
| 01 | Proxmox | HP Elite Mini PC | Hypervisor running all VMs |
| 02 | Ubuntu Server VM 1 | Proxmox | Docker host for all self-hosted services |
| 03 | Portainer | Ubuntu VM 1 | Web UI for managing Docker containers |
| 04 | Navidrome | Ubuntu VM 1 | Self-hosted music streaming server |
| 05 | Pi-hole | Ubuntu VM 1 | Network-wide DNS ad blocker |
| 06 | Splunk SIEM | Ubuntu VM 2 | Log aggregation and security monitoring |
| 07 | Tailscale | Ubuntu VM 1 + iPhone | Private VPN for remote access to Navidrome |

---

## Documentation

- [01 Proxmox Setup](docs/01%20Proxmox-setup.md)
- [02 Ubuntu Server Setup](docs/02%20Ubuntu%20Server%20Setup.md)
- [03 Portainer Setup](docs/03%20Portainer%20Setup.md)
- [04 Navidrome Setup](docs/04%20Navidrome%20Setup.md)
- [05 Pi-hole Setup](docs/05%20Pi-hole%20Setup.md)
- [06 Splunk SIEM Setup](docs/06%20Splunk%20SIEM%20Setup.md)
- [07 Tailscale Setup](docs/07%20Tailscale%20Setup.md)

---

## Stack

- **Hypervisor:** Proxmox VE
- **OS:** Ubuntu Server 24.04
- **Containers:** Docker / Portainer
- **SIEM:** Splunk Enterprise
- **Remote Access:** Tailscale
- **DNS:** Pi-hole
