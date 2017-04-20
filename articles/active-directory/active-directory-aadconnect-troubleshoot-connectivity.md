<properties
    pageTitle="Azure AD Connect: Problemen met verbinding oplossen | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe u problemen met verbindingen met Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Problemen met verbindingen met Azure AD Connect
In dit artikel wordt uitgelegd hoe de connectiviteit tussen Azure AD Connect en Azure AD werkt en het oplossen van problemen met de serververbinding. Deze problemen zijn waarschijnlijk kunnen worden bekeken in een omgeving met een proxyserver.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Problemen met verbinding in de installatiewizard
Azure AD Connect is moderne verificatie (met de ADAL bibliotheek) gebruikt voor verificatie. De wizard installeren en de synchronisatie-engine BEGINLETTERS vereisen machine.config correct is geconfigureerd aangezien die .NET-toepassingen.

In dit artikel wordt wordt weergegeven hoe Fabrikam verbinding maakt met Azure AD via de proxy. De proxyserver heet fabrikamproxy en poort 8080 wordt gebruikt.

Eerst moeten we Controleer of [**machine.config**](active-directory-aadconnect-prerequisites.md#connectivity) correct is geconfigureerd.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
In sommige niet-Microsoft-blogs wordt dat moet worden gewijzigd miiserver.exe.config in plaats daarvan beschreven. Dit bestand is echter overschreven op elke upgrade en ondanks dat als dit werkt tijdens de eerste installatie, het systeem werkt niet meer op de eerste upgrade. Om die reden dan ook is de aanbeveling machine.config in plaats daarvan bijwerken.

De proxyserver moet ook de vereiste URL's geopend. De officiële lijst wordt beschreven in [Office 365-URL's en IP-adresbereiken ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

De volgende tabel is van de volgende, absolute minimaal moeten kunnen verbinding maken met Azure AD helemaal. Deze lijst bevat geen een optionele functies, zoals wachtwoord write-backs of Azure AD verbinding status. Dit wordt hier beschreven om u te helpen bij het oplossen van voor de eerste configuratie.

URL | Poort | Beschrijving
---- | ---- | ----
mscrl.Microsoft.com | HTTP-/ 80 | Wordt gebruikt voor het downloaden van CRL lijsten.
\*. verisign.com | HTTP-/ 80 | Wordt gebruikt voor het downloaden van CRL lijsten.
\*. entrust.com | HTTP-/ 80 | Wordt gebruikt voor het downloaden van CRL lijsten voor MFA.
\*. windows.net | HTTPS/443 | Wordt gebruikt voor het aanmelden bij Azure AD.
Secure.aadcdn.microsoftonline-p.com | HTTPS/443 | Wordt gebruikt voor MFA.
\*. microsoftonline.com | HTTPS-443 | Wordt gebruikt voor het configureren van uw adreslijst Azure AD en gegevens importeren/exporteren.

## <a name="errors-in-the-wizard"></a>Fouten in de wizard
De installatiewizard twee verschillende beveiligingscontexten gebruikt. Klik op de pagina **verbinding maken met Azure AD** worden dat de gebruiker momenteel is aangemeld gebruikt. Op de pagina **configureren** verandert dit de [account de service voor de synchronisatie-engine uitgevoerd](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts). De proxyconfiguraties we geven globale met de computer zijn zodat als er een probleem, het probleem met de meest wordt waarschijnlijk al op de pagina **verbinding maken met Azure AD** in de wizard weergegeven.

Hierna ziet u de meest voorkomende fouten u in de installatiewizard ziet.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>De installatiewizard is niet correct geconfigureerd
Deze fout wordt weergegeven wanneer de wizard zelf de proxy kan niet worden bereikt.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Als u dit ziet, controleert u of dat de [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) correct is geconfigureerd.
- Als die er goed uitziet, volgt u de stappen in [controleren proxy connectivity](#verify-proxy-connectivity) om te zien als het probleem zich buiten de wizard ook.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>Het eindpunt MFA kan niet worden bereikt.
Deze fout wordt weergegeven als het eindpunt **https://secure.aadcdn.microsoftonline-p.com** kan niet worden bereikt en uw globale beheerder ingeschakeld MFA heeft.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Als u dit ziet, controleert u of dat de eindpunt secure.aadcdn.microsoftonline-p.com is toegevoegd aan de proxy.

### <a name="the-password-cannot-be-verified"></a>Het wachtwoord kan niet worden gecontroleerd
Als de installatiewizard geslaagd is in verbinding maken met Azure AD, maar het wachtwoord zelf kan niet worden geverifieerd ziet u dit: ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- Is het wachtwoord van een tijdelijk wachtwoord en moet worden gewijzigd? Is het dat is wel het juiste wachtwoord? Wil aanmelden bij https://login.microsoftonline.com (op een andere computer dan de Azure AD Connect-server) en controleer of dat het account kan worden gebruikt.

### <a name="verify-proxy-connectivity"></a>Controleer of de proxy-connectiviteit
Om te bevestigen als de server Azure AD Connect werkelijke connectiviteit met de Proxy en Internet heeft we enkele PowerShell gebruiken om te zien als de proxy webaanvragen is toestaan of niet. Voer in een PowerShell-prompt `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Technisch de eerste keer is voor het https://login.microsoftonline.com en dit ook werkt, maar de andere URI is sneller reageren.)

PowerShell gebruikt de configuratie machine.config contact opnemen met de proxy. De instellingen in winhttp/netsh moeten niet van invloed zijn op deze cmdlets.

Als de proxy correct is geconfigureerd, moet u de status van een success: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Als u **geen verbinding maken met de externe server** ontvangt vervolgens PowerShell probeert te bellen met directe zonder de proxy of DNS niet juist is geconfigureerd. Zorg ervoor dat het bestand **machine.config** correct is geconfigureerd.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Als de proxy is niet correct is geconfigureerd, krijgt we een foutmelding: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Fout | Fouttekst | Opmerking
---- | ---- | ---- |
403 | Verboden | De proxy is voor de gevraagde URL niet geopend. Terugkeren naar de proxyconfiguratie en zorg ervoor dat de [URL's](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) zijn geopend.
407 | Proxyverificatie vereist | De proxyserver vereist aanmelden en geen is opgegeven. Als uw proxyserver verificatie is vereist, zorg ervoor dat u hebt dit geconfigureerd in de machine.config. Ook voor zorgen dat u domeinaccounts gebruikt voor de gebruiker met de wizard ook als voor de serviceaccount.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Het communicatiepatroon tussen Azure AD Connect en Azure AD
Als u alle volgende stappen uit boven en nog steeds geen verbinding kunt maken hebt gevolgd kunt u nu beginnen netwerk logboeken kijken. In dit gedeelte is een patroon normale en succesvolle connectivity documenteert. Het is ook algemene rood herrings die kan worden genegeerd als u de logboeken netwerk leest vermelding.

- Er worden oproepen naar https://dc.services.visualstudio.com. Het is niet verplicht moet deze openen in de proxy voor de installatie te kunnen uitvoeren en deze kunnen worden genegeerd.
- U ziet dat DNS-resolutie wordt een lijst met de werkelijke hosts om te worden in de DNS-naam ruimte nsatc.net en andere naamruimten niet onder microsoftonline.com. Echter er worden niet alle serviceaanvragen web op de werkelijke servernamen en u geen hebt u deze naar de proxy.
- De eindpunten adminwebservice provisioningapi (Zie hieronder in de logboeken) discovery eindpunten en zijn gebruikt om te vinden van de werkelijke eindpunt te gebruiken en wordt worden verschilden afhankelijk van uw regio.

### <a name="reference-proxy-logs"></a>Verwijzing proxy-Logboeken
Hier volgt een die worden gemaakt bij een logboek werkelijke proxy en de installatiepagina van de wizard uit waar deze is genomen (hetzelfde eindpunt van dubbele items zijn verwijderd). Dit kan worden gebruikt als uitgangspunt voor uw eigen logboeken proxy en netwerk. De werkelijke eindpunten mogelijk anders in uw omgeving (met name in *cursief*).

**Verbinding maken met Azure AD**

Tijd | URL
--- | ---
1/11/2016 8:31 | Connect://login.microsoftonline.com:443
1/11/2016 8:31 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:32 | verbinding maken: / /*bba800-anker*. microsoftonline.com:443
1/11/2016 8:32 | Connect://login.microsoftonline.com:443
1/11/2016 8:33 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:33 | verbinding maken: / /*bwsc02-relay*. microsoftonline.com:443

**Configureren**

Tijd | URL
--- | ---
1/11/2016 8:43 | Connect://login.microsoftonline.com:443
1/11/2016 8:43 | verbinding maken: / /*bba800-anker*. microsoftonline.com:443
1/11/2016 8:43 | Connect://login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | verbinding maken: / /*bba900-anker*. microsoftonline.com:443
1/11/2016 8:44 | Connect://login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | verbinding maken: / /*bba800-anker*. microsoftonline.com:443
1/11/2016 8:44 | Connect://login.microsoftonline.com:443
1/11/2016 8:46 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:46 | verbinding maken: / /*bwsc02-relay*. microsoftonline.com:443

**Eerste synchronisatie**

Tijd | URL
--- | ---
1/11/2016 8:48 | Connect://login.Windows.NET:443
1/11/2016 8:49 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:49 | verbinding maken: / /*bba900-anker*. microsoftonline.com:443
1/11/2016 8:49 | verbinding maken: / /*bba800-anker*. microsoftonline.com:443

## <a name="authentication-errors"></a>Verificatiefouten
Deze sectie bevat fouten die kunnen worden geretourneerd uit ADAL (de verificatie-bibliotheek die worden gebruikt door Azure AD Connect) en PowerShell. De fout uitleg over kunt u in de volgende stappen begrijpen.

### <a name="invalid-grant"></a>Ongeldige verlenen
Ongeldige gebruikersnaam of wachtwoord. Zie [het wachtwoord kan niet worden gecontroleerd](#the-password-cannot-be-verified) voor meer informatie.

### <a name="unknown-user-type"></a>Type onbekende gebruiker
Het telefoonboek van uw Azure AD kan niet worden gevonden of opgelost. Wellicht wilt u aanmelden met een gebruikersnaam in een niet-geverifieerde domein?

### <a name="user-realm-discovery-failed"></a>Gebruiker Realm Discovery is mislukt
Configuratieproblemen voor netwerk- of proxyserver. Het netwerk kan niet worden bereikt, raadpleegt u [problemen connectivity oplossen in de installatiewizard](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Gebruikerswachtwoord is verlopen
Uw referenties zijn verlopen. Uw wachtwoord wijzigen.

### <a name="authorizationfailure"></a>AuthorizationFailure
Onbekend probleem.

### <a name="authentication-cancelled"></a>Verificatie geannuleerd
De uitdaging meervoudige verificatie (MFA) is geannuleerd.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Verificatie is gelukt, maar er is een verificatieprobleem met Azure AD PowerShell.

### <a name="azurerolemissing"></a>AzureRoleMissing
Verificatie is gelukt. U bent niet een globale beheerder.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Verificatie is gelukt. Bevoegdheden identiteitsbeheer is ingeschakeld en u momenteel niet een globale beheerder. Zie [Bevoegdheden identiteitsbeheer](active-directory-privileged-identity-management-getting-started.md) voor meer informatie.

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Verificatie is gelukt. Kan geen bedrijfsgegevens ophalen van Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Verificatie is gelukt. Kan geen domeingegevens ophalen van Azure AD.

## <a name="troubleshooting-steps-for-previous-releases"></a>Stappen voor probleemoplossing voor eerdere versies.
Met versies beginnen met het versienummer 1.1.105.0 (uitgebracht februari 2016) is de aanmeldhulp ingetrokken. In deze sectie en de configuratie niet langer is vereist, maar als verwijzing wordt bewaard.

Voor de eenmalige aanmelding in assistent om te werken, moet winhttp worden geconfigureerd. Dit kunt doen met [**netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![Netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>De aanmeldhulp is niet correct geconfigureerd
Deze fout wordt weergegeven wanneer de aanmeldhulp geen toegang tot de proxy of de proxy is niet toestaan van het verzoek.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Als u dit ziet, kijkt u naar de proxyconfiguratie van in [netsh](active-directory-aadconnect-prerequisites.md#connectivity) en controleer of dat deze juist is.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Als die er goed uitziet, volgt u de stappen in [controleren proxy connectivity](#verify-proxy-connectivity) om te zien als het probleem zich buiten de wizard ook.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
