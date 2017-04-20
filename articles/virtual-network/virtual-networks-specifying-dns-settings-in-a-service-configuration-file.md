<properties 
   pageTitle="DNS-instellingen op te geven in een configuratiebestand service | Microsoft Azure"
   description="aangepaste DNS-instellingen met behulp van service-configuratiebestand voor virtueel netwerk opgeven"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>DNS-instellingen op te geven in een configuratiebestand Service

## <a name="dns-elements"></a>DNS-elementen

Een configuratiebestand service kan een DnsServers-element met een lijst met IPv4-adressen voor de Domain Name System (DNS)-servers die worden gebruikt door de service bevatten. Instellingen in het configuratiebestand service heeft voorrang op instellingen in het configuratiebestand netwerk. Zie [Azure Service configuratieschema (.cscfg bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx)voor meer informatie.

**NetworkConfiguration-element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] Het kenmerk **name** in het **DNS-server** -element wordt gebruikt alleen als de verwijzingsnaam van een. Dit geeft geen de hostnaam voor de DNS-server. Elke **DNS-server** kenmerkwaarde moet uniek zijn in het hele Microsoft Azure-abonnement.

## <a name="see-also"></a>Zie ook

[Azure-Service configuratieschema (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure Virtual Network configuratieschema](http://go.microsoft.com/fwlink/?LinkId=248093)

[Een virtueel netwerk netwerk configuratiebestanden gebruiken configureren](http://go.microsoft.com/fwlink/?LinkId=248094)

[Over Virtual Network-instellingen in de beheerportal](http://go.microsoft.com/fwlink/?LinkId=248092)

