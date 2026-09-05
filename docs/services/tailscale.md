Tailscale is een mesh-VPN (gebaseerd op het WireGuard-protocol) waarmee apparaten zoals smartphones, laptops en servers op een veilige manier rechtstreeks met elkaar communiceren. Binnen mijn homelab gebruik ik Tailscale om onderweg een beveiligde verbinding op te bouwen met mijn thuisnetwerk.

**Container Specificaties**

De centrale node draait binnen een toegewijde LXC-container op Proxmox. Omdat Tailscale erg efficiënt is, vereist de container minimale resources:

* **CPU:** 1 Core
* **RAM:** 256 MiB
* **Opslag:** 8 GiB SSD
* **Netwerk:** Statisch IP-adres (voor een vaste koppeling met overige netwerkdiensten)

De basisinstallatie op de Linux-container is uitgevoerd via de [officiële Tailscale documentatie](https://tailscale.com/docs/install/linux#mainstream-distributions).

**Exit Node & Subnet Routing**

De container is geconfigureerd als **Exit Node** en **Subnet Router** (`tailscale-router`). Hierdoor kan ik al mijn internetverkeer vanaf een externe locatie versleuteld via mijn thuis netwerkverbinding laten lopen, en zijn ook interne apparaten zonder Tailscale-client direct bereikbaar.

> **Handleiding:** [Tailscale Exit Nodes documentatie](https://tailscale.com/docs/features/exit-nodes?tab=linux)

**In de Praktijk**

De LXC-container draait 24/7 en bedient al mijn apparaten (zoals mijn MacBook en iPhone). Dit biedt de volgende voordelen:

* **Beheer op afstand:** Direct toegang tot serverconsoles en dashboards vanaf elke locatie.
* **Snelle probleemoplossing:** Binnen enkele seconden de status van diensten (zoals de Minecraft-server) controleren en herstellen zonder fysiek thuis te hoeven zijn.
* **Veilige netwerklaag:** Geen open poorten nodig op de thuisrouter.

![Tailscale Admin Dashboard](/assets/tailscale-dashboard.png){width="30"}