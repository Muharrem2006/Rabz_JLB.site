DuckDNS is een gratis dynamische DNS-dienst (DDNS) die een vast webadres (zoals `jouw-subdomein.duckdns.org`) automatisch koppelt aan een dynamisch (wisselend) IP-adres. Dit zorgt ervoor dat services binnen het homelab altijd bereikbaar blijven op een vaste domeinnaam, zelfs wanneer de internetprovider het publieke IP-adres veranderd.

## Container Specificaties

DuckDNS draait als een extreem lichte Docker-container. Binnen mijn homelab draait deze container mee op de toegewijde [AdGuard Home LXC-container](adguard.md).

> Bekijk de [AdGuard Home-documentatie](adguard.md) voor de hardware- en netwerkspecificaties van deze container.

**Installatie via Docker Compose**

## DuckDNS Account & Token
1. Log in op [DuckDNS.org](https://www.duckdns.org) en maak een account aan.
2. Kies het gewenste subdomein en voeg dit toe.
3. Kopieer de unieke **token** die bovenaan het DuckDNS-dashboard getoond wordt.

## Map en Configuratie Aanmaken
Maak op de target-host een directory aan voor DuckDNS en open het `docker-compose.yml` bestand:

```
mkdir -p ~/duckdns && cd ~/duckdns
nano docker-compose.yml
```

Plak de volgende configuratie in het bestand:

```
services:
  duckdns:
    image: lscr.io/linuxserver/duckdns:latest
    container_name: duckdns
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Brussels
      - SUBDOMAINS=jouw-subdomein # Enkel het subdomein invoeren (zonder .duckdns.org)
      - TOKEN=jouw-token # Je persoonlijke DuckDNS-token
    volumes:
      - /opt/duckdns/config:/config # Locatie voor logbestanden
    restart: unless-stopped
```

##Container Starten & Controleren

Start de service op de achtergrond en controleer de status via de logbestanden:

```
docker compose up -d
docker logs -f duckdns
```
Zodra de melding `Your IP was updated successfully` of `OK` in de logs verschijnt, is de installatie geslaagd en wordt het IP-adres automatisch bijgewerkt.

[<img src="../assets/duckdns.png" alt="Adguard Home Admin Dashboard" width="50" />](https://www.duckdns.org/)
