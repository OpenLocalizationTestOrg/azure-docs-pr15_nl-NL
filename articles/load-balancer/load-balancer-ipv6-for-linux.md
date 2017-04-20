<properties
    pageTitle="DHCPv6 configureren voor Linux VMs | Microsoft Azure"
    description="Het configureren van DHCPv6 voor Linux VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6, azure taakverdeling, dubbele stapel, openbare IP-, systeemeigen ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>DHCPv6 configureren voor Linux VMs

Sommige Linux VM afbeeldingen in de Azure Marketplace is geen DHCPv6 standaard zo geconfigureerd. Als u wilt ondersteuning biedt voor IPv6, moet DHCPv6 worden geconfigureerd in de verdeling Linux-besturingssysteem dat u gebruikt. Verschillende Linux onderzoeken hebben verschillende manieren van het configureren van DHCPv6 omdat ze verschillende pakketten gebruiken.

>[AZURE.NOTE] Recente SUSE Linux en CoreOS afbeeldingen in de Azure Marketplace zijn vooraf geconfigureerde met DHCPv6. Geen extra wijzigingen zijn vereist bij gebruik van deze afbeeldingen.

In dit document wordt beschreven hoe DHCPv6 inschakelen zodat uw VM Linux wordt opgehaald een IPv6-adres.

>[AZURE.WARNING] Onjuist bewerken van netwerk configuratiebestanden kan ertoe leiden dat netwerktoegang voor uw VM verloren gaat. Het is raadzaam dat u uw configuratiewijzigingen op niet-productieve systemen testen. De instructies in dit artikel is getest voor de meest recente versies van de afbeeldingen Linux in de Azure Marketplace. Raadpleeg de documentatie voor uw specifieke versie van Linux voor gedetailleerde instructies.

## <a name="ubuntu"></a>Ubuntu

1. Bewerk het bestand `/etc/dhcp/dhclient6.conf` en voeg de volgende regel toe:

        timeout 10;

2. Bewerk de netwerkconfiguratie voor de interface eth0 met de volgende configuratie:

    * Klik op **Ubuntu 12.04 en 14.04**, het bestand bewerken`/etc/network/interfaces.d/eth0.cfg`
    * Klik op **Ubuntu 16.04**, het bestand bewerken`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. IPv6-adres verlengen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Bewerk het bestand `/etc/dhcp/dhclient6.conf` en voeg de volgende regel toe:

        timeout 10;

2. Bewerk het bestand `/etc/network/interfaces` en de configuratie van de volgende toevoegen:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6-adres verlengen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Bewerk het bestand `/etc/sysconfig/network` en de volgende parameter toevoegen:

        NETWORKING_IPV6=yes

2. Bewerk het bestand `/etc/sysconfig/network-scripts/ifcfg-eth0` en de volgende twee parameters toevoegen:

        IPV6INIT=yes
        DHCPV6C=yes

3. IPv6-adres verlengen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Recente SLES en openSUSE afbeeldingen in Azure zijn vooraf geconfigureerde met DHCPv6. Geen extra wijzigingen zijn vereist bij gebruik van deze afbeeldingen. Als u een VM op basis van een ouder of aangepaste SUSE-afbeelding hebt, gebruikt u de volgende stappen uit:

1. Installeer de `dhcp-client` inpakken, indien nodig:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Bewerk het bestand `/etc/sysconfig/network/ifcfg-eth0` en de volgende parameter toevoegen:

        DHCLIENT6_MODE='managed'

3. Het IPv6-adres verlengen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 en openSUSE sprong

Recente SLES en openSUSE afbeeldingen in Azure zijn vooraf geconfigureerde met DHCPv6. Geen extra wijzigingen zijn vereist bij gebruik van deze afbeeldingen. Als u een VM op basis van een ouder of aangepaste SUSE-afbeelding hebt, gebruikt u de volgende stappen uit:

1. Bewerk het bestand `/etc/sysconfig/network/ifcfg-eth0` en vervang deze parameter

        #BOOTPROTO='dhcp4'

    met de volgende waarde:

        BOOTPROTO='dhcp'

2. De volgende parameter toevoegen `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Het IPv6-adres verlengen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Recente CoreOS afbeeldingen in Azure zijn vooraf geconfigureerde met DHCPv6. Geen extra wijzigingen zijn vereist bij gebruik van deze afbeeldingen. Als u een VM op basis van een ouder of aangepaste CoreOS-afbeelding hebt, gebruikt u de volgende stappen uit:

1. Het bestand bewerken`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Het IPv6-adres verlengen:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
