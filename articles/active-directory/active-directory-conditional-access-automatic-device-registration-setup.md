<properties
    pageTitle="Automatische registratie van Windows domein behoren apparaten met Azure Active Directory instellen | Microsoft Azure"
    description="Uw domein behoren Windows-apparaten instellen voor het automatisch en stilte met Azure Active Directory registreren."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Automatische registratie van Windows instellen met een domein behoren apparaten met Azure Active Directory

Als u via [voorwaardelijke toegang Azure Active Directory op basis van het apparaat](active-directory-conditional-access.md), moeten uw Windows-computers domein behoren zijn geregistreerd met Azure Active Directory (Azure AD). In dit artikel leert u wat u moet doen als u de registratie van Windows domein behoren apparaten met Azure AD in uw organisatie wilt instellen.

Voorwaardelijke access gebruiken in Azure AD bevat de volgende voordelen:

-   Verbeterde eenmalige aanmelding (SSO)-ervaring in Azure AD-apps tot en met accounts voor werk of school
-   Enterprise roaming van instellingen op apparaten
-   Toegang tot Windows Store voor bedrijven
-   Sterker verificatie en handige aanmeldingsproblemen met Windows Hallo

> [AZURE.NOTE] De Update voor Windows 10 November biedt enkele van de verbeterde gebruikerservaring in Azure AD, maar het bijwerken van Windows 10 jubileum volledig apparaat gebaseerde voorwaardelijke toegang ondersteunt. Zie voor meer informatie over voorwaardelijke toegang, [voorwaardelijke toegang Azure Active Directory op basis van het apparaat](active-directory-conditional-access.md). Zie voor meer informatie over Windows 10-apparaten in het bedrijf en hoe een gebruiker een Windows-10-apparaat registreert bij Azure AD [Windows 10 voor de onderneming: apparaten gebruikt voor werk](active-directory-azureadjoin-windows10-devices-overview.md).

U kunt bepaalde eerdere versies van Windows, zoals deze versies registreren:

-   Windows 8.1
-   Windows 7

Als u een computer met Windows Server als een bureaublad gebruikt, kunt u deze platforms registreren:

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2


## <a name="prerequisites"></a>Vereisten voor

De belangrijkste vereiste voor automatische registratie van een domein behoren apparaten met behulp van Azure AD is een recente versie van Azure Active Directory Connect (Azure AD Connect) hebben.

Afhankelijk van hoe u Azure AD Connect geïmplementeerd en of u een expliciete of aangepaste installatie of een in-place upgrade gebruikt, de volgende vereisten mogelijk zijn geconfigureerd automatisch:

-   **Service-verbindingspunt in lokale Active Directory**. Voor detectie van gegevens van Azure AD-tenant met de computers registreren die voor Azure AD.

-   **Active Directory Federation Services (AD FS) uitgifte transformeren regels**. Voor de computerverificatie bij registratie (geldt voor federatieve configuraties).

Sommige apparaten in uw organisaties ontbreken in Windows 10-apparaten domein behoren, Controleer of dat u de volgende taken uitvoeren:

- Een beleid instellen in Azure AD, zodat gebruikers kunnen apparaten registreren
- Geïntegreerde Windows-verificatie als een geldige alternatief voor meervoudige verificatie in AD FS instellen



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>Een serviceverbindingspunt voor detectie van de Azure AD-tenant instellen

Een service connection punt-object moet bestaan in de configuratie naming context partition van de bos van het domein waar computers zijn gekoppeld. Het verbindingspunt dat service bevat discovery-informatie over de Azure AD-tenant waar computers registreren. In een meerdere bos Active Directory-configuratie, moet het verbindingspunt dat service bestaan in alle forests met computers domein behoren.

Het verbindingspunt dat service op deze locaties in Active Directory instellen:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Voor een bos met de Active Directory domain naam *example.com*, de configuratie naming context CN is = configuratie, domeincontroller = voorbeeld, domeincontroller = com.

