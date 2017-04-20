<properties
    pageTitle="Overzicht van Access handtekeningen gedeeld | Microsoft Azure"
    description="Wat zijn gedeeld Access handtekeningen, hoe ze werken, en hoe u ze in het knooppunt, PHP en C# gebruiken."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Gedeelde toegang handtekeningen

*Access handtekeningen gedeeld* (SA's) zijn de primaire beveiliging om Service Bus, met inbegrip van de gebeurtenis Hubs, brokered messaging (wachtrijen en onderwerpen) en doorgegeven messaging. In dit artikel wordt beschreven hoe gedeeld Access handtekeningen, hoe ze werken en hoe deze in een platform-agnostic manier kunt gebruiken.

## <a name="overview-of-sas"></a>Overzicht van SA 's

Gedeelde Access handtekeningen zijn een verificatiemethode op basis van de beveiligde hashes SHA-256 of URI's. SA's is een zeer krachtige methode die wordt gebruikt door alle Service Bus-services. Werkelijke wordt gebruikt, SA's bestaat uit twee onderdelen: een *Access-beleid gedeeld* en een *Access-handtekening gedeeld* (vaak wel een *token*genoemd).

U vindt meer gedetailleerde informatie over Access handtekeningen gedeeld met Service Bus in [Access-handtekening gedeeld verificatie met Service Bus](service-bus-shared-access-signature-authentication.md).

## <a name="shared-access-policy"></a>Gedeelde-beleid

Een belangrijk punt voor meer informatie over over SA's is dat alle met een beleid begint. Voor elk beleid, dat u besluit op drie soorten informatie: **naam**, het **bereik**en **machtigingen**. De **naam** is dit alleen. een unieke naam in dat bereik. Het bereik is eenvoudig genoeg: de URI van de desbetreffende resource is. Voor een naamruimte Service Bus, is het bereik de volledig gekwalificeerde domeinnaam (FQDN), zoals `https://<yournamespace>.servicebus.windows.net/`.

De beschikbare machtigingen voor een beleid zijn grotendeels geen uitleg:

  + Verzenden
  + Luisteren
  + Beheren

Nadat u het beleid hebt gemaakt, kunt deze een *Primaire sleutel* en een *Secundaire sleutel*is toegewezen. Hierna ziet u versleutelen sterke toetsen. Niet ze kwijtraakt of ze lekken - ze altijd waarna steeds beschikbaar is in de [portal van Azure][]. U kunt een van de gegenereerde toetsen en u deze op elk gewenst moment kunt genereren. Als u genereren of wijzigen van de primaire sleutel in het beleid, wordt u alle Access gedeeld handtekeningen gemaakt op basis van deze ongeldig.

Wanneer u een naamruimte Service Bus maakt, een beleid wordt automatisch gemaakt voor de gehele naamruimte **RootManageSharedAccessKey**genoemd, en dit beleid heeft alle machtigingen. U zich niet aanmelden als **hoofdsite**, zodat dit beleid niet gebruikt, tenzij er een goede reden is. U kunt extra beleidsregels maken op het tabblad **configureren** voor de naamruimte in de portal. Het is belangrijk dat notitie dat één niveau in de Service Bus structuur (naamruimte, wachtrij, gebeurtenis-Hub, enz.) tot 12 beleidsregels die zijn bijgevoegd bij deze alleen kan hebben.

## <a name="shared-access-signature-token"></a>Gedeelde Access handtekening (token)

Het beleid zelf is niet het toegangstoken voor Service Bus. Het object waaruit het toegangstoken wordt gegenereerd - met het primaire of secundaire sleutel is. Het token wordt gegenereerd door zorgvuldig door een tekenreeks in de volgende indeling:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Waar `signature-string` de hash SHA-256 van het bereik van de token (**bereik** zoals is beschreven in de vorige sectie) met een CRLF toegevoegd en een verlooptijd is (in seconden sinds de epoche: `00:00:00 UTC` op 1 januari 1970).

De hash ziet er ongeveer als de volgende pseudocode en geeft als resultaat 32 bytes.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

De waarden niet-hashing zijn in de tekenreeks **SharedAccessSignature** zodat de geadresseerde kunt hash met dezelfde parameters, om ervoor te zorgen dat deze heeft hetzelfde resultaat berekenen. De URI Hiermee geeft u het bereik en naam van de sleutel geeft aan wat het beleid moet worden gebruikt de hash te berekenen. Dit is belangrijk uit veiligheidsoverwegingen. Als de handtekening die niet overeenkomen met die waarin de geadresseerde (Service Bus) wordt berekend, is klikt u vervolgens toegang geweigerd. U kunt nu zorgen dat de afzender toegang tot de sleutel heeft en de rechten die zijn opgegeven in het beleid moet krijgen worden.

## <a name="generating-a-signature-from-a-policy"></a>Een handtekening uit een beleid te genereren

Hoe daadwerkelijk doet u dit in code? We even verder kijken we enkele van de volgende.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>C & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Gebruik van de toegang tot gedeelde handtekening (op het niveau van de HTTP)
 
Nu u weet hoe gedeeld Access handtekeningen maken voor alle entiteiten in Service Bus, bent u gereed om te voeren een HTTP-bericht:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Onthoud dat dit werkt voor alles. U kunt SA's maken voor een wachtrij, onderwerp, abonnement, gebeurtenis Hub of relay. Als u publisher-identiteit voor gebeurtenis Hubs gebruikt, kunt u gewoon toevoegen `/publishers/< publisherid>`.

Als u een afzender of een token SA's-client, ze niet rechtstreeks de sleutel hebben en ze de hash voor deze niet terugdraaien. Als zodanig, hebt u controle over wat ze toegang heeft tot, en hoe lang. Een belangrijke wat u moet onthouden is als u de primaire sleutel in het beleid wijzigt, wordt alle Access gedeeld handtekeningen gemaakt op basis van deze ongeldig.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Gebruik van de toegang tot gedeelde handtekening (op het niveau van de AMQP)

In de vorige sectie hebt gezien u hoe u het token SA's met een HTTP POST-aanvraag voor het verzenden van gegevens naar de Service-Bus. Als u weet, kunt u met de geavanceerde bericht Queuing-Protocol (AMQP) die de voorkeur protocol moet worden gebruikt voor betere prestaties in veel gevallen Bus voor Service kunt openen. De token gebruik SA's met AMQP wordt beschreven in het document [AMQP Claim-Based beveiliging versie 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) die in werken concept is sinds 2013 maar goed ondersteund door Azure vandaag.

Voordat u begint gegevens te verzenden naar Service-Bus, moet de uitgever het token SA's binnen een AMQP-mailbericht verzenden naar een duidelijke AMQP knooppunt met de naam **$cbs** (u kunt deze zien als een "speciale" wachtrij die door de service wordt gebruikt bij het aanschaffen en alle bijbehorende SA's tokens valideren). De uitgever moet de **ReplyTo** -veld in het bericht AMQP; opgeven Dit is het knooppunt waarin antwoorden in de service aan de uitgever met het resultaat van de token validatie (een eenvoudige aanvraag/beantwoorden patroon tussen publisher en service). Dit knooppunt antwoord wordt gemaakt 'in de browser,"spreekt over 'dynamische maken van extern knooppunt', zoals beschreven in de specificatie AMQP 1.0. Nadat u hebt gecontroleerd of het token SA's geldig is, kan de uitgever voorwaarts gaan en begin met het verzenden van gegevens naar de service.

