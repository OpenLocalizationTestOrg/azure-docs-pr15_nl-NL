<properties
    pageTitle="Gebruik scenario's en aandachtspunten voor de implementatie voor het deelnemen aan Azure AD | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe beheerders kunnen instellen Azure AD deelnemen aan voor hun eindgebruikers (medewerkers, studenten, andere gebruikers). Het is ook wordt beschreven hoe de verschillende echte-scenario's voor het gebruik van Azure AD deelnemen."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< labels ms.service= "active directory" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Gebruik scenario's en aandachtspunten voor de implementatie voor het deelnemen aan Azure AD

## <a name="usage-scenarios-for-azure-ad-join"></a>Gebruik scenario's voor het deelnemen aan Azure AD
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Scenario 1: Bedrijven grotendeels in de cloud

Azure Active Directory deelnemen aan (Azure AD deelnemen) profiteert u als u momenteel werkt en nu identiteiten beheer voor uw bedrijf in de cloud of naar de cloud binnenkort verplaatst. U kunt een account dat u hebt gemaakt in Azure AD te melden bij Windows 10 gebruiken. In [het eerste uitvoeren ervaring (FRX)-proces](active-directory-azureadjoin-user-frx.md)of het samenvoegen van Azure AD via [het instellingenmenu](active-directory-azureadjoin-user-upgrade.md), kunnen uw gebruikers kunnen hun computers lid naar Azure AD.  Uw gebruikers kunnen ook profiteren van eenmalige aanmelding (SSO) toegang tot cloud bronnen zoals Office 365, in de browser of in Office-toepassingen.

### <a name="scenario-2-educational-institutions"></a>Scenario 2: Onderwijsinstellingen

Onderwijsinstellingen hebben meestal twee gebruikerstypen: faculteit en leerlingen/studenten. Faculteitsleden worden beschouwd als langere leden van de organisatie. On-premises implementatie-accounts maken voor hen is wenselijk. Maar leerlingen/studenten korterlopende lid van de organisatie zijn en hun accounts kunnen worden beheerd in Azure AD. Dit betekent dat directory schaal naar de cloud in plaats van dat wordt opgeslagen verplaatst kunt on-premises implementatie. Het betekent ook dat leerlingen/studenten kunnen aanmelden bij Windows met hun Azure AD-account en toegang krijgen tot Office 365-informatiebronnen in Office-toepassingen.

### <a name="scenario-3-retail-businesses"></a>Scenario 3: Detailhandel bedrijven

Detailhandel bedrijven hebben seizoensgebonden werknemers en langdurige werknemers. U meestal on-premises implementatie-accounts maken en gebruiken van een domein behoren machines voor langere fulltime werknemers. Seizoensgebonden werknemers korterlopende lid van de organisatie zijn maar het is wenselijk voor het beheren van hun accounts waar gebruikerslicenties eenvoudiger rond kunnen worden verplaatst. Als u de gebruikersaccounts in de cloud met Office 365-licenties maken, krijgt deze gebruikers de voordelen aan te melden bij Windows en Office-toepassingen met een Azure AD-account, terwijl u meer flexibiliteit met hun licenties onderhouden nadat ze verlaten.

### <a name="scenario-4-additional-scenarios"></a>Scenario 4: Extra scenario 's

Samen met de voordelen besproken eerder, profiteert u dat uw gebruikers hun apparaten te koppelen aan Azure AD vanwege een eenvoudigere functionaliteit voor gekoppelde, efficiënt Apparaatbeheer automatische mobiele apparaat management registratie en eenmalige aanmelding naar Azure AD en on-premises bronnen.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Aandachtspunten voor de implementatie voor het deelnemen aan Azure AD

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Dat uw gebruikers kunnen deelnemen aan een bedrijf eigendom apparaat rechtstreeks aan Azure AD


Ondernemingen kunnen alleen de cloud accounts aan partnerbedrijven en organisaties worden verstrekt. Deze partners vervolgens eenvoudig toegang tot bedrijf apps en resources met eenmalige aanmelding. Dit scenario geldt voor gebruikers die toegang tot bronnen hoofdzakelijk in de cloud, zoals Office 365- of SaaS-apps die van Azure AD voor verificatie gebruikmaken.

### <a name="prerequisites"></a>Vereisten voor
**Op het ondernemingsniveau van (beheerder)**

*   Azure-abonnement met Azure Active Directory  

**Op het gebruikersniveau van de**

*   Windows-10 (Professional en Enterprise-edities)

### <a name="administrator-tasks"></a>Beheerderstaken
* [Apparaatregistratie instellen](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Gebruikerstaken
* [Een nieuwe Windows 10-apparaat met Azure AD tijdens de installatie instellen](active-directory-azureadjoin-user-frx.md)
* [Een Windows 10-apparaat met Azure AD instellen via het instellingenmenu](active-directory-azureadjoin-user-upgrade.md)
* [Een persoonlijke Windows 10-apparaat te koppelen aan uw organisatie](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>BYOD inschakelen in uw organisatie voor Windows 10
U kunt uw gebruikers en werknemers instellen voor het gebruik van hun persoonlijke Windows apparaten (BYOD) voor toegang tot bedrijf apps en bronnen. Uw gebruikers kunnen hun Azure AD-accounts (werk of school-accounts) toevoegen aan een persoonlijke Windows-apparaat voor toegang tot bronnen op een veilige en compatibele manier.

### <a name="prerequisites"></a>Vereisten voor
**Op het ondernemingsniveau van (beheerder)**

*   Azure AD-abonnement

**Op het gebruikersniveau van de**

*   Windows-10 (Professional en Enterprise-edities)


### <a name="administrator-tasks"></a>Beheerderstaken

* [Apparaatregistratie instellen](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Gebruikerstaken
* [Een persoonlijke Windows 10-apparaat te koppelen aan uw organisatie](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>Aanvullende informatie
* [Windows 10 voor de onderneming: manieren om apparaten voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Identiteiten zonder wachtwoorden via Microsoft Passport verifiëren](active-directory-azureadjoin-passport.md)
* [Meer informatie over het gebruik scenario's voor het deelnemen aan Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Deelnemen aan Azure AD instellen](active-directory-azureadjoin-setup.md)
