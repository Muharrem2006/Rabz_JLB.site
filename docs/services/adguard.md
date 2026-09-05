AdGuard Home is een netwerkbrede DNS-server die advertenties, trackers en schadelijke domeinen op DNS-niveau blokkeert. Wanneer een apparaat binnen het netwerk een domeinnaam opvraagt, vergelijkt AdGuard deze met een filterlijst. Wordt een domein als reclame of malware herkend, dan wordt de aanvraag direct geblokkeerd. Dit zorgt voor een snellere én veiligere surfervaring voor alle verbonden apparaten.

**Container Specificaties**

De applicatie draait binnen een LXC-container op Proxmox. Omdat AdGuard Home erg efficiënt is, zijn de systeemeisen erg laag:

* **CPU:** 1 Core
* **RAM:** 1024 MiB
* **Opslag:** 8 GiB
* **Netwerk:** Statisch IP-adres (noodzakelijk om als primaire DNS-server ingesteld te worden)

> **Opmerking over het geheugen:** Binnen deze container draait ook de DuckDNS-script-updater. Daarom is er 1024 MiB RAM toegewezen. Draai je enkel AdGuard Home? Dan is 256 MiB RAM ruim voldoende.

**Installatie**

De basisinstallatie is uitgevoerd via de [officiële AdGuard Home Docker documentatie](https://hub.docker.com/r/adguard/adguardhome). Een handige stapsgewijze handleiding is ook te vinden op [Medium](https://medium.com/@codyrwaits/installing-adguard-home-in-my-home-lab-648393f6064f).

**Gebruikte Filterlijsten**

De actieve blocklists worden beheerd via **Filters** -> **DNS blocklists**:

* **AdGuard DNS filter** - Standaard bescherming tegen advertenties en privacytrackers.
* **AdAway Default Blocklist** - Geoptimaliseerde blokkadelijst voor mobiele apparaten.
* **OISD Blocklist Big** - Brede, uitgebreide lijst met minimale kans op valse positieven.
* **Dandelion Sprout's Anti Push Notifications** - Blokkeert hinderlijke pushmelding-popups op websites.
* **Phishing URL Blocklist (PhishTank & OpenPhish)** - Actieve bescherming tegen phishing-pogingen.
* **Dandelion Sprout's Anti-Malware List** - Blokkeert domeinen die bekendstaan om malware en spyware.
* **NSFW Blocklist** - Filtert expliciete en volwassen content.
* **HaGeZi's Encrypted DNS/VPN/TOR/Proxy Bypass** - Voorkomt dat apparaten lokale DNS-filters omzeilen via versleutelde protocollen of proxy-netwerken.

**Upstream DNS configuratie**

De upstream DNS servers zijn ingesteld via **Settings** -> **DNS settings**:

```
https://dns10.quad9.net/dns-query
quic://dns.adguard-dns.com
```