U kunt de Azure AD-tenant object en discovery-waarden controleren met behulp van de volgende Windows PowerShell-script. (De context voor de configuratie-naam in het voorbeeld vervangen door uw configuratie naming context.)

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

De `$scp.Keywords` uitvoer ziet u de gegevens van de Azure AD-tenant:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Als het verbindingspunt dat service niet bestaat, maakt u er door het volgende PowerShell-script op de server Azure AD Connect uit te voeren:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Wanneer u uitvoert `$aadAdminCred = Get-Credential`, gebruikt u de notatie *user@example.com* voor de gebruikersnaam in het dialoogvenster **Get-referentie** .  
> Wanneer u de cmdlet initialisatie-ADSyncDomainJoinedComputerSync uitvoert, moet u [*naam van de connector-account*] vervangen door het domeinaccount dat wordt gebruikt in de Active Directory-connector-account.  
> De cmdlet wordt de Active Directory-PowerShell-module, is afhankelijk van Active Directory Web Services in een domeincontroller gebruikt. Active Directory Web Services wordt ondersteund domeincontroller in Windows Server 2008 R2 en nieuwere versies. De API System.DirectoryServices via PowerShell gebruiken om te maken van het verbindingspunt dat service voor Jan Smit in Windows Server 2008 of eerdere versies, en vervolgens de waarden trefwoorden toewijzen.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>Maak regels voor AD FS voor direct apparaatregistratie in federatieve organisaties

In een federatieve Azure AD-configuratie, apparaten zijn gebaseerd op AD FS (of op de federatieserver on-premises implementatie) om Azure AD te verifiëren. Ze zich vervolgens registreren ten opzichte van de Azure Active Directory apparaat registratieservice (Azure AD apparaat registratieservice).

> [AZURE.NOTE] Klik in een niet-federatief configuratie, ogen zijn gesynchroniseerd met Azure AD en Windows 10 en Windows Server 2016 domein behoren computers worden geverifieerd bij registratie-Service van Azure AD-apparaat. Een gebruiker wordt geverifieerd met behulp van een referentie die de gebruiker naar hun computeraccounts on-premises implementatie schrijft en dat is doorgegeven aan Azure AD via Azure AD Connect. Voor Windows 10 en Windows Server 2016 computers in een niet-federatief configuratie hebt u opties voor het instellen van een apparaat gebaseerde certificeringsinstantie in uw organisatie. Zie de sectie downloaden Windows Installer-pakketten voor Windows 10-computers in dit artikel voor meer informatie.

Voor Windows 10 en Windows Server 2016 computers koppelt Azure AD Connect het apparaatobject in Azure AD met de lokale computer account-object. De volgende claims moeten bestaan tijdens de verificatie voor Azure AD apparaat registratieservice om registratie voltooien en het apparaatobject te maken:

- http://schemas.Microsoft.com/ws/2012/01/accounttype bevat de waarde DJ, waarmee de hoofdsom verificator als een computer domein behoren.
- http://schemas.Microsoft.com/Identity/claims/onpremobjectguid bevat de waarde van het kenmerk **objectGUID** van het account van de lokale computer.
- http://schemas.Microsoft.com/ws/2008/06/Identity/claims/primarysid bevat van de computer primaire beveiliging-id, dat met de kenmerkwaarde **objectSid** van de on-premises implementatie-computeraccount overeenkomt.
- http://schemas.Microsoft.com/ws/2008/06/Identity/claims/issuerid bevat de waarde die Azure AD gebruikt om te vertrouwen het token uitgegeven van AD FS of vanuit de on-premises implementatie beveiliging Token-Service (STS). Dit is belangrijk in een meerdere bos Active Directory-configuratie. In deze configuratie wordt mogelijk een bos behalve het gedeelte dat is verbonden met Azure AD bij de AD FS of on-premises STS computers lid. Gebruik de waarde voor de AD FS, http://<*domeinnaam*>/adfs/services/vertrouwen /, waarbij <*domeinnaam*> de gevalideerde domeinnaam zich in Azure AD.

