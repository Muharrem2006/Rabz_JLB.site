Op deze pagina staat het netwerkontwerp van het homelab, inclusief de fysieke topologie, IP-adresbeheer, port-forwarding en VPN-integratie.

## Fysieke Netwerkinfrastructuur

* **Home Modem / Provider Router:** De basisverbinding met het internet.
* **TP-Link Router:** Primaire router voor het algemene thuisnetwerk (Wi-Fi & LAN).
* **GL.iNet GL-BE3600 Router:** Dedicated router voor het beheren en scheiden van het homelab-verkeer.
* **TP-Link TL-SG105-M2 Switch:** 2.5 Gbps switch voor snelle onderlinge datacommunicatie tussen de Proxmox-nodes en opslag.
* **Bekabeling:** 1 Gbps LAN-verbindingen op alle Lenovo-nodes.

## IP-Adresbeheer & DNS-Routing

* **Statische IP-adressen:** Alle Proxmox-nodes en kritieke containers (zoals AdGuard Home en Tailscale) hebben een vast IP-adres om IP-conflicten en verbroken koppelingen te voorkomen.
* **Netwerkbrede DNS (AdGuard Home):** Alle interne apparaten sturen hun DNS-aanvragen via de AdGuard LXC-container voor filtering op advertenties en malware.
* **Dynamische DNS (DuckDNS):** Houdt het wisselende publieke IP-adres van de internetprovider automatisch gekoppeld aan de DuckDNS-domeinnaam.

## Poortdoorsturing (Port Forwarding)

Om specifieke services vanaf het internet bereikbaar te maken voor externe gebruikers, zijn de volgende poorten doorgestuurd op de router naar de betreffende LXC-container:

> **Let op:** Stuur deze poorten door in alle tussenliggende netwerkapparaten (in mijn geval de Home Modem, TP-Link Router én de GL.iNet Router).

| Service | Poort / Protocol | Functie |
| :--- | :--- | :--- |
| **Minecraft Java** | `25565` / TCP | Standaard multiplayer verbinding |
| **Simple Voice Chat** | `24454` / UDP | In-game spraakverbinding |
| **Minecraft Bedrock** | `19132` / UDP | Cross-play ondersteuning via Geyser |

Zorg er eveneens voor dat de poorten worden toegestaan op de interne firewall (UFW) van de betreffende container en node:

```bash 
sudo ufw allow 25565/tcp
sudo ufw allow 24454/udp
sudo ufw allow 19132/udp
sudo ufw enable
```