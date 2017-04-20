<properties
    pageTitle="Wat is de functie zelf registratie voor Azure? | Microsoft Azure"
    description="Een overzicht van selfservice-aanmelding voor Azure, hoe u het aanmeldingsproces beheert en hoe u een DNS-domeinnaam overnemen."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>Wat is de functie zelf registratie voor Azure?

In dit onderwerp wordt uitgelegd het selfservice aanmeldingsproces en hoe u een DNS-domeinnaam overnemen.  

## <a name="why-use-self-service-signup"></a>Het nut van selfservice-aanmelding?

- Klanten services die ze sneller wilt openen.
- Maken op basis van een e-mailbericht aanbiedingen voor een service.
- Maken op basis van een e-aanmelding stromen die snel gebruikers toestaan zich te maken met hun werk eenvoudig te onthouden e-mailaliassen identiteiten.
- Onbeheerde Azure mappen kunnen worden aangebracht in beheerde mappen later en opnieuw worden gebruikt voor andere services.

## <a name="terms-and-definitions"></a>Termen en definities

+ **Zelf te kunnen registreren**: dit is de manier waarop een gebruiker zich registreert voor een cloudservice en heeft een identiteit die automatisch worden gemaakt voor deze in Azure Active Directory (Azure AD) op basis van hun e-maildomein.
+ **Onbeheerde Azure-directory**: dit is de map waar die identiteit is gemaakt. Een onbeheerde directory is een map die geen globale beheerder heeft.
+ **E-geverifieerde gebruiker**: dit is een type gebruikersaccount in Azure AD. Een gebruiker met een automatisch gemaakt nadat u zich registreert voor een aanbieding selfservice is bekend als een gebruiker e-mail geverifieerd identiteit. Een gebruiker e-mail geverifieerd is een gewone lid van een map die is gelabeld met creationmethod = EmailVerified.

## <a name="user-experience"></a>Gebruikerservaring

Stel dat bijvoorbeeld een gebruiker wiens e-mailbericht zich Dan@BellowsCollege.com gevoelige bestanden via e-mail ontvangt. De bestanden die zijn beveiligd met Azure Rights Management (Azure RMS). Maar Dan de organisatie, balg universiteit krijgen studenten niet heeft aangemeld voor Azure RMS of heeft het Active Directory-RMS geïmplementeerd. In dit geval kunt Tjeerd registreren voor een gratis abonnement op RMS voor personen om te kunnen de beveiligde bestanden lezen.

Als Dan de eerste gebruiker met een e-mailadres van BellowsCollege.com is te registreren voor deze zelf te kunnen bieden, en vervolgens een onbeheerde directory wordt gemaakt voor BellowsCollege.com in Azure AD. Als andere gebruikers van het domein BellowsCollege.com zich aanmeldt voor deze aanbieding of een vergelijkbare selfservice aanbod, hebben ze ook e-mail geverifieerd gebruikersaccounts die zijn gemaakt in dezelfde map onbeheerde in Azure wordt aangegeven.

## <a name="admin-experience"></a>Beheerder-ervaring

Een beheerder van wie de DNS-naam van een onbeheerde Azure-directory kunt overnemen of de map na aan te tonen dat eigendom samenvoegen. De volgende secties wordt uitgelegd de admin-ervaring in meer detail, maar hier volgt een overzicht:

- Wanneer u een onbeheerde Azure-directory overnemen, worden u gewoon de globale beheerder van de onbeheerde adreslijst. Dit is een interne overname soms genoemd.
- Wanneer u afdrukken een onbeheerde Azure-directory samenvoegt, u de DNS-naam van de onbeheerde map aan uw beheerde Azure-directory toevoegen en een toewijzing van gebruikers aan resources wordt gemaakt zodat gebruikers kunnen blijven voor toegang tot services zonder onderbreking. Dit wordt ook wel een externe overname genoemd.

## <a name="what-gets-created-in-azure-active-directory"></a>Wat wordt gemaakt in Azure Active Directory?

#### <a name="directory"></a>Directory

- Een Azure Active Directory-map voor het domein dat wordt gemaakt, één map per domein.
- De map Azure AD heeft geen globale beheerder.

#### <a name="users"></a>Gebruikers

- Voor elke gebruiker die zich registreert, een gebruikersobject gemaakt in de adreslijst Azure AD.
- Elke gebruikersobject is gemarkeerd als externe.
- Elke gebruiker is toegang krijgen tot de service die ze zich hebt geregistreerd.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Hoe ik een selfservice Azure AD claimen map voor een domein dat ik eigenaar ben?

U kunt een selfservice Azure AD claimen directory door het domein valideren. Domeinverificatie blijkt dat u eigenaar bent van het domein door te maken van DNS-records.

Er zijn twee manieren een DNS-overname van een Azure AD-directory doen:

