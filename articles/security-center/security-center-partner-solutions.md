<properties
   pageTitle="Partneroplossingen in Azure Beveiligingscentrum beheren | Microsoft Azure"
   description="Dit document leert u hoe Azure Beveiligingscentrum kunt u monitor in één oogopslag de status van uw partneroplossingen geïntegreerd met uw Azure-abonnement."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Partneroplossingen met Azure Beveiligingscentrum bewaken

In dit document begeleidt u bij het controleren van de status van uw partneroplossingen in Beveiligingscentrum Azure.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie. Dit is niet een stapsgewijze handleiding.

## <a name="monitoring-partner-solutions"></a>Partneroplossingen bewaken

De tegel **partneroplossingen** op het blad **Beveiligingscentrum** kunt u monitor in één oogopslag de status van uw partneroplossingen geïntegreerd met uw Azure-abonnement.
![Partneroplossingen tegel gemarkeerd][1]

De tegel **partneroplossingen** geeft het getal van partneroplossingen en de status samenvatting voor deze oplossingen.

De **STATUS** van een partneroplossing van een kan zijn:

- Beveiligde (groen): Er is geen probleem met de servicestatus.
- Beschadigde (rood) - er is een probleem met de systeemstatus dat onmiddellijke aandacht vereist.
- Gestopt (oranje) rapportage - rapportage van de status van de oplossing is gestopt.
- Onbekend beveiliging status (oranje) - de status van de oplossing is onbekend op dit moment vanwege een mislukt proces van het toevoegen van een nieuwe resource aan de bestaande oplossing.
- Niet gemeld (grijs) - de oplossing geen is gerapporteerd nog, de status van een oplossing is mogelijk niet-aangegeven als dit alleen is aangesloten en nog steeds implementeert.

Als er geen oplossingen die zijn geïntegreerd met uw abonnement kan de tegel wordt aangegeven dat er geen oplossingen. De tegel **partneroplossingen** selecteren, kunt u het blad **aanbevelingen** als u wilt implementeren beveiligings partneroplossingen openen.
![Geen partneroplossingen][2]

De status van uw partneroplossingen weergeven:

1. Selecteer de tegel **partneroplossingen** . Een blade wordt geopend met een lijst met uw partneroplossingen verbonden met Beveiligingscentrum.
![Partneroplossingen][3]

2. Selecteer een partneroplossing. In dit voorbeeld kunt de oplossing **F5-WAF2** selecteren.  Er wordt een blade geopend met de status van de partneroplossing en van de oplossing die is gekoppeld resources. Selecteer **oplossing console** openen van de partner management ervaring voor deze oplossing.
![Partner oplossing detail][4]

3. Ga terug naar het blad **F5-WAF2** en selecteer **koppeling app**. Hiermee opent u het blad **Koppeling-toepassingen** . Hier kunt u resources verbinding maken met de partneroplossing.
![Koppeling resources naar partneroplossing][5]

## <a name="see-also"></a>Zie ook
In dit document, u zijn toegevoegd aan de tegel **Partneroplossingen** in Beveiligingscentrum. Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) , informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging in Azure Beveiligingscentrum beheren](security-center-recommendations.md) , kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging servicestatus bewaken in Azure Beveiligingscentrum](security-center-monitoring.md) , informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingsmeldingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) , informatie over het beheren en reageren op beveiligingsmeldingen.
- [Veelgestelde vragen over Azure beveiliging beheercentrum](security-center-faq.md) , zoeken Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) , het laatste nieuws van Azure beveiliging en informatie.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
