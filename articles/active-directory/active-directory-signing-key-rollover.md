<properties
    pageTitle="Sleutelrollover aanmelden met Azure AD | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe de ondertekend sleutelrollover aanbevolen procedures voor Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Ondertekening sleutelrollover in Azure Active Directory

In dit onderwerp wordt uitgelegd wat u moet weten over de openbare sleutels die worden gebruikt in Azure Active Directory (Azure AD) beveiligingstokens zich aan te melden. Het is belangrijk te weten dat deze toetsen rollover periodiek en klikt u in noodgevallen, direct kan worden overgebracht. Alle toepassingen die gebruikmaken van Azure AD moet kunnen via programmacode verwerken van het proces sleutelrollover of stand brengen van een proces periodiek handmatige rollover. Lezen als u wilt weten hoe de toetsen werkt, hoe u nagaan wat het resultaat van de rollover in uw toepassing en het bijwerken van uw toepassing of stand brengen van een proces periodiek handmatige rollover sleutelrollover afhandelen, indien nodig.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Overzicht aan te melden toetsen Azure AD

Azure AD worden openbare sleutel cryptografische gebouwd op industrienormen tot stand vertrouwensrelatie tussen zichzelf en door de toepassingen die gebruikmaken van deze. In de praktijk dit werkt in de volgende manier: Azure AD een ondertekend sleutel die uit een combinatie van openbare en persoonlijke sleutel bestaat wordt gebruikt. Wanneer een gebruiker zich aanmeldt bij een toepassing die gebruikmaakt van Azure AD voor verificatie, Azure AD Hiermee maakt u een beveiligingstoken dat informatie over de gebruiker bevat. Deze token is ondertekend door Azure AD met bijbehorende persoonlijke sleutel voordat deze wordt verzonden terug naar de toepassing. Om te bevestigen dat de token geldig en dat is wel is van Azure AD is, moet de toepassing van het token handtekening met de openbare sleutel die worden aangeboden door Azure AD die is opgenomen in de tenant [OpenID verbinding discovery-document](http://openid.net/specs/openid-connect-discovery-1_0.html) of SAML/WS-Fed [Federatie metagegevensdocument](active-directory-federation-metadata.md)valideren.

Veiligheidsoverwegingen kan Azure AD ondertekening belangrijke rollen periodiek en, voor noodgevallen, worden verlengd onmiddellijk. Een toepassing die wordt geïntegreerd met Azure AD moet worden opgesteld u omgaat met een gebeurtenis sleutelrollover ongeacht hoe vaak deze kan optreden. Als dit niet het geval en uw toepassing probeert te gebruiken van een verlopen sleutel om te controleren of de handtekening in een token, mislukt het verzoek aanmelden.

Er is altijd meer dan één geldige sleutel beschikbaar in het document discovery OpenID verbinding maken en het document Federatie-metagegevens. Uw toepassing moet een van de toetsen die zijn opgegeven in het document, aangezien één toets spoedig mogelijk worden geïmplementeerd, een andere mogelijk worden de vervangende, enzovoort.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Hoe u als uw toepassing worden beïnvloed beoordelen en wat u moet doen

Hoe uw toepassing sleutelrollover verwerkt, is afhankelijk van variabelen zoals het type toepassing of welke identiteit protocol en bibliotheek is gebruikt. De secties hierna beoordelen of de meest voorkomende van toepassingen worden beïnvloed door de sleutelrollover en geven richtlijnen voor het bijwerken van de toepassing ondersteunt automatische rollover of de toets handmatig bijwerken.

* [Systeemeigen clienttoepassingen bronnen](#nativeclient)
* [Webtoepassingen / API's bronnen](#webclient)
* [Webtoepassingen / API's voor het beveiligen van resources en gemaakt met Azure App Services](#appservices)
* [Webtoepassingen / API's resources met .NET OWIN OpenID verbinden, WS-Fed of WindowsAzureActiveDirectoryBearerAuthentication middleware beveiligen](#owin)
* [Webtoepassingen / API's resources met .NET Core OpenID verbinding of JwtBearerAuthentication middleware beveiligen](#owincore)
* [Webtoepassingen / API's het beveiligen van resources gebruik Node.js passport-azure-ad-module](#passport)
* [Webtoepassingen / API's voor het beveiligen van resources en die zijn gemaakt met Visual Studio-2015](#vs2015)
* [Webtoepassingen beveiligen van resources en die zijn gemaakt met Visual Studio 2013](#vs2013)
* [Web-API's voor het beveiligen van resources en die zijn gemaakt met Visual Studio 2013](#vs2013_webapi)
* [Webtoepassingen beveiligen van resources en die zijn gemaakt met Visual Studio 2012](#vs2012)
* [Webtoepassingen beveiligen van resources en die zijn gemaakt met Visual Studio 2010, 2008 o Windows identiteit Foundation gebruiken](#vs2010)
* [Webtoepassingen / API's het beveiligen van resources geen andere bibliotheken te gebruiken of handmatig implementeren van de ondersteunde protocollen](#other)

Deze richtlijnen **niet** van toepassing op:

* Toepassingen van Azure AD-toepassing galerie (met inbegrip van aangepaste) hebt toegevoegd, hebben afzonderlijk richtlijnen voor het ondertekenen van toetsen. [Meer informatie.](active-directory-sso-certs.md)
* On-premises implementatie toepassingen die zijn gepubliceerd via toepassingsproxy geen zorgen over het ondertekenen van toetsen te.

### <a name="nativeclient"></a>Systeemeigen clienttoepassingen bronnen

Toepassingen die alleen toegang krijgt resources (Internet Explorer tot Microsoft Graph, KeyVault, Outlook-API en andere Microsoft-APIs) gewoonlijk alleen een token verkrijgen en doorgegeven langs aan de eigenaar van de resource. Gezien het feit dat ze niet alle bronnen beveiligen zijn, worden ze doen het token niet controleren en hoeft dus niet te zorgen dat deze juist is ondertekend.

Systeemeigen clienttoepassingen, of het bureaublad of mobiele, vallen in deze categorie en worden dus niet beïnvloed door de rollover.

### <a name="webclient"></a>Webtoepassingen / API's bronnen

Toepassingen die alleen toegang krijgt resources (Internet Explorer tot Microsoft Graph, KeyVault, Outlook-API en andere Microsoft-APIs) gewoonlijk alleen een token verkrijgen en doorgegeven langs aan de eigenaar van de resource. Gezien het feit dat ze niet alle bronnen beveiligen zijn, worden ze doen het token niet controleren en hoeft dus niet te zorgen dat deze juist is ondertekend.

Webtoepassingen en -API's die van de app alleen-lezen-stroom gebruikmaken (clientreferenties / clientcertificaat), vallen in deze categorie en worden dus niet beïnvloed door de rollover.

### <a name="appservices"></a>Webtoepassingen / API's voor het beveiligen van resources en gemaakt met Azure App Services

Azure App Services verificatie / autorisatie (EasyAuth) functionaliteit al heeft de benodigde logica sleutelrollover automatisch verwerken.

### <a name="owin"></a>Webtoepassingen / API's resources met .NET OWIN OpenID verbinden, WS-Fed of WindowsAzureActiveDirectoryBearerAuthentication middleware beveiligen

Als uw toepassing van de .NET OWIN OpenID verbinding kunt maken, WS-Fed of WindowsAzureActiveDirectoryBearerAuthentication middleware gebruikmaakt, heeft dit al de benodigde logica sleutelrollover automatisch verwerken.

U kunt bevestigen dat uw toepassing een van deze door te zoeken naar een van de volgende knipsels in de Startup.cs of Startup.Auth.cs van de toepassing wordt gebruikt

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Webtoepassingen / API's resources met .NET Core OpenID verbinding of JwtBearerAuthentication middleware beveiligen

Als uw toepassing de verbinding van .NET Core OWIN OpenID of JwtBearerAuthentication middleware gebruikt wordt, is deze al de benodigde logica sleutelrollover automatisch verwerken.

U kunt bevestigen dat uw toepassing een van deze door te zoeken naar een van de volgende knipsels in de Startup.cs of Startup.Auth.cs van de toepassing wordt gebruikt

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Webtoepassingen / API's het beveiligen van resources gebruik Node.js passport-azure-ad-module

Als uw toepassing de Node.js passport-ad-module gebruikt wordt, is deze al de benodigde logica sleutelrollover automatisch verwerken.

U kunt bevestigen dat uw toepassing passport-ad door te zoeken naar het volgende fragment in van de toepassing app.js

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Webtoepassingen / API's voor het beveiligen van resources en die zijn gemaakt met Visual Studio-2015

Als uw toepassing is gemaakt met een sjabloon in Visual Studio-2015 en u **Werk en Schoolaccounts** geselecteerd in het menu **Wijzigen verificatie** , heeft dit al de benodigde logica sleutelrollover automatisch verwerken. Deze logica, ingesloten in de middleware OWIN OpenID verbinding zijn opgehaald en slaat de toetsen uit het document discovery OpenID verbinding maken en regelmatig vernieuwd.

Als u verificatie handmatig toegevoegd aan uw oplossing, is uw toepassing mogelijk niet de logica nodig sleutelrollover. U moet deze zelf schrijven of volg de stappen in [webtoepassingen / API's geen andere bibliotheken te gebruiken of handmatig implementeren van de ondersteunde protocollen.](#other).

### <a name="vs2013"></a>Webtoepassingen beveiligen van resources en die zijn gemaakt met Visual Studio 2013

Als uw toepassing is gemaakt met een sjabloon in Visual Studio 2013 en u **Organisatie-Accounts** hebt geselecteerd in het menu **Wijzigen verificatie** , heeft dit al de benodigde logica sleutelrollover automatisch verwerken. Deze logica slaat de unieke id van uw organisatie en de ondertekend belangrijke informatie in twee databasetabellen die is gekoppeld aan het project. U vindt de verbindingsreeks voor de database in het projectbestand Web.config.

Als u verificatie handmatig toegevoegd aan uw oplossing, is uw toepassing mogelijk niet de logica nodig sleutelrollover. U moet deze zelf schrijven of volg de stappen in [webtoepassingen / API's geen andere bibliotheken te gebruiken of handmatig implementeren van de ondersteunde protocollen.](#other).

De volgende stappen kunt u controleren of de logica goed in uw toepassing werkt.

1. Open de oplossing in Visual Studio 2013, en klik op het tabblad **Server Explorer** op het rechtervenster.
2. Vouw **Data Connections**, **DefaultConnection**en **tabellen**. Zoek in de tabel **IssuingAuthorityKeys** , met de rechtermuisknop en klik vervolgens op **Gegevens in een tabel weergeven**.
3. In de tabel **IssuingAuthorityKeys** zullen er minimaal één rij die overeenkomt met de vingerafdrukwaarde voor de sleutel. Verwijder alle rijen in de tabel.
4. Met de rechtermuisknop op de tabel **Tenants** en klik vervolgens op **Gegevens in een tabel weergeven**.
5. In de tabel **Tenants** , wordt er ten minste één rij, wat overeenkomt met een unieke directory tenant-id. Verwijder alle rijen in de tabel. Als u niet de rijen in zowel de **Tenants** tabel en **IssuingAuthorityKeys** verwijdert, wordt er een fout tijdens runtime.
6. Maken en uitvoeren van de toepassing. Nadat u zich hebt aangemeld bij uw account, kunt u de toepassing kunt stoppen.
7. Ga terug naar de **Server Explorer** en kijkt u naar de waarden in de tabel **IssuingAuthorityKeys** en **Tenants** . U ziet dat ze hebt is automatisch opnieuw gevuld met de vereiste gegevens uit het document Federatie-metagegevens.

### <a name="vs2013"></a>Web-API's voor het beveiligen van resources en die zijn gemaakt met Visual Studio 2013

Als u een web API-toepassing in Visual Studio 2013 met de Web API-sjabloon gemaakt en **Organisatie-Accounts** wordt geselecteerd in het menu **Wijzigen verificatie** , hebt u al de benodigde logica in uw toepassing.

Als u handmatig verificatie hebt geconfigureerd, volgt u de onderstaande instructies voor meer informatie over het configureren van uw Web-API als u wilt de belangrijkste informatie automatisch bijgewerkt.

Het volgende codefragment laat zien hoe de meest recente sleutels ophalen uit het document Federatie-metagegevens en gebruik vervolgens de [JWT Token-Handler](https://msdn.microsoft.com/library/dn205065.aspx) voor het valideren van het token. Het codefragment wordt ervan uitgegaan dat u zult gebruiken uw eigen in de cache om voor persistent maken van de sleutel voor het valideren van toekomstige tokens afgeleid van Azure AD, ongeacht of deze in een database, configuratiebestand of ergens anders.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Webtoepassingen beveiligen van resources en die zijn gemaakt met Visual Studio 2012

Als uw toepassing is gemaakt in Visual Studio 2012, u waarschijnlijk gebruikt de identiteit en de Access-hulpprogramma voor het configureren van uw toepassing. Het is ook waarschijnlijk dat u de [Valideren uitgever naam register (VINR gebruikt)](https://msdn.microsoft.com/library/dn205067.aspx). De VINR is verantwoordelijk voor het onderhouden van informatie over vertrouwde identiteitsprovider (Azure AD) en de toetsen die worden gebruikt om tokens uitgegeven door deze te valideren. De VINR ook kunt u heel gemakkelijk de belangrijkste informatie die zijn opgeslagen in een Web.config-bestand door te downloaden van de meest recente Federatie metagegevensdocument dat is gekoppeld aan uw adreslijst, automatisch worden bijgewerkt als de configuratie verouderd met de meest recente document is controleren en bijwerken van de toepassing voor het gebruik van de nieuwe sleutel zo nodig.

Als u uw-toepassing via een van de voorbeelden van code of scenario documentatie bij Microsoft hebt gemaakt, wordt de logica sleutelrollover al opgenomen in uw project. U ziet dat de onderstaande code al in uw project bestaat. Als uw toepassing wordt niet al deze logica, de volgende stappen toe te voegen en te controleren of deze correct werkt.

1. In **Solution Explorer**toevoegen een verwijzing naar de vergadering **System.IdentityModel** voor het juiste project.
2. Open het bestand **Global.asax.cs** en voeg de volgende richtlijnen gebruiken:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. De volgende methode toevoegen aan het bestand **Global.asax.cs** :
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. De methode **RefreshValidationSettings()** in de methode **Application_Start()** in **Global.asax.cs** roepen zoals wordt weergegeven:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Nadat u deze stappen hebt gevolgd, worden van uw toepassing Web.config worden bijgewerkt met de meest recente gegevens uit het document Federatie metagegevens, inclusief de meest recente sleutels. Deze update zal plaatsvinden telkens wanneer uw groep van toepassingen wordt herhaald in IIS. IIS is standaard ingesteld op Prullenbak toepassingen om 29 uur.

Volg de onderstaande stappen om te controleren of de logica sleutelrollover functioneert.

1. Nadat u hebt gecontroleerd dat uw toepassing de bovenstaande code wordt gebruikt, opent u het bestand **Web.config** en navigeer naar de **<issuerNameRegistry>** blok, specifiek zoekt u het volgende paar regels:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. In de **<add thumbprint=””>** instellen, wijzigt u de vingerafdrukwaarde willekeurig teken vervangen door een andere naam door. Sla het bestand **Web.config** .

3. De toepassing maken en voer dit uit. Als u het aanmeldingsproces voltooien kunt, is uw toepassing de toets met succes bijwerken door de vereiste gegevens downloaden uit van uw map Federatie metagegevensdocument. Als u hebt met het aanmelden problemen, of u de wijzigingen in uw toepassing juist zijn door het [Toevoegen van eenmalige aanmelding naar uw Web-toepassing werken met Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) -onderwerp lezen of te downloaden en te controleren in het volgende voorbeeld: [Meerdere Tenant Cloud-toepassing voor Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="vs2010"></a>Webtoepassingen beveiligen van resources en die zijn gemaakt met Visual Studio 2008 of 2010 en Windows identiteit Foundation (WIF) v1.0 voor .NET 3.5

Als u een toepassing op WIF v1.0 hebt gemaakt, is er geen meegeleverde methode om de configuratie van uw toepassing een nieuwe sleutel automatisch te vernieuwen.

- *Gemakkelijkst* Gebruik de FedUtil tooling opgenomen in de SDK WIF, die kunnen worden opgehaald van de meest recente metagegevensdocument en bijwerken van uw configuratie.
- Werk uw toepassing naar .NET 4.5, waaronder de nieuwste versie van WIF zich bevindt in de naamruimte systeem. U kunt vervolgens de [Valideren uitgever naam register (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) om uit te voeren automatische updates van de configuratie van de toepassing.
- Een handmatige rollover aan de hand van de instructies aan het einde van dit document richtlijnen uitvoeren.

Instructies voor het gebruik van de FedUtil de configuratie bij te werken:

1. Controleer of u de WIF v1.0 SDK is geïnstalleerd op uw computer ontwikkeling voor Visual Studio 2008 of 2010. U kunt [deze hier downloaden](https://www.microsoft.com/en-us/download/details.aspx?id=4451) als u nog niet hebt geïnstalleerd deze.
2. Visual Studio, opent u de oplossing, klik met de rechtermuisknop op het toepasselijke project en selecteer **Federatie metagegevens bijwerken**. Als deze optie niet beschikbaar is, is FedUtil en/of de WIF v1.0 SDK niet geïnstalleerd.
3. Selecteer in de prompt **bijwerken** om te beginnen met het bijwerken van de metagegevens van uw Federatie. Als u toegang tot de server-omgeving waarin de toepassing wordt gehost hebt, kunt u desgewenst FedUtil van [Automatische metagegevens scheduler bijwerken](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klik op **Voltooien** om het updateproces.

### <a name="other"></a>Webtoepassingen / API's het beveiligen van resources geen andere bibliotheken te gebruiken of handmatig implementeren van de ondersteunde protocollen

Als u enkele andere bibliotheek gebruikt of een van de ondersteunde protocollen handmatig geïmplementeerd, moet u de bibliotheek of uw implementatie om ervoor te zorgen dat de toets wordt opgehaald uit de OpenID verbinding discovery-document of het Federatie metagegevensdocument controleren. Een manier om dit te controleren is een zoekopdracht in uw code of van de bibliotheek code voor oproepen af aan het OpenID discovery-document of het document Federatie-metagegevens.

Als ze belangrijke ergens worden opgeslagen of vastgelegde in uw toepassing, u handmatig kunt de sleutel en update dienovereenkomstig gewijzigd deze door te voeren een handmatige rollover aan de hand van de instructies aan het einde van dit document richtlijnen ophalen. In dit artikel om te voorkomen toekomstige onderbrekingen **is ten zeerste aangeraden dat u uw toepassing voor de ondersteuning van automatische rollover verbeteren** met de methoden overzicht en belasting als Azure AD toeneemt rollover cadence is of een noodgevallen out-van-band-rollover heeft.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Hoe u kunt uw toepassing om te bepalen als deze worden beïnvloed testen

Kunt u nagaan of uw toepassing automatische sleutelrollover ondersteunt door te downloaden van de scripts en volgens de instructies in [deze opslagplaats GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Hoe u een handmatige rollover uitvoeren als u toepassingen biedt geen ondersteuning voor automatische rollover

Als uw toepassing **geen** ondersteuning voor automatische rollover biedt, moet u tot stand brengen van een proces die regelmatig Azure AD-ondertekend toetsen bewaakt en voert een handmatige rollover dienovereenkomstig gewijzigd. [Deze GitHub bibliotheek](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) bevat scripts en instructies voor hoe u dit wilt doen.
