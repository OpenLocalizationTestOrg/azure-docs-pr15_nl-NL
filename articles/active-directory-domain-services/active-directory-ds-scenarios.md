<properties
    pageTitle="Azure Active Directory Domain Services: Scenario's implementatie | Microsoft Azure"
    description="Implementatie-scenario's voor Azure AD-domeinservices"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>Scenario's voor implementatie en gebruik-dozen
In dit gedeelte kijken we naar een paar scenario's en zaken die van Azure Active Directory (AD) Domain Services profiteren.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Veilig en eenvoudig beheer van Azure virtuele machines
Azure Active Directory Domain Services kunt u uw Azure virtuele machines op een gestroomlijnde manier beheren. Azure virtuele machines kunnen worden toegevoegd aan het beheerde domein, zodat u uw zakelijke AD-referenties gebruiken voor aanmelding. Deze methode voorkomt referentie management voorkomt zoals lokale administrator-accounts op elk van uw Azure virtuele machines onderhouden.

Server virtuele machines die zijn gekoppeld aan de beheerde domein kan ook worden beheerd en beveiligd met Groepsbeleid. U kunt de vereiste basislijnen toepassen op uw Azure virtuele machines en vergrendelen volgens de richtlijnen voor zakelijke beveiliging. U kunt bijvoorbeeld groepsbeleid beheermogelijkheden om de typen toepassingen die kunnen worden gestart op deze virtuele machines te beperken.

