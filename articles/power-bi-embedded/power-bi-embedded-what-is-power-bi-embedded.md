<properties
   pageTitle="Wat is Microsoft Power BI ingesloten?"
   description="Power BI ingesloten kunt u Power BI-rapporten integreren in uw web- of mobiele toepassingen, zodat u niet hoeft te maken van aangepaste oplossingen om gegevens voor uw gebruikers te visualiseren"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="what-is-microsoft-power-bi-embedded"></a>Wat is Microsoft Power BI ingesloten?

Met **Power BI ingesloten**, kunt u Power BI-rapporten integreren in uw web- of mobiele toepassingen.

![](media\powerbi-embedded-whats-is\what-is.png)

Power BI ingesloten is een **Azure-service** waarmee ISV's en app ontwikkelaars surface gegevens van Power BI-mogelijkheden binnen hun toepassingen. Als ontwikkelaar u toepassingen hebt gemaakt en deze toepassingen hun eigen gebruikers en een unieke set functies. Deze apps kunnen ook gebeuren dat bepaalde gegevenselementen ingebouwde zoals grafieken en rapporten die u nu worden mogelijk door Microsoft Power BI ingesloten gemaakt kunnen. Gebruikers nodig niet een Power BI-account gebruik van uw app. Ze kunnen Meld u aan bij uw toepassing net als voordat, en weergeven en werken met de Power BI rapportagemogelijkheden zonder eventuele aanvullende licentieverlening blijven.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Licenties voor Microsoft Power BI ingesloten

In het **Microsoft Power BI ingesloten** gebruiksmodel is licenties voor Power BI niet de verantwoordelijkheid van de eindgebruiker.  In plaats daarvan **worden weergegeven door** door de ontwikkelaar van de app die de visuele elementen verbruikt zijn gekocht en op het abonnement dat eigenaar is van de resources in rekening worden gebracht.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI ingesloten conceptueel Model

![](media\powerbi-embedded-whats-is\model.png)

Informatiebronnen voor Power BI ingesloten is zoals elke andere service in Azure wordt aangegeven ingericht tot en met de [Azure ARM-API's](https://msdn.microsoft.com/library/mt712306.aspx). In dit geval is de bron die u inrichten een **Verzameling van Power BI-werkruimte**.

## <a name="workspace-collection"></a>Werkruimte siteverzameling

Een **Siteverzameling van de werkruimte** is de op het hoogste niveau Azure container voor resources die 0 of meer **werkruimten**bevat.  Een **werkruimte** **siteverzameling** heeft alle van de Azure standaardeigenschappen, evenals de volgende:

-   **Toegangstoetsen** – toetsen gebruikt om de Power BI-API's (Zie verderop) veilig te bellen.
-   **Gebruikers** – Azure Active Directory (AAD)-gebruikers die beschikken over beheerdersmachtigingen voor het verzamelen van Power BI-werkruimte via de portal van Azure of ARM API beheren.
-   **Regio** – als onderdeel van de inrichting van een **Siteverzameling van de werkruimte**, kunt u een gebied te richten. Zie [Azure regio's](https://azure.microsoft.com/regions/)voor meer informatie.

## <a name="workspace"></a>Werkruimte

Een **werkruimte** is een container met Power BI-inhoud, waaronder gegevenssets, rapporten en dashboards. Een **werkruimte** is leeg wanneer de eerste keer wordt gemaakt. In de voorbeeldweergave, moet u alle inhoud met Power BI Desktop creëert en uploadt u deze naar een van uw werkruimten met de [Power BI REST API's](http://docs.powerbi.apiary.io/reference).

## <a name="using-workspace-collections-and-workspaces"></a>Gebruik van de werkruimte verzamelingen en werkruimten
**Werkruimte verzamelingen** en **werkruimten** zijn containers van inhoud die worden gebruikt en ingedeeld in ongeacht de manier waarop best past het ontwerp van de toepassing die u maakt. Er zijn verschillende manieren dat u de inhoud ervan kan rangschikken. U kunt alle inhoud in een werkruimte worden geplaatst en app tokens later verder onderverdelen de inhoud onder uw klanten te gebruiken. U kunt ook moeten worden geplaatst al uw klanten in afzonderlijke werkruimten zodat er enkele scheiding ertussen. Of u kunt gebruikers per regio in plaats van de klant indelen. Deze flexibele ontwerpen kunt u kiest de beste manier om inhoud te ordenen.

## <a name="cached-datasets"></a>In de cache gegevenssets

In de cache gegevenssets kan worden gebruikt in de proefversie.  U kunt echter in de cache opgeslagen gegevens niet vernieuwen zodra deze is geladen in **Microsoft Power BI ingesloten**.

## <a name="authentication-and-authorization-with-app-tokens"></a>Verificatie en machtiging met app tokens

**Microsoft Power BI ingesloten** past omleiding naar uw toepassing om uit te voeren van alle benodigde gebruikersverificatie en autorisatie. Er is geen expliciete vereiste dat uw eindgebruikers klanten van Azure Active Directory (Azure AD) zijn.  In plaats daarvan drukt uw toepassing naar **Microsoft Power BI ingesloten** autorisatie om weer te geven van een Power BI-rapport met behulp van de **Toepassing verificatietokens (App Tokens)**.  Deze **App Tokens** worden gemaakt naar wens wanneer uw app wil weergave van een rapport.  Zie [App Tokens](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

**Toepassing verificatietokens (App Tokens)** worden gebruikt om te worden geverifieerd bij **Microsoft Power BI ingesloten**.  Er zijn drie soorten **App Tokens**:

1.  Tokens - die worden gebruikt bij de inrichting van een nieuwe **werkruimte** in een **Werkruimte siteverzameling** inrichting
2.  Ontwikkeling Tokens - gebruikt bij het maken van oproepen meteen naar de **Power BI REST API 's**
3.  Tokens - gebruikt bij het maken van oproepen om weer te geven van een rapport in de ingesloten iframe insluiten

Deze tokens worden gebruikt voor de verschillende fasen van uw interacties met **Microsoft Power BI ingesloten**.  De tokens zijn zo ontworpen dat u machtigingen in uw app voor Power BI delegeren kunt. Zie de [App Token Flow](power-bi-embedded-app-token-flow.md)voor meer informatie.

## <a name="see-also"></a>Zie ook
- [Veelvoorkomende scenario's voor Microsoft Power BI ingesloten](power-bi-embedded-scenarios.md)
- [Aan de slag met Microsoft Power BI ingesloten](power-bi-embedded-get-started.md)
