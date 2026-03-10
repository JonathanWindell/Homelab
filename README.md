# [Project Name: e.g., Heimdall: Homelab & Infrastructure]

![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=for-the-badge&logo=proxmox&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Tailscale](https://img.shields.io/badge/Tailscale-FFFFFF?style=for-the-badge&logo=Tailscale&logoColor=black)
![Ubiquiti](https://img.shields.io/badge/Ubiquiti-0559C9?style=for-the-badge&logo=ubiquiti&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=for-the-badge&logo=Cloudflare&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/-RaspberryPi-C51A4A?style=for-the-badge&logo=Raspberry-Pi)

An overview and documentation of my personal homelab environment, network architecture, and self-hosted infrastructure.

## Overview
This repository contains the configurations, docker-compose files, and Infrastructure as Code (IaC) for my homelab. The primary goals of this environment are to learn new technologies, self-host essential services, and experiment with network security and automation. *[Homelab Diagram](./Homelab_Diagram.png)*

> **Looking for Homelab guides?** > All hardware-agnostic documentation and step-by-step guides for setting up these services from scratch can be found in my separate repository: *[Homelab Manuals](URL_TO_MANUALS*)*.

---

## Architecture & Hardware

My infrastructure is logically divided into distinct VLANs to separate the core home network from isolated security testing environments. 

### Networking & External Services
* **Gateway:** Ubiquiti Cloud Gateway Ultra (UCG Ultra)
* **Switch:** Ubiquiti USW Flex
* **VLAN 1 (Home Network):** Main secure network for trusted end devices and core services.
* **VLAN 2 (Honeypot):** Isolated network strictly for security monitoring and capturing malicious traffic.

**External Integrations:**
* **Cloudflare:** Domain name management and DNS.
* **Discord:** Utilized for incoming webhooks to route simple system alerts.
* **GitHub:** Version control and backups for docker configurations.
* **UniFi Remote Site Manager:** Remote management of routing and switching infrastructure.

### Compute & Storage Nodes
* **Compute (Proxmox VE):** HP Prodesk 600 G3 Mini. Handles the heavy lifting, running a mix of VMs and LXC containers.
* **Storage (NAS):** Ugreen DXP2800. Dedicated to bulk storage and media serving.
* **Security Node:** Raspberry Pi Model 3. Placed securely on the isolated VLAN 2.

---

## Services & Containers

Here is a detailed breakdown of the services currently running across the nodes:

### Node 1: HP Prodesk 600 G3 Mini (Proxmox)
*This node runs multiple environments, utilizing both Virtual Machines and Linux Containers (LXC).*

**VM: Torrenting Box**
* **qBittorrent & VPN:** An isolated Docker environment for secure, routed P2P downloading through a dedicated VPN tunnel.

**LXC: Core Services (Docker)**
* **Network & Security:** * **Nginx Proxy Manager (NPM):** Manages incoming web traffic, SSL certificates, and internal routing. `[Tailscale Node]` `[Wazuh Agent]`
  * **AdGuard Home:** Network-wide DNS sinkhole for blocking ads and tracking domains (includes Tailscale subnet routing). `[Tailscale Node]` `[Wazuh Agent]`
* **Monitoring & Alerting:**
  * **Wazuh:** Open-source security platform for threat detection and SIEM.
  * **Grafana & Prometheus:** Metric collection and visual dashboards for system health.
  * **Glances:** Real-time system monitoring. 
  * **Uptime Kuma:** Uptime tracking and alerting for all internal/external services. `[Tailscale Node]`
  * **Prometheus Alerts:** Self-hosted notification server for routing system alerts.
* **Dev & Automation:**
  * **n8n:** My primary workflow automation engine. *[See my n8n-workflows repo](https://github.com/JonathanWindell/n8n-workflows)*
  * **Gitea:** Self-hosted Git service for local version control (runs with scheduled cron backups). 
  * **Auto-updaters:** Automated container lifecycle management.
* **Productivity & Tools:**
  * **Paperless-ngx:** Digital document management and OCR.
  * **Syncthing:** P2P file synchronization between devices to protect data locally. 
  * **Linkwarden:** Bookmark manager to easily organize and archive web links. 
  * **File Browser:** Web interface for easy browsing of files across LXC containers and VMs.
  * **Gotenberg:** Developer-friendly API for converting various formats to PDF. 
* **Dashboard & Portfolio:**
  * **Homepage:** Centralized starting page for quick access to all services. `[Tailscale Node]`
  * **Personal Portfolio:** Self-hosting personal portfolio. *[Portfolio](https://portfolio.jonathans-labb.org/)*

### Node 2: NAS Ugreen DXP2800 (Docker)
* **Jellyfin:** Open-source media server for streaming movies and shows to end devices.
* **Photo/Media Management:** Dedicated containers for backing up and organizing personal media.

### Node 3: Raspberry Pi Model 3 (Docker)
* **Web-Honeypot:** Placed on the isolated VLAN 2, this container deliberately exposes simulated vulnerable services to the internet to log, monitor, and analyze malicious traffic. `[Wazuh Agent]`

---

## Inspiration & Resources
If you wish to get started with your own homelabbing journey, I hope you can find some inspiration here! Below are some of my favorite resources that helped me learn:

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
This project is licensed under the [Creative Commons Attribution-ShareAlike 4.0 International License (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/). You are free to share and adapt the material, provided you give appropriate credit and distribute your contributions under the same license.