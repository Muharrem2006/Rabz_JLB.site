Op deze pagina leg ik de opzet van mijn homelab-cluster uit, inclusief de hardwarecomponenten, netwerkapparatuur en de maatwerkoplossingen voor voeding en opslag.
## Cluster specificaties

**Computers:**

* Lenovo ThinkCentre M920q:

    * **CPU**: Intel Core i5-8700T
    * **RAM**: 8 GiB (2666 MHz)
    * **Opslag**: 256 GiB SSD
    * **Netwerk**: 1 Gbps LAN

* Lenovo ThinkCentre M720q (Node 1):

    * **CPU**: Intel Core i5-8500T
    * **RAM**: 16 GiB (2666 MHz)
    * **Opslag**: 256 GiB SSD
    * **Netwerk**: 1 Gbps LAN

* Lenovo ThinkCentre M720q (Node 2):

    * **CPU**: Intel Core i5-8500T
    * **RAM**: 8 GiB (2666 MHz)
    * **Opslag**: 256 GiB SSD
    * **Netwerk**: 1 Gbps LAN

**Netwerk & Opslag**

* **Switch**: TP-Link TL-SG105-M2 (2.5 Gbps)
* **Router**: GL.iNet GL-BE3600
* **Mini-NAS**: 2x 2 TB Toshiba P300 (3.5" HDDs)

## Hardware Modifications & Details

* **Centrale Voeding (800W):** Om een overvloed aan losse adapters (power bricks) in het stopcontact te vermijden, gebruik ik één centrale voeding van 800 Watt. Met op maat gemaakte kabels en verloopstekkers verdeel ik de stroom vanaf de positieve en negatieve klemmen direct naar de stroomingangen van de drie Lenovo Micro-PC's.

* **Custom Mini-NAS Opbouw:** De twee Toshiba-hardeschijven zitten in een externe enclosure die ruimte biedt aan 4 SATA-schijven. De schijven krijgen hun stroom via een eigen dedicated voedingsadapter. Voor de dataverbinding lopen er SATA-kabels vanuit de enclosure rechtstreeks naar een PCIe-naar-SATA uitbreidingskaart in een van de Lenovo-nodes.