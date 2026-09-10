# System Design & Architecture

Op deze pagina leg ik de softwarematige architectuur van het homelab uit, inclusief de ontwerpkeuzes voor Proxmox VE en de initiële instelling van het cluster.

## Ontwerpkeuzes

* **LXC boven VM's:** Om system resources (RAM en CPU) optimaal te benutten, draaien lichte diensten zoals AdGuard en Tailscale in LXC-containers. Alleen voor complexere workloads (zoals de Minecraft-server) wordt een zwaardere, dedicated container-omgeving gebruikt.
* **Decentrale Compute (3 Nodes):** Door de werkbelasting te verdelen over drie mini-PC's blijft het stroomverbruik laag en kunnen zware taken worden geïsoleerd. Omdat de belasting verspreid is, blijven de temperaturen laag en hoeven de fans zelden op hoge toeren te draaien.

## Cluster Initialisatie

> Zie ook [Netwerk & IP-initialisatie](networking.md) voor de bijbehorende netwerkinstellingen.

1. **Proxmox VE Installatie:** Elke Lenovo-node is voorzien van een schone Proxmox VE-installatie op de interne 256 GB SSD (geïnstalleerd via een USB-stick). Open vervolgens de webinterface van de node die je als hoofdcomputer wilt gebruiken.
2. **Cluster Aanmaken:**
    * **Via Webinterface:** Ga naar **Datacenter** -> **Cluster** en klik op **Create Cluster**. Vul een clusternaam in en selecteer de juiste netwerkinterface.
    * **Via Terminal / SSH:**
      ```bash
      pvecm create CLUSTERNAME
      ```
      Controleer de status van het nieuwe cluster met:
      ```bash
      pvecm status
      ```

3. **Toevoegen van een Computer (Node):**
    * **Via Webinterface:** Log in op de webinterface van de nieuwe node (in een nieuw tabblad). Ga naar **Datacenter** -> **Cluster** en klik op **Join Information**. Klik op **Copy Information**. Ga terug naar de webinterface van de hoofdnode, navigeer naar **Datacenter** -> **Cluster** en klik op **Join Cluster**. Plak de tekst in het veld **Information**. Zodra je op de Join-knop drukt, start het koppelproces. Na afloop moet je mogelijk opnieuw inloggen op de webinterface.
    * **Via Terminal / SSH:**
      ```bash
      pvecm add IP-ADRES-HOOFDNODE
      ```
      Controleer de status via:
      ```bash
      pvecm status
      ```
      Voor een overzicht van alle gekoppelde nodes gebruik je:
      ```bash
      pvecm nodes
      ```

## Opslagarchitectuur (Storage Design)

De centrale opslag draait op de **Lenovo M720q (Node 1)** binnen een TrueNAS omgeving. De fysieke opslag is direct verbonden met deze node. Het versturen van opslagverkeer tussen verschillende fysieke computers wordt zo voorkomen, wat vertragingen beperkt.

Je voegt een HDD toe door in de Proxmox webinterface naar de betreffende VM of LXC te navigeren. Ga naar **Hardware** -> **Add** en kies de gewenste schijf. Zodra je de container of VM start, is de harde schijf direct beschikbaar.

## Netwerk- & Service-overzicht

```text
[ Home Modem ]
      │
[ Router (TP-Link) ] ── (Wi-Fi & LAN)
      │
[ Router (GL.iNet) ]
      ├── AdGuard Home (LXC)
      └── Tailscale Router (LXC)
```

## Toekomstplannen & Roadmap

* Instellen van geautomatiseerde back-ups via Proxmox Backup Server (PBS).
* Implementeren van High Availability (HA) voor kritieke LXC-containers.