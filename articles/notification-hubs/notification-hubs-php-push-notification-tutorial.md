<properties 
    pageTitle="Het gebruik van de melding Hubs met PHP" 
    description="Informatie over het gebruik van Azure melding Hubs vanuit een PHP back-end." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Het gebruik van de melding Hubs van PHP
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

U kunt alle functies van de melding Hubs openen vanaf een Java/PHP/Ruby back-enddatabase met de melding Hub REST-interface, zoals wordt beschreven in het onderwerp MSDN [Melding Hubs REST API's](http://msdn.microsoft.com/library/dn223264.aspx).

In dit onderwerp wordt gedemonstreerd hoe u:

* Een REST-client voor melding Hubs functies in PHP;
* Volg de [slag Get-zelfstudie](notification-hubs-ios-apple-push-notification-apns-get-started.md) voor uw mobiele platform van keuze het deel van de back-enddatabase implementeren in PHP.

## <a name="client-interface"></a>Clientinterface
De belangrijkste clientinterface kunt bieden dezelfde methoden die beschikbaar in de [.NET melding Hubs SDK zijn](http://msdn.microsoft.com/library/jj933431.aspx), Hierdoor kunt u rechtstreeks vertalen alle zelfstudies en voorbeelden die momenteel beschikbaar zijn op deze site, en de bijdrage van de community op internet.

U vindt de code die zijn beschikbaar in de [REST van de PHP Wikkel voorbeeld].

Als u bijvoorbeeld een client maken:

    $hub = new NotificationHub("connection string", "hubname"); 

Een iOS systeemeigen bericht verzenden:
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementatie
Als u nog niet hebt gedaan, volgt u onze [Get gestart zelfstudie] snel aan de laatste sectie waar u hebt willen implementeren van de back-enddatabase.
Ook als u wilt dat kunt u de code uit de [REST van de PHP Wikkel voorbeeld] gebruiken en Ga rechtstreeks naar de sectie [voltooid de zelfstudie](#complete-tutorial) .

Alle gegevens voor de uitvoering van een volledige REST Wikkel vindt u op [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). In deze sectie wordt de PHP uitvoering van de belangrijkste stappen die is vereist voor toegang tot melding Hubs REST eindpunten worden beschreven:

1. Parseren van de verbindingsreeks
2. De autorisatietoken genereren
3. De HTTP-oproep uitvoeren

### <a name="parse-the-connection-string"></a>Parseren van de verbindingsreeks

Hier ziet u de belangrijkste klasse implementeren van de client, waarvan constructor die de verbindingsreeks parseert:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Beveiligingstoken maken
De details van de beveiliging token aanmaakdatum vindt u [hier](http://msdn.microsoft.com/library/dn495627.aspx).
De volgende methode heeft moet worden toegevoegd aan de klasse **NotificationHub** maken van het token op basis van de URI van de huidige aanvraag en de referenties die zijn opgehaald uit de verbindingsreeks.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Een bericht verzenden
Laat ons definiëren eerst een klasse dat staat voor een melding.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Deze klasse is een container voor de tekst voor een systeemeigen melding of een set met eigenschappen op hoofdletters/kleine letters van de melding van een sjabloon en een reeks kopteksten bevat opmaak (systeemeigen platform of sjabloon) en platform-specifieke eigenschappen (zoals Apple verlooptijd eigenschap en WNS kop).

Raadpleeg de [documentatie van de melding Hubs REST API's](http://msdn.microsoft.com/library/dn495827.aspx) en de specifieke melding-platforms notaties voor de beschikbare opties.

Uitgerust met deze klasse, kunnen we nu schrijven het verzenden Meldingsmethoden binnen de klasse **NotificationHub** .

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Een HTTP POST de bovenstaande methoden verzendt naar het eindpunt /messages van uw hub melding met de juiste hoofdtekst en koppen aan wie de melding.

##<a name="complete-tutorial"></a>Voltooi de zelfstudie
Nu kunt u de zelfstudie aan de slag door te sturen van de melding van een PHP back-end voltooien.

De melding Hubs klant (SUBSTITUEREN tekenreeks en hub naam van de verbinding als gebruiksaanwijzing in deze [ophalen gestart zelfstudie]) geïnitialiseerd:

    $hub = new NotificationHub("connection string", "hubname"); 

Voeg de code verzenden afhankelijk van uw mobiele doel-platform toe.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store- en Windows Phone 8.1 (niet-Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android-apparaat
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 en 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Uitvoering van de programmacode PHP moet produceren nu een melding op uw apparaat wordt weergegeven.


## <a name="next-steps"></a>Volgende stappen
In dit onderwerp bevat die het maken van een eenvoudige Java REST-client voor melding Hubs. Hier kunt u het volgende doen:

* Download de volledige [PHP REST Wikkel voorbeeld], waarin de bovenstaande code.
* Blijven leren over melding Hubs functie tags in de [verbreken nieuws zelfstudie]
* Meer informatie over meldingen voor afzonderlijke gebruikers drukken in [hoogte gebruikers zelfstudie]

Zie ook het [PHP Developer Center](/develop/php/)voor meer informatie.

[PHP REST Wikkel steekproef]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Get-slag zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
