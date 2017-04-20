<properties 
    pageTitle="Het gebruik van de melding Hubs met Python" 
    description="Informatie over het gebruik van Azure melding Hubs vanuit een Python back-end." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Het gebruik van de melding Hubs van Python
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
U kunt alle functies van de melding Hubs openen vanaf een Java/PHP/Python/Ruby back-enddatabase met de melding Hub REST-interface, zoals wordt beschreven in het onderwerp MSDN [Melding Hubs REST API's](http://msdn.microsoft.com/library/dn223264.aspx).

> [AZURE.NOTE] Dit is de implementatie van een steekproef verwijzing voor de uitvoering van de melding verzendt in Python en is niet de officieel ondersteunde meldingen Hub Python SDK.
>
> In dit voorbeeld is geschreven met Python 3.4.

In dit onderwerp wordt gedemonstreerd hoe u:

* Een REST-client voor melding Hubs functies in Python maken.
* De interface van Python gebruiken de melding Hub REST API's meldingen verzenden. 
* Voor foutopsporing/educatieve doel, zodat u een dump van de REST van de HTTP-aanvraag en respons. 

U kunt de [slag Get-zelfstudie](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) volgen voor uw mobiele platform van keuze het deel van de back-enddatabase implementeren in Python.

> [AZURE.NOTE] Het bereik van de steekproef is alleen beperkte om meldingen te verzenden en deze heeft geen eventuele registratie-management.

## <a name="client-interface"></a>Clientinterface
De belangrijkste clientinterface kunt dezelfde methoden die beschikbaar in de [.NET melding Hubs SDK zijn](http://msdn.microsoft.com/library/jj933431.aspx)bieden. Hiermee kunt u rechtstreeks vertalen alle zelfstudies en voorbeelden die momenteel beschikbaar zijn op deze site en de bijdrage van de community op internet.

U vindt de code die zijn beschikbaar in de [REST van de Python Wikkel steekproef].

Als u bijvoorbeeld een client maken:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
Een Windows-mailpop-upmelding verzenden:
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Implementatie
Als u nog niet hebt gedaan, volgt u onze [Get gestart zelfstudie] snel aan de laatste sectie waar u hebt willen implementeren van de back-enddatabase.

Alle gegevens voor de uitvoering van een volledige REST Wikkel vindt u op [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). In deze sectie worden beschreven de Python uitvoering van de belangrijkste stappen die nodig is voor toegang tot de eindpunten van de melding Hubs REST en meldingen verzenden

1. Parseren van de verbindingsreeks
2. De autorisatietoken genereren
3. Verzenden van een melding dat met HTTP REST API

### <a name="parse-the-connection-string"></a>Parseren van de verbindingsreeks

Hier ziet u de belangrijkste klasse implementeren van de client, waarvan constructor de verbindingsreeks parseert:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Beveiligingstoken maken
De details van de beveiliging token aanmaakdatum vindt u [hier](http://msdn.microsoft.com/library/dn495627.aspx).
De volgende methoden moeten worden toegevoegd aan de klasse **NotificationHub** maken van het token op basis van de URI van de huidige aanvraag en de referenties die zijn opgehaald uit de verbindingsreeks.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Verzenden van een melding dat met HTTP REST API
Eerste, laat gebruik definiëren een klasse dat staat voor een melding.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Deze klasse is een container voor de hoofdtekst van een systeemeigen melding of een set met eigenschappen voor het geval de melding van een sjabloon, een reeks kopteksten bevat opmaak (systeemeigen platform of sjabloon) en platform-specifieke eigenschappen (zoals Apple verlooptijd eigenschap en WNS kop).

Raadpleeg de [documentatie van de melding Hubs REST API's](http://msdn.microsoft.com/library/dn495827.aspx) en de specifieke melding-platforms notaties voor de beschikbare opties.

Nu met deze klasse, kunt we schrijven de verzenden Meldingsmethoden binnen de klasse **NotificationHub** .

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Een HTTP POST de bovenstaande methoden verzendt naar het eindpunt /messages van uw hub melding met de juiste hoofdtekst en koppen aan wie de melding.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Gebruik van foutopsporing eigenschap uitgebreide logboekregistratie inschakelen
Foutopsporing eigenschap inschakelen tijdens de initialisatie van de Hub melding wordt schrijven gedetailleerde logboekregistratie informatie over de HTTP-aanvraag en reactie dump, evenals gedetailleerde Meldingsbericht verzenden resultaat. We deze eigenschap met de naam van [eigenschap melding Hubs TestZenden](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) die resulteert in gedetailleerde informatie over het resultaat van de verzenden melding onlangs toegevoegd. U gebruikt deze - geïnitialiseerd met behulp van de volgende handelingen uit:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Het verzoek melding Hub verzenden HTTP URL een queryreeks "test" Hierdoor wordt toegevoegd. 

##<a name="complete-tutorial"></a>Voltooi de zelfstudie
Nu kunt u de zelfstudie aan de slag door te sturen van de melding van een Python back-end voltooien.

De melding Hubs klant (SUBSTITUEREN tekenreeks en hub naam van de verbinding als gebruiksaanwijzing in deze [ophalen gestart zelfstudie]) geïnitialiseerd:

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Voeg de code verzenden afhankelijk van uw mobiele doel-platform toe. In dit voorbeeld wordt ook toegevoegd hogere niveau methoden als u wilt verzenden op basis van het platform bijvoorbeeld send_windows_notification voor windows; van meldingen inschakelen send_apple_notification (voor apple) enzovoort. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store- en Windows Phone 8.1 (niet-Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 en 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android-apparaat
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Uitvoering van de programmacode Python, moet een melding weergegeven op uw doelapparaat produceren.

## <a name="examples"></a>Voorbeelden:

### <a name="enabling-debug-property"></a>De eigenschap foutopsporing inschakelen
Wanneer u foutopsporing vlag inschakelen tijdens de initialisatie van de NotificationHub ziet u gedetailleerde HTTP-aanvraag en respons dump, evenals NotificationOutcome als volgt te werk waarin u welk HTTP-headers worden doorgegeven in de uitnodiging en welke HTTP-antwoord is ontvangen van de Hub melding kan begrijpen:    ![][1]

Ziet u gedetailleerde melding Hub resultaat bijvoorbeeld 

- Wanneer het bericht is verzonden naar de Push Notification-Service. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Als er geen doelen gevonden voor een push-bericht zijn vervolgens wilt u waarschijnlijk zien van de volgende handelingen uit in het antwoord (waarmee wordt aangegeven dat er geen registraties gevonden voor het leveren van de melding waarschijnlijk omdat de registraties sommige niet-overeenkomende tags zijn)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Broadcast-mailpop-upmelding met Windows 

U ziet u de koppen die verzonden krijgen wanneer u een broadcast-mailpop-upmelding naar Windows client verzendt. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Verzenden van melding geven een tag (of code-expressie)

Zoals u ziet, de labels HTTP-kop die wordt toegevoegd aan de HTTP-aanvraag (in het onderstaande voorbeeld wordt we de melding alleen naar registraties met 'sports' payload verzonden)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Melding opgeven van meerdere labels verzenden

U ziet hoe de labels HTTP-header verandert wanneer meerdere tags worden verzonden. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Sjablonen melding

U ziet dat de wijzigingen van de koptekst opmaken HTTP en de hoofdtekst nettolading als onderdeel van het hoofdgedeelte van de aanvraag wordt verzonden:

**Clientzijde - geregistreerde sjabloon**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Serverzijde - de nettolading verzenden**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Volgende stappen
In dit onderwerp bevat die het maken van een eenvoudige Python REST-client voor melding Hubs. Hier kunt u het volgende doen:

* Download de volledige [Python REST Wikkel voorbeeld], waarin de bovenstaande code.
* Blijven leren over melding Hubs functie labelen in deze [zelfstudie nieuws verbreken]
* Blijven leren over de functie melding Hubs sjablonen in de [lokaliseren nieuws zelfstudie]

<!-- URLs -->
[Python REST Wikkel steekproef]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Get-slag zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Nieuws zelfstudie verbreken]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Nieuws zelfstudie lokaliseren]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
