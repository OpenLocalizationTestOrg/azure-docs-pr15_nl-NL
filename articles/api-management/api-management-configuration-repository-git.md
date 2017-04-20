<properties 
    pageTitle="Hoe op te slaan en de API-Management service-configuratie met cijfer configureren" 
    description="Informatie over het opslaan en de API-Management service-configuratie met cijfer configureren." 
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


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Hoe op te slaan en de API-Management service-configuratie met cijfer configureren

>[AZURE.IMPORTANT] Cijfer configuratie voor API Management is momenteel in de proefversie. Dit is functioneel voltooid, maar is in de proefversie van omdat we actief feedback over deze functie zoekt. Het is mogelijk dat we een recente wijzigen in antwoord op feedback van klanten, zodat u wordt aangeraden niet afhankelijk van de functie voor gebruik in productieomgevingen mogelijk maken. Als u feedback of vragen hebt, laat het ons weten op `apimgmt@microsoft.com`.

Elk exemplaar van de service API Management onderhoudt een configuratiedatabase met informatie over de configuratie en metagegevens voor het exemplaar van de service. Wijzigingen kunnen worden aangebracht in het exemplaar van de service door een instelling in de portal publisher wijzigen, een PowerShell-cmdlet, of u een REST API-oproep plaatst. U kunt ook uw service exemplaar te configureren cijfer gebruiken, zoals het inschakelen van scenario's voor het beheer van service beheren naast deze methoden:

-   Configuratie versiebeheer - downloaden en bewaren van verschillende versies van uw service te configureren
-   Bulksgewijs configuratiewijzigingen - wijzigingen aanbrengen in meerdere onderdelen van de serviceconfiguratie van uw in uw lokale opslagplaats en de wijzigingen terug naar de server integreren met één bewerking
-   Vertrouwde cijfer toolchain en de werkstroom - het cijfer tooling en werkstromen die u al bekend met bent gebruiken

In het volgende diagram ziet u een overzicht van de verschillende manieren voor het configureren van uw exemplaar van de service API Management.

![Cijfer configureren][api-management-git-configure]

Wanneer u wijzigingen in uw service met de publisher-portal, PowerShell-cmdlets of voor de REST API aanbrengt, u beheert uw service configuratie database met de `https://{name}.management.azure-api.net` eindpunt, zoals wordt weergegeven aan de rechterkant van het diagram. De linkerkant van het diagram ziet u hoe u uw service te configureren met cijfer kan beheren en cijfer opslagplaats voor uw service te vinden op `https://{name}.scm.azure-api.net`.

De volgende stappen wordt een overzicht gegeven van het beheren van uw API Management service-exemplaar cijfer gebruiken.

1.  Cijfer toegang in uw service inschakelen
2.  Het opslaan van uw service configuratie-database naar uw bibliotheek cijfer
3.  Het cijfer cessies‑retrocessies naar uw lokale computer klonen
4.  De meest recente cessies‑retrocessies naar beneden af op uw lokale computer te halen en wijzigingen doorvoeren en push terug naar uw cessies‑retrocessies
5.  De wijzigingen van uw cessies‑retrocessies implementeren in uw service configuratie-database

In dit artikel wordt beschreven hoe u inschakelen en cijfer gebruiken voor het beheren van uw service te configureren en biedt een verwijzing voor de bestanden en mappen in de bibliotheek cijfer.

## <a name="to-enable-git-access"></a>Cijfer toegang in te schakelen

U kunt de status van uw configuratie cijfer snel weergeven door te bekijken van het cijfer-pictogram in de rechterbovenhoek van de portal van publisher. In dit voorbeeld is nog geen cijfer toegang ingeschakeld.

![Cijfer status][api-management-git-icon-enable]

Als u wilt weergeven en uw cijfer configuratie-instellingen configureren, kunt u op het pictogram cijfer of klik op het menu **beveiliging** en Ga naar het tabblad **bibliotheek van de configuratie** .

![CIJFER inschakelen][api-management-enable-git]

Schakel het selectievakje **inschakelen cijfer toegang** als cijfer access.

Na even de wijziging is opgeslagen en wordt een bevestigingsbericht weergegeven. Houd er rekening mee dat cijfer pictogram kleur om aan te geven dat cijfer toegang is ingeschakeld en het statusbericht geeft nu aan dat er niet-opgeslagen wijzigingen naar de bibliotheek is gewijzigd. Dit komt omdat de API Management service configuratiedatabase nog niet naar de bibliotheek is opgeslagen.

![Cijfer ingeschakeld][api-management-git-enabled]

