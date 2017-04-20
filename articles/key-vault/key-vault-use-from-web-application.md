<properties
    pageTitle="Gebruik van Azure belangrijke kluis uit een webtoepassing | Microsoft Azure"
    description="Met deze zelfstudie kunt u informatie over het gebruik van Azure-toets kluis uit een webtoepassing."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Azure belangrijke kluis uit een webtoepassing gebruiken #

## <a name="introduction"></a>Inleiding  
Met deze zelfstudie kunt u informatie over het gebruik van Azure-toets kluis uit een webtoepassing in Azure wordt aangegeven. Deze begeleidt u bij het proces van het toegang krijgen tot een geheim vanuit een Azure-toets kluis zodat deze kan worden gebruikt in uw webtoepassing.

**Geschatte tijd in beslag:** 15 minuten


Zie voor informatie over Azure-toets kluis [Wat Azure-toets kluis is?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende:

- Een URI naar een geheim in een kluis Azure-toets
- Een Client-ID en een geheim Client voor een webtoepassing geregistreerd met Azure Active Directory die toegang tot uw kluis-toets heeft
- Een webtoepassing. We worden de stappen voor een ASP.NET-MVC-toepassing geïmplementeerd in Azure wordt aangegeven als een Web-App weergegeven.

> [AZURE.NOTE]  Het is essentieel dat u de stappen in [Aan de slag met Azure toets kluis](key-vault-get-started.md) voor deze zelfstudie zodat u de URI voor een geheim en de Client-ID en geheim Client voor een webtoepassing hebt hebt voltooid.

De webtoepassing die toegang heeft tot de kluis sleutel is het account dat is geregistreerd in Azure Active Directory en heeft toegang is verleend tot uw sleutel kluis. Als dit niet het geval is, gaat u terug naar Register een toepassing in deze zelfstudie aan de slag en herhaalt u de stappen vermeld.

Deze zelfstudie is bedoeld voor webontwikkelaars die de basisbeginselen van het maken van webtoepassingen op Azure. Zie [overzicht van de Web Apps](../app-service-web/app-service-web-overview.md)voor meer informatie over Azure Web Apps.



## <a id="packages"></a>Nuget pakketten toevoegen ##
Er zijn twee pakketten dat uw webtoepassing moet hebt geïnstalleerd.

- Bibliotheek voor Active Directory-verificatie - bevat methoden voor interactie met Azure Active Directory en beheren van gebruikers-id
- Azure toets kluis bibliotheek - bevat methoden voor de communicatie met Azure-toets kluis


Beide van deze pakketten kunnen worden geïnstalleerd met de Package Manager-Console met de opdracht installatiepakket.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Web.Config wijzigen ##
Er zijn drie toepassingsinstellingen die moeten worden toegevoegd aan het bestand web.config als volgt.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Als u geen voor het hosten van uw toepassing als een Azure-Web-App, moet u de werkelijke ClientId, geheim Client en geheim URI waarden toevoegen aan de web.config. Laat deze pop waarden anders omdat we de werkelijke waarden in de Portal Azure voor een extra beveiligingsniveau toevoegen.


## <a id="gettoken"></a>Methode als u een Access Token toevoegen ##
Pas de sleutel kluis-API gebruiken, moet u een toegangstoken. De toets kluis gewoon oproepen naar de toets kluis API verwerkt, maar moet u deze met een functie waarvoor u het toegangstoken opgeven.  

Hieronder volgt de code aan een access token van Azure Active Directory. Overal in uw toepassing kunt u deze code. Ik wil graag een klasse Utils of EncryptionHelper toevoegen.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Met een Client-ID en geheim Client is de eenvoudigste manier om te verifiëren van een Azure AD-toepassing. En gebruikt deze in uw webtoepassing kunt voor een scheiding van taken en meer controle over uw key management. Maar deze is afhankelijk van de op de Client geheim plaatsen in uw configuratieinstellingen die voor sommige als risico als het plaatsen van het geheim dat u wilt beveiligen in de configuratieinstellingen. Zie hieronder voor informatie over het gebruik van een Client-ID en het certificaat in plaats van de Client-ID en geheim Client voor de verificatie van de Azure AD-toepassing.



## <a id="appstart"></a>Het geheim op toepassing gestart ophalen ##
Nu nodig we hebt voor de sleutel kluis API aanroepen en het geheim op te halen. De volgende code kan overal worden geplaatst zo lang maken als dit wordt genoemd, voordat u dit wilt gebruiken. Ik heb deze code hebt opgeslagen in de gebeurtenis-toepassing Start in de Global.asax zodat deze wordt uitgevoerd eenmaal op start en zorgt ervoor dat het geheim beschikbaar voor de toepassing.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>App-instellingen in de Portal Azure (optioneel) toevoegen ##
U kunt nu de werkelijke waarden toevoegen voor de AppSettings in de Portal Azure, hebt u een Azure-Web-App. Dit doet, is de werkelijke waarden worden niet in de web.config, maar beveiligd via de Portal waar u de mogelijkheden voor het beheer van afzonderlijke toegang hebt. Deze waarden voor de waarden die u hebt ingevoerd in uw web.config vervangen. Zorg ervoor dat de namen hetzelfde zijn.

![Toepassingsinstellingen weergegeven in de Portal van Azure][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Verificatie met een certificaat in plaats van een Client geheim
Er is een andere manier om te verifiëren van een Azure AD-toepassing met behulp van een Client-ID en een certificaat in plaats van een Client-ID en geheim Client. Hier volgen de stappen voor het gebruik van een certificaat in een Azure-Web-App:

1. Of als een digitaal certificaat maken
2. Het certificaat koppelen aan een Azure AD-toepassing
3. Code toevoegen aan uw Web-App het certificaat gebruiken
4. Een certificaat toevoegen aan uw Web-App


**Of als een digitaal certificaat maken** Voor ons doel doen wij een testcertificaat. Hier volgen een paar van opdrachten die u kunt een opdrachtprompt van ontwikkelaars een digitaal certificaat maken. De map wijzigen aan waar u de bestanden certificaat moet worden gemaakt.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Noteer de einddatum in te voeren en het wachtwoord voor de .pfx (in dit voorbeeld: 07/31/2016 en test123). U moet ze onderstaande.

Zie voor meer informatie over het maken van een testcertificaat [hoe: uw eigen testen certificaat maken](https://msdn.microsoft.com/library/ff699202.aspx)


**Het certificaat met een Azure AD-toepassing koppelen** Nu u een certificaat hebt, moet u koppelen aan een Azure AD-toepassing. Maar de beheerportal Azure biedt geen ondersteuning voor dit nu. U moet in plaats daarvan Powershell gebruiken. Hier volgen de opdrachten die u nodig hebt om uit te voeren:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Nadat u deze opdrachten hebt uitgevoerd, kunt u de toepassing in Azure AD zien. Als u de toepassing eerst zoeken naar "Toepassingen mijn bedrijf eigenaar is van" niet ziet in plaats van "Toepassingen mijn bedrijf heeft".

Zie meer informatie over objecten van Azure AD-toepassing en ServicePrincipal objecten, [objecten toepassing en Service Principal objecten](../active-directory/active-directory-application-objects.md)



**De code toevoegen aan uw Web-App het certificaat gebruiken** Nu wordt we code aan uw Web-App voor toegang tot het certificaat en deze gebruiken voor verificatie toegevoegd.

Er is eerst code voor toegang tot het certificaat.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Houd er rekening mee dat het StoreLocation CurrentUser in plaats van LocalMachine is. En dat we 'false' met de methode zoeken levert omdat we een test-certificaat gebruikt.


Volgende is code die gebruikmaakt van de CertificateHelper en Hiermee maakt u een ClientAssertionCertificate dat nodig voor verificatie is.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Hier ziet u de nieuwe code om het toegangstoken. Hiermee vervangt de bovenstaande GetToken-methode. Ik hebt gegeven om deze een andere naam voor het gemak.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Ik heb al deze code hebt opgeslagen in van mijn project Web App Utils klasse voor gebruiksgemak.

De laatste codewijziging is in de methode Application_Start. Eerst moeten we de methode GetCert() om te laden van de ClientAssertionCertificate bellen. En kies vervolgens we de methode voor terugbellen die we leveren bij het maken van een nieuwe KeyVaultClient wijzigen. Houd er rekening mee dat deze optie vervangt u de code die we had hierboven.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Een certificaat toevoegen aan uw Web-App via de Portal van Azure** Een certificaat toevoegen aan uw Web-App is een eenvoudig proces. Eerst gaat u naar de Azure-Portal en Ga naar uw Web-App. Klik op het blad instellingen voor uw Web-App, op de vermelding voor 'aangepaste domeinen en SSL'. Klik op het blad dat wordt geopend en u kunnen te uploaden van het certificaat dat u hiervoor KVWebApp.pfx hebt gemaakt, zorgt u ervoor dat u het wachtwoord voor de pfx niet.

![Een certificaat toevoegen aan een Web-App in de Portal van Azure][2]


Het laatste wat die u moet doen, is een toepassingsinstelling toevoegen aan uw Web-App met de naam van WEBSITE\_laden\_certificaten en een waarde van *. Dit zorgt ervoor dat dat alle certificaten worden geladen. Als u laden alleen de certificaten die u hebt geüpload wilt, kunt u een door komma's gescheiden lijst met hun vingerafdrukken invoeren.

Zie voor meer informatie over het toevoegen van een certificaat aan een Web-App, [Certificaten gebruiken in Azure Websites toepassingen](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Een certificaat wilt toets kluis als een geheim toevoegen** U kunt in plaats van uw certificaat rechtstreeks naar de Web App-service uploaden, opslaan in de sleutel kluis als een geheim en het van daaruit dashboard implementeren. Dit is een proces die wordt beschreven in het volgende blogbericht, [Implementeert Azure Web App certificaat tot en met de toets kluis](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)



## <a id="next"></a>Volgende stappen ##


Voor het programmeren verwijzingen, Zie [Azure toets kluis C# Client API tablets](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
