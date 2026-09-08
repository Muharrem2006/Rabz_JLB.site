Dit is een Minecraft server die ik zelf host voor klasgenoten en medestudenten Informatica aan de UHasselt. Net als bij de andere applicaties gebruik ik hiervoor een Docker container. De installatie was redelijk moeilijk vanwege de nodige netwerkconfiguraties en geheugenallocatie.
## Container Specificaties
De Docker container draait binnen een LXC-container op Proxmox (oorspronkelijk gestart als VM, maar later omgezet voor betere prestaties). Om meerdere spelers tegelijk én een hele reeks mods te ondersteunen, zijn de systeemeisen relatief hoog ingesteld:

* **CPU:** 4 Cores 
* **RAM:** 10 GiB
* **Opslag:** 57 GiB
* **Netwerk:** Statische IP-adres (noodzakelijk voor Port-forwarding; zie [PortForwarding](../infrastructure/networking.md))

## Installatie

De basisinstallatie is uitgevoerd via [Minecraft server on docker](https://docker-minecraft-server.readthedocs.io/en/latest/).

De `docker_container.yalm` bestand van mijn server ziet er als volgt uit:
```bash
services:
  mc:
    image: itzg/minecraft-server:java21 # Vaste Java-versie om problemen met modpacks te voorkomen
    pull_policy: daily
    tty: true
    stdin_open: true

    ports:
      - "25565:25565"     # Standaard Minecraft multiplayer
      - "24454:24454/udp" # Simple Voice Chat mod
      - "19132:19132/udp" # Bedrock-ondersteuning (Geyser)

    environment:
      ONLINE_MODE: "TRUE"
      VERSION: "1.21.1"   # Vaste Minecraft-versie voor mod-compatibiliteit
      EULA: "TRUE"

      MEMORY: 8G
      MOTD: "Welcome to the UHasselt Informatica server gehost door Muharrem ;) " # Welkomstbericht in de serverlijst
      MAX_PLAYERS: 40
      DIFFICULTY: "hard"
      OPS: "Rabz_JLB"      # In-game admin

      # Server-side weergave en simulatie
      VIEW_DISTANCE: 10
      SIMULATION_DISTANCE: 6

      # Energiebesparing wanneer er niemand speelt
      PAUSE_WHEN_EMPTY_SECONDS: 60

      MODRINTH_ALLOWED_VERSION_TYPE: "alpha"

      # Fabric loader inschakelen voor mod-ondersteuning
      TYPE: "FABRIC"
      # Automatische download en updates van mods via Modrinth bij het opstarten
      MODRINTH_PROJECTS: |
        fabric-api 
        lithium
        spark
        ferrite-core
        servercore
        chunky
        krypton
        simple-voice-chat
        anti-xray
        geyser
        floodgate
        cloth-config
        balm
        yungs-api
        yungs-better-dungeons
        yungs-better-strongholds
        yungs-better-mineshafts
        yungs-better-ocean-monuments
        yungs-better-nether-fortresses
        yungs-better-witch-huts
        yungs-better-desert-temples
        yungs-better-jungle-temples
        yungs-better-end-island
        yungs-extras
        c2me-fabric
        netherportalfix
        fallingtree
        dynamic-lights
        boat-break-fix
        terralith
        lithostitched
        invview

      # Geheugenoptimalisatie en Garbage Collection vlaggen (G1GC)
      JVM_OPTS: "-XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1HeapWastePercent=5 -XX:G1MixedGCLiveThresholdPercent=35 -XX:G1MaxNewSizePercent=20 -XX:G1NewSizePercent=10 -XX:G1HeapRegionSize=4M"
    volumes:
      # Koppel de lokale 'data' map aan de container
      - ./data:/data
    restart: unless-stopped
```

Start de server wel op, maar is hij niet bereikbaar voor anderen? Controleer dan de port-forwarding op je router en verifieer dat de container-poorten correct zijn doorgeschakeld dit is een veelvoorkomend struikelblok.

<img src="../assets/minecraft.png" alt="Minecraft" width="50"/>