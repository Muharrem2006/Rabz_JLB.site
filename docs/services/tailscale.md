Tailscale is een mesh-VPN (gebaseerd op het WireGuard-protocol) waarmee apparaten zoals smartphones, laptops en servers op een veilige manier rechtstreeks met elkaar communiceren. Binnen mijn homelab gebruik ik Tailscale om onderweg een beveiligde verbinding op te bouwen met mijn thuisnetwerk.

## Container Specificaties

De centrale node draait binnen een toegewijde LXC-container op Proxmox. Omdat Tailscale erg efficiënt is, vereist de container minimale resources:

* **CPU:** 1 Core
* **RAM:** 256 MiB
* **Opslag:** 8 GiB SSD
* **Netwerk:** Statisch IP-adres (voor een vaste koppeling met overige netwerkdiensten)

De basisinstallatie op de Linux-container is uitgevoerd via de [officiële Tailscale documentatie](https://tailscale.com/docs/install/linux#mainstream-distributions).

**Exit Node & Subnet Routing**

De container is geconfigureerd als **Exit Node** en **Subnet Router** (`tailscale-router`). Hierdoor kan ik al mijn internetverkeer vanaf een externe locatie versleuteld via mijn thuis netwerkverbinding laten lopen, en zijn ook interne apparaten zonder Tailscale-client direct bereikbaar.

> **Handleiding:** [Tailscale Exit Nodes documentatie](https://tailscale.com/docs/features/exit-nodes?tab=linux)

## DNS-instellingen (AdGuard Integratie)

Om ook onderweg te profiteren van AdGuard, wordt het DNS-verkeer binnen het Tailscale-netwerk direct via de AdGuard Home container geleid. Als AdGuard om een of andere reden uitvalt, dienen publieke DNS-servers (zoals Google en Cloudflare) als fallback.

**Configuratie via het Tailscale Admin Panel:**

1. Navigeer naar **Netwerk** -> **DNS** -> **Global nameservers**.
2. Klik op **Add nameserver** -> **Custom**.
3. Vul het IP-adres in van de AdGuard Home container.
4. Voeg optioneel secundaire fallback DNS-servers toe (zoals `1.1.1.1` of `8.8.8.8`).
5. Schakel de optie **Override DNS servers** in om het gebruik van de ingestelde DNS-servers af te dwingen op alle verbonden apparaten.

> Bekijk de [AdGuard Home-documentatie](adguard.md) voor meer details over de instellingen van de DNS-container.

## In de Praktijk

De LXC-container draait 24/7 en bedient al mijn apparaten (zoals mijn MacBook en iPhone). Dit biedt de volgende voordelen:

* **Beheer op afstand:** Direct toegang tot serverconsoles en dashboards vanaf elke locatie.
* **Snelle probleemoplossing:** Binnen enkele seconden de status van diensten (zoals de Minecraft-server) controleren en herstellen zonder fysiek thuis te hoeven zijn.
* **Veilige netwerklaag:** Geen open poorten nodig op de thuisrouter.

<img src="../assets/tailscale-dashboard.png" alt="Tailscale Admin Dashboard" width="50" />