<properties
    pageTitle="Azure Media Services-foutcodes | Microsoft Azure"
    description="Het onderwerp geeft een overzicht van Azure Media Services-foutcodes."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>

# <a name="azure-media-services-error-codes"></a>Azure Media Services-foutcodes

Wanneer u een Microsoft Azure Media Services gebruikt, ontvangt u mogelijk HTTP-foutcodes van de service afhankelijk van problemen, zoals verificatietokens bijna verlopen naar acties die niet worden ondersteund in Media Services. Hier volgt een lijst met **HTTP-foutcodes** die mogelijk worden geretourneerd door de Media Services en de mogelijke oorzaken voor hen.  
  
## <a name="400-bad-request"></a>400 Ongeldige aanvraag

De aanvraag bevat ongeldige gegevens en is geweigerd vanwege een van de volgende oorzaken hebben:

- Een niet-ondersteunde versie van de API is opgegeven. Zie [configuratie van voor de ontwikkeling van Media Services REST API](media-services-rest-how-to-use.md)voor de meest recente versie.
- De API-versie van Media Services is niet opgegeven. Zie [verbinding maken met Media-Services met de Media Services REST API](media-services-rest-connect-programmatically.md)voor informatie over het opgeven van de API-versie. 
   
    >[AZURE.NOTE] Als u de .NET of Java SDK's verbinding maken met Media-Services gebruikt, wordt de API-versie is opgegeven voor u telkens wanneer u probeert en een actie ten opzichte van de Media Services.
