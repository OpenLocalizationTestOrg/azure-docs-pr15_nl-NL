<properties
   pageTitle="Overzicht van Azure DNS | Microsoft Azure"
   description="Overzicht van de DNS-hostingprovider services op Microsoft Azure Azure. Uw domein op Microsoft Azure hosten."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="azure-dns-overview"></a>Azure DNS-overzicht


Het Domain Name System of de DNS-, is verantwoordelijk voor het vertalen van (of het oplossen van) de naam van een website of service het IP-adres. Azure DNS is een hostingservice voor DNS-domeinen, mailnamen omzetten met Microsoft Azure-infrastructuur leveren. Door uw domeinen in Azure host, kunt u uw DNS-records met dezelfde referenties, API's, hulpmiddelen en facturering als uw andere Azure services beheren.


DNS-domeinen in Azure DNS worden gehost op een van de Azure wereldwijd netwerk van DNS-naamservers. We gebruiken Anycast netwerkproblemen zodat elke DNS-query wordt beantwoord door de dichtstbijzijnde beschikbare DNS-server. Hier vindt u zowel snelle prestaties en beschikbaarheid voor uw domein.

De DNS-Azure-service is gebaseerd op Azure Resource Manager (ARM). Zo kunt profiteren van ARM functies zoals Rolgebaseerd toegangsbeheer, controlelogboeken bijhouden en resource vergrendelen. Uw domeinen en records kunnen worden beheerd via de portal van Azure, Azure PowerShell-cmdlets en de platforms Azure CLI. Toepassingen waarbij automatische DNS-beheer kunnen integreren met de service via de REST API en SDK's.

Azure DNS ondersteunt geen momenteel kopen van domeinnamen. Als u aanschaffen domeinen wilt, moet u een derde partij domeinnaamregistrar gebruiken. De domeinregistrar, worden meestal kosten een kleine jaarlijkse. De domeinen kunnen vervolgens worden gehost in Azure DNS voor het beheer van DNS-records. Zie [gemachtigde een domein aan Azure DNS](dns-domain-delegation.md) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

[Een DNS-zone maken](dns-getstarted-create-dnszone-portal.md)





