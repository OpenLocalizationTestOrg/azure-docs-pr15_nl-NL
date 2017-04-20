<properties
   pageTitle="Testen van uw voorstel gegevensservice Marketplace | Microsoft Azure"
   description="Meer informatie over het testen van uw voorstel gegevensservice Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="testing-your-data-service-offer-in-staging"></a>Uw aanbod gegevensservice testen in tijdelijke

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding eventuele nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet ophalen goedgekeurd voor de aanbieding.** Als u een SaaS bedrijfstoepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u een IaaS-toepassingen hebt of ontwikkelaars-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Na het voltooien van de eerste twee stappen van [de ontwikkelaar van de Microsoft-account maken](marketplace-publishing-accounts-creation-registration.md) en het [maken van uw gegevens Service bieden in de Portal voor publiceren](marketplace-publishing-data-service-creation.md) bent u gereed is voor het maken van uw voorstel beschikbaar in de Azure Marketplace. In dit onderwerp wordt u begeleid de eerste, intermediate, stap 'Tijdelijke' genoemd

Tijdelijke betekent dat uw voorstel in een privé "sandbox" waar u kunt testen en de functionaliteit verifiëren voordat het uploaden daarvan naar productie implementeren. De aanbieding wordt weergegeven in de tijdelijke net zoals u deze naar een klant die deze heeft geïmplementeerd zou doen.

## <a name="step-1-pushing-your-offer-to-staging"></a>Stap 1. Zet uw aanbieding wilt tijdelijke nieuwe
Zet uw aanbieding wilt tijdelijke nieuwe, kunt u de aanbieding testen voordat deze verandert in beschikbaar voor toekomstige abonnees.  U kunt zien hoe uw aanbieding wordt weergegeven en, functie voor gebruikers met computers abonneren op uw gegevens.  

  ![tekenen](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  Aanmelden bij de [Portal publiceren](https://publish.windowsazure.com)
2.  Selecteer **Gegevensservices** in het linker navigatiegedeelte-venster
3.  Selecteer uw aanbod die u wilt voeren naar tijdelijke. Hier ziet u de bovenstaande scherm.
4.  Klik op de knop **Tijdelijke Push** .  
5.  Als er problemen met de aanbieding die worden voltooid moeten voordat het drukken op tijdelijke, ziet u een lijst die wordt weergegeven.  Corrigeer deze items door te klikken op elk item in de lijst. Wanneer alle correcties hebt aangebracht, klikt u nogmaals op knop **Push naar tijdelijke** .

Als er geen problemen met uw aanbod zijn ziet u de onderstaande pop-upvenster.  

Als u bent niet planning/niet goedgekeurd om het oppervlak van uw voorstel in Azure-Portal (momenteel biedt beperkte capaciteit), sluit u gewoon het pop-upvenster.

Als u wilt testen uw gegevensservice in Azure-Portal (naast de portal DataMarket), moet u een Azure abonnements-ID om te testen met.  Deze abonnements-ID wordt het account dat is toegestaan om te testen van uw voorstel identificeren.  

Knippen en plakken van uw abonnements-ID en klik op het vinkje om door te gaan.

  ![tekenen](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] Deze Azure abonnementen id's zijn alleen vereist voor het testen en waarin u tijdelijk opslaat in de [Beheerportal van Azure](https://manage.windowsazure.com). Ze moeten zijn niet testen in Azure Marketplace.

Het volgende scherm ziet u dat publicatie plaatsvindt door middel van het pictogram "Bezig" geel onderstaande gemarkeerd. Zet nieuwe naar tijdelijke gaat tussen 10 tot 15 minuten.  Als het duurt langer, vernieuwt u eerst uw browser (druk op F5 in IE).  Klik op de koppeling naar contact in de uitzonderlijke gevallen waarin uw aanbod is nog steeds zet nieuwe naar tijdelijke na een uur, laat het ons weten dat er een probleem.

  ![tekenen](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Wanneer de Push naar gefaseerde installatie is voltooid het pictogram "Bezig" wordt stoppen verplaatsen en de status bijgewerkt naar "Gefaseerde".  U bent nu klaar om te testen van uw aanbod.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Stap 2. De gefaseerde aanbieding testen in DataMarket

Klik op de koppeling volgen van de tekst **"Zien uw service bieden bij..."** naar het scherm dat de abonnee ziet wanneer uw aanbod Hiermee gaat u naar productie en wordt weergegeven in DataMarket weergeven.

  ![tekenen](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Test of Controleer of elk van de 12 items hierboven hebt gemarkeerd, zodat alle logo's, prijzen/transacties, tekst, afbeeldingen, documentatie en koppelingen juist zijn en werkt goed.  Dit is een goed moment om te zorgen dat u hebt ingevoerd bij het maken van uw voorstel testwaarden zijn vervangen door werkelijke waarden.

1. Aanbieding-logo
2. De naam van de aanbieding
3. Publisher naam/koppeling naar de website van uw bedrijf
4. Zoekcategorieën voor uw aanbieding
5. Van uw voorstel ondersteuningskoppeling om u te helpen abonnees
6. Contextuele beschrijving voor de aanbieding
7. Plan aanduiding Factureringsdetails bieden
8. Koppeling naar de implementatiecode
9. Voorbeeldafbeeldingen die illustreren gebruik van gegevens van de aanbieding
10. Invoer/uitvoer metagegevens voor elke service binnen de aanbieding
11. Gebruiksvoorwaarden voor de aanbieding
12. Voorbeeld van de gegevens van de aanbieding


Controleer ten slotte dat de service werken via het Datamarket door te klikken op de koppeling "Deze GEGEVENSSET VERKENNEN".  Een nieuw venster wordt geopend in het hulpmiddel verwijst naar de "Service Explorer" zodat u de resultaten van een query ten opzichte van uw service kunt bekijken.  In dit venster kunt u de parameters invoeren en de resultaten weergegeven door een query op uw service weer te geven.   Ook is weergegeven de URL voor uw Query.  

> [AZURE.NOTE] Zorg ervoor dat moet worden gereviseerd de beschrijving van de service boven weergegeven.  En als wordt uw aanbod bestaat uit meer dan één service bellen, klikt u op de tabbladen aan de onderkant om te schakelen naar de volgende service om te controleren en testen.



## <a name="next-step"></a>Volgende stap
Als u problemen ondervindt en voor hulp bij het oplossen van deze Neem contact op met [Ondersteuning van Azure Publisher]( http://go.microsoft.com/fwlink/?LinkId=272975).

Als u tevreden en klaar voor publiceren bevat uw aanbod Lees de documentatie [Goedkeuring aanvragen voor Push naar productie](marketplace-publishing-push-to-production.md) .

## <a name="see-also"></a>Zie ook
- [Aan de slag: Hoe u een aanbieding publiceren naar de Azure Marketplace](marketplace-publishing-getting-started.md)
