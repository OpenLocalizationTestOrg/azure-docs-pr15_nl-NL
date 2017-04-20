<properties
    pageTitle="Meld u aan analyses HTTP-gegevens verzamelen API | Microsoft Azure"
    description="U kunt de Log Analytics HTTP gegevens verzamelen API bericht JSON gegevens toevoegen aan de bibliotheek Log analyses van een client die de REST API kunt bellen. In dit artikel wordt beschreven hoe u de API en voorbeelden van het publiceren van gegevens met behulp van verschillende programming talen heeft."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Meld u aan analyses HTTP-gegevens verzamelen API

Wanneer u de API Azure Log Analytics HTTP gegevens verzamelen gebruikt, kunt u posten JavaScript Object notatie (JSON) gegevens toevoegen aan de opslagplaats Log analyses van een client die de REST API kunt bellen. Met deze methode, kunt u gegevens vanuit toepassingen van derden of scripts, zoals verzenden vanuit een runbook in Azure automatisering.  

## <a name="create-a-request"></a>Een nieuw vergaderverzoek maken

De volgende twee tabellen bevatten de kenmerken die vereist voor elk verzoek tot de API Log Analytics HTTP gegevens verzamelen zijn. Elk kenmerk later in het artikel uitvoeriger beschreven.

### <a name="request-uri"></a>URI aanvragen

| Kenmerk | Eigenschap |
|:--|:--|
| Methode | Verzenden |
| URI | https://\<klantnummer\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Inhoudstype | toepassing/json |

### <a name="request-uri-parameters"></a>URI parameters aanvragen
| Parameter | Beschrijving |
|:--|:--|
| Klant-id  | De unieke id voor de werkruimte Microsoft bewerkingen Management Suite. |
| Resource    | De naam van de API-resource: / api/Logboeken. |
| API-versie | De versie van de API voor gebruik met dit verzoek. Dit is momenteel 04-01-2016. |

### <a name="request-headers"></a>Kopteksten aanvragen
| Koptekst | Beschrijving |
|:--|:--|
| Autorisatie | De handtekening autorisatie. Later in dit artikel, kunt u lezen over het maken van een koptekst HMAC-SHA256. |
| Log-Type | Geef het recordtype van de gegevens die wordt verzonden. Het logboektype ondersteunt momenteel alleen alfanumerieke tekens. Numerieke waarden of speciale tekens worden niet ondersteund. |
| x-ms-datum | De datum waarop de aanvraag is verwerkt, in RFC 1123-indeling. |
| tijd-gegenereerd-veld | De naam van een veld in de gegevens die de tijdstempel van het gegevensitem bevat. Als u een veld en vervolgens de inhoud ervan worden gebruikt voor **TimeGenerated**. Als dit veld niet wordt opgegeven, is de standaardwaarde voor **TimeGenerated** de tijd die het bericht is bij. De inhoud van het berichtenveld moeten volgen de ISO 8601-notatie jjjj-MM-ddTHH. |


## <a name="authorization"></a>Autorisatie

Elk verzoek tot de API Log Analytics HTTP gegevens verzamelen moet een koptekst autorisatie bevatten. Als u wilt een verzoek om te verifiëren, moet u zich aanmelden het verzoek met de primaire of de secundaire sleutel voor de werkruimte die het verzoek. Vervolgens doorgeven die handtekening als onderdeel van de aanvraag kunt invullen.   