- De eigenschap van een ongedefinieerd is opgegeven. De naam van de eigenschap is in het foutbericht wordt weergegeven. Alleen die eigenschappen die deel uitmaken van een bepaalde entiteit kunnen worden opgegeven. Zie [Azure Media Services REST API-naslaggids](http://msdn.microsoft.com/library/azure/hh973617.aspx) voor een lijst met entiteiten en hun eigenschappen.
- Een ongeldige waarde is opgegeven. De naam van de eigenschap is in het foutbericht wordt weergegeven. Zie de vorige koppeling voor geldige eigenschaptypen en de bijbehorende waarden.
- Een waarde onroerend goed ontbreekt en is vereist.
- Gedeelte van de opgegeven URL bevat een ongeldige waarde.
- Er is geprobeerd een eigenschap WriteOnce bij te werken.
- Er is geprobeerd om te maken van een taak met invoer activa met een primaire AssetFile die is niet opgegeven of kan niet worden bepaald.
- Bij een Locator SA's is een poging gedaan. SA's Locator kunnen alleen worden gemaakt of verwijderd. Streaming Locator kan worden bijgewerkt. Zie [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx)voor meer informatie.
- Een niet-ondersteunde bewerking of de query is verzonden. 

## <a name="401-unauthorized"></a>401 geen toestemming

Het verzoek kan niet worden geverifieerd (voordat deze kan worden geautoriseerd) vanwege een van de volgende oorzaken hebben:

- Ontbrekende verificatie kop.
- Ongeldige verificatie koptekst waarde.
    - Het token is verlopen. Als de REST API's rechtstreeks gebruikt, raadpleegt u [verbinding maken met Media-Services met de Media Services REST API](media-services-rest-connect_programmatically.md) voor meer informatie over hoe u een nieuwe verificatietoken genereren. Als u de .NET of Java SDK's gebruikt, maakt u een object CloudMediaContext of MediaContract als u wilt het token genereren. Zie [verbinding maken met Media-Services met de Media Services SDK voor .NET](media-services-dotnet-connect-programmatically.md)voor meer informatie over hoe u dit wilt doen.
    - Het token bevat een ongeldige handtekening.</li></ul></li></ul>

## <a name="403-forbidden"></a>403

Het verzoek is niet toegestaan vanwege een van de volgende oorzaken hebben:

- De Media Services-account kan niet worden gevonden of is verwijderd.
- De Media Services-account is uitgeschakeld en het aanvraagtype is niet HTTP GET. Bewerkingen service retourneert ook een 403 reactie.
- De verificatietoken bevat geen van de gebruiker referentie-informatie: accountnaam en/of SubscriptionId. U vindt deze informatie in de gebruikersinterface van Media Services-extensie voor uw account Media Services in de beheerportal Azure.
- De resource kan niet worden geopend.
    - Gebruik van een MediaProcessor die is niet beschikbaar voor uw account Media Services is een poging gedaan.
    - Er is geprobeerd een taaksjabloon gedefinieerd door de Media Services bijwerken.
    - Er is geprobeerd te overschrijven Locator enkele andere Media Services van account.
    - Er is geprobeerd te overschrijven enkele andere Media Services-account van ContentKey.

- De resource kan niet worden gemaakt vanwege de quota van een service die is bereikt voor de Media Services-account. Zie [quota en beperkingen](media-services-quotas-and-limitations.md)voor meer informatie over de servicequota.

## <a name="404-not-found"></a>404 niet gevonden

Het verzoek is niet toegestaan voor een resource vanwege een van de volgende oorzaken hebben:

- Er is een poging gedaan naar het bijwerken van een entiteit die niet bestaat.
- Er is geprobeerd om te verwijderen van een entiteit die niet bestaat.
- Er is geprobeerd om te maken van een entiteit die is gekoppeld aan een entiteit die niet bestaat.
- Er is geprobeerd om een entiteit die niet bestaat.
- Er is geprobeerd om op te geven van een opslag-account dat is niet gekoppeld aan de Media Services-account.  

## <a name="409-conflict"></a>409 conflict

Het verzoek is niet toegestaan vanwege een van de volgende oorzaken hebben:

- Meer dan één AssetFile heeft de naam van de opgegeven in de activa.
- Er is geprobeerd om te maken van een tweede primaire AssetFile binnen de activa.
- Er is geprobeerd een ContentKey maken met de opgegeven Id al gebruikt.
- Er is geprobeerd een Locator maken met de opgegeven Id al gebruikt.
- Meer dan één IngestManifestFile heeft de naam van de opgegeven in de IngestManifest.
- Er is geprobeerd om de koppeling van een tweede opslag-versleuteling ContentKey naar de activa opslag versleuteld.
- Er is geprobeerd de dezelfde ContentKey koppelen aan de activa.
- Er is geprobeerd om te maken van een locator naar activa waarvan container opslag ontbreekt of is niet langer gekoppeld aan de activa.
- Er is geprobeerd om te maken van een locator naar activa die al 5 Locator wordt gebruikt. (Azure opslag zorgt ervoor dat de limiet van vijf gedeelde-beleid op één opslag container.)
- Opslag-account van activa koppelen aan een IngestManifestAsset is niet hetzelfde als de opslagruimte-account gebruikt door de bovenliggende IngestManifest.  

## <a name="500-internal-server-error"></a>500 Interne serverfout

Media Services aangetroffen tijdens het verwerken van de aanvraag, sommige problemen die voorkomen de verwerking van doorlopende dat. Dit kan worden veroorzaakt door een van de volgende oorzaken hebben:

- Maken van een actief of taak mislukt, omdat de gegevens over van de Media Services-account Services quotum is tijdelijk niet beschikbaar.
- Maken van een container actief of IngestManifest blob storage mislukt omdat de accountgegevens van de account opslag tijdelijk niet beschikbaar is is.
- Andere onverwachte fout opgetreden. 

## <a name="503-service-unavailable"></a>503 Service niet beschikbaar

De server is momenteel niet mogelijk aanvragen ontvangen. Deze fout kan worden veroorzaakt door overtollige aanvragen voor de service. Media-Services om beperken beperkt het gebruik van de bronnen voor toepassingen die overtollige verzoek in de service aanbrengt.

>[AZURE.NOTE]Controleer het foutbericht wordt weergegeven en codetekenreeks in fout als u meer gedetailleerde informatie over de reden dat u hebt ontvangen de 503-fout. Deze fout betekent niet altijd beperken.

Mogelijke statusbeschrijvingen zijn:

- "Server is bezet. Eerder uitgevoerde van dit type aanvraag duurde langer dan {0} seconden."
- "Server is bezet. Meer dan {0} aanvragen per seconde kan worden vertraagd."
- "Server is bezet. Meer dan {0} aanvragen binnen {1} seconden kan worden vertraagd."

Als u wilt deze fout afhandelen, wordt u aangeraden exponentiële back-uitschakelen opnieuw logica te gebruiken. Dat betekent dat er geleidelijk langer wacht tussen nieuwe pogingen voor opeenvolgende fout antwoorden gebruiken.  Zie voor meer informatie, [Tijdelijke foutenstructuuranalyse afhandelen toepassing blokkeren](https://msdn.microsoft.com/library/hh680905.aspx). 

>[AZURE.NOTE]Als u [Azure Media Services SDK voor .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master)gebruikt, is door de SDK de logica opnieuw voor de 503-fout geïmplementeerd.  
  
## <a name="see-also"></a>Zie ook  

[Media Services Management foutcodes](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