- interne overname (beheerder een onbeheerde Azure-directory ontdekt en wil omzetten in een beheerde map)
- externe overname (beheerder pogingen doen naar een nieuw domein hebt toegevoegd aan hun beheerde Azure-directory)

U misschien ook valideren dat u eigenaar bent van een domein omdat u op een onbeheerde directory duurt nadat een gebruiker uitgevoerd selfservice-aanmelding, of u mogelijk worden een nieuw domein aan een bestaande beheerde map toevoegt. Zoals u een naam contoso.com domein hebt en u wilt toevoegen van een nieuw domein met de naam contoso.co.uk of contoso.uk.

## <a name="what-is-domain-takeover"></a>Wat is de overname domein?  

In deze sectie wordt uitgelegd hoe u voor het valideren van dat u eigenaar bent van een domein

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Wat is domeinverificatie en waarom wordt het gebruikt?

Om te kunnen uitvoeren op een map, vereist Azure AD dat u eigenaar van het DNS-domein valideren.  Gegevensvalidatie van het domein kunt u de map en een niveau verhogen de selfservice-map aan een beheerde map, claimen of de map zelf samenvoegen in een bestaande beheerde map.

## <a name="examples-of-domain-validation"></a>Voorbeelden van het domein gegevensvalidatie

Er zijn twee manieren een DNS-overname van een map doen:

+ interne overname (bijvoorbeeld een beheerder een selfservice, onbeheerde adreslijst ontdekt en wil omzetten in beheerde map)
+ externe overname (bijvoorbeeld een beheerder probeert een nieuw domein toevoegen aan een beheerde directory)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Interne overname - verhogen van een map zelf, onbeheerde als u wilt een beheerde map

Wanneer u interne overname doet, wordt de map uit een onbeheerde adreslijst geconverteerd naar een beheerde map. U moet DNS-domein naam validatie, waar u een MX-record of een TXT-record in de DNS-zone maken voltooid. Deze actie:

+ Gevalideerd dat u eigenaar bent van het domein
+ Zorgt ervoor dat de map beheerd
+ Zorgt ervoor dat u de globale beheerder van de map

Stel dat een IT-beheerder van de school balg ontdekt dat gebruikers van de school voor selfservice aanbiedingen hebben geregistreerd. De geregistreerde eigenaar van de DNS-namen, BellowsCollege.com, kan de IT-beheerder valideren eigendom van de naam van de DNS-in Azure en maakt u onbeheerde Active Directory. De map wordt vervolgens een beheerde directory en de IT-beheerder de rol van globale beheerder voor de map BellowsCollege.com is toegewezen.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externe overname - een adreslijst selfservice samenvoegen in een bestaande beheerde map

In een externe overname u al een beheerde map hebt en u wilt dat alle gebruikers en groepen uit een onbeheerde adreslijst deel te nemen aan deze beheerde map in plaats eigen twee afzonderlijke mappen.

Als beheerder van een beheerde map toevoegen van een domein en dat domein gebeurt er met het hebben van een onbeheerde directory gekoppeld.

Stel dat bijvoorbeeld u een IT-beheerder bent en u al een beheerde map voor Contoso.com, een domeinnaam die is geregistreerd bij uw organisatie. U ontdekt die gebruikers van uw organisatie hebt uitgevoerd zelfregistratie omhoog voor een aanbod met behulp van e-domeinnaam user@contoso.co.uk, welke is een andere domeinnaam die uw organisatie beschikt. Accounts hebben die gebruikers in een onbeheerde directory voor contoso.co.uk.

U wilt niet beheren, twee afzonderlijke mappen, zodat u de onbeheerde map voor contoso.co.uk in het telefoonboek van uw bestaande IT beheerd voor contoso.com samenvoegen.

Externe overname volgt dezelfde DNS-validatie manier als interne overname.  Verschil wordt: gebruikers en -services opnieuw zijn toegewezen aan de beheerde IT-map.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Wat zijn de gevolgen van het uitvoeren van een externe overname?

Een toewijzing van gebruikers aan resources wordt met een externe overname gemaakt zodat gebruikers toegang tot services zonder onderbreking kunnen blijven. Veel toepassingen, waaronder RMS voor personen, moeite met de toewijzing van gebruikers aan resources en gebruikers kunnen voor toegang tot deze services ongewijzigd blijven. Als een toepassing de toewijzing van gebruikers aan resources niet effectief verwerkt, externe overname mogelijk expliciet geblokkeerd om te voorkomen dat gebruikers een slechte ervaring.

#### <a name="directory-takeover-support-by-service"></a>Directory overname ondersteuning door service

De volgende services ondersteunen momenteel overname:

- RMS


De volgende services wordt binnenkort ondersteunende overname:

- PowerBI

Het volgende niet doen en vereisen extra beheerderactie moet worden gegevens van na een externe overname migreren.

