<properties
   pageTitle="Hoe toepassingen worden toegevoegd aan de Azure Active Directory."
   description="In dit artikel wordt beschreven hoe toepassingen worden toegevoegd aan een exemplaar van Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Hoe en waarom toepassingen Azure AD worden toegevoegd

Een van de eerste instantie verwarrende dingen bij het weergeven van een lijst met toepassingen in uw exemplaar van Azure Active Directory wordt informatie over waar de toepassingen afkomstig zijn en waarom ze er zijn.  In dit artikel, vindt u een hoog niveau overzicht van hoe toepassingen worden aangegeven in de adreslijst en voorziet van de context die u helpt bij het begrijpen hoe een toepassing in uw map hebt gekregen.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Voor welke services biedt Azure AD naar toepassingen?

Toepassingen worden toegevoegd aan Azure AD om te profiteren van een of meer van de services die deze bevat.  Deze services zijn:

* App-verificatie en machtiging
* Gebruikersverificatie & autorisatie
* Eenmalige aanmelding (SSO) met Federatie of wachtwoord
* Gebruiker inrichting & synchroniseren
* Rolgebaseerd toegangsbeheer; Gebruik de adreslijst de rollen van toepassing als u wilt uitvoeren van rollen definiëren op basis van autorisatie controles in een app.
* oAuth-verificatieservices (gebruikt door Office 365 en andere Microsoft-apps toegang tot de API's / resources toe te staan.)
* Toepassing publiceren en proxy; Een app in een privé-netwerk met internet publiceren

## <a name="how-are-applications-represented-in-the-directory"></a>Hoe worden toepassingen weergegeven in de adreslijst?

Toepassingen worden aangegeven in de Azure AD 2 objecten: een application-object en een service principal object.  Er is één toepassingsobject, geregistreerd in een 'huis"/"eigenaar"of"publicerende"directory en een of meer service-principal objecten dat staat voor de toepassing in elke map waarin deze worden verwerkt.  

De toepassingsobject beschrijft de Azure AD-app (de tenant multi-service), maar mogelijk ook een van de volgende handelingen uit: (*notitie*: dit is niet volledig overzicht.)

* Naam, Logo en Publisher
* Geheimen (symmetric en/of asymmetrische sleutels gebruikt om te verifiëren van de app)
* API afhankelijkheden (oAuth)
* API's / resources/bereiken gepubliceerd (oAuth)
* App-functies (RBAC)
* Eenmalige aanmelding van metagegevens en configuratie (SSO)
* Metagegevens en configuratie van gebruikers
* Proxy-metagegevens en configuratie

De hoofdsom voor de service is een record van de toepassing in elke map, waar de toepassing fungeert inclusief de basismap.  De hoofdsom service:

* Naar een application-object via de app-id-eigenschap verwijst
* De lokale gebruiker records en groep app-roltoewijzingen
* Records van de lokale gebruiker en beheer machtigingen verleend bij de app
    * Bijvoorbeeld: machtigingen voor de app voor toegang tot een bepaalde gebruikers e-mail
* Records lokaal beleid inclusief voorwaardelijke-beleid
* Records lokale alternatieve lokale instellingen voor een app
    * Op claims transformatie regels
    * Kenmerktoewijzingen (inrichting van gebruiker)
    * Specifieke app rollen tenant (als de app aangepaste rollen ondersteunt)
    * Naam/Logo

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Een diagram van toepassingsobjecten en service principes in mappen

![Een diagram waarin wordt uitgelegd hoe toepassing objecten en service-principes bestaande met Azure AD-exemplaren.][apps_service_principals_directory]

Zoals u in het bovenstaande diagram zien kunt.  Microsoft behoudt twee mappen intern (aan de linkerkant) wordt gebruikt voor het publiceren van toepassingen.

* Een handtekening voor Microsoft-Apps (Microsoft services directory)
* Een handtekening voor vooraf geïntegreerde 3e partijen Apps (App-galerie directory)

Toepassing uitgevers/leveranciers die met Azure AD integreren moeten zijn een publicatie bestaan.  (Sommige SAAS map).

Toepassingen die u zelf toevoegen onder andere:

* Apps die u hebt ontwikkeld (geïntegreerd met AAD)
* Apps die u verbonden voor eenmalige aanmelding
* Apps die u met de Azure AD-toepassingsproxy hebt gepubliceerd.

### <a name="a-couple-of-notes-and-exceptions"></a>Een aantal notities en uitzonderingen

* Niet alle service principes wijst u terug naar toepassingsobjecten.  Huh? Azure AD oorspronkelijk is gebouwd de services naar toepassingen veel meer beperkt zijn als de hoofdsom voor de service is voldoende voor het instellen van een app-identiteit.  De oorspronkelijke service principal is nabij in vorm aan de Windows Server Active Directory-account.  Om die reden is het nog steeds mogelijk te maken van service principes voor het gebruik van de Azure AD-PowerShell zonder eerst te maken een application-object.  De grafiek-API vereist een app-object voordat u een service principal maakt.
* Niet alle gegevens van de hierboven beschreven worden momenteel via programmacode weergegeven.  De volgende zijn alleen beschikbaar in de gebruikersinterface:
    * Op claims transformatie regels
    * Kenmerktoewijzingen (inrichting van gebruiker)
* Voor meer gedetailleerde informatie op de hoofdsom service en toepassingsobjecten Zie de documentatie van Azure AD Graph REST API verwijzing.  *Tip*: het Azure AD Graph API-documentatie is het dichtstbijzijnde wat u moet een schemaverwijzing voor Azure AD die momenteel beschikbaar is.  
    * [Toepassing](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Service Principal](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Hoe worden apps toegevoegd aan mijn Azure AD-exemplaar?
Er zijn tal van manieren die een app kan worden toegevoegd aan Azure AD:

* Een app toevoegen vanuit de [Azure Active Directory-App-galerie](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Teken omhoog/in een 3e partijen App geïntegreerd met Azure Active Directory (bijvoorbeeld: [Smartsheet](https://app.smartsheet.com/b/home) of [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Tijdens het aanmelden omhoog/in gebruikers wordt gevraagd om machtigingen voor de app voor toegang tot hun profiel en andere machtigingen te geven.  De eerste persoon toestemming geven zorgt ervoor dat een service principal dat staat voor de app moet worden toegevoegd aan de map.
* Meld u omhoog/naar Microsoft online services zoals [Office 365](http://products.office.com/)
    * Als u zich hebt geabonneerd op Office 365 of een proefabonnement op een of meer service principes zijn gemaakt in de map dat staat voor de verschillende services die worden gebruikt voor het leveren van de functionaliteit die is gekoppeld aan Office 365 begint.
    * Service principes maken sommige Office 365-services, zoals SharePoint op basis van continue toe te staan dat beveiligde communicatie tussen onderdelen, met inbegrip van werkstromen.
* Een app die u ontwikkelt toevoegen in de beheerportal van Azure zien: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Een app die u ontwikkelt gebruik van Visual Studio Raadpleeg toevoegen:
    * [ASP.Net-verificatiemethoden](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Verbonden Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Een app die u wilt gebruiken voor het gebruik van de [Azure AD-toepassingsproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx) toevoegen
* Verbinding maken met een app voor eenmalige aanmelding met SAML of wachtwoord eenmalige aanmelding
* Vele andere verschillende ontwikkelaars ervaringen op te nemen in Azure en/in API explorer ervaringen over ontwikkelaars centers

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Wie heeft er machtigingen voor het toevoegen van mijn exemplaar Azure AD-toepassingen?

Alleen globale beheerders kunt doen:

* Apps toevoegen vanuit de galerie met Azure AD-app (vooraf geïntegreerde 3e partijen Apps)
* Een app voor het gebruik van de Azure AD-toepassingsproxy publiceren

Alle gebruikers in uw adreslijst hebben machtigen voor het toevoegen van toepassingen die ze ontwikkelen en keuze via welke toepassingen ze delen/geven toegang tot hun organisatie-gegevens.  *Gebruiker aanmelden omhoog/aan een app te onthouden is en toewijzen van machtigingen kan dit leiden tot de hoofdsom van een service wordt gemaakt.*

In dit mogelijk in eerste instantie geluid betreffende, maar het volgende in gedachten houden:

* Apps gelukt om te profiteren van Windows Server Active Directory voor gebruikersverificatie jaren zonder de toepassing worden geregistreerd/opgenomen in de adreslijst.  Zichtbaarheid naar precies hoeveel apps de map en what for gebruikt wordt nu aan de organisatie hebt verbeterd.
* Niet nodig voor beheer van basis van hoeveelheid werk app publiceren/registratieproces.  Met Active Directory Federation Services was deze waarschijnlijk die een beheerder van een app toevoegen als een gebruikmakende partij namens ontwikkelaars had.  Nu kunnen ontwikkelaars zelf te kunnen.
* Gebruikers melden in/snel aan de apps met behulp van hun organisatie-account voor zakelijke doeleinden is een goede ding.  Als ze vervolgens de organisatie die ze geen toegang meer op de account in de toepassing die ze zijn gebruikt laat.
* Een record van welke gegevens werd gedeeld waaraan toepassing goed is niet.  Gegevens meer vervoerbare dan ooit is en dat een wissen record van wie welke gegevens gedeeld met welke toepassingen is handig.
* Apps die gebruikmaken van Azure AD voor oAuth besluit precies welke machtigingen die gebruikers kunnen verlenen naar toepassingen en waarvoor machtigingen beheerder om te accepteren.  Dit moet gaan zonder te zeggen dat alleen beheerders kunnen toestemming groter bereik en meer aanzienlijk machtigingen.
* Gebruikers toevoegen en toestaan apps toegang tot hun gegevens zijn gecontroleerd gebeurtenissen, zodat u de Controleverslagrapporten die binnen de portal Azure Management om te bepalen hoe een app is toegevoegd aan de map kunt bekijken.

**Notitie:** *Microsoft zelf is besturingsomgeving met behulp van de standaardconfiguratie voor groot aantal maanden nu.*

Gezegd met die al is het mogelijk te voorkomen dat gebruikers in uw adreslijst toepassingen toevoegen en uit te oefenen keuze over welke gegevens zij delen met toepassingen doordat de configuratie van de map in de beheerportal van Azure.  De volgende configuratie kan worden geopend in de beheerportal van Azure op van uw map 'Configureren' tabblad.

![Schermafbeelding van de gebruikersinterface voor het configureren van instellingen voor geïntegreerde app][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Meer informatie over het toevoegen van Azure AD-toepassingen en hoe u services configureren voor apps.

* Ontwikkelaars: [meer informatie over het integreren van een toepassing met AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Ontwikkelaars: [controleren steekproef code voor apps is geïntegreerd met Azure Active Directory op Github](https://github.com/AzureADSamples)
* Ontwikkelaars en IT-professionals: [de REST API-documentatie van de Azure Active Directory Graph API controleren](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT-professionals: [informatie over het gebruik van de Azure Active Directory vooraf geïntegreerde toepassingen uit de App-galerie](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT-professionals: [vindt de zelfstudies voor het configureren van specifieke vooraf geïntegreerde apps](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT-professionals: [informatie over het publiceren van een app met de toepassingsproxy Azure Active Directory](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Zie ook

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
