<properties 
    pageTitle="Meldingen en e-mailsjablonen in Azure API Management configureren" 
    description="Leer hoe u meldingen configureren en e-mailsjablonen in Azure API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Meldingen en e-mailsjablonen in Azure API Management configureren

API-beheer biedt de mogelijkheid voor het configureren van meldingen voor specifieke gebeurtenissen en voor het configureren van de e-mailsjablonen die worden gebruikt om te communiceren met de beheerders en ontwikkelaars een exemplaar van de API Management. In dit onderwerp ziet u hoe u meldingen over de beschikbare gebeurtenissen configureren en krijgt u een overzicht van het configureren van de e-mailsjablonen die wordt gebruikt voor deze gebeurtenissen.

## <a name="publisher-notifications"> </a>Publisher-meldingen configureren

Als u wilt configureren meldingen, klikt u op **beheren** in de klassieke Azure-Portal voor uw service API Management. Hiermee gaat u naar de publisher-API-beheerportal.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

Klik op **meldingen** in het menu **API beheer** aan de linkerkant om de beschikbare meldingen weergeven.

![Publisher-meldingen][api-management-publisher-notifications]

De volgende lijst met gebeurtenissen kan worden geconfigureerd voor meldingen.

-   Van **abonnement aanvragen (vereisen van goedkeuring)** - de opgegeven e-mailgeadresseerden en gebruikers, ontvangt e-mailmeldingen over abonnement aanvragen voor API-producten vereisen van goedkeuring.
-   **Nieuwe abonnementen** - de opgegeven e-mailgeadresseerden en gebruikers, e-mailmeldingen over nieuwe API product abonnementen ontvangt.
-   **Galerie aanvragen** - de opgegeven e-mailgeadresseerden en gebruikers, e-mailmeldingen ontvangt wanneer nieuwe toepassingen naar de galerie toepassing worden verzonden.
-   **BCC** - de opgegeven e-mailgeadresseerden en gebruikers, blind carbon copy van alle e-mailberichten naar ontwikkelaars verzonden e-mailbericht ontvangt.
-   **Nieuwe kwestie of opmerking** - de opgegeven e-mailgeadresseerden en gebruikers, e-mailmeldingen wanneer een probleem met de nieuwe ontvangt of opmerking op de developer portal is verzonden.
-   **Account sluiten bericht** - de opgegeven e-mailgeadresseerden en gebruikers, e-mailberichten ontvangt wanneer een account is gesloten.
-   **Quotumlimiet voor Approaching abonnement** - de volgende e-mailgeadresseerden en gebruikers, e-mailmeldingen ontvangt wanneer abonnement gebruik dicht bij de quota Resourcegebruik krijgt.

Voor elke gebeurtenis, kunt u e-mailgeadresseerden in het tekstvak voor e-mailadres opgeven of u kunt gebruikers selecteren uit een lijst.

Als u wilt opgeven van de e-mailadressen moeten meldingen ontvangen, geeft u deze in het tekstvak e-mailadres. Als er meerdere e-mailadressen, scheiden met komma's.

![Ontvangers van meldingen][api-management-email-addresses]

Als u wilt opgeven gebruikers moeten meldingen ontvangen, klik op **Voeg geadresseerden**, schakel het selectievakje in naast de gebruikers moeten meldingen ontvangen en klik op **OK**.

>Houd er rekening mee dat alleen beheerders worden weergegeven in de lijst.

Nadat de ontvangers van meldingen zijn geconfigureerd, klikt u op **Opslaan** als u wilt toepassen van de ontvangers van de bijgewerkte meldingen.

>Als u ergens buiten het tabblad **Publisher meldingen** navigeren de publisher-portal wordt u gewaarschuwd als er niet-opgeslagen wijzigingen.

## <a name="email-templates"> </a>E-mailsjablonen configureren

E-mailsjablonen biedt API Management voor de e-mailberichten die zijn verzonden in de loop van de beheren en het gebruik van de service. De volgende e-mailsjablonen worden gegeven.

-   Toepassing galerie indiening goedgekeurd
-   Ontwikkelaars afscheidstekst brief
-   Quotumlimiet voor ontwikkelaars melding bijna is bereikt
-   Gebruiker uitnodigen
-   Nieuwe opmerking is toegevoegd aan een probleem
-   Nieuwe probleem ontvangen
-   Nieuw abonnement geactiveerd
-   Abonnement verlengd bevestiging
-   Abonnement verzoek logboekrecords
-   Aanvraag voor een abonnement hebt ontvangen

Deze sjablonen kunnen worden gewijzigd naar wens.

Als u wilt weergeven en het e-mailsjablonen voor uw exemplaar van de API Management configureren, klik op **meldingen** in het menu **API beheer** aan de linkerkant en selecteer het tabblad **E-mailsjablonen** .

![E-mailsjablonen][api-management-email-templates]

Als u wilt bekijken of wijzigen van een specifieke sjabloon, selecteert u deze in de vervolgkeuzelijst **sjablonen** .

![Lijst met e-sjablonen][api-management-email-templates-list]

Elke e-mailsjabloon heeft een onderwerp in tekst zonder opmaak en de definitie van een hoofdtekst in HTML-indeling. Elk item kan worden aangepast naar wens.

![E-maileditor van sjabloon][api-management-email-template]

De lijst **Parameters** bevat een lijst met parameters die tijdens in het onderwerp of de hoofdtekst ingevoegd, worden de opgegeven waarde te vervangen wanneer het e-mailbericht is verzonden. Als u wilt invoegen als u een parameter, plaats de cursor waar u wilt dat de parameter om te gaan en klik op de pijl links van de parameternaam van de.

Klik op **voorbeeld** of **verzenden van een toets** om te zien hoe het e-mailbericht wordt Kijk of een test-e-mailbericht verzenden.

>Houd er rekening mee dat de parameters niet worden vervangen door werkelijke waarden bij het afluisteren van of een toets verzenden.
>
>De wijzigingen in de e-mailsjabloon opslaan, klikt u op **Opslaan**of om op te zeggen de wijzigingen klikt u op **Annuleren**.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Aan de slag met Azure API Management]: api-management-get-started.md
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance