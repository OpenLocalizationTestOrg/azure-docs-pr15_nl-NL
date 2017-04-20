<properties
    pageTitle="Een domein behoren apparaten verbinden met Azure AD voor Windows 10-ervaringen | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe kunnen beheerders Groepsbeleid om in te schakelen apparaten moeten domein die is gekoppeld aan het netwerk voor ondernemingen configureren."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen

Domein join is dat de gebruikelijke manier om organisaties zijn verbonden apparaten voor werk voor de afgelopen 15 jaar en meer. Dit heeft ingeschakeld gebruikers kunnen aanmelden bij hun apparaten met behulp van hun Windows Server Active Directory (Active Directory) werk- of schoolaccounts en toegestane IT volledig deze apparaten beheren. Organisaties meestal zijn afhankelijk van de afbeeldingen methoden voor het inrichten apparaten aan gebruikers en doorgaans gebruikmaken van System Center Configuration Manager (SCCM) of Groepsbeleid te beheren.

Domein-join in Windows 10 bieden de volgende voordelen wanneer u de apparaten met Azure Active Directory (Azure AD verbonden):

- Eenmalige aanmelding (SSO) naar Azure AD-bronnen vanaf elke locatie
- Toegang tot de onderneming Windows Store met behulp van werk of school accounts (geen Microsoft-account vereist)
- Enterprise-compatibele roaming van gebruikersinstellingen op apparaten met behulp van werk of school accounts (geen Microsoft-account vereist)
- Sterke verificatie en handige aanmelden voor werk of school-accounts met Microsoft Passport en Windows Hallo
- Mogelijkheid om te toegang beperken tot alleen apparaten die voldoen aan de groepsbeleidsinstellingen organisatie-apparaat

## <a name="prerequisites"></a>Vereisten voor

Domein join blijft nuttig zijn. Echter te krijgen van de Azure AD-voordelen van eenmalige aanmelding, roaming met werk of schoolaccounts en access-instellingen naar de Windows Store met accounts voor werk of school, moet u de volgende handelingen uit:

- Azure AD-abonnement
- Azure AD Connect uit te breiden de on-premises adreslijst Azure AD
- Beleid dat is ingesteld op een domein behoren apparaten verbinden met Azure AD
- Windows 10 opbouwen (build 10551 of hoger) voor apparaten

Als u wilt inschakelen Microsoft Passport voor werk en Windows Hallo, moet u ook het volgende:

- Openbare sleutel sleutelinfrastructuur voor gebruiker certificaten uitgifte.
- System Center Configuration Manager versie 1509 voor Technical Preview. Zie [Microsoft System Center Configuration Manager Technical Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) en [System Center Configuration Manager-teamblog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)voor meer informatie. Dit is vereist voor het implementeren van certificaten op basis van Microsoft Passport toetsen.

Als alternatief voor de vereiste PKI-implementatie, kunt u het volgende doen:

- Hebben een paar domeincontrollers met Windows Server 2016 Active Directory Domain Services.

Als u wilt voorwaardelijke toegang inschakelt, kunt u groepsbeleidsinstellingen die toegang geeft tot domein behoren apparaten waarop geen aanvullende installaties. Als u wilt beheren toegangsbeheer op basis van de naleving van het apparaat, moet u het volgende:

- System Center Configuration Manager versie 1509 voor Technical Preview voor Passport scenario 's

## <a name="deployment-instructions"></a>Aanwijzingen voor implementatie



### <a name="step-1-deploy-azure-active-directory-connect"></a>Stap 1: Implementatie van Azure Active Directory verbinding maken