>[AZURE.IMPORTANT] Geen geheimen die niet zijn gedefinieerd als eigenschappen in de bibliotheek worden opgeslagen en in de geschiedenis totdat u blijft uitschakelen en cijfer access opnieuw in te schakelen. Eigenschappen bieden een veilige manier beheren constante tekenreekswaarden, geheimen, inclusief voor alle API-configuratie en beleidsregels, zodat u niet hoeft te ze rechtstreeks in uw beleid-instructies hebt opgeslagen. Zie [het gebruik van eigenschappen in beleidsregels voor informatiebeheer van Azure-API](api-management-howto-properties.md)voor meer informatie.

Zie voor informatie over het in- of uitschakelen via de REST API toegang tot een cijfer, [in- of uitschakelen via de REST API toegang tot een cijfer](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>De serviceconfiguratie opslaan naar de bibliotheek cijfer

De eerste stap voordat u de opslagplaats klonen is de bibliotheek opslaan in de huidige status van de service te configureren. Klik op **configuratie naar bibliotheek opslaan**.

![Configuratie opslaan][api-management-save-configuration]

Breng de gewenste wijzigingen in het bevestigingsvenster en klik op **Ok** om op te slaan.

![Configuratie opslaan][api-management-save-configuration-confirm]

Na een paar seconden de configuratie is opgeslagen en de status van de configuratie van de bibliotheek wordt weergegeven, waaronder de datum en tijd van de laatste configuratiewijziging en de laatste synchronisatie tussen de service te configureren en de bibliotheek.

![Configuratiestatus][api-management-configuration-status]

Zodra de configuratie naar de bibliotheek is opgeslagen, kunt u deze kunt gekopieerd.

Voor informatie over het uitvoeren van deze bewerking de REST API gebruiken, Zie [configuratie momentopname met de REST API doorvoeren](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>De bibliotheek naar uw lokale computer kopiëren

Een bibliotheek klonen, moet u de URL van uw bibliotheek, een gebruikersnaam in te voeren en een wachtwoord. De gebruikersnaam in te voeren en de URL worden weergegeven aan de bovenkant van het tabblad **bibliotheek van de configuratie** .

![Cijfer klonen][api-management-configuration-git-clone]

Het wachtwoord wordt gegenereerd onderaan op het tabblad **bibliotheek van de configuratie** .

![Wachtwoord genereren][api-management-generate-password]

Genereren van een wachtwoord, eerst Zorg ervoor dat het **verstrijken** is ingesteld op de gewenste vervaldatum en de tijd en klik vervolgens op **Token genereren**.

![Wachtwoord][api-management-password]

>[AZURE.IMPORTANT] Noteer dit wachtwoord. Nadat u deze pagina verlaat wordt het wachtwoord niet meer weergegeven.

De volgende voorbeelden gebruik het hulpmiddel cijfer we vaker doen op [Cijfer voor Windows](http://www.git-scm.com/downloads) , maar u kunt elk cijfer-programma dat u bekend met bent.

Open uw hulpmiddel cijfer in de gewenste map en voer de volgende opdracht om te klonen de opslagplaats cijfer naar uw lokale computer, met de opdracht die is verstrekt door de publisher-portal.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Geef de gebruikersnaam en wachtwoord wanneer u wordt gevraagd.

Als u eventuele fouten ontvangt, kunt u het wijzigen van uw `git clone` opdracht om op te nemen de gebruikersnaam en wachtwoord, zoals wordt weergegeven in het volgende voorbeeld.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Als dit een fout bevat, kunt u de URL-codering van het gedeelte van het wachtwoord van de opdracht. Een snelle manier hiervoor is te Visual Studio openen en de volgende opdracht in het **Venster Direct**. U opent het **Venster Direct**, een oplossing of een project openen in Visual Studio (of een nieuwe lege console-toepassing maken), en kies van **Windows**, **direct** vanuit het menu **Foutopsporing** .

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

Gecodeerd wachtwoord samen met uw naam en locatie van een gebruikerslocatie gebruiken om samen te stellen van de opdracht cijfer.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

U kunt weergeven en ermee werken in uw lokale bestandssysteem zodra de opslagplaats is gekopieerd. Zie [Naslaginformatie over een bestands- en structuur van de lokale cijfer opslagplaats](#file-and-folder-structure-reference-of-local-git-repository)voor meer informatie.

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Bijwerken van uw lokale opslagplaats met de configuratie van de meest recente service-exemplaar

Als u wijzigingen in uw exemplaar van de service API Management in de publisher-portal of de REST API gebruiken aanbrengt, moet u deze wijzigingen opslaan naar de bibliotheek voordat u uw lokale opslagplaats met de meest recente wijzigingen bijwerken kunt. Klik hiertoe op **configuratie naar bibliotheek opslaan** op het tabblad **bibliotheek van de configuratie** in de publisher-portal en geef vervolgens de volgende opdracht in uw lokale opslagplaats.

    git pull

Voordat u `git pull` ervoor te zorgen dat u in de map voor uw lokale opslagplaats. Als u net hebt voltooid de `git clone` opdracht, en vervolgens moet u de map op uw cessies‑retrocessies door een opdracht zoals de volgende handelingen uit te voeren.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Aan de wijzigingen van uw lokale cessies‑retrocessies push naar de server cessies‑retrocessies

Als u wilt push wijzigingen van uw lokale opslagplaats naar de bibliotheek van de server, moet u uw wijzigingen en push ze vervolgens naar de bibliotheek van de server. Als u wilt uw wijzigingen doorvoeren, open uw cijfer opdrachthulpprogramma, overschakelen naar de map van uw lokale opslagplaats en de volgende opdrachten.

    git add --all
    git commit -m "Description of your changes"

Voer de volgende opdracht om door te voeren alle de doorvoercoördinatie op de server.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Service configuratiewijzigingen aanbrengt in het exemplaar van de service API Management implementeren

Nadat uw lokale wijzigingen zijn doorgevoerd en worden verplaatst naar de bibliotheek van de server, kunt u ze kunt implementeren in uw exemplaar van de service API Management.

![Implementeren][api-management-configuration-deploy]

Voor informatie over het uitvoeren van deze bewerking de REST API gebruiken, [cijfer implementeren-wijzigingen in de REST API gebruiken configuratiedatabase](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges)te bekijken.

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Verwijzing in de bestands- en de structuur van de lokale cijfer opslagplaats

De bestanden en mappen in de lokale cijfer opslagplaats bevatten de configuratie-informatie over het service-exemplaar.

| Item                       | Beschrijving                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| api-management hoofdmap | Op het hoogste niveau configuratie voor de service-exemplaar bevat                                  |
| API's map                | De configuratie voor de API's in de service-exemplaar bevat                            |
| map groepen              | De configuratie voor de groepen in de service-exemplaar bevat                          |
| map beleid            | Het beleid in de service-exemplaar bevat                                              |
| portalStyles map        | De configuratie voor de portal aanpassingen voor ontwikkelaars in de service-exemplaar bevat |
| map Products            | De configuratie voor de producten in de service-instantie bevat                        |
| sjablonenmap           | De configuratie voor de e-mailsjablonen in de service-exemplaar bevat                 |

Elke map kan bevatten een of meer bestanden en in sommige gevallen een of meer mappen, bijvoorbeeld een map voor elke API, product of groep. De bestanden in elke map zijn specifiek voor het entiteitstype beschreven door de mapnaam.

| Bestandstype | Doel                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | Configuratie van informatie over de desbetreffende entiteit                  |
| HTML      | Beschrijvingen over de entiteit, vaak weergegeven in de developer portal |
| XML       | Beleid-instructies                                                      |
| CSS       | Opmaakmodellen voor ontwikkelaars portal aanpassen                        |

Deze bestanden kunnen worden gemaakt, verwijderd, bewerkt en beheerd op uw lokale bestandssysteem en de wijzigingen die zijn geïmplementeerd terug naar de uw exemplaar van de service API Management.

>[AZURE.NOTE] De volgende entiteiten zich niet in de bibliotheek cijfer en kunnen niet worden geconfigureerd met cijfer.
>
>-    Gebruikers
>-    Abonnementen
>-    Eigenschappen
>-    Developer portal entiteiten dan stijlen

### <a name="root-api-management-folder"></a>Api-management hoofdmap

Ga naar de hoofdsite `api-management` map bevat een `configuration.json` bestand met op het hoogste niveau informatie over het service-exemplaar in de volgende indeling.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

De eerste vier instellingen (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, en `UserRegistrationTermsConsentRequired`) toewijzen aan de volgende instellingen op het tabblad **identiteiten** in de sectie **beveiliging** .

| Identiteit                     | Toegewezen aan                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | **Anonieme gebruikers om te leiden aanmeldingspagina** selectievakje |
| UserRegistrationTerms                | **Gebruiksvoorwaarden op gebruiker registratie** tekstvak               |
| UserRegistrationTermsEnabled         | **Weergeven van de gebruiksvoorwaarden op aanmeldingspagina** selectievakje         |
| UserRegistrationTermsConsentRequired | Selectievakje **toestemming vereisen**                          |

![Instellingen voor de identiteiten][api-management-identity-settings]

De volgende vier instellingen (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, en `DelegationValidationKey`) toewijzen aan de volgende instellingen op het tabblad **overdracht** in de sectie **beveiliging** .

| Delegatie-instelling           | Toegewezen aan                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | Selectievakje **gemachtigde aanmelden en aanmelding**    |
| DelegationUrl                | **Delegeren eindpunt URL** -tekstvak        |
| DelegatedSubscriptionEnabled | Selectievakje **gemachtigde product abonnement** |
| DelegationValidationKey      | **Gemachtigde validatie toets** tekstvak        |

![Instellingen voor doorschakelen naar gemachtigden][api-management-delegation-settings]

De instelling van de uiteindelijke, `$ref-policy`, kaarten naar het bestand globaal beleid-instructies voor het exemplaar van de service.

### <a name="apis-folder"></a>API's map

De `apis` een map voor elke API in de service-sessie met de volgende items bevat.

-   `apis\<api name>\configuration.json`-Dit is de configuratie voor de API en bevat informatie over de URL van de backend-service en de bewerkingen. Dit is dezelfde informatie die wordt geretourneerd als u wilt bellen met het [ophalen van een specifieke API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) met `export=true` in `application/json` opmaken.
-   `apis\<api name>\api.description.html`-Dit is de beschrijving van de API en komt overeen met de `description` eigenschap van de [API entiteit](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
-   `apis\<api name>\operations\`-Deze map bevat `<operation name>.description.html` bestanden die zijn toegewezen aan de bewerkingen in de API. Elk bestand bevat de omschrijving van één bewerking in de API dat met overeenkomt de `description` eigenschap van de [bewerking entiteit](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in de REST API.

### <a name="groups-folder"></a>map groepen

De `groups` een map voor elke groep die is gedefinieerd in het exemplaar van de service bevat.

-   `groups\<group name>\configuration.json`-Dit is de configuratie voor de groep. Dit is dezelfde informatie die wordt geretourneerd als u de bewerking voor het [ophalen van een specifieke groep](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) bellen.
-   `groups\<group name>\description.html`-Dit is de beschrijving van de groep en komt overeen met de `description` eigenschap van de [groep entiteit](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>map beleid

De `policies` map bevat het beleid-instructies voor uw exemplaar van de service.

-   `policies\global.xml`-bevat beleid gedefinieerd op globaal bereik voor uw exemplaar van de service.
-   `policies\apis\<api name>\`-Als u gedefinieerd in het bereik van de API beleid hebt, ze zijn opgenomen in deze map.
-   `policies\apis\<api name>\<operation name>\`map - als u gedefinieerd in het bewerkingsbereik beleid hebt, ze zijn opgenomen in deze map in `<operation name>.xml` bestanden die zijn toegewezen aan het beleid-instructies voor elke bewerking.
-   `policies\products\`-Als er bij product bereik gedefinieerd beleid, ze zijn opgenomen in deze map, die bevat `<product name>.xml` bestanden die zijn toegewezen aan het beleid-instructies voor elk product.

### <a name="portalstyles-folder"></a>portalStyles map

De `portalStyles` map bevat configuratie en opmaakmodellen voor ontwikkelaars portal aanpassingen voor het exemplaar van de service.

-   `portalStyles\configuration.json`-bevat de namen van de opmaakmodellen die worden gebruikt door de developer portal
-   `portalStyles\<style name>.css`-elke `<style name>.css` bestand bevat stijlen voor de developer portal (`Preview.css` en `Production.css` al dan niet standaard).

### <a name="products-folder"></a>map Products

De `products` een map voor elk product die is gedefinieerd in het exemplaar van de service bevat.

-   `products\<product name>\configuration.json`-Dit is de configuratie voor het product. Dit is dezelfde informatie die wordt geretourneerd als u de bewerking voor het [ophalen van een specifiek product](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) bellen.
-   `products\<product name>\product.description.html`-Dit is de beschrijving van het product en komt overeen met de `description` eigenschap van het [product entiteit](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in de REST API.

### <a name="templates"></a>sjablonen

De `templates` map bevat de configuratie voor de [e-mailsjablonen](api-management-howto-configure-notifications.md) van de service-exemplaar.

-   `<template name>\configuration.json`-Dit is de configuratie voor de e-mailsjabloon.
-   `<template name>\body.html`-Dit is de hoofdtekst van het e-mailsjabloon.

## <a name="next-steps"></a>Volgende stappen

Voor informatie over andere manieren voor het beheren van uw exemplaar van de service, raadpleegt u:

-   Uw service-exemplaren die met het volgende PowerShell-cmdlets beheren
    -   [Service-implementatie verwijzing naar de PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Servicebeheer verwijzing naar de PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   Uw service-exemplaren in de publisher-portal beheren
    -   [Uw eerste API beheren](api-management-get-started.md)
-   Uw service-exemplaren met de REST API beheren
    -   [API Management REST API-verwijzing](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Bekijk een video-overzicht

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




