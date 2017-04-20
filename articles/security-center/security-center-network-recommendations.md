<properties
   pageTitle="Beveiligen van uw netwerk in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document adressen aanbevelingen in Azure Beveiligingscentrum die u helpen beveiligen uw Azure netwerk en het effectief overeenkomstig de beleidsregels van beveiligingsbeleid voor apparaten."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-network-in-azure-security-center"></a>Beveiligen van uw netwerk in Azure Beveiligingscentrum

Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw Azure resources. Wanneer Beveiligingscentrum worden aangegeven mogelijke beveiligingsrisico's, wordt u begeleid bij het proces van het configureren van de benodigde besturingselementen aanbevelingen.  Aanbevelingen hebben betrekking op Azure resourcetypen: virtuele machines (VMs), netwerken, SQL en toepassingen.

In dit artikel biedt aanbevelingen die van toepassing op uw netwerk.  Netwerkcentrum aanbevelingen rond de volgende generatie firewalls, netwerk-beveiligingsgroepen, configureren regels voor binnenkomende verkeer en meer.  De volgende tabel als een overzicht gebruiken om u te helpen u meer informatie over de beschikbare netwerkhulpprogramma aanbevelingen en wat elke doen als u deze hebt toegepast.

## <a name="available-network-recommendations"></a>Beschikbare netwerkhulpprogramma aanbevelingen

|Aanbeveling|Beschrijving|
|-----|-----|
|[Een volgende generatie Firewall toevoegen](security-center-add-next-generation-firewall.md)|Wordt aanbevolen dat u een volgende generatie Firewall (NGFW) uit een Microsoft-partner toevoegen om te kunnen vergroten uw beveiligingsinstellingen.|
|[Route verkeer via NGFW alleen](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Wordt aanbevolen dat u netwerk groep (NSG) regels voor binnenkomende verkeer naar uw VM via uw NGFW afdwingen configureren.|
|[Beveiligingsgroepen netwerk inschakelen voor subnetten of een virtuele machines](security-center-enable-network-security-groups.md)|Aanbevolen NSGs op subnetten of VMs in te schakelen.|
|[Toegang via Internet die tegenover elkaar liggen eindpunt beperken](security-center-restrict-access-through-internet-facing-endpoints.md)|Wordt aanbevolen dat u regels voor binnenkomende verkeer voor NSGs configureren.|

## <a name="see-also"></a>Zie ook

Zie de volgende onderwerpen voor meer informatie over aanbevelingen die van toepassing op andere resourcetypen Azure:

- [Beveiligen van uw virtuele machines in Azure Beveiligingscentrum](security-center-virtual-machine-recommendations.md)
- [Beveiligen van uw toepassingen in Azure Beveiligingscentrum](security-center-application-recommendations.md)
- [De SQL Azure-service in Azure Beveiligingscentrum beveiligen](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
