<properties
   pageTitle="Handleiding voor het maken van een Service van gegevens voor de Marketplace | Microsoft Azure"
   description="Uitgebreide instructies over het maken, certificeren en implementeren van een Service van de gegevens voor kopen op Azure Marketplace."
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

# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Gegevensservice publiceren van de handleiding voor het Azure Marketplace

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding eventuele nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet ophalen goedgekeurd voor de aanbieding.** Als u een SaaS bedrijfstoepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u een IaaS-toepassingen hebt of ontwikkelaars-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Na het voltooien van de stap 1, [maken en registratie](marketplace-publishing-accounts-creation-registration.md), we geholpen u bij de [niet-technische algemene](marketplace-publishing-pre-requisites.md) instellingen en [Technische vereisten](marketplace-publishing-data-service-creation-prerequisites.md) van een aanbieding gegevensservice op Azure Marketplace. Nu we u door de stappen begeleiden wordt voor het maken van een aanbieding gegevensservice op de [Portal voor publiceren] [ link-pubportal] Azure Marketplace.

## <a name="1---login-to-the-publishing-portal"></a>1. Meld u aan bij de publicatie van de Portal.

Ga naar [https://publish.windowsazure.com](https://publish.windowsazure.com. )

**Voor de eerste keer aanmelden bij de Portal voor publiceren, met die van uw bedrijf verkoper profiel is geregistreerd in Developer Center dezelfde account te gebruiken.**  (U later kunt toevoegen elke werknemer van uw bedrijf als co-beheerder in de Portal voor publiceren).

Klik op de tegel **publiceren een gegevensservices** als dit de eerste aanmelding bij de portal voor publiceren.

## <a name="2---choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2. **Gegevensservices** kiezen in het navigatiemenu aan de linkerkant.

  ![tekenen](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3---create-a-new-data-service"></a>3. een nieuwe gegevensservice maken

Vul de titel in voor uw nieuwe gegevens Service bieden en klikt u op 'en' aan de rechterkant op.

  ![tekenen](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4---review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4. de opties voor AutoAanpassen onder het zojuist gemaakte gegevensservice in het navigatiemenu controleren.

Klik op het tabblad **Stapsgewijze instructies** en Controleer alle benodigde stappen nodig zijn om te publiceren correct de gegevensservice op Azure Marketplace.

> [AZURE.TIP] U kunt altijd klikt u op de koppelingen in de pagina 'Procedure' of gebruik de tabbladen in de gegevensservice-aanbieding submenu aan de linkerkant.

## <a name="5---create-a-new-plan"></a>5. Maak een nieuw abonnement.

### <a name="offers-plans-transactions"></a>Voorstellen, plannen, transacties.

Elke aanbieding kan meerdere abonnementen, maar moet ten minste één (1)-abonnement hebt. Eindgebruikers abonneren op uw aanbod ze zich bij aanmelden voor een van de aanbieding van abonnement. Elk abonnement bepaalt hoe eindgebruikers kunnen het gebruik van uw service.

Op dit moment Azure Marketplace model alleen maandkalender abonnement transactie gebaseerd ondersteuning voor Data Services, dat wil zeggen eindgebruikers maandkalender kosten volgens de prijs van het specifieke plan dat ze geabonneerd op betalen en kunnen gebruiken van elke maand aantal transactie gedefinieerd door het abonnement.

Elke transactie gewoonlijk gedefinieerd als het aantal records aan die uw gegevensservice zullen retourneren op basis van de query die is verzonden naar de Service. De standaardinstelling is 100. Telefoonnummers van transacties die worden geretourneerd naar beide query's worden getal van de records die zijn gedeeld door 100 en afgerond op het dichtstbijzijnde gehele getal.

Het is Azure Marketplace-Service laag verantwoordelijkheid om te controleren (meter) transacties door elke query verbruikt aantal.

> [AZURE.IMPORTANT] Eindgebruikers die de transactie tijdens de maand bereikt wordt geblokkeerd voor gebruik van de service tot einde van hun maandelijkse abonnement cyclus voortzetten.

> Het abonnement of een van de abonnementen kunt (maar niet moet) bevatten onbeperkt aantal transacties.

### <a name="create-a-plan"></a>Maak een plan.
1. Klik op **'en'** naast de 'toevoegen een nieuw abonnement'.

2. Kies een van de opties: **onbeperkt** of **beperkt** gebruik voor dit abonnement.  Als beperkt Geef het aantal transactie kunt het abonnement gebruiken in een maand.

    ![tekenen](media/marketplace-publishing-data-service-creation/step-5.1.png)  

    Portal publiceren, wordt 'Plan id", die wordt gebruikt om te communiceren voor de eindgebruikers de naam van het abonnement in de gebruikersinterface en ook worden gebruikt door de Service markt identificeren van het abonnement ook gevraagd. Als u wilt, kunt u de "plannen id" wijzigen.

    > [AZURE.NOTE] De "plannen id" moet uniek zijn binnen het bereik van elke aanbieding. Zoals vele andere id's in de publicatie-Portal plannen-id gebruikt wordt vergrendeld na de eerste publicatie op productie en u niet mogelijk om te wijzigen van deze id.

3. Klik op om het accepteren van uw keuze.

4. Klik wordt u gevraagd enkele extra vragen met betrekking tot uw nieuwe abonnement.

    ![tekenen](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Vraag|Significantie|
|----|----|
|**Dit abonnement is gratis en beschikbaar wereldwijde?**|U kunt een volledig vrij te geven van-gratis abonnement maken. Als dit de enige abonnement voor deze aanbieding is – betekent dit dat u 'Gratis bieden' in de Marketplace publiceren wilt. Als u alleen voor een (van weinig)-abonnement het it biedt u een optie voor het aanbieden van eindgebruikers voor meer informatie over uw service met een relatief klein aantal transacties per maand.  Als het antwoord "Ja" luidt, klikt u vervolgens geen verdere vragen krijgt het verzoek.|

> [AZURE.NOTE] Eindgebruikers kan altijd upgraden naar de betaalde abonnementen.

|Vraag|Significantie|
|----|----|
|**Gratis proefversie beschikbaar is?**|U kunt kiezen tussen "Geen proefabonnement" helemaal of de optie voor het gebruiken van uw abonnement voor "Één maand" te geven. Uitgevers zoals u met deze optie kunt bieden eindgebruikers de mogelijkheid om informatie over de voordelen van de aanbieding vrije voor één maand.|

> [AZURE.IMPORTANT] Eindgebruikers alleen kunnen een gratis proefversie kopen als ze betalingsmethode bijvoorbeeld creditcard, enterprise agreement hebt ingesteld.

> Na een maand van de gratis proefversie zijn Azure Marketplace verzonden, opladen klanten in de prijs op de datum van het abonnement, tenzij de klant de abonnement is geannuleerd gestart. Geen speciale melding krijgt voor de eindgebruikers.

|Vraag|Significantie|
|----|----|
|**Dit abonnement is vereist een promotiecode kunt kopen?**| Uitgevers hebben een optie voor het beperken van toegang tot de Service-abonnementen doordat een speciale code, 'A Promocode' genoemd naar specifieke klanten. Alleen eindgebruikers dat deze Promocode kunnen abonneren op het abonnement. Als u "Geen" kiest, stemt u ermee dat iedereen in de regio waar de aanbieding beschikbaar (Zie [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) voor meer informatie is) kunnen abonneren op dit abonnement. Geen verdere vragen wordt gevraagd.|
|**Dit abonnement van iedereen die een geldige promotiecode geen ook verbergen?**|Als het antwoord naar de vorige vraag is 'Ja' heeft de uitgever een optie voor het verwijderen van dit abonnement wordt weergegeven in de gebruikersinterface van de Marketplace. Betekent dit?, klanten wordt niet weergegeven in dit abonnement in de pagina details van de aanbieding. Eindgebruikers waaraan een promocode om aan te schaffen, kunnen abonneren op deze via deze promocode.|

## <a name="6---create-your-marketplace-marketing-content"></a>6. uw Marketplace marketinginhoud maken
Voor meer informatie op te geven informatie vereist in tabbladen **Marketing, prijs, ondersteuning en categorieën** neemt Bezoek [Marketplace-handleiding voor marketingactiviteit inhoud](marketplace-publishing-push-to-staging.md) die voor alle onderdelen geldt die zijn gepubliceerd in de Azure Marketplace.  

## <a name="7---connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7. verbinden met uw aanbod uw Service (SQL Azure op basis of webservice op basis).

Klik op de opties voor AutoAanpassen **Gegevensservices** .

Klik op de bovenste helft van de pagina wordt u gevraagd om te leveren van de aanbieding **Namespace**.  

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.png)

De onder vraag wordt opgeven hoe de uitgever expose nieuwe aanbieding met Azure Marketplace. (Zie de [Data Services technische vereiste Guide](marketplace-publishing-data-service-creation-prerequisites.md)voor meer informatie).

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Publicatie van de Database op basis-service**

Klik op **Database**. De volgende pagina wordt weergegeven:

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.3.png)

Een toewijzing CSDL voor de gegevensset op basis van de SQL Azure-DB maken:

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.4.png)

En kiest u vervolgens voor elke tabel

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.6.png)