Als u wilt maken van deze regels handmatig in AD FS, gebruikt u het volgende PowerShell-script in een sessie die is verbonden met uw server. De eerste regel vervangen door de naam van uw organisatie gevalideerde-domein in Azure AD.

> [AZURE.NOTE] Als u niet de AD FS voor uw on-premises implementatie federatieserver gebruikt, volgt u van de leverancier instructies voor het maken van de regels die deze claims verlenen.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] Gebruik alleen de eerste drie regels als uw omgeving on-premises is één. Als uw computers zijn in een ander domein dan de synchroniseren met Azure AD of als het u alternatieve namen als u de kleuren die in de synchronisatieconfiguratie wilt gebruiken, moet u ook de resterende regels opnemen.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Windows 10 en Windows Server 2016 domein behoren computers verifiëren met geïntegreerde Windows om een actieve WS-Trust eindpunt die wordt gehost door AD FS. Zorg ervoor dat het eindpunt actief is. Als u de Web-toepassingsproxy gebruikt, moet u dat dit eindpunt wordt gepubliceerd via de proxy. Het eindpunt is adfs/services/vertrouwen/13/windowstransport. Om te controleren of deze actief is, in de AD FS-beheerconsole Ga naar **Service** > **eindpunten**. Als u niet de AD FS voor uw on-premises implementatie federatieserver gebruikt, volgt u de instructies van uw leverancier om ervoor te zorgen dat het bijbehorende eindpunt actief is.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>AD FS voor de verificatie van apparaatregistratie instellen

Zorg dat de geïntegreerde Windows is ingesteld als een geldige alternatief voor meervoudige verificatie voor apparaatregistratie in AD FS. Hiervoor moet u beschikken over een regel die de verificatiemethode passeert transformeren uitgifte.

1. In de AD FS-beheerconsole, gaat u naar de **AD FS** > **Vertrouwensrelaties** > **Partijen vertrouwensrelaties te vertrouwen**.

2. Met de rechtermuisknop op het Microsoft Office 365-identiteit Platform gebruikmakende partij vertrouwensobject en selecteer vervolgens **Claimen regels bewerken**.

3.  Selecteer op het tabblad **Regels voor uitgifte transformeren** **Regel toevoegen**.

4.  Selecteer **een aangepaste regel Claims verzenden**in de lijst van de sjabloon **claimen regel** .

5.  Selecteer **volgende**.

6.  Voer in het vak **naam van de regel Claim** **Auth methode claimen regel**.

7.  Voer de volgende opdracht in het vak **claimen regel** :

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Voer de volgende PowerShell-opdracht op de federatieserver:

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> is de naam van het gebruikmakende partij object voor uw Azure AD gebruikmakende partij vertrouwensobject. Dit object is meestal de naam *Microsoft Office 365-identiteit Platform*.



## <a name="deployment-and-rollout"></a>Implementatie- en -uitrol

Een domein behoren computers aan de vereisten voldoen, worden deze klaar met Azure AD registreren.

De computers van Windows 10 jubileum Update en Windows Server 2016 domein behoren registreren automatisch met Azure AD de volgende keer dat het apparaat opnieuw wordt gestart of wanneer een gebruiker zich aanmeldt bij Windows. Nieuwe computers die zijn toegevoegd aan het domein hebt geregistreerd met Azure AD wanneer het apparaat opnieuw wordt gestart na de join-bewerking van het domein.

> [AZURE.NOTE] Windows 10 domein behoren computers worden automatisch geregistreerd met Azure AD alleen als het uitrol Groepsbeleid-object is ingesteld.

U kunt een object Groepsbeleid gebruiken om te bepalen de implementatie van automatische registratie van Windows 10 en Windows Server 2016 domein behoren computers. Als u wilt implementeren van automatische registratie van Windows 10 domein behoren computers, kunt u een Windows-installatiepakket implementeren op computers die u selecteert.

