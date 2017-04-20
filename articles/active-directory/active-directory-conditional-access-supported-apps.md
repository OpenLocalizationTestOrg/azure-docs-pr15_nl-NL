<properties
    pageTitle="Toepassingen die gebruikmaken van voorwaardelijke toegangsregels in Azure Active Directory | Microsoft Azure"
    description="Met voorwaardelijke toegangsbeheer controleert Azure Active Directory voor specifieke voorwaarden wanneer deze wordt geverifieerd door de gebruiker en om toepassing toegang te krijgen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Toepassingen die gebruikmaken van voorwaardelijke toegangsregels in Azure Active Directory

Regels voor voorwaardelijke toegang worden ondersteund in Azure Active Directory (Azure AD)-toepassingen verbonden, vooraf geïntegreerde federatieve software als een service (SaaS)-toepassingen, toepassingen die gebruikmaken van wachtwoord eenmalige aanmelding (SSO), lijn-of-business-toepassingen en toepassingen die gebruikmaken van Azure AD-toepassingsproxy. Zie [Services met voorwaardelijke toegang ingeschakeld](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access)voor een gedetailleerde lijst met toepassingen waarvoor u voorwaardelijke toegang kunt gebruiken. Voorwaardelijke toegang werkt zowel met mobiele en bureaublad toepassingen die gebruikmaken van moderne verificatie. In dit artikel wordt behandeld hoe voorwaardelijke access werkt in mobiele en bureaublad-apps.

U kunt Azure AD aanmeldingsproblemen pagina's gebruiken in toepassingen die gebruikmaken van moderne verificatie. Een gebruiker wordt gevraagd met een aanmeldingspagina voor meervoudige verificatie. Een bericht wordt weergegeven als van de gebruiker toegang is geblokkeerd. Moderne verificatie is vereist voor het apparaat om te verifiëren met Azure AD, zodat apparaat gebaseerd voorwaardelijke-beleid worden geëvalueerd.

Het is belangrijk te weten welke toepassingen kunt regels voor voorwaardelijke toegang en de stappen die u mogelijk moet uitvoeren om te beveiligen van de andere toepassing invoer wordt verwezen.

## <a name="applications-that-use-modern-authentication"></a>Toepassingen die gebruikmaken van moderne verificatie

> [AZURE.NOTE] Als u een voorwaardelijke-beleid Azure AD waarop een equivalent in Office 365 hebt, configureert u beide voorwaardelijke clienttoegangsbeleid. Dit wilt toepassen, bijvoorbeeld voorwaardelijke-beleid voor Exchange Online of SharePoint Online.

De volgende toepassingen ondersteunen voorwaardelijke toegang voor Office 365 en andere Azure AD-verbonden servicetoepassingen:

| Target-service  | Platform  | Toepassing                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Office 365 Exchange Online | Windows 10|Agenda-mail/personen-app, in Outlook 2016, Outlook 2013 (met moderne verificatie), Skype voor bedrijven (met moderne verificatie)|
|Office 365 Exchange Online| Windows 8.1, Windows 7 |Outlook 2016, Outlook 2013 (met moderne verificatie), Skype voor bedrijven (met moderne verificatie)|
|Office 365 Exchange Online|iOS, android-apparaat|  De mobiele app van Outlook|
|Office 365 Exchange Online|Mac OS X| Outlook 2016 voor meervoudige verificatie en locatie. ondersteuning voor Groepsbeleid op basis van het apparaat is gepland voor de toekomst, Skype voor bedrijven-ondersteuning voor de toekomst is gepland|
|Office 365 SharePoint Online|Windows 10| Apps voor Office 2016, Universal Office-apps, Office 2013 (met moderne verificatie), OneDrive voor bedrijven-app (volgende generatie-synchronisatieclient of NGSC) ondersteunen gepland voor de toekomst, Office-groepen ondersteuning voor de toekomst, ondersteuning van de SharePoint-Apps die is gepland voor de toekomst is gepland|
|Office 365 SharePoint Online|Windows 8.1, Windows 7|Apps voor Office 2016, Office 2013 (met moderne verificatie), OneDrive voor bedrijven-app (Groove-synchronisatieclient)|
|Office 365 SharePoint Online|iOS, android-apparaat|  Office mobile-apps |
|Office 365 SharePoint Online|Mac OS X| Office 2016-apps voor meervoudige verificatie en locatie. ondersteuning voor Groepsbeleid op basis van het apparaat is gepland voor de toekomst|
|Office 365 Yammer|Windows 10, iOS en Android | Yammer-app voor Office|
|Dynamics CRM|Windows 10, Windows 8.1, Windows 7, iOS en Android | Dynamics CRM-app|
|PowerBI-service|Windows 10, Windows 8.1, Windows 7, iOS en Android | PowerBI-app|
|Azure externe App-service|Windows 10, Windows 8.1, Windows 7, iOS, Android en Mac OS X |Azure Remote-app|
|Een service van de app mijn Apps|Android en iOS|Een service van de app mijn Apps |