Als webservice

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] Lees de [toewijzing van een bestaande webservice met OData tot en met CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) voor gedetailleerde instructies en voorbeelden voor het maken van een webservice CSDL.

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw aanbod gegevensservice hebt gemaakt, zorg ervoor dat u de instructies in de [Marketplace marketingactiviteit Content Guide](marketplace-publishing-push-to-staging.md) worden voltooid voordat u verder gaan naar het [testen van uw gegevens-Service in tijdelijke](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Zie ook
- [Aan de slag: Hoe u een aanbieding publiceren naar de Azure Marketplace](marketplace-publishing-getting-started.md)
- Als u de algehele OData toewijzingsproces en doel begrijpen geïnteresseerd bent, leest u in dit artikel [Gegevens Service OData toewijzing](marketplace-publishing-data-service-creation-odata-mapping.md) moet worden gereviseerd definities, structuren en instructies.
- Als u geïnteresseerd bent in zal het leren van informatie over de specifieke knooppunten en parameters, Lees dit artikel [Gegevens Service OData toewijzing knooppunten](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) voor definities en uitleg, voorbeelden en gebruik van hoofdletters/kleine letters context.
- Als u wilt beoordelen voorbeelden, Lees dit artikel [Gegevens Service OData toewijzing voorbeelden](marketplace-publishing-data-service-creation-odata-mapping-examples.md) om te zien van de steekproef code en meer informatie over de codesyntaxis van de en context.


[link-pubportal]:https://publish.windowsazure.com
