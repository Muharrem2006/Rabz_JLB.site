# Homelab Documentation Site

Een overzichtelijke en lichtgewicht documentatiesite waarin ik de opbouw, hardware en services van mijn persoonlijke homelab bijhoud. Gebouwd met **Zensical / MkDocs** en gehost via GitHub Pages.

**Bekijk de live site:** [jouw-username.github.io/jouw-repo](https://jouw-username.github.io/jouw-repo)

---

## Over dit Project

In deze repository documenteer ik de volledige configuratie van mijn Proxmox-cluster, netwerkarchitectuur en gehoste applicaties. Het doel is om ervaringen te delen, obstakels (zoals netwerkproblemen en geheugenallocatie) vast te leggen en concrete oplossingen te bieden voor medestudenten en homelab-enthousiastelingen.

### Hardware & Infrastructure
* **Compute Cluster:** Proxmox VE op 1x Lenovo M920q & 2x Lenovo M720q nodes.
* **Netwerk:** GL.iNet GL-BE3600 Router, TP-Link TL-SG105-M2 (2.5Gbps) Switch.
* **Opslag:** Custom Mini-NAS (TrueNAS via PCIe-to-SATA) met 2x 2TB Toshiba P300 HDDs.
* **Power Mod:** Centrale 800W voeding voor alle micro-PC's.

### Gehoste Services
* **[AdGuard Home](docs/services/adguard.md):** Netwerkbrede DNS-filtering en advertentieblokkering.
* **[Tailscale](docs/services/tailscale.md):** Mesh-VPN met Subnet Routing en Exit Node.
* **[DuckDNS](docs/services/duckdns.md):** Automatische Dynamische DNS (DDNS) updater.
* **[Minecraft Server](docs/services/minecraft.md):** Fabric/Modrinth multiplayer server met Geyser cross-play.

---

## Mappenstructuur

```text
.
├── docs/
│   ├── assets/              # Afbeeldingen en screenshots
│   ├── extra.css            # Custom CSS styling (logo scaling, kleuren)
│   ├── infrastructure/      # Hardware, netwerk & cluster architecture
│   │   ├── cluster.md
│   │   ├── design.md
│   │   └── networking.md
│   ├── services/            # Docker configs & service handleidingen
│   │   ├── adguard.md
│   │   ├── duckdns.md
│   │   ├── minecraft.md
│   │   └── tailscale.md
│   └── index.md             # Welkomstpagina
├── zensical.toml            # Site-configuratie en navigatiestructuur
└── README.md                # Repository documentatie
```
