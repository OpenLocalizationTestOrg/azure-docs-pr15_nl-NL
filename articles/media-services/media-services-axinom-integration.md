<properties 
    pageTitle="Gebruik van Axinom moet geven Widevine licenties voor Azure Media Services | Microsoft Azure" 
    description="In dit artikel wordt beschreven hoe u Azure Media Services (AMS) kunt gebruiken voor het leveren van een stroom die dynamisch door AMS met zowel PlayReady als Widevine DRMs is versleuteld. De licentie PlayReady afkomstig van Media Services PlayReady licentieserver en Widevine licentie wordt geleverd door de licentieserver Axinom." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Axinom moet geven Widevine licenties voor Azure Media Services gebruiken  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>Overzicht

Azure Media Services (AMS) heeft toegevoegd Google Widevine dynamische beveiliging (Zie [van Mingfei-blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) voor meer informatie). Daarnaast Azure Media Player (AMP) ook Widevine ondersteuning heeft toegevoegd (Zie [AMP document](http://amp.azure.net/libs/amp/latest/docs/) voor meer informatie). Dit is een primaire iets streaming streepje inhoud die zijn beveiligd met CENC met meerdere-native-DRM (PlayReady en Widevine) in moderne browsers voorzien MSE en EME.

Media-Services kunt beginnen met de Media Services .NET SDK versie 3.5.2, u Widevine licentie sjabloon configureren en Widevine licenties kunt ophalen. U kunt ook de volgende AMS partners gebruiken om te leveren Widevine licenties: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

In dit artikel wordt beschreven hoe integreren en test u de licentieserver Widevine beheerd door Axinom. Specifiek, bedekt deze:  

- Dynamische algemene versleuteling configureren met multi-DRM (PlayReady en Widevine) met de bijbehorende licentie acquisition-URL's;
- Een JWT token genereren om te voldoen aan de licentievereisten server;
- Het ontwikkelen van Azure Media Player app dat omgaat met licentie acquisition met JWT token verificatie.

Het volledige systeem en de stroom van inhoud die belangrijkste, key-ID, key zaaizaad, JTW token en haar vorderingen best door het volgende diagram kan worden beschreven.

![STREEPJE en CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Inhoudsbeveiliging

Voor het configureren van dynamische protection en belangrijke bezorging beleid, raadpleegt u de Mingfei-blog: [Widevine verpakking met Azure Media Services configureren](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

U kunt dynamische CENC beveiliging met multi-DRM configureren voor streepje streaming met beide van de volgende opties:

1. PlayReady beveiliging voor MS Edge en IE11, die een autorisatie voor token-beperkingen kan hebben. Het beleid voor token beperkte vergezeld van een token dat is uitgegeven door een Secure Token Service (STS), zoals Azure Active Directory.
1. Widevine beveiliging voor Chrome, kan er token verificatie met token uitgegeven door een andere STS is vereist. 

Zie [JWT Token genereren](media-services-axinom-integration.md#jwt-token-generation) sectie voor waarom Azure Active Directory kan niet worden gebruikt als een STS voor Axinom van Widevine licentieserver.

###<a name="considerations"></a>Overwegingen

1. Moet u de opgegeven Axinom belangrijke zaad (8888000000000000000000000000000000000000) en uw gegenereerde of geselecteerde belangrijke ID om de inhoud sleutel voor het configureren van belangrijke bezorgingsservice te genereren. Axinom wordt licentieserver alle licenties met inhoud toetsen op basis van dezelfde belangrijke basis, dat wil geldig voor het testen en productie zeggen.
1. De URL Widevine licentie acquisition voor het testen van: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP- en HTTS zijn toegestaan.

##<a name="azure-media-player-preparation"></a>Voorbereiding van Azure Media Player

AMP v1.4.0 ondersteunt afspelen AMS inhoud die dynamisch wordt geleverd met zowel PlayReady als Widevine DRM.
Als de licentieserver Widevine token verificatie niet vereist, wordt niets extra dat u moet doen om te testen van de inhoud van een streepje beveiligd met Widevine. Het team AMP bevat bijvoorbeeld een eenvoudig [voorbeeld](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), waar u deze werken in de rand en IE11 met PlayReady en Chrome met Widevine kunt zien.
JWT token-verificatie is vereist door de server Widevine licentie van Axinom. Het token JWT moet worden verzonden met verzoek om de licentie via een HTTP-header 'X-AxDRM-bericht'. Hiervoor moet u de volgende javascript toevoegen aan de pagina met webonderdelen AMP hostingprovider voordat de bron instelt:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

De rest van AMP code is standaard AMP API zoals in het document AMP [hier](http://amp.azure.net/libs/amp/latest/docs/).

Houd er rekening mee dat het bovenstaande javascript voor instelling autorisatie voor aangepaste kop nog steeds een benadering van de korte termijn vóór de officiële langdurig aanpak in AMP is uitgebracht.

##<a name="jwt-token-generation"></a>JWT Token genereren

JWT token-verificatie is vereist door de licentieserver Axinom Widevine voor het testen. Daarnaast is een van de claims in het token JWT van een complexe objecttype in plaats van primitieve gegevenstype.

Helaas kunt Azure AD kan alleen worden verleend JWT tokens met primitieve typen. Daarnaast kunt .NET Framework-API (System.IdentityModel.Tokens.SecurityTokenHandler en JwtPayload) u alleen complexe object invoertype als claims. De claims zijn echter nog steeds serienummer als tekenreeks. Daarom niet we gebruiken een van de twee voor het genereren van het token JWT voor Widevine licentie verzoek.


John Sheehan van [JWT Nuget pakket](https://www.nuget.org/packages/JWT) voldoet aan de behoeften zodat gaan we dit Nuget-pakket gebruiken.

Hieronder ziet u de code voor genereren JWT token met de benodigde claims zoals vereist door de licentieserver Axinom Widevine voor het testen:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

De licentieserver Axinom Widevine

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Overwegingen

1.  Hoewel bezorgingsservice AMS PlayReady-licentie is vereist ' vruchtdragende = "voorafgaand aan een verificatietoken, Axinom Widevine licentieserver maakt geen gebruik.
2.  De Axinom communicatie-toets wordt gebruikt als de sleutel ondertekenen. Houd er rekening mee dat de sleutel een hexadecimale tekenreeks is, maar deze moet worden behandeld als een reeks bytes niet een tekenreeks wanneer codering. Dit is bereikt door de methode ConvertHexStringToByteArray.

##<a name="retrieving-key-id"></a>Sleutel-ID ophalen

U misschien opgevallen dat in de code voor het genereren van een JWT token, key ID vereist is. Aangezien de JWT token moet klaar zijn voordat laden AMP speler, belangrijke moet-ID worden opgehaald om te kunnen JWT token genereren.

Id van cursus er zijn verschillende manieren om toegang te krijgen houdt van sleutel Bijvoorbeeld een mogelijk opslaan ID sleutel samen met inhoud metagegevens in een database. Of u kunt ophalen ID uit MPD-streepje (Media presentatiebeschrijving) bestand sleutel. De onderstaande code is voor de laatste.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Overzicht

Met de meest recente toevoeging van ondersteuning voor Widevine in zowel Azure Media Services en inhoudsbeveiliging en Azure Media Player kunnen we implementeren streaming streepje + meerdere-native-DRM (PlayReady + Widevine) met beide PlayReady licentie-service in AMS en Widevine licentieserver uit Axinom voor de volgende moderne browsers:

- Chrome
- Microsoft Edge op Windows 10
- IE 11 in Windows 8.1 en Windows 10
- Zowel (bureaublad) Firefox en Safari op Mac (niet iOS) worden ook ondersteund via Silverlight en dezelfde URL met Azure Media Player

De volgende parameters zijn in de licentieserver Mini oplossing inzetten Axinom Widevine vereist. Behalve voor de sleutel-ID, de rest van de parameters worden verstrekt door Axinom op basis van hun Widevine server-instellingen.


Parameter|Hoe deze worden gebruikt
---|---
Communicatie ID sleutel|Moet worden opgenomen als de waarde van het claimen "com_key_id" in JWT token (Zie [hieronder](media-services-axinom-integration.md#jwt-token-generation) ).
Communicatie-toets|Moet worden gebruikt als de bijbehorende sleutel van JWT token (Zie [hieronder](media-services-axinom-integration.md#jwt-token-generation) ).
Belangrijke zaad|Moet worden gebruikt om inhoud sleutel met een bepaalde inhoud te genereren ID sleutel (Zie [hieronder](media-services-axinom-integration.md#content-protection) ).
De URL Widevine licentie|Moet worden gebruikt in het configureren van activa bezorging beleid voor streepje streaming [(Zie hieronder)](media-services-axinom-integration.md#content-protection) .
Inhoud sleutel-ID|Moet deel uit van de waarde van recht bericht claim van JWT token (Zie [hieronder](media-services-axinom-integration.md#jwt-token-generation) ). 


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Bevestigingen 

Willen we Bevestig het volgen van mensen die bijgedragen tot het maken van dit document: Kristjan Jõgi van Axinom, Mingfei Yan en Amit Rajput.
