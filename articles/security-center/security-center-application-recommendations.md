<properties
   pageTitle="Beveiligen van uw toepassingen in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document adressen aanbevelingen in Azure Beveiligingscentrum die u helpen beveiligen uw Azure-toepassingen en blijf overeenkomstig de beleidsregels van beveiligingsbeleid voor apparaten."
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

# <a name="protecting-your-applications-in-azure-security-center"></a>Beveiligen van uw toepassingen in Azure Beveiligingscentrum

Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw Azure resources. Wanneer Beveiligingscentrum worden aangegeven mogelijke beveiligingsrisico's, wordt u begeleid bij het proces van het configureren van de benodigde besturingselementen aanbevelingen.  Aanbevelingen hebben betrekking op Azure resourcetypen: virtuele machines (VMs), netwerken, SQL en toepassingen.

In dit artikel biedt aanbevelingen die van toepassing op toepassingen.  Beheercentrum van de toepassing aanbevelingen rond de implementatie van een web application-firewall.  De volgende tabel als een overzicht gebruiken om u te helpen u meer informatie over de beschikbare toepassing aanbevelingen en wat elke doen als u deze hebt toegepast.

## <a name="available-application-recommendations"></a>Beschikbare toepassing aanbevelingen

|Aanbeveling|Beschrijving|
|-----|-----|
|[Een web application-firewall toevoegen](security-center-add-web-application-firewall.md)|Raadt een firewall-toepassing (WAF) voor web eindpunten. U kunt meerdere webtoepassingen in Beveiligingscentrum beveiligen door deze toepassingen toevoegen aan uw bestaande WAF-implementaties. WAF toestellen (gemaakt met het implementatiemodel resourcemanager-) moeten worden geïmplementeerd op een aparte virtueel netwerk. WAF toestellen (gemaakt met het klassieke implementatiemodel) zijn beperkt tot het gebruik van de beveiligingsgroep van een netwerk. In de toekomst wordt deze ondersteuning uitgebreid voor een volledig aangepast distributie van een toestel WAF (klassieke).|
|[Beveiliging voltooien](security-center-add-web-application-firewall.md#finalize-application-protection)|U voltooit de configuratie van een WAF, moet verkeer worden gerouteerd naar het toestel WAF. Deze aanbeveling volgen, wordt de wijzigingen nodig setup voltooid.|

## <a name="see-also"></a>Zie ook

Zie de volgende onderwerpen voor meer informatie over aanbevelingen die van toepassing op andere resourcetypen Azure:

- [Beveiligen van uw virtuele machines in Azure Beveiligingscentrum](security-center-virtual-machine-recommendations.md)
- [Beveiligen van uw netwerk in Azure Beveiligingscentrum](security-center-network-recommendations.md)
- [De SQL Azure-service in Azure Beveiligingscentrum beveiligen](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
