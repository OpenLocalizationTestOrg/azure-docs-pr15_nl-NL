<properties
   pageTitle="Contactgegevens in Azure Beveiligingscentrum beveiliging bieden | Microsoft Azure"
   description="In dit document ziet u hoe u contactgegevens in Azure Beveiligingscentrum beveiliging bieden."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="provide-security-contact-details-in-azure-security-center"></a>Contactgegevens in Azure Beveiligingscentrum beveiliging bieden

Azure Beveiligingscentrum wordt aangeraden dat u beveiliging contactgegevens voor uw Azure-abonnement opgeeft als u nog niet is gedaan. Deze gegevens worden gebruikt door Microsoft contact met u kunnen als de MSRC Microsoft Security antwoord Center () ontdekt dat uw gegevens van de klant is geopend door een feest onrechtmatige of door onbevoegden. MSRC voert selecteert u beveiliging cmdlets voor controle van de Azure netwerk en de infrastructuur en bedreiging intelligence en abuse klachten van derden ontvangt.

Een e-mailbericht is verzonden op het eerste dagelijkse exemplaar van een melding en alleen voor hoge prioriteit waarschuwingen. E-mailvoorkeuren kunnen alleen worden geconfigureerd voor beleid voor het abonnement. Resourcegroepen binnen een abonnement nemen over deze instellingen.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in het blad **aanbevelingen** **opgeven beveiliging contactgegevens**.
![Beveiliging contactpersoon opgeven][1]

2. Hiermee opent u het blad **opgeven beveiliging contactgegevens**. Selecteer het Azure abonnement voor contactgegevens op.
![Contactgegevens beveiliging bieden][2]

3. Een tweede **contactgegevens beveiliging bieden** blade wordt geopend.

  - Voer de beveiliging e-mailadres contactpersoon of adressen gescheiden door komma's. Er is een beperking voor het aantal e-mailadressen die u kunt invoeren.
  - Voer één beveiliging internationaal telefoonnummer.
  - E-mailberichten over hoge prioriteit waarschuwingen ontvangen, schakelt u de optie **Stuur me een e-mail over waarschuwingen**.
  - U moet in de toekomst, de optie voor het verzenden van e-mailmeldingen naar abonnement eigenaren. Deze optie momenteel niet beschikbaar is.
  - Klik op **OK** de contactgegevens van beveiliging toepassen op uw abonnement.

## <a name="see-also"></a>Zie ook

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --het laatste nieuws van Azure beveiliging en informatie.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
