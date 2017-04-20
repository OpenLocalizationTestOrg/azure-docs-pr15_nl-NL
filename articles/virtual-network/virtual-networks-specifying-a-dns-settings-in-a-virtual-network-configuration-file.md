<properties 
   pageTitle="DNS-instellingen op te geven in een virtueel netwerk configuratiebestand | Microsoft Azure"
   description="Hoe u DNS-server-instellingen wijzigen in een virtueel netwerk met een virtueel netwerk configuratie-bestand in het implementatiemodel klassieke"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>DNS-instellingen op te geven in een virtueel netwerk configuratiebestand

Een netwerk configuratiebestand bestaat uit twee elementen die u gebruiken kunt om op te geven Domain Name System (DNS)-instellingen: **DnsServers** en **DnsServerRef**. U kunt een lijst met DNS-servers toevoegen door hun IP-adressen en namen van de verwijzing naar het element **DnsServers** te geven. U kunt een element **DnsServerRef** vervolgens gebruiken om op te geven welke DNS-server-items uit het element DnsServers worden gebruikt om verschillende netwerksites binnen het netwerk van uw virtuele.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke.

Het configuratiebestand netwerk mogelijk de volgende elementen bevatten. De titel van elk element wordt gekoppeld aan een pagina met aanvullende informatie over de instellingen van de waarde element.

>[AZURE.IMPORTANT] Zie [configureren van een virtuele netwerk met een netwerk configuratiebestand](virtual-networks-using-network-configuration-file.md)voor informatie over het configureren van een bestand voor de configuratie van het netwerk. Zie voor informatie over elk element in het netwerk configuratie-bestand staan, [Azure virtuele netwerk configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx).

[DNS-Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] Het kenmerk **name** in het **DNS-server** -element wordt alleen als een verwijzing gebruikt voor het element **DnsServerRef** . Dit geeft geen de hostnaam voor de DNS-server. Elke **DNS-server** kenmerkwaarde moet uniek zijn in het hele Microsoft Azure-abonnement

[Virtual Network Sites Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Als u wilt opgeven met deze instelling voor het virtuele netwerksites-element, moet deze worden eerder gedefinieerd in het DNS-element. De DnsServerRef- *naam* in het element virtuele netwerksites moet verwijzen naar een naamwaarde in het DNS-element voor *naam*van de DNS-server.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [Schema van Azure Virtual Network configuratie](http://go.microsoft.com/fwlink/?LinkId=248093).
- Meer informatie over het [Schema van Azure-Service configureren](https://msdn.microsoft.com/library/windowsazure/ee758710).
- [Een virtuele netwerk configureren netwerk configuratiebestanden gebruiken](virtual-networks-using-network-configuration-file.md).
