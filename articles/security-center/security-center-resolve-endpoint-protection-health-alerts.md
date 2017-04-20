<properties
   pageTitle="Eindpunt beveiliging systeemstatus waarschuwingen in Azure Beveiligingscentrum oplossen | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de aanbeveling Azure Beveiligingscentrum **oplossen Endpoint Protection systeemstatus waarschuwingen**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Eindpunt beveiliging systeemstatus waarschuwingen in Azure Beveiligingscentrum oplossen

Azure Beveiligingscentrum wordt aangeraden dat u gevonden endpoint protection systeemstatus waarschuwingen oplossen.  Beveiligingscentrum kunt u zien welke virtuele machines (VMs) eindpunt beveiliging fouten en hoeveel fouten hebben.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie. Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in de **aanbevelingen blade**, **oplossen Endpoint Protection systeemstatus waarschuwingen**.
![Endpoint protection systeemstatus waarschuwingen oplossen][1]

2. Hiermee opent u het blad **Endpoint Protection mislukt** waarin VMs met fouten en het aantal ongunstige uitkomsten voor elke VM. Selecteer een VM in de lijst.
![Endpoint protection is mislukt][2]

3. Een **Lijst met fouten** blade wordt geopend voor de geselecteerde VM, het weergeven van een lijst met fouten. Selecteer een fout in de lijst voor meer informatie. Hiermee wordt een blade geopend met informatie over de geselecteerde mislukte.
![Lijst met fouten][3]
  ![mislukt gebeurtenis][4]

## <a name="see-also"></a>Zie ook

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md)--meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging in Azure Beveiligingscentrum beheren](security-center-recommendations.md)--Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging servicestatus bewaken in Azure Beveiligingscentrum](security-center-monitoring.md)--informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md)--meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md)--zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/)--het laatste nieuws van Azure beveiliging en informatie.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