Azure AD Connect kunt u voor het inrichten van lokale computers als apparaatobjecten in de cloud. Als u wilt implementeren Azure AD Connect, raadpleegt u "installatiefout Azure AD Connect' in het artikel [uw on-premises implementatie identiteiten met Azure Active Directory integreren](active-directory-aadconnect.md#install-azure-ad-connect).

 - Als u een [aangepaste installatie voor Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md) (niet de normale installatie) hebt gevolgd, voert u de procedure **maken een verbindingspunt service in Active Directory van on-premises implementatie**, verderop in deze stap.
 - Als er een federatieve configuratie met Azure AD voordat u installeert Azure AD Connect (bijvoorbeeld als u Active Directory Federation Services (AD FS) hebt geïmplementeerd voordat), volgt u de **regels voor het claimen van configureren AD FS** -procedure, verderop in deze stap.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Maken van een serviceverbindingspunt in de lokale Active Directory

Een domein behoren apparaten gebruiken het verbindingspunt dat service te vinden, gegevens van Azure AD-tenant op het moment van automatische registratie met de registratie-service van Azure apparaat.

Voer de volgende PowerShell-opdrachten op de server Azure AD Connect:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Wanneer u zich de cmdlet $aadAdminCred = Get-referentie, gebruikt u de notatie *user@example.com* voor de gebruikersnaam van de referenties die wordt ingevoerd wanneer de Get-referentie pop-up wordt weergegeven.

Wanneer u zich de cmdlet initialisatie-ADSyncDomainJoinedComputerSync..., vervangt u [*naam van de connector-account*] door het domeinaccount dat wordt gebruikt als de Active Directory-connector-account.

#### <a name="configure-ad-fs-claim-rules"></a>Regels voor het claimen van AD FS configureren
Configuratie van de regels voor het claimen van AD FS kunt momentopname registratie van een computer met Azure apparaat registratieservice doordat computers om te verifiëren met behulp van Kerberos/NTLM via AD FS. Zonder deze stap krijgt computers naar Azure AD zodanig vertraagde (afhankelijk van Azure AD Connect synchronisatie tijden).

>[AZURE.NOTE]
Als u geen AD FS als de Federatie server on-premises implementatie, volgt u de instructies van de leverancier van uw maken van de regels claimen.

Klik op de AD FS-server (of op een sessie die is verbonden met de AD FS-server), voert u de volgende PowerShell-opdrachten:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10 computers verifieert met behulp van geïntegreerde Windows-verificatie naar een actieve WS-Trust eindpunt gehost door AD FS. Zorg ervoor dat dit eindpunt is ingeschakeld. Als u de Web-verificatieproxy gebruikt, ook voor zorgen dat dit eindpunt wordt gepubliceerd via de proxy. U kunt dit doen door te schakelen van de adfs-services/vertrouwen/13/windowstransport. Deze moet worden weergegeven als ingeschakeld in de AD FS-beheerconsole onder **Service** > **eindpunten**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Stap 2: Configureer automatische apparaatregistratie via Groepsbeleid in Active Directory

U kunt in Active Directory groepsbeleid voor het configureren van uw Windows 10 domein behoren apparaten automatisch met Azure AD kunnen registreren.

> [AZURE.NOTE]
> Meest recente Zie voor instructies voor het instellen van automatische apparaatregistratie, [het instellen van automatische registratie van Windows-domein apparaten met Azure Active Directory die zijn gekoppeld](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Deze sjabloon voor een Groepsbeleid heeft gekregen in Windows 10. Als u het hulpprogramma Groepsbeleid vanaf een computer met Windows 10 uitvoert, wordt het beleid wordt weergegeven als: <br>
> **Domein toegevoegd computers registreren als apparaten**<br>
> Het beleid is in de volgende locatie:<br>
> ***Computer configuratie/beleidsregels/administratieve sjablonen en/of Windows-onderdelen/apparaatregistratie***


## <a name="additional-information"></a>Aanvullende informatie
* [Windows 10 voor de onderneming: manieren om apparaten voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Meer informatie over het gebruik scenario's voor het deelnemen aan Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Deelnemen aan Azure AD instellen](active-directory-azureadjoin-setup.md)
