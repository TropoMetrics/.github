<div align="center">

<img src="https://img.shields.io/badge/Status-Afgerond-brightgreen?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Periode-Sept%202025%20–%20Jan%202026-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Instelling-De%20Haagse%20Hogeschool-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Gedemonstreerd-21%20januari%202026-purple?style=for-the-badge"/>

# 🌦️ TropoMetrics Infrastructure Project

**Een volledig semester IT-infrastructuurproject** — van entreprise netwerk tot cloud-native weerdienst, VoIP-telefonie, GitOps en professionele monitoring. Ontworpen, gebouwd, getest en gedemonstreerd als werkend productiesysteem.

[📖 Volledige documentatie (NL/EN)](https://github.com/TropoMetrics/tropometrics-docs) · [🌐 Weerdienst frontend](https://github.com/TropoMetrics/look-at-this-weather) · [⚙️ API Server](https://github.com/TropoMetrics/api-server) · [🔄 GitOps / ArgoCD](https://github.com/TropoMetrics/ArgoCI-CD) · [🖧 Netwerkconfiguraties](https://github.com/TropoMetrics/Packet-Tracer-configs)

</div>

---

## Over dit project

TropoMetrics is een fictief internationaal weerdata-bedrijf, ingezet als casus voor een semester-overspannend infrastructuurproject aan **De Haagse Hogeschool (Network Infrastructure Design)**. De opdracht bestond uit twee gelijkwaardige delen:

1. **Enterprise IT-infrastructuur** ontwerpen en bouwen voor een bedrijf met kantoren in drie landen (Delft, Aruba en Melbourne) — van netwerktopologie en IP-plan tot Proxmox-virtualisatie en Active Directory.
2. **Een eigen weerdienst ontwikkelen** die op die infrastructuur draait — een volledige full-stack applicatie met React-frontend, Node.js/Redis API en Kubernetes-deployment. Voor de weerdata werd [Open-Meteo](https://open-meteo.com) als upstream API gebruikt, zodat de focus kon liggen op de infrastructuur en architectuur.

Het project volgde de **Design Science Research (DSR)**-methodiek en werd afgesloten met een live demonstratie voor beoordelaars op 21 januari 2026.

---

## Wat er gebouwd is

### 🌐 Enterprisenetwerk (Cisco IOS / Packet Tracer)
Hiërarchisch drielaags netwerk over Delft, Aruba en Melbourne:
- Dual-stack **IPv4 + IPv6** · **OSPF** · **BGP** (AS 2000 ↔ ISP AS 1000) · **HSRP** failover
- **IPsec/GRE VPN**-tunnels tussen alle locaties · **EtherChannel (LACP)** uplinks
- VLAN-segmentatie: `STAFFNET` · `CALCNET` · `INTRANET` · `DMZNET`
- Laag-2-beveiliging: Port Security · DHCP Snooping · DAI · IP Source Guard · Blackhole VLANs

### 🖥️ Serverinfrastructuur (Proxmox VE)
Alle diensten als VM's op één fysieke Proxmox VE-host, verbonden via **één Linux-bridge (`vmbr0`) met VLAN-trunk**:

| VM | VLAN | IP | Rol |
|----|------|----|-----|
| `k3s-w1/w2/w3` | 190 | `10.10.90.11–13` | K3s Kubernetes-nodes (Alpine Linux) |
| `ticketsystem-intranet` | 190 | `10.10.90.44` | Zammad ticketsysteem |
| `npmplus-dmz-pub` | 191 | `145.34.44.80` | NPM Plus — publieke reverse proxy (DMZ) |
| `grafana-intranet` | 180 | `10.10.80.21` | Grafana monitoring (standalone) |
| `crm-intranet` | 180 | `10.10.80.90` | CRM-applicatie |
| Windows Server | 180 | `10.10.80.11` | Active Directory DC · DHCP · PRTG |
| Issabel PBX | 180 | INTRANET | VoIP-telefooncentrale |
| Stalwart mail | 180 | INTRANET | Mailserver `@tropometrics.tech` |

### ☁️ Cloud-native Weerdienst (K3s / Kubernetes)
- **`look-at-this-weather`** — React 18 + TypeScript + Vite, realtime weerdata, 7-daagse voorspelling, maritiem overzicht, dark/light mode
- **`api-server`** — Node.js/Express caching-proxy voor Open-Meteo, Redis TTL-caching (rate limiting preventie)
- **Horizontal Pod Autoscaler** — automatisch schalen van 2 naar 10 replicas op 50% CPU
- **ArgoCD GitOps** — declaratief, self-healing, volledig geautomatiseerd

### 📞 Telefonie (Issabel PBX / FreePBX + Asterisk)
- IVR-menu voor inkomende gespreksroutering naar afdelingen
- SIP-softphones · SIP-bureautelefoons · analoge telefoons via **Grandstream ATA**

### 📊 Monitoring (tweelaagsig)
- **PRTG** (op Windows Server) — SNMPv3-monitoring van alle Cisco-apparaten, servers en servicebeschikbaarheid
- **Prometheus + Grafana** (standalone VM) — applicatie- en clustermetrics
- **Alertmanager** — alertroutering vanuit K3s

### 🔐 Beveiliging
Cisco Firepower 1140 (firewall/IDS/IPS) · Active Directory + LDAP · 802.1X · SNMPv3 · SSH-only beheer · IPsec-versleuteld inter-siteverkeer

---

## Technologiestack

`Cisco IOS` · `Proxmox VE` · `K3s / Kubernetes` · `ArgoCD` · `GitHub Actions` · `Docker` · `MetalLB` · `Traefik` · `NPM Plus` · `PRTG` · `Prometheus` · `Grafana` · `Alertmanager` · `Windows Server AD` · `Issabel PBX` · `Grandstream ATA` · `Stalwart Mail` · `Zammad` · `React 18` · `TypeScript` · `Node.js` · `Redis` · `Tailwind CSS` · `shadcn/ui`

---

## Repositories

| Repository | Beschrijving |
|------------|-------------|
| [tropometrics-docs](https://github.com/TropoMetrics/tropometrics-docs) | Centrale documentatie — volledig in Nederlands én Engels |
| [look-at-this-weather](https://github.com/TropoMetrics/look-at-this-weather) | React/TypeScript weerdienst frontend |
| [api-server](https://github.com/TropoMetrics/api-server) | Node.js/Redis caching API-proxy |
| [ArgoCI-CD](https://github.com/TropoMetrics/ArgoCI-CD) | ArgoCD GitOps-manifests en Kubernetes-configuraties |
| [Packet-Tracer-configs](https://github.com/TropoMetrics/Packet-Tracer-configs) | Cisco IOS-configuraties en Packet Tracer-bestanden |

---

## Team

| Naam | Rol |
|------|-----|
| **Kai Diemel** | Projectleider · Netwerk · Frontend · Documentatie |
| **Max Blaauw** | 2e projectleider · Netwerktechniek · Infra · DevOps · Front- & Backend |
| **Ole Spiegelenberg** | DevOps · Beveiliging · Onderzoek |
| **Arne Jansonius** | Netwerk · Netwerkontwerp · Documentatie · Onderzoek |

---

*De Haagse Hogeschool · Network Infrastructure Design · September 2025 – Januari 2026*
*Dit project is uitgevoerd als Proof of Concept in een onderwijscontext. Niet alle keuzes zijn bedoeld voor directe productie-implementatie, maar zijn technisch onderbouwd en volledig reproduceerbaar.*
