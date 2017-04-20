<properties
    pageTitle="Overzicht van Azure Active Directory Domain Services | Microsoft Azure"
    description="Overzicht van Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure AD-domeinservices

## <a name="overview"></a>Overzicht
Azure infrastructuurservices kunt u een groot aantal oplossingen computing op een flexibele manier implementeren. Met Azure virtuele Machines, kunt u vrijwel onmiddellijk distribueren en u alleen met de minuut betaalt. Ondersteuning voor Windows, Linux, SQL Server, Oracle, IBM, SAP en BizTalk gebruikt, kunt u een werkbelasting, een taal, klikt u op vrijwel elk besturingssysteem implementeren. Deze voordelen kunt u oudere toepassingen geïmplementeerd on-premises implementatie migreren naar Azure om op te slaan op operationele kosten.

Een belangrijke aspecten van migreren on-premises implementatie toepassingen Azure is de behoeften van de identiteit van deze toepassingen verwerken. Directory toepassingen mogelijk LDAP afhankelijk voor lezen of schrijven toegang tot het telefoonboek van het bedrijf of aanmelding geïntegreerde Windows-verificatie (Kerberos of NTLM-verificatie) om te verifiëren eindgebruikers. Lijn-of-business (LOB) toepassingen op Windows Server zijn meestal geïmplementeerd op computers domein toegevoegd, zodat ze kunnen worden beheerd veilig via Groepsbeleid. Tot 'lift-en-shift' on-premises implementatie-toepassingen in de cloud moeten deze afhankelijkheden op de infrastructuur zakelijke identiteit worden opgelost.

Beheerders u vaak naar een van de volgende oplossingen om te voldoen aan de behoeften van de identiteit van hun toepassingen die zijn geïmplementeerd in Azure wordt aangegeven:

- Een site-naar-site VPN-verbinding tussen werkbelasting met Azure-infrastructuurservices en de adreslijst het bedrijf on-premises implementatie.
- De zakelijke AD domein/infrastructuur uitbreiden door het instellen van replicadomeincontrollers met Azure virtuele machines.
- Een zelfstandig domein in Azure wordt aangegeven met domeincontrollers geïmplementeerd als Azure virtuele machines implementeren.

Alle volgende manieren afnemen hoge kosten en administratieve onkosten. Beheerders moeten zijn domeincontrollers virtuele machines in Azure wordt aangegeven met implementeren. Daarnaast kunnen moeten ze beheren, secure patch, controleren, back-up maken en deze virtuele machines oplossen. De afhankelijkheid van VPN-verbindingen met de on-premises adreslijst wordt geïmplementeerd in Azure last tijdelijke netwerk voren of bijvoorbeeld werkbelasting. Deze netwerkstoringen beurtelings leiden tot lagere beschikbaarheid en verminderde betrouwbaarheid voor deze toepassingen.

We ontworpen Azure AD-domeinservices op te geven van een eenvoudiger alternatief.


## <a name="introducing-azure-ad-domain-services"></a>Inleiding tot Azure AD-domeinservices
Azure AD-domeinservices biedt beheerde domeinservices zoals join-domein, Groepsbeleid, LDAP, Kerberos/NTLM-verificatie die volledig compatibel zijn met Windows Server Active Directory. U kunt deze domeinservices zonder dat u kunt implementeren, beheren en domeincontrollers in de cloud patch gebruiken. Azure AD-domeinservices werkt naadloos samen met uw bestaande Azure AD-tenant, en waardoor voor gebruikers aan te melden met hun bedrijfsreferenties. Bovendien kunt u bestaande groepen en gebruikersaccounts toegang te krijgen tot bronnen, zodat de via de telefoon soepeler 'lift-en-verschuiving' van een on-premises bronnen die u wilt Azure-infrastructuurservices.

