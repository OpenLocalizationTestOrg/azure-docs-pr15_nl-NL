<properties
    pageTitle="Uw abonnement op Azure overschakelen naar een ander voorstel | Microsoft Azure"
    description="Meer informatie over hoe u uw abonnement op Azure wijzigen en overschakelen naar de verschillende aanbiedingen met behulp van het abonnement management portal"
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="genli"/>

# <a name="switch-your-azure-subscription-to-another-offer"></a>Uw abonnement op Azure overschakelen naar een andere aanbieding

Als een klant [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) , is het mogelijk om te schakelen van uw Azure abonnement naar een ander voorstel in het [Midden van de Account](https://account.windowsazure.com/Subscriptions). Bijvoorbeeld, kunt u deze functie gebruiken om te profiteren van de [maandelijkse tegoeden voor abonnees met Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Als u [Gratis proefversie](https://azure.microsoft.com/free/)bent, leert u hoe u een [upgrade uitvoeren naar Pay-As-You-Go](billing-upgrade-azure-subscription.md).

#### <a name="whats-supported"></a>Wat wordt ondersteund:

| Van                                                              | Aan                                                                                      |
|-------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Pay-As-You-Go ontwikkelaar/testen](https://azure.microsoft.com/offers/ms-azr-0023p/)              |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)          |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)     |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [MSDN-Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/)                      |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)            |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Enterprise (Bizspark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |

> [AZURE.NOTE] Bieden wijzigingen, [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)voor andere.
    
## <a name="switch-subscription-offer"></a>Schakeloptie abonnement aanbieding

> [AZURE.VIDEO switch-to-a-different-azure-offer]

1.  Meld u aan bij het [Beheercentrum van Azure-Account](https://account.windowsazure.com/Subscriptions).

2.  Selecteer uw Pay-As-You-Go abonnement.

3.  Klik op **overschakelen naar een andere aanbieding**. De knop is alleen beschikbaar als u zich op Pay-As-You-Go en gereed met uw eerste betalingsperiode.

    ![Zoals u ziet de knop schakelen aanbieding aan de rechterkant van de pagina](./media/billing-how-to-switch-azure-offer/switchbutton.png)
    
4.  **Selecteer de gewenste aanbieding** in de lijst met aanbiedingen die uw abonnement kan worden verplaatst naar. Deze lijst afhankelijk van de lidmaatschappen dat uw account is gekoppeld aan. Als er niets beschikbaar is, raadpleegt u de [lijst met beschikbare aanbiedingen die u naar overschakelen kunt](#whats-supported) en te controleren of u de juiste lidmaatschappen. 

    ![Selecteer een aanbieding die u overschakelen wilt naar](./media/billing-how-to-switch-azure-offer/selectoffer.png)

5.  Afhankelijk van de aanbieding waarnaar u overstapt, ziet u mogelijk een opmerking over de gevolgen van het overstappen. Ga door deze lijst zorgvuldig en volg de instructies voordat u verdergaat.

    ![Notities bekijken](./media/billing-how-to-switch-azure-offer/thingstonote.png)

6.  U kunt uw abonnement wijzigen. Standaard Stel we deze in op de nieuwe naam voor de aanbieding. Klik op de **Schakeloptie bieden** om het proces te voltooien.

    ![Klik op de groene knop](./media/billing-how-to-switch-azure-offer/confirmpage.png)

7.  Succes! Uw abonnement wordt nu gewijzigd in de nieuwe aanbieding.

## <a name="why-cant-i-switch-offers"></a>Waarom kan niet ik aanbiedingen veranderen?

**Overschakelen naar een ander voorstel** kan niet worden weergegeven als:

- U bent niet op [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/). Momenteel alleen Pay-As-You-Go abonnementen kunnen worden verplaatst naar een andere aanbieding.

    - Als u [Gratis proefversie](https://azure.microsoft.com/free/)bent, leert u hoe u een [upgrade uitvoeren naar Pay-As-You-Go](billing-upgrade-azure-subscription.md).

    - U wilt overschakelen aanbieding uit een ander abonnement, [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- U bent nog steeds op uw eerste factureringsperiode; u moet uw eerste factureringsperiode beëindigen voordat u aanbiedingen overstappen kunt wachten.

U kunt **Er zijn geen voorstellen beschikbaar in uw regio of land op dit moment** kan optreden als:

- U bent niet in aanmerking komen voor elke aanbieding schakelopties. Controleer de [lijst met beschikbare aanbiedingen die u kunt overstappen](#whats-supported).

## <a name="what-does-switching-azure-offers-do-to-my-service-and-billing"></a>Wat betekent het overstappen van Azure aanbiedingen Ga naar Mijn service en facturering?

Dit zijn de details van wat er gebeurt wanneer u overstapt van Azure abonnementen in het midden van het Account.

### <a name="access-to-services"></a>Toegang tot services

Er is geen downtime service voor gebruikers die zijn gekoppeld aan het abonnement. De aanbieding die u naar de overschakelen kan echter beperkingen hebben. Sommige aanbiedingen verboden bijvoorbeeld gebruik in productieomgeving, dus u moet productieresources verplaatsen naar een ander abonnement.

### <a name="billing"></a>Facturering

Een factuur wordt op de dag die u wilt schakelen, gegenereerd voor alle uitstaande kosten. Uw abonnement is vervolgens gefactureerd per prijzen termen van de nieuwe aanbieding. De betalingsdatum van uw abonnement wordt gewijzigd in de datum waarop u aanbiedingen hebt gewijzigd. Gebruik en facturering gegevens voordat de aanbieding wijziging niet behouden blijft, zodat het is raadzaam dat u een kopie voordat van abonnement downloaden.

> [AZURE.NOTE] Vanwege beperkingen facturering-gerelateerde zijn aanbieding schakelopties niet mogelijk in de eerste factureringscyclus na het maken van een abonnement.

## <a name="can-i-migrate-from-pay-as-you-go-to-cloud-solution-providerhttpspartnermicrosoftcomsolutionscloud-reseller-overview-csp-or-enterprise-agreementhttpsazuremicrosoftcompricingenterprise-agreement-ea"></a>Kan ik migreren van Pay-As-You-Go naar [Cloud oplossingsprovider](https://partner.microsoft.com/Solutions/cloud-reseller-overview) (CSP) of [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (EA)?

Wordt het overschakelen van de aanbieding naar CSP of EA momenteel niet ondersteund in het midden-Accounts. Als u wilt uw bestaande abonnement naar EA verplaatst, hebt u de registratie-beheerder uw account toevoegen in de EA. Ontvangt u een uitnodiging voor de e-mailbericht. Wanneer u de instructies om de uitnodiging te accepteren, wordt uw abonnementen worden automatisch verplaatst onder de Enterprise Agreement. Als u wilt migreren naar CSP, raadpleegt u [Azure abonnement migratie naar CSP](https://blogs.technet.microsoft.com/hybridcloudbp/2016/08/26/azure-subscription-migration-to-csp/).

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u de [van beheerdersrollen beheren](billing-add-change-azure-subscription-administrator.md) voor uw abonnement

- Uw gebruik bijhouden door gegevens over het gebruik en de factuur te [downloaden](billing-download-azure-invoice-daily-usage-date.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning.

Als u nog verdere vragen hebben, neemt [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om het probleem opgelost snel.