- SharePoint/OneDrive


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Het uitvoeren van een DNS-domein naam overname

Hebt u een paar opties voor het uitvoeren van een domein validatie (en een overname Voer desgewenst):

1.  Azure Management Portal

    Een overname wordt geactiveerd door te doen een domein toevoegen.  Als er al een map voor het domein bestaat, hebt u de optie voor het uitvoeren van een externe overname.

    Meld u aan bij de Azure-portal met uw referenties.  Ga naar het telefoonboek van uw bestaande en vervolgens naar **domein toevoegen**.

2.  Office 365

    U kunt de opties op de pagina [domeinen beheren](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) in Office 365 gebruiken om te werken met uw domeinen en DNS-records. Zie [uw domein verifiëren in Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).

3.  Windows PowerShell

    De volgende stappen zijn vereist om een validatie via Windows PowerShell te voeren.

    Stap    |   Gebruik te102826339-cmdlet
    ------- | -------------
    Een referentie-object maken | Get-referenties
    Verbinding maken met Azure AD | Verbinding maken met MsolService
    een lijst met domeinen   | Get-MsolDomain
    Een vraag maken  | Get-MsolDomainVerificationDns
    DNS-record maken   | Dit doet op uw DNS-server
    Controleer of de uitdaging    | Bevestig MsolEmailVerifiedDomain

Bijvoorbeeld:

1. Verbinding maken met Azure AD met de referenties die zijn gebruikt om te reageren op de aanbod selfservice: importeren-module MSOnline $msolcred = get-referentie verbinding-msolservice-$msolcred referenties

2. Krijg een lijst met domeinen:

    Get-MsolDomain

3. Voer de cmdlet Get-MsolDomainVerificationDns als u wilt maken van een vraag:

    Get-MsolDomainVerificationDns – domeinnaam *your_domain_name* – modus DnsTxtRecord

    Bijvoorbeeld:

    Get-MsolDomainVerificationDns – domeinnaam contoso.com – modus DnsTxtRecord

4. Kopieer de waarde die wordt geretourneerd door deze opdracht (waar).

    Bijvoorbeeld:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. In de naamruimte van uw openbare DNS, maak een DNS-txt-record met de waarde die u hebt gekopieerd in de vorige stap.

    De naam voor deze record is de naam van het bovenliggende domein, dus als u deze bronrecord maakt met behulp van de rol van de DNS van Windows Server, laat u de Record naam leeg en alleen plakken de waarde in het tekstvak

6. Voer de cmdlet bevestigen-MsolDomain om te controleren of de vraag:

    Bevestig MsolEmailVerifiedDomain - DomainName *your_domain_name*

    bijvoorbeeld:

    Bevestig MsolEmailVerifiedDomain - DomainName contoso.com

Een succesvolle uitdaging keert u terug naar de vraag zonder een fout.

## <a name="how-do-i-control-self-service-settings"></a>Hoe beheer ik selfservice-instellingen?

Beheerders hebben vandaag twee selfservice-besturingselementen. Ze kunnen bepalen:

- Gebruikers kunnen al dan niet deelnemen aan de map via e-mail.
- Gebruikers kunnen de zelf al dan niet licenties voor toepassingen en -services.


### <a name="how-can-i-control-these-capabilities"></a>Hoe kan ik deze mogelijkheden beheren?

Een beheerder kan deze mogelijkheden met deze Azure AD-cmdlet Set-MsolCompanySettings-parameters configureren:

+ **AllowEmailVerifiedUsers** bepaalt of een gebruiker kunt maken of deelnemen aan een onbeheerde directory. Als u deze parameter op $false instelt, kunnen geen e-geverifieerd gebruikers lid worden van de map.
+ **AllowAdHocSubscriptions** Hiermee stelt u de mogelijkheid voor gebruikers om uit te voeren zelfregistratie omhoog. Als u deze parameter op $false instelt, kunnen geen gebruikers zelf registratie uitvoeren.


### <a name="how-do-the-controls-work-together"></a>Hoe werken de besturingselementen samen?

Deze twee parameters kunnen worden gebruikt in combinatie meer controle over zelfregistratie omhoog definiëren. Bijvoorbeeld de volgende opdracht uit, kunnen gebruikers zelfregistratie uitvoeren, maar alleen, als deze gebruikers al een account in Azure AD (dat wil zeggen gebruikers die een e-geverifieerd-account moet worden gemaakt moet niet kunnen worden uitgevoerd zelfregistratie omhoog):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Het volgende stroomdiagram wordt uitgelegd van alle verschillende combinaties voor deze parameters en de resulterende voorwaarden voor de map en zelfregistratie omhoog.

![][1]

Zie [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)voor meer informatie en voorbeelden van het gebruik van deze parameters.

## <a name="see-also"></a>Zie ook

-  [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)

-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure-Cmdlet-verwijzing](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