Azure AD-domeinservices functionaliteit werkt naadloos samen ongeacht of uw Azure AD-tenant alleen de cloud gebruikt of gesynchroniseerde met uw lokale Active Directory is.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure AD-domeinservices voor alleen de cloud organisaties
Een alleen de cloud Azure AD-tenant (vaak 'beheerde tenants' genoemd), hoeft niet alle on-premises implementatie identiteit milieu. Gebruikersaccounts, hun wachtwoorden en groepslidmaatschap zijn met andere woorden, alle systeemeigen in de cloud - dat wil zeggen gemaakt en beheerd in Azure AD. Kijk eens naar dat Contoso een cloud-lezen is Azure AD-tenant. Zoals u ziet in de volgende afbeelding, kunnen van Contoso-beheerder een virtueel netwerk heeft geconfigureerd in Azure-infrastructuurservices. Toepassingen en server werkbelasting zijn geïmplementeerd in deze virtuele netwerk in Azure virtuele machines. Aangezien Contoso een tenant alleen de cloud gebruikt is, worden alle identiteit van de gebruikers hun referenties en groepslidmaatschap gemaakt en beheerd in Azure AD.

![Azure AD Domain Services-overzicht](./media/active-directory-domain-services-overview/aadds-overview.png)

Van Contoso IT-beheerder kunt inschakelen Azure AD-domeinservices voor hun Azure AD-tenant en kies domeinservices beschikbaar maken in deze virtuele netwerk. Daarna Azure AD-domeinservices een beheerde domein bepalingen en beschikbaar zijn in het virtuele netwerk. Alle gebruikersaccounts, groepslidmaatschap en de referenties van de gebruiker is beschikbaar in van Contoso Azure AD-tenant zijn ook beschikbaar in deze nieuwe domein. Deze functie kan gebruikers in de organisatie Meld u aan bij het domein met hun bedrijfs-referenties - bijvoorbeeld als u extern koppelt aan een domein behoren machines via Extern bureaublad. Beheerders kunnen toegang tot bronnen in het domein met bestaande groepslidmaatschappen inrichten. Toepassingen die zijn geïmplementeerd in virtuele machines op het virtuele netwerk kunnen functies beschikbaar, zoals aan domein toevoegen, LDAP lezen, LDAP-binding, NTLM als Kerberos-verificatie en Groepsbeleid gebruiken.

Enkele belangrijke aspecten van het beheerde domein dat is ingericht door Azure AD-domeinservices zijn als volgt:

- Van Contoso IT-beheerder hoeft niet te beheren, patch of dit domein of elk domein-controller voor dit domein beheerde controleren.
- Er is niet nodig voor het beheren van AD replicatie voor dit domein. Gebruikersaccounts, groepslidmaatschap en referenties van van Contoso Azure AD-tenant automatisch beschikbaar zijn in deze beheerde domein.
- Aangezien het domein wordt beheerd door Azure AD-domeinservices, Contoso van IT-beheerder geen domein-beheerder of Ondernemingsadministrator bevoegdheden voor dit domein.


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure AD-domeinservices voor hybride organisaties
Organisaties met een hybride IT-infrastructuur gebruik een combinatie van on-premises implementatie middelen en cloud. Deze organisaties synchroniseren identiteitsgegevens van de on-premises adreslijst naar hun Azure AD-tenant. Hybride organisaties zoeken voor het migreren van meer van de on-premises implementatie-toepassingen in de cloud, met name oudere directory geschikt toepassingen, Azure AD-domeinservices is soms handig om ze.

Litware Corporation is geïmplementeerd [Azure AD Connect](../active-directory/active-directory-aadconnect.md), identiteitsgegevens van de on-premises adreslijst hun Azure AD-tenant wilt synchroniseren. De identiteit-informatie die wordt gesynchroniseerd bevat gebruikersaccounts, hun hashes referentie voor verificatie (Wachtwoordsynchronisatie) en groepslidmaatschap.

> [AZURE.NOTE] **Wachtwoordsynchronisatie is verplicht voor hybride organisaties Azure AD Domain Services gebruiken**. Deze vereiste geldt omdat de referenties van gebruikers in de beheerde domein verstrekt door Azure AD-domeinservices, nodig zijn om deze gebruikers via NTLM of Kerberos verificatiemethoden te verifiëren.