De volgende stappen hoe het token SA's verzenden met AMQP protocol met de [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) -bibliotheek. Dit is handig als u niet de officiële Service Bus SDK gebruiken (bijvoorbeeld op WinRT, .net Compact Framework, .net Framework Micro en zwart) ontwikkelen in C\#. Deze bibliotheek is natuurlijk nuttig om u te helpen begrijpen hoe op claims gebaseerde beveiliging op het niveau van AMQP werkt zoals u hebt gezien hoe dit werkt op het niveau van de HTTP (met een HTTP POST-aanvraag en de SA's token verzonden in de koptekst "Autorisatie"). Als u niet zoals uitgebreide kennis van AMQP nodig hebt, kunt u de officiële Service Bus SDK met .net Framework-toepassingen, waarin het voor u doen.

### <a name="c35"></a>C & #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

De `PutCbsToken()` methode ontvangt de *verbinding* (AMQP verbinding class exemplaar als opgegeven door de [AMQP .NET Lite bibliotheek](https://github.com/Azure/amqpnetlite)) die de TCP-verbinding vertegenwoordigt op de service en de *sasToken* parameter weer die is het token SA's om te verzenden. 

> [AZURE.NOTE] Het is belangrijk dat de verbinding wordt gemaakt met **SASL verificatiemethode ingesteld op extern** (en niet de standaard zonder opmaak met gebruikersnaam en wachtwoord gebruikt wanneer u niet nodig hebt om te verzenden, het token SA's).

Het publisher maakt vervolgens twee AMQP koppelingen voor het token SA's verzenden en ontvangen van het antwoord (het validatieresultaat van de token) van de service.

Het bericht AMQP bevat een set met eigenschappen en meer informatie dan een eenvoudig bericht. Het token SA's is de hoofdtekst van het bericht (via de constructor). De eigenschap **"ReplyTo"** is ingesteld op de naam van het knooppunt voor het ontvangen van het validatieresultaat naar de ontvanger-koppeling (u kunt de naam wijzigen als u wilt, en deze dynamisch door de service gemaakt wordt). De laatste drie toepassing/aangepaste eigenschappen worden gebruikt door de service om aan te geven welk type bewerking deze uit te voeren. Zoals beschreven in de specificatie van CBS concept, moeten ze de **naam van de bewerking** ("opslag-token"), het **type token** (in dit geval een "servicebus.windows.net:sastoken") en de **"naam" van de doelgroep** waarop het token van toepassing is (het geheel).

Na het token SA's op de koppeling van de afzender verzendt, moet de uitgever het antwoord op de koppeling van de ontvanger lezen. Het antwoord is een eenvoudige AMQP-bericht met de eigenschap van een toepassing met de naam **"statuscode"** dat dezelfde waarden als een HTTP-statuscode kan bevatten. 

## <a name="next-steps"></a>Volgende stappen

Zie de [Service Bus REST API verwijzing](https://msdn.microsoft.com/library/azure/hh780717.aspx) voor meer informatie over wat u met deze tokens SA's doen kunt.

Zie voor meer informatie over de Service Bus verificatie, [Service Bus verificatie en machtiging](service-bus-authentication-and-authorization.md). 

Als u meer voorbeelden van SA's in C# en JavaScript zijn opgeslagen in [dit blogbericht](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx).

[Azure-portal]: https://portal.azure.com