## <a name="applications-that-do-not-use-modern-authentication"></a>Toepassingen die geen van moderne verificatie gebruikmaakt

Momenteel, moet u andere methoden toegang tot de apps die geen van moderne verificatie gebruikmaakt blokkeren. Toegangsregels voor apps die moderne verificatie niet gebruikt, worden niet afgedwongen door voorwaardelijke toegang. Dit is hoofdzakelijk een aandacht voor Exchange en SharePoint-toegang. De meeste eerdere versies van apps oudere protocollen voor access-besturingselement gebruiken.

### <a name="control-access-in-office-365-sharepoint-online"></a>Toegangsbeheer in Office 365 SharePoint Online
U kunt oudere protocollen voor toegang tot SharePoint uitschakelen via de cmdlet Set-SPOTenant. Gebruik deze cmdlet om te voorkomen dat de Office-clients die gebruikmaken van niet-modern verificatieprotocollen toegang krijgen tot SharePoint Online-informatiebronnen.

**Voorbeeld**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Toegangsbeheer in Office 365 Exchange Online

Exchange biedt twee belangrijkste categorieën van protocollen. Controleer de volgende opties en selecteer vervolgens het beleid dat geschikt is voor uw organisatie.

-   **Exchange ActiveSync**. Standaard worden niet voorwaardelijke-beleid voor meervoudige verificatie en locatie afgedwongen voor Exchange ActiveSync. Moet u toegang tot deze services beveiligen door het configureren van Exchange ActiveSync-beleid rechtstreeks of door het blokkeren van Exchange ActiveSync met behulp van regels voor Active Directory Federation Services (AD FS).
-   **Verouderd protocollen**. U kunt oudere protocollen met AD FS blokkeren. Deze blokkeert de toegang tot oudere Office-clients, zoals Office 2013 zonder een moderne verificatie is ingeschakeld en eerdere versies van Office.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>AD FS oudere protocol blokkeren gebruiken

U kunt het volgende voorbeeld van regels voor oudere protocol toegang op het niveau van de AD FS blokkeren. Kiezen uit twee algemene configuraties.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Optie 1: Exchange ActiveSync toestaan en oudere apps, maar alleen op het intranet toestaan

Door de volgende drie regels toepassen op de AD FS te vertrouwen partijen vertrouwen voor Microsoft Office 365-identiteit Platform, Exchange ActiveSync-verkeer is toegestaan, en browser en moderne verificatie-verkeer is toegestaan, moet u toegang hebt. Oudere apps worden het extranet geblokkeerd.

##### <a name="rule-1"></a>Regel 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Regel 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Regel 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Optie 2: Exchange ActiveSync toestaan en oudere apps blokkeren

Door de volgende drie regels toepassen op de AD FS te vertrouwen partijen vertrouwen voor Microsoft Office 365-identiteit Platform, Exchange ActiveSync-verkeer is toegestaan, en browser en moderne verificatie-verkeer is toegestaan, moet u toegang hebt. Oudere apps worden geblokkeerd vanaf elke locatie.

##### <a name="rule-1"></a>Regel 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Regel 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Regel 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