![Azure AD-domein Services voor Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

De voorgaande afbeelding ziet hoe organisaties met een hybride IT-infrastructuur, zoals Litware Corporation, Azure AD Domain Services kunnen gebruiken. Van Litware-toepassingen en de werklast van de servers die opgevolgd domeinservices moeten zijn geïmplementeerd in een virtueel netwerk in Azure-infrastructuurservices. Litware van IT-beheerder kunt inschakelen Azure AD-domeinservices voor hun Azure AD-tenant en kiest u een beheerde domein beschikbaar maakt in deze virtuele netwerk. Aangezien Litware een organisatie met een hybride IT-infrastructuur is, worden gebruikersaccounts, groepen en referenties gesynchroniseerd met hun Azure AD-tenant van hun on-premises adreslijst. Deze functie kan gebruikers Meld u aan bij het domein dat u hun bedrijfsreferenties - bijvoorbeeld gebruikt als extern verbinding met de computers toegevoegd aan het domein via Extern bureaublad. Beheerders kunnen toegang tot bronnen in het domein met bestaande groepslidmaatschappen inrichten. Toepassingen die zijn geïmplementeerd in virtuele machines op het virtuele netwerk kunnen functies beschikbaar, zoals aan domein toevoegen, LDAP lezen, LDAP-binding, NTLM als Kerberos-verificatie en Groepsbeleid gebruiken.

Enkele belangrijke aspecten van het beheerde domein dat is ingericht door Azure AD-domeinservices zijn als volgt:

- De beheerde domein is een zelfstandig domein. Het is niet een uitbreiding van Litware van on-premises domein.
- Litware van IT-beheerder hoeft niet te beheren, patch, of domeincontrollers voor dit domein beheerde controleren.
- Er is niet nodig voor het beheren van AD-replicatie aan dit domein. Gebruikersaccounts, groepslidmaatschap en referenties van Litware van on-premises adreslijst worden gesynchroniseerd met Azure AD via Azure AD Connect. Deze gebruikersaccounts, groepslidmaatschap en referenties automatisch beschikbaar zijn in de beheerde domein.
- Aangezien het domein wordt beheerd door Azure AD-domeinservices, Litware van IT-beheerder geen domein-beheerder of Ondernemingsadministrator bevoegdheden voor dit domein.


## <a name="benefits"></a>Voordelen
Met Azure AD-domeinservices, kunt u profiteren van de volgende voordelen:

-   **Eenvoudig** : U kunt voorzien in de behoeften van de identiteit van virtuele machines geïmplementeerd in Azure-infrastructuurservices met een paar eenvoudige klikken. U hoeft niet te gebruiken en beheren van identiteit infrastructuur in Azure of setup connectivity terug naar de infrastructuur van uw on-premises implementatie-identiteit.

-   **Geïntegreerde** – Azure AD-domeinservices is nauw geïntegreerd met uw Azure AD-tenant. U kunt nu Azure AD gebruiken als een geïntegreerde cloudgebaseerde enterprise-map die caters aan de behoeften van uw moderne toepassingen en de traditionele directory geschikt toepassingen.

-   **Compatibele** – Azure AD-domeinservices is gebaseerd op de beproefde enterprise grade-infrastructuur van Windows Server Active Directory. Daarom kunnen uw toepassingen vertrouwen op een grotere mate van compatibiliteit met Windows Server Active Directory-functies. Niet alle functies beschikbaar in Windows Server AD zijn momenteel beschikbaar in Azure AD Domain Services. Beschikbare functies zijn echter compatibel is met de bijbehorende Windows Server AD-functies die u in de infrastructuur van uw on-premises implementatie op vertrouwen. De mogelijkheden van de join LDAP, Kerberos NTLM, Groepsbeleid en domein vormen een volwaardige aanbod die is getest en verfijnd via verschillende versies van Windows Server.

-   **Efficiënt** – met Azure AD-domeinservices, kunt u voorkomen dat de infrastructuur en beheer last die is gekoppeld aan de identiteit infrastructuur voor de ondersteuning van traditionele directory geschikt toepassingen beheren. U kunt deze toepassingen Azure-infrastructuurservices verplaatsen en profiteren van lagere operationele kosten.
