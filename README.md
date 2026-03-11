# [Project Name: e.g., Heimdall: Homelab & Infrastructure]

![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=for-the-badge&logo=proxmox&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Tailscale](https://img.shields.io/badge/Tailscale-FFFFFF?style=for-the-badge&logo=Tailscale&logoColor=black)
![Ubiquiti](https://img.shields.io/badge/Ubiquiti-0559C9?style=for-the-badge&logo=ubiquiti&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=for-the-badge&logo=Cloudflare&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/-RaspberryPi-C51A4A?style=for-the-badge&logo=Raspberry-Pi)

An overview and documentation of my personal homelab environment, network architecture, and self-hosted infrastructure.

## Overview
This repository contains the configurations, docker-compose files, and Infrastructure as Code (IaC) for my homelab. The primary goals of this environment are to learn new technologies, self-host essential services, and experiment with network security and automation. *[**Homelab Diagram**](./Homelab_Diagram.png)*

> **Looking for Homelab guides?** All hardware-agnostic documentation and step-by-step guides for setting up these services from scratch can be found in my separate repository: *[**Homelab Manuals**](https://github.com/JonathanWindell/Homelab-Manuals)*.

---

## Architecture & Hardware

### Networking & VLAN Configuration
My infrastructure is logically divided into distinct VLANs to separate the core home network from isolated security testing environments.

| Network | Description | Purpose |
| :--- | :--- | :--- |
| **VLAN 1** | Home Network | Main secure network for trusted end devices and core services. |
| **VLAN 2** | Honeypot | Isolated network strictly for security monitoring and capturing malicious traffic. |

| External Service | Category | Badge |
| :--- | :--- | :--- |
| **Ubiquiti Ecosystem** | Gateway & Switching | ![Ubiquiti](https://img.shields.io/badge/Ubiquiti-0559C9?logo=ubiquiti&logoColor=white) |
| **Cloudflare** | DNS & Domain Management | ![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?logo=Cloudflare&logoColor=white) |
| **GitHub** | Version Control & Backups | ![GitHub](https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white) |
| **Discord** | System Alerts (Webhooks) | ![Discord](https://img.shields.io/badge/Discord-5865F2?logo=discord&logoColor=white) |

### Hardware Nodes

| Node | Hardware | OS/Hypervisor | Primary Role |
| :--- | :--- | :--- | :--- |
| **Node 1** | HP Prodesk 600 G3 | ![Proxmox](https://img.shields.io/badge/Proxmox-E57000?logo=proxmox&logoColor=white) | Main Compute (VMs/LXC) |
| **Node 2** | Ugreen DXP2800 | ![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white) | NAS & Media Storage |
| **Node 3** | Raspberry Pi 3 | ![Linux](https://img.shields.io/badge/Raspberry_Pi-A22846?logo=raspberrypi&logoColor=white) | Security Node (VLAN 2) |

---

## Services & Containers

### Node 1: HP Prodesk 600 G3 Mini (Proxmox)

#### Virtual Machines (VM)
| Service | Badge | Description | Tags |
| :--- | :--- | :--- | :--- |
| **Torrenting Box** | ![qBittorrent](https://img.shields.io/badge/qBittorrent-61DAFB?logo=qbittorrent&logoColor=black) | Isolated Docker environment for secure P2P via VPN. | `VM` `VPN` |

#### Linux Containers (LXC)

**Network & Security**
| Service | Badge | Description | Tags |
| :--- | :--- | :--- | :--- |
| **Nginx Proxy Manager** | ![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white) | Reverse Proxy & SSL Management | `[Tailscale Node]` `[Wazuh Agent]` |
| **AdGuard Home** | ![AdGuard](https://img.shields.io/badge/AdGuard-67B279?logo=adguard&logoColor=white) | DNS Sinkhole & Tailscale routing | `[Tailscale Node]` `[Wazuh Agent]` |

**Monitoring & Alerting**
| Service | Badge | Description | Tags |
| :--- | :--- | :--- | :--- |
| **Wazuh** | ![Wazuh](https://img.shields.io/badge/Wazuh-00A9E0?logo=wazuh&logoColor=white) | SIEM & Threat Detection | |
| **Grafana** | ![Grafana](https://img.shields.io/badge/Grafana-F46800?logo=grafana&logoColor=white) | Metrics Visualization Dashboards | |
| **Prometheus** | ![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?logo=prometheus&logoColor=white) | Time-series Metric Collection | |
| **Glances** | ![Monitoring](https://img.shields.io/badge/Glances-999?logo=activitypub&logoColor=white) | Real-time System Monitoring | |
| **Uptime Kuma** | ![Uptime](https://img.shields.io/badge/Uptime_Kuma-61DBFB?logo=statuspage&logoColor=black) | Uptime tracking for services | `[Tailscale Node]` |
| **Prometheus Alerts** | ![Alerts](https://img.shields.io/badge/Alerts-E6522C?logo=prometheus&logoColor=white) | Self-hosted notification routing | |

**Dev & Automation**
| Service | Badge | Description | Tags |
| :--- | :--- | :--- | :--- |
| **n8n** | ![n8n](https://img.shields.io/badge/n8n-FF6D5B?logo=n8n&logoColor=white) | Workflow Engine *[**n8n-workflows**](https://github.com/JonathanWindell/n8n-workflows)* | |
| **Gitea** | ![Gitea](https://img.shields.io/badge/Gitea-609926?logo=gitea&logoColor=white) | Self-hosted Git with Cron backups | |
| **Auto-updaters** | ![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white) | Automated container management | |

**Productivity & Tools**
| Service | Badge | Description | Tags |
| :--- | :--- | :--- | :--- |
| **Paperless-ngx** | ![Paperless](https://img.shields.io/badge/Paperless-000?logo=googledocs&logoColor=white) | Document Management & OCR | |
| **Syncthing** | ![Syncthing](https://img.shields.io/badge/Syncthing-0882C0?logo=syncthing&logoColor=white) | P2P File Synchronization | |
| **Linkwarden** | ![Linkwarden](https://img.shields.io/badge/Linkwarden-5C2D91?logo=bookmark&logoColor=white) | Bookmark Archive & Manager | |
| **File Browser** | ![Files](https://img.shields.io/badge/File_Browser-00A4EF?logo=files&logoColor=white) | Web UI for filesystem access | |
| **Gotenberg** | ![Gotenberg](https://img.shields.io/badge/Gotenberg-blue?logo=adobeacrobatreader&logoColor=white) | API for PDF conversions | |

**Dashboard & Portfolio**
| Service | Badge | Description | Tags |
| :--- | :--- | :--- | :--- |
| **Homepage** | ![Dashboard](https://img.shields.io/badge/Homepage-333?logo=speedtest&logoColor=white) | Central Service Dashboard | `[Tailscale Node]` |
| **Personal Portfolio** | ![Portfolio](https://img.shields.io/badge/Portfolio-61DBFB?logo=react&logoColor=black) | Self-hosted *[**Portfolio Site**](https://portfolio.jonathans-labb.org/)* | |

---

### Node 2: NAS Ugreen DXP2800 (Docker)
| Service | Badge | Description |
| :--- | :--- | :--- |
| **Jellyfin** | ![Jellyfin](https://img.shields.io/badge/Jellyfin-00A4EF?logo=jellyfin&logoColor=white) | Media server for local streaming |
| **Photo/Media** | ![Media](https://img.shields.io/badge/Photos-FFB000?logo=googlephotos&logoColor=white) | Dedicated media backup containers |

---

### Node 3: Raspberry Pi Model 3 (Docker)
| Service | Badge | Description | Tags |
| :--- | :--- | :--- | :--- |
| **Web-Honeypot** | ![Honeypot](https://img.shields.io/badge/Honeypot-E01E5A?logo=target&logoColor=white) | Captures malicious traffic on VLAN 2 | `[Wazuh Agent]` `VLAN 2` |

---

## Inspiration & Resources
* [NetworkChuck](https://www.youtube.com/@NetworkChuck)
* [Tailscale](https://www.youtube.com/@Tailscale)
* [Lawrence Systems](https://www.youtube.com/@LAWRENCESYSTEMS)
* [Barmine Tech](https://www.youtube.com/@BarmineTech)

---

## Author
I'm Jonathan, and I develop projects in my spare time that help myself and others become better and more efficient developers!
* [LinkedIn](https://www.linkedin.com/in/jonathan-windell-418a55232/)
* [Portfolio](https://portfolio.jonathans-labb.org/)

---

## License
This project is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).