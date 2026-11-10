# Home Lab di rete autoprogettato

### Un laboratorio domestico self-hosted in stile enterprise costruito per sviluppare competenze pratiche di ingegneria di rete utilizzando hardware e software reali

---

## Panoramica

Costruito utilizzando **Proxmox VE**, **pfSense** configurato su un **Fujitsu Futro S900** con doppia NIC, un **HP EliteDesk 800 G2** e un **TP-Link TL-sg108e**, questo laboratorio applica concetti di networking a livello IT Support in un ambiente che replica uno scenario reale.

---

## Hardware

- **Host principale**: HP EliteDesk 800 G2 (SFF)
  - CPU: Intel Core i7-6700
  - RAM: 32 GB DDR4 (2x16 GB)
  - Storage: SSD NVMe (sistema) + SSD SATA (storage dati)
  - NIC: Intel I219-LM (driver `e1000e`)
- **Switch gestito**: TP-Link TL-sg108e
- **Firewall**: Fujitsu Futro S900
  - doppia scheda di rete
- **Router**: Vodafone Station

## Stack software

- Proxmox VE 
- Debian / LXC
- Ubuntu Server
- Windows server
- Windows 11


---

## 🏗️ Network Architecture

```text
Openreach ONT
→ pfSense (Edge Router / Firewall / DHCP / DNS / VPN)
→ Cisco Catalyst 2960 (Managed Switch / VLAN Trunk)
→ VLAN Segmentation
→ Proxmox VE Host → Ubuntu VMs / Services
→ EE Router (Access Point — VLAN 40 Wi-Fi only)
→ User Devices
```

pfSense connects directly to the Openreach ONT via **PPPoE**, eliminating the ISP router from the routing path entirely and resolving the previous double-NAT issues.

### 📐 Network Diagram

<!-- Replace with your diagram image link -->
![Network Diagram](docs/diagrams/network-diagram.png)

---

## 🌐 VLAN Design

| VLAN | Purpose | Subnet |
| --- | --- | --- |
| **VLAN 10** | Admin Devices | `192.168.10.0/24` |
| **VLAN 20** | Servers / Lab | `192.168.20.0/24` |
| **VLAN 30** | IoT Devices | `192.168.30.0/24` |
| **VLAN 40** | Users / Wi-Fi Clients | `192.168.40.0/24` |
| **VLAN 99** | Management Network | `192.168.99.0/24` |

---

## 🔒 Security Design

### Firewall Policy

- Least-privilege inter-VLAN access rules
- IoT and user devices isolated from admin and server networks
- Users permitted HTTP/HTTPS to servers only
- pfSense GUI and SSH locked to VLAN 99 (management) only
- WAN exposure reviewed and unnecessary services disabled

### DNS Hardening

- DNS Resolver ACLs restricting query sources
- DNSSEC enabled
- DNS traffic enforcement per VLAN

### SSH Hardening

- Public key authentication only — password login disabled
- Management access restricted to trusted VLAN

### Zone-Based Firewall Policy

<!-- Replace with your diagram image link -->
![Firewall Zone Diagram](docs/diagrams/zone-firewall.png)

---

## 🔐 WireGuard VPN

Deployed WireGuard for secure remote access to the lab:

- pfSense-hosted WireGuard server
- Remote peer configured on MacBook
- Access scoped to management VLAN and infrastructure only
- Validated externally using mobile hotspot to confirm true remote access
- Required resolving PPPoE edge routing, NAT, and ISP router conflicts before functioning correctly

---

## 🖥️ Linux & Server Administration

Hands-on Ubuntu Server administration carried out throughout this project:

- CLI navigation, file permissions, filesystem management
- Package management with `apt`
- Service management with `systemctl` and `journalctl`
- SSH key generation and hardened remote access
- Proxmox VM and LXC container deployment
- Virtual bridge and network interface configuration
- Snapshot and backup management
- Real-world Windows recovery using SFC and CHKDSK

---

## 🔧 Troubleshooting Experience

The most valuable part of this project. Every issue below was diagnosed and resolved without external assistance.

| Problema | Layer | Area |
| --- | --- | --- |
| DHCP failures — missing WAN lease | L3 | WAN / ISP connectivity |

esempi 
| Switch port CRC errors | L1 | Physical cabling |
| FastEthernet trunk bottleneck | L1 | Interface speed limitation |
| Trunk link migrated to Gigabit interfaces | L1 | Hardware upgrade |

---

## 📁 Documentation

| Document | Description |
| --- | --- |
| [01 — L'inizio](docs/01-integrazione-lan.md) | Installazione bare-metal di proxmox|
| [02 — Proxmox Setup](docs/02-proxmox-setup.md) | Installazione e prime configurazioni dell'Hypervisor |
| [03 — integrazione-lan](docs/03-integrazione-lan.md) | I primi problemi riscontrati|
| [04 — creazione servizi](docs/04-1-servizio.md) | Il primo servizio installato e configurato |

---

## Il perchè del mio progetto

Questo progetto nasce per mettere in pratica ed imparare a livello tecnico, quanto appreso durante lo studio autonomo (supportato da corsi e materiale fruibile online), del mondo dell' IT.

Questa scelta parte dalla passione per le tecnologie e per la magia di risolvere i problemi, nonchè dalla voglia di iniziare una carriera nel settore.

Il percorso non è stato semplice fino ad ora, ma la passione e gli obiettivi alimentano la volontà e portano lontano. 
Sono partito come un semplice user qualche anno fa e ad oggi, mi ritrovo un background tecnico di cui vado fiero.

Con questo progetto ho imparato e sto continuando ad imparare:
- Configurazioni di hardware 
- Server windows/linux e la loro gestione;
- Ambienti virtualizzati, da zero fino a creare una reale infrastuttura;
- Come i vari layer del modello TCP/IP comunicano tra loro;
- I vari protocolli (DNS, DHCP,HTTP,VPN,IP ecc...)
- 










- Network Engineering
- Infrastructure / Systems Administration
- Datacenter Operations
- NOC / Cloud Infrastructure roles

---

Built and maintained by Gustavo | Ongoing development
