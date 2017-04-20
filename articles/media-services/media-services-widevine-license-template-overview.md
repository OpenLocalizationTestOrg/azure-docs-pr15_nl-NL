<properties 
    pageTitle="Overzicht van de sjabloon Widevine licentie | Microsoft Azure" 
    description="Dit onderwerp biedt een overzicht van een sjabloon van Widevine licentie waarmee Widevine licenties configureren." 
    authors="juliako" 
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
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="widevine-license-template-overview"></a>Overzicht van de sjabloon Widevine licentie

##<a name="overview"></a>Overzicht

Azure Media Services kunt u nu configureren en Widevine licenties aanvragen. Wanneer de eindgebruiker-speler probeert uw Widevine beveiligde inhoud af te spelen, wordt een aanvraag verzonden naar de bezorgingsservice licentie om een licentie te verkrijgen. Als de service licentie de aanvraag goedkeurt, oplossen deze de licentie die wordt verzonden naar de client en u ontsleutelt en het afspelen van de opgegeven inhoud kan worden gebruikt.

Widevine licentieaanvraag is opgemaakt als een bericht JSON.  

Houd er rekening mee dat u kunt een lege bericht maken zonder waarden alleen '{}"en een licentie-sjabloon wordt gemaakt met alle standaardwaarden.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON-bericht

Naam | Waarde | Beschrijving
---|---|---
nettolading |Base64-gecodeerde tekenreeks |De licentieaanvraag van een klant. 
content_id | Base64-gecodeerde tekenreeks|ID gebruikt om te leiden KeyId(s) en inhoud (s) voor elke content_key_specs.track_type.
provider |tekenreeks |Wordt gebruikt voor het opzoeken van inhoud toetsen en beleid. Vereist.
Naam_van_beleid | tekenreeks |De naam van een eerder geregistreerde beleid. Optionele
allowed_track_types | opsommen  | SD_ONLY of SD_HD. Besturingselementen voor inhoud van sleutels moeten worden opgenomen in een licentie
content_key_specs | matrix van JSON structuren, Bekijk **Inhoud toets specificaties** onderstaande | Een betere gegreineerd besturingselement op welke inhoud de pijltoetsen om terug te keren. Zie inhoud toets specificatie hieronder voor meer informatie.  Er kan slechts één van allowed_track_types en content_key_specs worden opgegeven. 
use_policy_overrides_exclusively | Booleaanse waarde. waar of ONWAAR | Beleid kenmerken die zijn opgegeven door policy_overrides gebruiken en weglaten van alle eerder opgeslagen beleid.
policy_overrides | JSON ordenen, Zie onderstaande worden **Overschreven** | Beleidsinstellingen voor deze licentie.  In het geval van deze activa heeft een vooraf gedefinieerd beleid, wordt deze opgegeven waarden worden gebruikt. 
session_init | JSON ordenen, Zie onderstaande **Initialisatie van de sessie** | Optionele gegevens doorgegeven aan de licentie.
parse_only | Booleaanse waarde. waar of ONWAAR | Het licentieverzoek moet worden verdeeld, maar geen licentie is verleend. Echter waarden, het licentieverzoek worden geretourneerd in het formulier.  

##<a name="content-key-specs"></a>Inhoud belangrijke kenmerken 

Als een bestaande beleid bestaat, wordt niet nodig om op te geven een van de waarden in de inhoud toets specificatie.  Het bestaande beleid dat is gekoppeld aan dit inhoudstype worden gebruikt om te bepalen de beveiliging uitvoer zoals HDCP en CGMS.  Als een bestaande beleid is niet geregistreerd met de licentieserver Widevine, kan de provider van de inhoud de waarden invoeren in het licentieverzoek.   


Elke content_key_specs moet worden opgegeven voor alle sporen, ongeacht de optie use_policy_overrides_exclusively. 


Naam | Waarde | Beschrijving
---|---|---
content_key_specs. track_type | tekenreeks | Een naam van het type bijhouden. Als content_key_specs is opgegeven in het licentieverzoek, moet u opgeven dat alle typen expliciet bijhouden. Niet doet treden mislukt afspelen afgelopen 10 seconden. 
content_key_specs  <br/> security_level | UInt32 | Hiermee definieert u client stabiliteit vereisten voor afspelen. <br/> 1 - software gebaseerde whitebox cryptografische is vereist. <br/> 2 - software cryptografische en een verborgen mediadecoder is vereist. <br/> 3 - de belangrijkste materiaal en de cryptografische bewerkingen moeten worden uitgevoerd binnen een hardware back-ups vertrouwde omgeving. <br/> 4 - de cryptografische en decoderen van inhoud moeten worden uitgevoerd binnen een hardware back-ups vertrouwde omgeving.  <br/> 5 - de cryptografische, decoderen en alle verwerkingstijd van de media (gecomprimeerd en niet-gecomprimeerd) moeten worden afgehandeld binnen een hardware back-ups vertrouwde omgeving.  
content_key_specs <br/> required_output_protection.hDC | tekenreeks - een van: HDCP_NONE, HDCP_V1, HDCP_V2 | Geeft aan of HDCP is vereist
content_key_specs <br/>toets | Base64 <br/>gecodeerde tekenreeks|Inhoud sleutel wilt gebruiken voor deze bijhouden. Als u opgeeft, is de track_type of key_id vereist.  Deze optie kunt de provider van de inhoud naar de inhoud sleutel voor dit nummer in plaats van de licentieserver Widevine genereren of het opzoeken van een sleutel laten invoeren.
content_key_specs.key_id| Base64 gecodeerde tekenreeks binaire, 16 bytes | Unieke id voor de sleutel. 


