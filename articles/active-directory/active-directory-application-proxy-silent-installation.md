<properties
    pageTitle="Het installeren van de Azure AD-toepassing Proxy verbindingslijn stilte | Microsoft Azure"
    description="Behandelt hoe u een installatie op de achtergrond van Azure AD-toepassing Proxy verbindingslijn voor secure externe toegang tot uw apps on-premises implementatie uitvoert."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Het installeren van de Azure AD-toepassing Proxy verbindingslijn stilte

U wilt kunnen een installatiescript verzenden naar meerdere Windows-servers of Windows-Servers die geen gebruikersinterface die zijn ingeschakeld. In dit onderwerp wordt uitgelegd hoe u een Windows PowerShell-script waarmee de installatie zonder toezicht installeren en registreren van de Azure AD-toepassing Proxy verbindingslijn maakt.

## <a name="enabling-access"></a>Toegang inschakelen
Toepassingsproxy werkt door een klein Windows Server-service genoemd de verbindingslijn binnen uw netwerk te installeren. Voor de toepassing Proxy verbindingslijn om te werken deze moet worden geregistreerd bij uw Azure AD-directory met behulp van een globale beheerder en een wachtwoord. Dit is gewoonlijk ingevoerd tijdens de installatie van de verbindingslijn in het dialoogvenster een Venstermenu. U kunt ook kunt u Windows PowerShell gebruiken om een referentie-object als u wilt invoeren uw registratiegegevens te maken, of u kunt uw eigen token maken en deze gebruiken om in te voeren uw registratiegegevens.

## <a name="step-1--install-the-connector-without-registration"></a>Stap 1: De Connector zonder registratie installeren


Installeer de verbindingslijn MSI-bestanden zonder te registreren de verbindingslijn als volgt:


1. Open een opdrachtprompt.
2. Voer de volgende opdracht waarin de/q stille installatie - betekent wordt de installatie niet gevraagd u aan de gebruiksrechtovereenkomst te accepteren.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Stap 2: De verbindingslijn met Azure Active Directory registreren
Dit kunt doen met een van de volgende manieren:


- De verbindingslijn met behulp van een Windows PowerShell referentie-object registreren
- De verbindingslijn een token gemaakt offline registreren

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>De verbindingslijn met behulp van een Windows PowerShell referentie-object registreren


1. Het object Windows PowerShell-referenties maken door te voeren van de volgende opties, waar "<username>'en'<password>' moet worden vervangen door de gebruikersnaam en wachtwoord voor uw map:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Ga naar **C:\Program Files\Microsoft AAD App Proxy-Connector** en uitvoert het script met behulp van de PowerShell referenties object dat u hebt gemaakt, waarbij $cred is de naam van de PowerShell referenties object dat u hebt gemaakt:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>De verbindingslijn een token gemaakt offline registreren

1. Een offline token met behulp van de AuthenticationContext klasse waarbij de waarden in het codefragment maken:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Nadat u het maken van een SecureString met het token token hebt: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Voer de volgende Windows PowerShell-opdracht, waarbij SecureToken is de naam van het token die u hierboven hebt gemaakt en tenantID GUID van uw tenant is: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Zie ook

- [Toepassingsproxy voor Azure Active Directory inschakelen](active-directory-application-proxy-enable.md)
- [Toepassingen met uw eigen domeinnaam publiceren](active-directory-application-proxy-custom-domains.md)
- [Inschakelen voor eenmalige aanmelding](active-directory-application-proxy-sso-using-kcd.md)
- [Problemen die u met toepassingsproxy ondervindt](active-directory-application-proxy-troubleshoot.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
