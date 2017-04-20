<properties
   pageTitle="Dynamische DNS gebruiken om u te registreren hostnamen"
   description="Deze pagina kunt meer informatie over het instellen van dynamische DNS hostnamen registreren in uw eigen DNS-servers."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Dynamische DNS hostnamen registreren in uw eigen DNS-server gebruiken

[Azure biedt naamresolutie](virtual-networks-name-resolution-for-vms-and-role-instances.md) voor virtuele machines (VMs) en rol exemplaren. Wanneer uw naamresolutie nodig heeft dan de verstrekt door Azure, kunt u uw eigen DNS-servers opgeeft. Hiermee kunt u uw DNS-oplossing aan uw eigen specifieke behoeften aanpassen. Bijvoorbeeld, moet u toegang tot lokale bronnen via uw Active Directory-domeincontroller.

Wanneer uw aangepaste DNS-servers worden gehost als Azure VMs kunt u hostname query's voor de dezelfde vnet doorsturen naar Azure om op te lossen hostnamen. Als u niet deze route gebruikt wilt, kunt u uw hostnamen VM registreren in uw DNS-server met dynamische DNS.  Azure heeft de mogelijkheid (bijvoorbeeld referenties) geen rechtstreeks om records te maken in uw DNS-servers, zodat alternatieve rangschikkingen vaak nodig zijn. Hier volgen enkele veelvoorkomende scenario's met alternatieven.

## <a name="windows-clients"></a>Windows-clients

Windows-clients niet tot een domein behoren proberen onbeveiligde dynamische DNS (DDNS) updates wanneer ze opstart of als hun IP-adres wordt gewijzigd. De naam van de DNS-is de hostnaam plus het primaire achtervoegsel. Azure het primaire achtervoegsel leeg laat, maar u kunt dit instellen in de VM, via de [gebruikersinterface](https://technet.microsoft.com/library/cc794784.aspx) of [met behulp van automatisering](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Windows-clients domein behoren registreren hun IP-adressen met de domeincontroller met behulp van secure dynamische DNS. Het proces van het domein-join Hiermee stelt u het primaire achtervoegsel op de client en gemaakt en onderhouden van de vertrouwensrelatie.

## <a name="linux-clients"></a>Linux-clients

Linux-clients in het algemeen niet geregistreerd met de DNS-server bij het opstarten, ze wordt ervan uitgegaan dat de DHCP-server moet. Van Azure DHCP-servers beschikt niet over de mogelijkheid of referenties om records te registreren in uw DNS-server.  Een hulpmiddel *nsupdate*, die is opgenomen in het pakket binding, kunt u dynamische DNS-updates verzenden. Omdat het dynamische DNS-protocol is gestandaardiseerd, kunt u *nsupdate* gebruiken, zelfs wanneer u niet afhankelijk van de DNS-server gebruikt.

U kunt de haken die worden verstrekt door de DHCP-client maken en beheren van de vermelding hostname in de DNS-server. De client Hiermee tijdens de cyclus DHCP voert u de scripts in */etc/dhcp/dhclient-exit-hooks.d/*. Dit kan worden gebruikt om u te registreren van het nieuwe IP-adres met behulp van *nsupdate*. Bijvoorbeeld:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

U kunt ook de opdracht *nsupdate* gebruiken om uit te voeren beveiligde dynamische DNS-updates. Bijvoorbeeld wanneer u een binden DNS-server gebruikt, is een combinatie van een openbare en persoonlijke sleutel [gegenereerd](http://linux.yyz.us/nsupdate/).  De DNS-server is [geconfigureerd](http://linux.yyz.us/dns/ddns-server.html) met de openbare deel van de sleutel zodat de handtekening in het verzoek kan worden gecontroleerd. U moet de optie *-k* gebruiken op te geven op dat de toets-twee *nsupdate* om de dynamische DNS-records bijwerken aanvraag moet worden ondertekend.

Als u een Windows-DNS-server gebruikt, kunt u Kerberos-verificatie gebruiken met de parameter *-g* in *nsupdate* (niet beschikbaar in de Windows-versie van *nsupdate*). Gebruik hiervoor *kinit* laden van de referenties (bijvoorbeeld via een [keytab-bestand](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). Vervolgens gewoon *nsupdate -g* door de referenties uit de cache.

Indien nodig kunt u een zoekopdracht DNS-achtervoegsel toevoegen aan uw VMs. Het achtervoegsel is opgegeven in het bestand */etc/resolv.conf* . De meeste Linux distros automatisch beheer de inhoud van dit bestand, dus meestal niet worden bewerkt. U kunt echter het achtervoegsel overschrijven met behulp van de opdracht *vervangen* van de DHCP-client. Klik hiervoor in */etc/dhcp/dhclient.conf*toevoegen:

        supersede domain-name <required-dns-suffix>;