> [AZURE.NOTE] Het groepsbeleid voor uitrol besturingselement activeert ook de registratie van Windows 8.1 domein behoren computers. U kunt het beleid voor het registreren van Windows 8.1 domein behoren computers. Of, als u een combinatie van Windows-versies, met inbegrip van versies van Windows 7 of Windows Server, hebt u al uw Windows 10 en Windows Server 2016 computers met behulp van een Windows-installatiepakket kunt registreren.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Een object om te bepalen de implementatie van automatische registratie maakt

Om te bepalen de implementatie van automatische registratie van een domein behoren computers met Azure AD, kunt u het groepsbeleid **domein behoren computers als apparaten registreren** implementeren met de computers die u wilt registreren. U kunt bijvoorbeeld het beleid implementeren naar een organisatie-eenheid of naar een beveiligingsgroep.

Als u wilt instellen het beleid, voer deze stappen uit:

1. Open Serverbeheer en ga vervolgens naar **Extra** > **Groepsbeleid beheren**.

2. Ga naar het knooppunt van het domein dat overeenkomt met het domein waar u wilt activeren automatische registratie van Windows 10 of Windows Server 2016 computers.

3. Met de rechtermuisknop op het **Beleid-objecten groeperen**en selecteer **Nieuw**.

4. Voer een naam voor het Groepsbeleid-object. Stel *Automatische registratie naar Azure AD*. Selecteer **OK**.

4. Met de rechtermuisknop op het nieuwe Groepsbeleid-object en selecteer vervolgens **bewerken**.

5. Ga naar de **Computerconfiguratie** > **beleidsregels** > **administratieve sjablonen** > **Windows-onderdelen** > **apparaatregistratie**. Met de rechtermuisknop op de **Register-domein computers als apparaten die zijn gekoppeld**en selecteer vervolgens **bewerken**.

    > [AZURE.NOTE] Deze sjabloon Groepsbeleid heeft gekregen uit eerdere versies van de Groepsbeleid Management console. Als u een eerdere versie van de console gebruikt, gaat u naar de **Computerconfiguratie** > **beleidsregels** > **Administratieve sjablonen** > **Windows-onderdelen** > **Bedrijf Join** > **automatisch werkplek join clientcomputers**.

6. Selecteer **ingeschakeld**en selecteer vervolgens **toepassen**.

7. Selecteer **OK**.

8. Het Groepsbeleid-object een koppeling naar een locatie van uw keuze. Bijvoorbeeld, kunt u deze koppelen aan een specifieke organisatie-eenheid. U kan ook deze koppelen aan een specifieke beveiligingsgroep met de computers die automatisch met Azure AD registreren. Koppel u dit beleid instellen voor alle domein behoren Windows 10 en Windows Server 2016 computers in uw organisatie, het Groepsbeleid-object aan het domein.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>Downloaden van Windows Installer-pakketten voor Windows 10-computers  

Als u wilt een domein behoren computers met Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 hebt geregistreerd, kunt u downloaden en installeren van deze pakketbestanden van Windows Installer (msi):

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Het pakket implementeren met behulp van een verdeling systeem voor software zoals System Center Configuration Manager. Het pakket ondersteunt de stille standaardinstallatie-opties met de *stille* parameter. System Center Configuratiemanager 2016 biedt aanvullende voordelen uit eerdere versies, zoals de mogelijkheid voor het bijhouden van voltooide registraties. Voor meer informatie raadpleegt u [systeem Center 2016](https://www.microsoft.com/en-us/cloud-platform/system-center).

Het installatieprogramma maakt u een geplande taak op het systeem dat wordt uitgevoerd in de context van de gebruiker. De taak wordt gestart wanneer de gebruiker zich aanmeldt bij Windows. Het apparaat dat registreert de taak stilte met Azure AD met de gebruikersreferenties nadat u hebt geverifieerd via uitgebreid. Als u wilt zien van de geplande taak, gaat u naar **Microsoft** > **Bedrijf deelnemen aan**en klik vervolgens Ga naar de bibliotheek voor Taakplanner.



## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access.md)
