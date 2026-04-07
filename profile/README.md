<div align="center">

<img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Period-Sept%202025%20–%20Jan%202026-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Institution-De%20Haagse%20Hogeschool-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Demonstrated-21%20January%202026-purple?style=for-the-badge"/>

# TropoMetrics Infrastructure Project

**A full-semester IT infrastructure project** - from enterprise network to cloud-native weather service, VoIP telephony, GitOps, and professional monitoring. Designed, built, tested, and demonstrated as a working production system.

[Full documentation (EN/NL)](https://github.com/TropoMetrics/tropometrics-docs) · [Weather service frontend](https://github.com/TropoMetrics/look-at-this-weather) · [API Server](https://github.com/TropoMetrics/api-server) · [GitOps / ArgoCD](https://github.com/TropoMetrics/ArgoCI-CD) · [Network configs](https://github.com/TropoMetrics/Packet-Tracer-configs)

[Lees deze pagina in het Nederlands](./README.nl.md)

</div>

---

## Executive Summary

This GitHub organisation documents a semester-long infrastructure project completed by a team of four IT students at De Haagse Hogeschool. Over roughly four months, the team designed and delivered a fully functional enterprise IT environment from the ground up - covering multi-site Cisco networking, virtualisation, cloud-native application deployment, VoIP telephony, monitoring, and security.

The scope was deliberately broad: rather than specialising in one area, the team was required to integrate every layer of the stack - physical network design, hypervisor configuration, Kubernetes orchestration, GitOps pipelines, Active Directory, and a custom-built web application - into a single cohesive system that could be demonstrated live. Every component listed in this organisation is something the team built, configured, and validated themselves.

The project demonstrates practical, hands-on competency across network engineering, DevOps, Linux systems administration, and full-stack development - skills directly applicable to roles in infrastructure, cloud, and platform engineering.

---

## About This Project

TropoMetrics is a fictional international weather data company, used as the case study for a semester-spanning infrastructure project at **De Haagse Hogeschool (Network Infrastructure Design)**. The assignment consisted of two equal parts:

1. **Design and build an enterprise IT infrastructure** for a company with offices in three countries (Delft, Aruba, and Melbourne) - from network topology and IP plan to Proxmox virtualisation and Active Directory.
2. **Develop a custom weather service** running on that infrastructure - a full-stack application with a React frontend, Node.js/Redis API, and Kubernetes deployment. [Open-Meteo](https://open-meteo.com) was used as the upstream weather data API, allowing the focus to remain on the infrastructure and architecture rather than data acquisition.

The project followed the **Design Science Research (DSR)** methodology and concluded with a live demonstration for assessors on 21 January 2026.

---

## What Was Built

### Enterprise Network (Cisco IOS / Packet Tracer)
Hierarchical three-tier network across Delft, Aruba, and Melbourne:
- Dual-stack **IPv4 + IPv6** · **OSPF** · **BGP** (AS 100 <-> ISP AS 110) · **HSRP** failover
- **IPsec/GRE VPN** tunnels between all sites · **EtherChannel (LACP)** uplinks
- VLAN segmentation: `STAFFNET` · `CALCNET` · `INTRANET` · `DMZNET`
- Layer 2 security: Port Security · DHCP Snooping · DAI · IP Source Guard · Blackhole VLANs

### Server Infrastructure (Proxmox VE)
All services as VMs on a single physical Proxmox VE host, connected via a **single Linux bridge (`vmbr0`) with VLAN trunk**:

| VM | VLAN | IP | Role |
|----|------|----|------|
| `k3s-w1/w2/w3` | 190 | `10.10.90.11–13` | K3s Kubernetes nodes (Alpine Linux) |
| `ticketsystem-intranet` | 190 | `10.10.90.44` | Zammad ticketing system |
| `npmplus-dmz-pub` | 191 | `145.34.44.80` | NPM Plus - public reverse proxy (DMZ) |
| `grafana-intranet` | 180 | `10.10.80.21` | Grafana monitoring (standalone) |
| `crm-intranet` | 180 | `10.10.80.90` | CRM application |
| Windows Server | 180 | `10.10.80.11` | Active Directory DC · DHCP · print/file · PRTG |
| Issabel PBX | 180 | INTRANET | VoIP telephone exchange |
| Stalwart mail | 180 | INTRANET | Mail server `@tropometrics.tech` |

### Cloud-Native Weather Service (K3s / Kubernetes)
- **`look-at-this-weather`** - React 18 + TypeScript + Vite, real-time weather data, 7-day forecast, maritime overview, dark/light mode
- **`api-server`** - Node.js/Express caching proxy for Open-Meteo, Redis TTL caching (rate limiting prevention)
- **Horizontal Pod Autoscaler** - automatically scales from 2 to 10 replicas at 50% CPU
- **ArgoCD GitOps** - declarative, self-healing, fully automated

### Telephony (Issabel PBX / FreePBX + Asterisk)
- IVR menu for routing inbound calls to departments
- SIP softphones · SIP desk phones · analogue phones via **Grandstream ATA**

### Monitoring (two-layer)
- **PRTG** (on Windows Server) - SNMPv3 monitoring of all Cisco devices, servers, and service availability
- **Prometheus + Grafana** (standalone VM) - application and cluster metrics
- **Alertmanager** - alert routing from K3s

### Security
Cisco ISR 4331 (router/ZBF) · Active Directory + LDAP · 802.1X · SNMPv3 · SSH-only management · IPsec-encrypted inter-site traffic

---

## Technology Stack

`Cisco IOS` · `Proxmox VE` · `K3s / Kubernetes` · `ArgoCD` · `GitHub Actions` · `Docker` · `MetalLB` · `Traefik` · `NPM Plus` · `PRTG` · `Prometheus` · `Grafana` · `Alertmanager` · `Windows Server AD` · `Issabel PBX` · `Grandstream ATA` · `Stalwart Mail` · `Zammad` · `React 18` · `TypeScript` · `Node.js` · `Redis` · `Tailwind CSS`

---

## Repositories

| Repository | Description |
|------------|-------------|
| [tropometrics-docs](https://github.com/TropoMetrics/tropometrics-docs) | Central documentation - fully available in both Dutch and English |
| [look-at-this-weather](https://github.com/TropoMetrics/look-at-this-weather) | React/TypeScript weather service frontend |
| [api-server](https://github.com/TropoMetrics/api-server) | Node.js/Redis caching API proxy |
| [ArgoCI-CD](https://github.com/TropoMetrics/ArgoCI-CD) | ArgoCD GitOps manifests and Kubernetes configurations |
| [Packet-Tracer-configs](https://github.com/TropoMetrics/Packet-Tracer-configs) | Cisco IOS configurations and Packet Tracer files |

---

## Team

| Name | Role |
|------|------|
| **Kai Diemel** | Project Lead · Network · Frontend · Documentation |
| **Max Blaauw** | 2nd Project Lead · Network Engineering · Infrastructure · DevOps · Front- & Backend |
| **Ole Spiegelenberg** | DevOps · Security · Research |
| **Arne Jansonius** | Network · Network Design · Documentation · Research |

---

*De Haagse Hogeschool · Network Infrastructure Design · September 2025 – January 2026*
*This project was carried out as a Proof of Concept in an educational context. Not all choices are intended for direct production deployment, but are technically sound and fully reproducible.*