Hier is de notatie voor de kop autorisatie:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* is de unieke id voor de werkruimte bewerkingen Management Suite. *Handtekening* is een [Hash gebaseerde bericht verificatie Code (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) die is gemaakt op basis van de aanvraag en vervolgens worden berekend met behulp van de [algoritme van de SHA256](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Klik coderen u deze met behulp van Base64-codering.

Gebruik deze notatie voor het coderen van de tekenreeks van de handtekening **SharedKey** :

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Hier volgt een voorbeeld van een tekenreeks handtekening:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Wanneer u de tekenreeks handtekening hebt, deze coderen met behulp van de algoritme van de HMAC-SHA256 op de tekenreeks UTF-8-codering en klikt u vervolgens het resultaat als Base64 coderen. Gebruik deze notatie:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

In de voorbeelden in de volgende secties hebben steekproef code om te maken van een koptekst autorisatie.

## <a name="request-body"></a>Hoofdtekst aanvragen

De hoofdtekst van het bericht moet in JSON. Dit moet een of meer records met de eigenschap naam- en waardeparen opnemen in deze indeling:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

U kunt meerdere records samen in één aanvraag batch met behulp van de volgende indeling. Alle records moet hetzelfde recordtype.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Record van het type en eigenschappen

U kunt een aangepaste recordtype definiëren wanneer u gegevens via de Log Analytics HTTP gegevens verzamelen API indienen. Op dit moment kan u gegevens naar bestaande recordtypen die zijn gemaakt door andere gegevenstypen en oplossingen schrijven. Log Analytics de binnenkomende gegevens leest en maakt vervolgens eigenschappen die overeenkomen met de gegevenstypen van de waarden die u invoert.

Elk verzoek tot de Log Analytics-API moet een **Logboek-Type** koptekst met de naam voor het recordtype bevatten. Het achtervoegsel **_CL** wordt automatisch toegevoegd aan de naam die u invoert als u wilt onderscheiden van andere typen log als een aangepaste logboek. Als u de naam **MyNewRecordType**invoert, maakt Log Analytics u bijvoorbeeld een record met het type **MyNewRecordType_CL**. Hiermee kunt zorgen ervoor dat er geen conflicten tussen de gebruiker gedefinieerde namen en de meegeleverde in de huidige of toekomstige oplossingen van Microsoft.

Als u wilt identificeren van een eigenschap-gegevenstype, voegt Log Analytics het achtervoegsel aan de naam van de eigenschap. Als u een eigenschap een null-waarde bevat, wordt de eigenschap niet opgenomen in die record. In deze tabel bevat de eigenschap-gegevenstype en de bijbehorende achtervoegsel:

| Eigenschap-gegevenstype | Achtervoegsel |
|:--|:--|
| Tekenreeks    | _K |
| Booleaanse waarde   | _b |
| Dubbeltik    | _D |
| Datum/tijd | _Nauw |
| GUID      | _g |


Het gegevenstype die Log Analytics voor elke eigenschap wordt gebruikt, is afhankelijk van de vraag of de record van het type voor de nieuwe record al bestaat.

- Als het recordtype niet bestaat, wordt een nieuw gemaakt door Log Analytics. De JSON type implicatie log Analytics gebruikt om te bepalen van het gegevenstype voor elke eigenschap voor de nieuwe record.
- Als het recordtype bestaat, wordt Log Analytics probeert te maken van een nieuwe record op basis van bestaande eigenschappen. Als het gegevenstype voor een eigenschap in de nieuwe record niet overeenkomen met en kan niet worden geconverteerd naar het bestaande type of als de record een eigenschap die niet bestaat bevat, Log Analytics wordt een nieuwe eigenschap die de relevante achtervoegsel heeft gemaakt.

Dit item indiening zou bijvoorbeeld een record te maken met drie eigenschappen, **number_d** **boolean_b**en **string_s**:

![Voorbeeldrecord 1](media/log-analytics-data-collector-api/record-01.png)

Als u dit volgende item, klikt u vervolgens ingediend met alle waarden die zijn opgemaakt als tekenreeksen, worden de eigenschappen zou niet wijzigen. Deze waarden kunnen worden geconverteerd naar bestaande gegevenstypen:

![Voorbeeldrecord 2](media/log-analytics-data-collector-api/record-02.png)

Maar als u deze volgende indiening vervolgens hebt aangebracht, Log Analytics maakt de nieuwe eigenschappen **boolean_d** en **string_d**. Deze waarden kunnen niet worden geconverteerd:

![Voorbeeldrecord 3](media/log-analytics-data-collector-api/record-03.png)

Als u de volgende vermelding, klikt u vervolgens ingediend voordat het recordtype dat is gemaakt, maak Log Analytics een record in met drie eigenschappen, **aantal-gunstig**, **boolean_s**en **string_s**. In dit item, is elk van de beginwaarden opgemaakt als een tekenreeks:

![Voorbeeldrecord 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Limieten voor gegevens
Zijn er enkele beperkingen rond de gegevens die aan de collectie Analytics logboekgegevens API worden geplaatst.

- Maximaal 30 MB per posten naar Log Analytics gegevens verzamelen API. Dit is een limiet voor een enkel bericht. Als de gegevens uit één die boeken groter is dan 30 MB, moet u de gegevens snel aan de kleiner formaat stukken splitsen en stuurt u gelijktijdig. 
- Maximaal 32 KB limiet voor veldwaarden. Als de waarde van het veld groter dan 32 KB is, worden de gegevens afgekapt. 
- Aanbevolen maximum aantal velden voor een bepaald type is 50. Dit is een limiet van een bruikbaarheid en de zoeken-ervaring perspectief.  


## <a name="return-codes"></a>Codes retourneren

De HTTP-statuscode 202 betekent dat de aanvraag is geaccepteerd voor verwerking, maar verwerking is nog niet voltooid. Hiermee wordt aangeduid dat de bewerking is voltooid.

Deze tabel bevat de volledige reeks statuscodes die u kunt de service mogelijk geretourneerd:

| Code | Status | Foutcode | Beschrijving |
|:--|:--|:--|:--|
| 202 | Geaccepteerd |  | De aanvraag is geaccepteerd. |
| 400 | Ongeldige aanvraag | InactiveCustomer | De werkruimte is gesloten. |
| 400 | Ongeldige aanvraag | InvalidApiVersion | De API-versie die u hebt opgegeven, wordt niet herkend door de service. |
| 400 | Ongeldige aanvraag | InvalidCustomerId | De opgegeven ID voor de werkruimte is ongeldig. |
| 400 | Ongeldige aanvraag | InvalidDataFormat | Ongeldige JSON is verzonden. De hoofdtekst van het antwoord mogelijk meer informatie over het oplossen van de fout bevatten. |
| 400 | Ongeldige aanvraag | InvalidLogType | Het logboektype opgegeven container speciale tekens of numerieke waarden. |
| 400 | Ongeldige aanvraag | MissingApiVersion | De API-versie niet is opgegeven. |
| 400 | Ongeldige aanvraag | MissingContentType | Het type inhoud is niet opgegeven. |
| 400 | Ongeldige aanvraag | MissingLogType | De vereiste waardetype log is niet opgegeven. |
| 400 | Ongeldige aanvraag | UnsupportedContentType | Het inhoudstype niet is ingesteld op **toepassing/json**. |
| 403 | Verboden | InvalidAuthorization | De service is mislukt voor de verificatie van het verzoek. Controleer of de werkruimte-ID en verbinding toets geldig zijn. |
| 500 | Interne serverfout | UnspecifiedError | De service is een interne fout opgetreden. Probeer het verzoek. |
| 503 | Service niet beschikbaar | ServiceUnavailable | De service is momenteel niet beschikbaar voor het ontvangen van aanvragen. Probeer uw aanvraag kunt invullen. |

## <a name="query-data"></a>Querygegevens

Gegevens die zijn verzonden door de Log Analytics HTTP gegevens verzamelen API query, zoeken naar records met **Type** die gelijk is aan de waarde **LogType** die u hebt opgegeven, met toegevoegde **_CL**. Bijvoorbeeld als u **MyCustomLog**hebt gebruikt, u zou retourneert alle records met **Type = MyCustomLog_CL**.


## <a name="sample-requests"></a>Voorbeeld aanvragen

In de volgende secties vindt u voorbeelden van het indienen van gegevens tot de API Log Analytics HTTP gegevens verzamelen met behulp van verschillende talen.

Voer deze stappen voor het instellen van de variabelen voor de kop autorisatie voor elk monster:

1. Klik in de portal bewerkingen Management Suite selecteert u de tegel **Instellingen** en selecteer vervolgens het tabblad **Bronnen verbonden** .
2. Aan de rechterkant van de **Werkruimte-ID**, selecteer het pictogram kopiëren en plak de ID als de waarde van de **Klant-ID** -variabele.
3. Aan de rechterkant van de **Primaire sleutel**, selecteer het pictogram kopiëren en plak de ID als de waarde van de variabele **Shared Key** .

U kunt ook de variabelen voor de Logboektype en de JSON-gegevens wijzigen.

### <a name="powershell-sample"></a>PowerShell-voorbeeld

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C#-voorbeeld

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Voorbeeld van Python

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Volgende stappen

- Gebruik de [Ontwerpfunctie voor weergaven](log-analytics-view-designer.md) maken van aangepaste weergaven op de gegevens die u verzendt.