##<a name="policy-overrides"></a>Beleid negeren 

Naam | Waarde | Beschrijving
---|---|---
policy_overrides. can_play | Booleaanse waarde. waar of ONWAAR | Geeft aan dat afspelen van de inhoud is toegestaan. Standaardwaarde is ONWAAR.
policy_overrides. can_persist | Booleaanse waarde. waar of ONWAAR |Geeft aan dat de licentie mogelijk gehandhaafd voor niet-vluchtige opslag voor gebruik offline. Standaardwaarde is ONWAAR.
policy_overrides. can_renew | Booleaanse waar of ONWAAR |Geeft aan dat de verlenging van deze licentie is toegestaan. Als dit waar is, kan de duur van de licentie worden uitgebreid door heartbeat. Standaardwaarde is ONWAAR. 
policy_overrides. license_duration_seconds | Int64 | Geeft het tijdvenster voor deze specifieke licentie. De waarde 0 geeft aan dat er geen limiet voor de duur. Standaard is aan 0. 
policy_overrides. rental_duration_seconds | Int64 | Geeft het tijdvenster tijdens het afspelen is toegestaan. De waarde 0 geeft aan dat er geen limiet voor de duur. Standaard is aan 0. 
policy_overrides. playback_duration_seconds | Int64 | Het weergavevenster tijd nadat afgespeeld binnen de duur van de licentie. De waarde 0 geeft aan dat er geen limiet voor de duur. Standaard is aan 0. 
policy_overrides. renewal_server_url |tekenreeks | Alle heartbeat (verlenging) aanvragen voor deze licentie wordt doorgestuurd naar de opgegeven URL. Dit veld wordt alleen gebruikt als can_renew waar is.
policy_overrides. renewal_delay_seconds |Int64 |Hoeveel seconden na license_start_time, voordat de verlenging voor het eerst wordt uitgevoerd. Dit veld wordt alleen gebruikt als can_renew waar is. Standaard is aan 0 
policy_overrides. renewal_retry_interval_seconds | Int64 | Hiermee geeft u de vertraging in seconden tussen de volgende licentie verlenging aanvragen voor het geval is mislukt. Dit veld wordt alleen gebruikt als can_renew waar is. 
policy_overrides. renewal_recovery_duration_seconds | Int64 | Het venster van de tijd, waarin afspelen is toegestaan om door te gaan terwijl verlenging is poging tot, nog niet lukt vanwege backend problemen met de licentieserver. De waarde 0 geeft aan dat er geen limiet voor de duur. Dit veld wordt alleen gebruikt als can_renew waar is.
policy_overrides. renew_with_usage | Booleaanse waar of ONWAAR |Geeft aan dat de licentie wordt toegezonden voor verlenging wanneer gebruik wordt gestart. Dit veld wordt alleen gebruikt als can_renew waar is. 

##<a name="session-initialization"></a>Initialisatie van de sessie

Naam | Waarde | Beschrijving
---|---|---
provider_session_token | Base64 codering tekenreeks |Deze sessietoken wordt doorgegeven terug in de licentie en in de volgende vernieuwingen aanwezig.  De sessietoken wordt niet permanent buiten sessies. 
provider_client_token | Base64-gecodeerde tekenreeks | Clienttoken verzenden terug in het antwoord licentie.  Als het licentieverzoek een clienttoken bevat, wordt deze waarde genegeerd. De clienttoken blijft voorbij licentie sessies.
override_provider_client_token | Booleaanse waarde. waar of ONWAAR |Als onwaar en de licentieaanvraag bevat een clienttoken, gebruikt u het token van de aanvraag, zelfs als een clienttoken is opgegeven in deze structuur.  Als de waarde true, altijd de token die is opgegeven in deze structuur te gebruiken.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Uw Widevine licenties met .NET-gegevenstypen configureren

Media Services biedt .NET-API's waarmee u uw licenties Widevine configureren. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klassen zoals gedefinieerd in de Media Services .NET SDK

Hier volgen de definities van deze diagramtypen.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Voorbeeld

Het volgende voorbeeld ziet u hoe u .NET-API's gebruikt voor het configureren van een eenvoudige Widevine-licentie.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Zie ook

[PlayReady en/of Widevine dynamisch algemene versleuteling gebruikt](media-services-protect-with-drm.md)