![Gestroomlijnde beheer van Azure virtuele machines](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Zoals servers en andere infrastructuur bereikt einde van de levenscyclus, worden Contoso veel toepassingen momenteel wordt gehost on-premises in de cloud verplaatst. De huidige IT-standaard bepaalt dat de servers die host bedrijfstoepassingen zijn moeten domein zijn toegevoegd en beheerde Groepsbeleid. Van Contoso IT-beheerder domein join virtuele machines geïmplementeerd in Azure wordt aangegeven, liever om beheer te vereenvoudigen. Hierdoor kunnen kunnen beheerders en gebruikers aanmelden met hun bedrijfs-referenties. Tegelijkertijd, kunnen machines worden geconfigureerd akkoord gaat met de vereiste basislijnen Groepsbeleid. Contoso liever niet hoeft te implementeren, controleren en beheren van domeincontrollers in Azure Azure virtuele machines beveiligen. Azure AD-domeinservices is dus een geweldige geschikte optie voor deze gebruiksvoorbeeld.

**Implementatie van notities**

Houd rekening met de volgende belangrijke punten in dit scenario implementatie:

- Beheerde domeinen verstrekt door Azure AD Domain Services bieden een enkele plat OU (organisatie-eenheid)-structuur al dan niet standaard. Alle computers domein behoren bevinden zich in een enkel plat organisatie-eenheid. U kunt echter aangepaste organisatie-eenheden aan.

- Azure AD-domeinservices ondersteunt eenvoudige Groepsbeleid in de vorm van een ingebouwde GPO elke voor de gebruikers en groepen containers. U niet kunt afstemmen 1U door OU/afdeling, uitvoeren WMI-filtering of aangepaste GPO's maken.

- Azure AD-domeinservices ondersteunt het grondtal AD computer object schema. U kunt de van de computerobject-schema niet uitbreiden.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Lift-en-shift een on-premises implementatie-toepassing die gebruikmaakt van LDAP bindingsverificatie naar Azure-infrastructuurservices

![LDAP-binding](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso heeft een on-premises implementatie-toepassing hebt aangeschaft bij een ISV groot aantal jaar geleden. De toepassing is momenteel in de onderhoudsmodus voor door website en beperkend dure voor Contoso aanvragen van wijzigingen in de toepassing is. Deze toepassing heeft een web gebaseerde frontend die worden verzameld gebruikersreferenties via een formulier en vervolgens gebruikers worden geverifieerd door een LDAP-binding met het bedrijfsnetwerk Active Directory te voeren. Contoso wilt migreren van deze toepassing Azure-infrastructuurservices. Het is wenselijk of de toepassing werkt zoals is, zonder wijzigingen. Daarnaast kunnen gebruikers moeten kunnen om te verifiëren hun bestaande zakelijke referenties met en zonder te hoeven gebruikers om dingen te doen anders retrain. Met andere woorden, eindgebruikers moet oblivious van waarop de toepassing wordt uitgevoerd en de migratie transparant ze moet zijn.

**Implementatie van notities**

Houd rekening met de volgende belangrijke punten in dit scenario implementatie:

- Zorg ervoor dat de toepassing niet hoeft te wijzigen/schrijven naar de map. LDAP-schrijftoegang tot beheerde domeinen verstrekt door Azure AD-domeinservices wordt niet ondersteund.

- U kunt wachtwoorden rechtstreeks ten opzichte van het domein dat beheerde niet wijzigen. Eindgebruikers op hun wachtwoord kunnen wijzigen met Azure AD selfservice-wachtwoord wijzigen om of ten opzichte van de on-premises adreslijst. Deze wijzigingen worden automatisch gesynchroniseerd en beschikbaar zijn in de beheerde domein.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Lift en shift een on-premises implementatie-toepassing die gebruikmaakt van LDAP lezen voor toegang tot de map Azure-infrastructuurservices
Contoso heeft een on-premises implementatie-LOB (LOB)-toepassing die bijna een tien jaar geleden is ontwikkeld. Deze toepassing is op de hoogte directory en is ontworpen voor gebruik met Windows Server AD. De toepassing gebruikt LDAP (Lightweight Directory Access Protocol) om te lezen informatie/kenmerken over gebruikers uit Active Directory. De toepassing niet kenmerken wijzigen of anderszins schrijven naar de map. Contoso wilt migreren van deze toepassing Azure-infrastructuurservices en intrekken van de verouderende on-premises implementatie hardware momenteel hostingprovider deze toepassing. De toepassing kan niet worden herschreven als moderne directory API's zoals de Azure AD grafiek op basis van een REST API wilt gebruiken. Een optie lift en shift wordt daarom gewenst waarbij de toepassing kan worden gemigreerd om uit te voeren in de cloud, zonder code wijzigen of de toepassing herschrijven.

**Implementatie van notities**

Houd rekening met de volgende belangrijke punten in dit scenario implementatie:

- Zorg ervoor dat de toepassing niet hoeft te wijzigen/schrijven naar de map. LDAP-schrijftoegang tot beheerde domeinen verstrekt door Azure AD-domeinservices wordt niet ondersteund.

- Zorg ervoor dat de toepassing niet een schema aangepaste/uitgebreid Active Directory hoeft. Schema-uitbreidingen worden niet ondersteund in Azure AD Domain Services.


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Migreren van een toepassing voor on-premises implementatie service of daemon naar Azure-infrastructuurservices
Sommige toepassingen bestaan uit meerdere niveaus, waar een lagere nodig zijn voor geverifieerde oproepen naar een laag backend zoals een databaselaag. Active Directory-service-accounts zijn meestal gebruikt voor deze gebruik-gevallen. U kunt lift en shift dergelijke toepassingen naar Azure-infrastructuurservices en gebruik Azure AD-domeinservices voor de behoeften van de identiteit van deze toepassingen. U kunt de dezelfde serviceaccount die uit de adreslijst van uw on-premises implementatie is gesynchroniseerd met Azure AD moet worden gebruikt. U kunt ook eerst maken een aangepaste OU en klikt u vervolgens een afzonderlijke service-account maken in de organisatie-eenheid, om te implementeren van dergelijke toepassingen.

![Met behulp van WIA-serviceaccount](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso heeft een op maat kluis softwaretoepassing die de front-end van een webpagina, een SQL server en een FTP-endserver bevat. Geïntegreerde Windows-verificatie van serviceaccounts wordt gebruikt voor verificatie van de front-end van de web op de FTP-server. De front-end van de web is geconfigureerd om uit te voeren als een serviceaccount. De backend-server is geconfigureerd voor het toestaan van toegang via het serviceaccount voor de front-end van de web. Contoso liever niet hoeft te implementeren van een domeincontroller virtuele machine in de cloud verplaatsen van deze toepassing Azure-infrastructuurservices. Van Contoso IT-beheerder de servers die host de front-end van de web, SQL server en de FTP-server Azure virtuele machines kunt implementeren. Deze machines zijn vervolgens gekoppeld aan een beheerde domeinservices van Azure AD-domein. Vervolgens kunnen ze dezelfde serviceaccount gebruiken in hun on-premises adreslijst ter verificatie van de app. Deze serviceaccount is gesynchroniseerd met de beheerde domeinservices van Azure AD-domein en is beschikbaar voor gebruik.

**Implementatie van notities**

Houd rekening met de volgende belangrijke punten in dit scenario implementatie:

- Zorg ervoor dat de toepassing gebruikmaakt van gebruikersnaam en wachtwoord voor verificatie. Op basis van certificaat/Smartcard verificatie wordt niet ondersteund door Azure AD Domain Services.

- U kunt wachtwoorden rechtstreeks ten opzichte van het domein dat beheerde niet wijzigen. Eindgebruikers op hun wachtwoord kunnen wijzigen met Azure AD selfservice-wachtwoord wijzigen om of ten opzichte van de on-premises adreslijst. Deze wijzigingen worden automatisch gesynchroniseerd en beschikbaar zijn in de beheerde domein.


## <a name="azure-remoteapp"></a>Azure RemoteApp
Azure RemoteApp kan de beheerder van Contoso te maken van een siteverzameling domein behoren. Deze functie kunnen externe toepassingen die door Azure RemoteApp wordt bediend om uit te voeren op een domein behoren computers en voor toegang tot andere resources met geïntegreerde Windows-verificatie. Contoso kunt Azure AD Domain Services gebruiken om een beheerde domein wordt gebruikt door Azure RemoteApp domein behoren verzamelingen bieden.

![Azure RemoteApp](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

Voor meer informatie over dit implementatiescenario, Zie het artikel Remote Desktop Services Blog [Lift en shift uw werkbelasting met Azure RemoteApp en Azure AD-domeinservices](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx).
