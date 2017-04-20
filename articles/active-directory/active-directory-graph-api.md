<properties
   pageTitle="Azure Active Directory Graph API | Microsoft Azure"
   description="Een overzicht en quickstart handleiding voor de grafiek-API waarmee toegang tot het Azure AD tot en met REST API-eindpunten."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API

> [AZURE.IMPORTANT] Azure AD Graph API-functionaliteit is ook beschikbaar via de [Microsoft Graph](https://graph.microsoft.io/), een geïntegreerd API die API's van andere Microsoft-services zoals Outlook, OneDrive, OneNote, teamplanner en Office Graph, toegankelijk via een enkelvoudig eindpunt en met een enkel toegangstoken bevat.

De Azure Active Directory Graph API biedt programma toegang tot Azure AD tot en met REST API-eindpunten. Toepassingen kunnen de grafiek-API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen voor directory-gegevens en objecten. De grafiek-API ondersteunt bijvoorbeeld de volgende algemene bewerkingen voor een gebruikersobject:

- Een nieuwe gebruiker in een map maken

- Gedetailleerde eigenschappen van een gebruiker, zoals hun groepen krijgen

- Bijwerken van een gebruiker eigenschappen, zoals de locatie en een telefoonnummer of hun wachtwoord wijzigen

- Controleer de groepslidmaatschap van een gebruiker voor access op basis van rollen

- Een gebruikersaccount uitschakelen of volledig verwijderen

Naast het gebruikersobjecten, kunt u vergelijkbare bewerkingen op andere objecten zoals groepen en toepassingen uitvoeren. Als u wilt bellen de API grafiek op een map, wordt de toepassing moet zijn geregistreerd bij Azure AD en worden geconfigureerd voor toegang tot de adreslijst. Dit is gewoonlijk bereikt door een gebruiker of beheerder toestemming stroom.

Begin van de Azure Active Directory Graph-API gebruiken, raadpleegt u de [Introductiehandleiding voor Graph-API](active-directory-graph-api-quickstart.md)of bekijkt u de [documentatie voor interactieve Graph API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Functies

De grafiek-API biedt de volgende functies:

- **REST API eindpunten**: Graph beheren om de API is een RESTful service bestaat uit de eindpunten die worden geopend met standaard HTTP-aanvragen. De grafiek-API ondersteunt XML- of Javascript-Object notatie (JSON) inhoudstypen voor vergaderverzoeken en antwoorden. Zie [Azure AD Graph REST API-naslaggids](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)voor meer informatie.

- **Verificatie met Azure AD**: elk verzoek tot de API Graph moet worden geverifieerd door een JSON Web Token (JWT) in de koptekst van de verificatie van de aanvraag toe te voegen. Deze token wordt opgehaald door een nieuw vergaderverzoek aanbrengt in token Azure AD-eindpunt en geldige referenties opgeeft. U kunt de stroom van OAuth 2.0-client referenties gebruiken of de autorisatiecode verlenen doorlopen naar in het bezit van een token om te bellen van de grafiek. Voor meer informatie [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Rollen gebaseerde verificatie (RBAC)**: beveiligingsgroepen RBAC uitvoeren in de grafiek-API worden gebruikt. Als u bepalen wilt of een gebruiker toegang tot een specifieke bron heeft, kan de toepassing bijvoorbeeld belt u de bewerking [Controleren groepslidmaatschap (overgankelijke)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) true of false retourneert.

- **Differentiële Query**: als u controleren op wijzigingen in een map tussen de twee perioden wilt zonder dat u wilt aanbrengen veelgebruikte query's in de grafiek-API, kunt u een queryverzoek voor een differentiële. Dit type aanvraag retourneert alleen de wijzigingen die zijn gedaan tussen de vorige differentiële queryaanvraag en de huidige aanvraag. Zie [Azure AD Graph API differentiële Query](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query)voor meer informatie.

- **Directory-extensies**: als u een toepassing dat u wilt lezen of schrijven van unieke eigenschappen voor directory-objecten ontwikkelt, kunt u registreren en het gebruik van de waarden van de extensie met behulp van de grafiek-API. Als uw toepassing een eigenschap Skype-ID voor elke gebruiker vereist, bijvoorbeeld, kunt u de nieuwe eigenschap registreren in de map en is beschikbaar op elke gebruikersobject. Zie [Azure AD Graph API Directory Schema-uitbreidingen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)voor meer informatie.

- **Beveiligd met machtiging bereiken**: AAD Graph-API maakt machtiging bereiken die u secure/ingestemd toegang tot gegevens AAD en ondersteuning voor allerlei app clienttypen, waaronder:
 - mensen met een gebruikersinterface die zijn opgegeven toegang tot gegevens via autorisatie gedelegeerd van de ondertekend gebruiker (gedelegeerd)
  - items die gebruiken definieert toepassing-Rolgebaseerd toegangsbeheer zoals service/daemon werknemers (app rollen)

    Zowel gedelegeerde en app rol machtiging bereiken vertegenwoordigen een bevoegdheid die worden aangeboden door de API Graph en kunnen worden opgevraagd door clienttoepassingen tot en met [functies in de portal van Azure klassieke](https://manage.windowsazure.com)machtigingen toepassing. Clients kunnen controleren of de bereiken machtigingen verleend toe door te controleren van de reikwijdte ("scp") claimen ontvangen in het toegangstoken voor gedelegeerd machtigingen en de rollen ('rollen') claimen voor app-machtigingen voor rol. Meer informatie over [Azure AD Graph API machtiging bereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Scenario 's

De grafiek-API kunnen veel toepassing-scenario's. De volgende scenario's zijn de meest gebruikte:

- **Lijn van (één Tenant) bedrijfstoepassing**: In dit scenario wordt een enterprise-ontwikkelaars werkt voor een organisatie met Office 365-abonnement. De ontwikkelaar is bouwen van een webtoepassing waarin interactie met Azure AD taken uit te voeren zoals een licentie toewijzen aan een gebruiker. Deze taak vereist toegang tot de API Graph, zodat de ontwikkelaar kassa's Eén tenant-toepassing in Azure AD en configureert lezen en voor de grafiek-API schrijfmachtigingen. En vervolgens de toepassing is geconfigureerd voor het gebruik van eigen referenties of die van de gebruiker momenteel aanmeldingsproblemen waar u een token om te bellen van de grafiek-API.

- **Software als een servicetoepassing (meerdere Tenant)**: In dit scenario is een onafhankelijke software softwareleverancier gehoste meerdere tenant-webtoepassing waarmee gebruiker beheerfuncties voor andere organisaties die gebruikmaken van Azure AD ontwikkelen. Deze functies zijn vereist voor toegang tot directory-objecten en zodat de toepassing moet de grafiek-API aan te roepen. De ontwikkelaar registreert u de toepassing in Azure AD, configureert vereisen lees- en schrijfmachtigingen voor de grafiek-API en vervolgens externe toegang ingeschakeld, zodat andere organisaties kunnen toestemming voor het gebruik van de toepassing in de map. Wanneer een gebruiker in een andere organisatie geverifieerd bij de toepassing voor de eerste keer, worden ze een dialoogvenster toestemming met de machtigingen voor die het aanvragen van de toepassing weergegeven.  Toekennen toestemming geeft vervolgens de toepassing die machtigingen tot de API Graph in de map van de gebruiker gevraagd. Zie [overzicht van het kader toestemming](active-directory-integrating-applications.md)voor meer informatie over het kader toestemming.

## <a name="see-also"></a>Zie ook

[Introductiehandleiding voor Azure AD Graph API](active-directory-graph-api-quickstart.md)

[AD Graph REST documentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Handleiding voor Azure Active Directory-ontwikkelaars](active-directory-developers-guide.md)
