<properties 
    pageTitle="Overzicht van de gebeurtenis Hubs verificatie en beveiliging model | Microsoft Azure"
    description="Gebeurtenis Hubs verificatie en beveiliging model overzicht."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Gebeurtenis Hubs verificatie en beveiliging model overzicht

De gebeurtenis Hubs beveiligingsmodel voldoet aan de volgende vereisten:

- Alleen apparaten die geldige referenties kunnen gegevens verzenden naar een Hub voor de gebeurtenis.
- Een apparaat niet kan zich voordoen als een ander apparaat.
- Een apparaat rogue kan worden geblokkeerd uit het verzenden van gegevens naar een Hub voor de gebeurtenis.

## <a name="device-authentication"></a>Verificatie apparaat

De gebeurtenis Hubs beveiligingsmodel is gebaseerd op een combinatie van tokens [Gedeeld Access handtekening (SA's)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) en *gebeurtenisuitgevers*. Een gebeurtenis-aanbieder Hiermee definieert u een virtuele eindpunt voor de Hub van een gebeurtenis. De uitgever kan alleen worden gebruikt om berichten te verzenden naar een Hub gebeurtenis. Het is niet mogelijk is voor het ontvangen van berichten van een uitgever.

Een gebeurtenis Hub dienst meestal één publisher per apparaat. Alle berichten die zijn verzonden naar een van de uitgevers van de Hub van een gebeurtenis zijn in wachtrij binnen de Hub van die gebeurtenis. Uitgevers inschakelen fijnmazige toegangsbeheer en beperken.

Elk apparaat kan een unieke token, waarmee wordt geüpload naar het apparaat dat is toegewezen. De tokens worden geproduceerd zodat elke unieke token toegang aan een andere unieke uitgever verleent. Een apparaat die beschikken over een token kan alleen verzenden naar één publisher, maar geen andere publisher. Als meerdere apparaten hetzelfde wilt delen, klikt u vervolgens deelt elk van deze apparaten een uitgever.

Hoewel het niet aanbevolen, is het mogelijk zowel apparaten met tokens die directe toegang aan de Hub van een gebeurtenis verlenen. Elk apparaat waarin dit token kunt berichten verzenden rechtstreeks in de Hub van die gebeurtenis. Dergelijk apparaat, worden niet detailpagina te beperken. Het apparaat niet kunt bovendien worden gebeurd verzenden op de Hub van die gebeurtenis.

Alle tokens hebben aangemeld met een sleutel SA's. Meestal hebben alle tokens aangemeld met dezelfde sleutel. Apparaten zijn niet op de hoogte van de sleutel uit. Hiermee voorkomt u dat apparaten tokens in productieomgevingen.

### <a name="create-the-sas-key"></a>De sleutel SA's maken

Bij het maken van een gebeurtenis Hubs naamruimte, genereert Azure gebeurtenis Hubs een 256-bits SA's gebruiken met de naam **RootManageSharedAccessKey**. Deze toets verleent verzenden, beluisteren en machtigen voor de naamruimte beheren. U kunt extra toetsen maken. Het wordt aanbevolen dat u een sleutel dat verleent machtigingen naar de specifieke gebeurtenis Hub verzenden produceren. Voor de rest van dit onderwerp, wordt uitgegaan dat u deze toets naam `EventHubSendKey`.

Het volgende voorbeeld, wordt er een sleutel verzenden alleen-lezen gemaakt bij het maken van de Hub gebeurtenis:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Tokens genereren

U kunt met de toets SA's tokens genereren. U kunt slechts één token per apparaat moet opleveren. Tokens kunnen vervolgens worden gemaakt met behulp van de volgende methode. Alle tokens worden gegenereerd met de toets **EventHubSendKey** . Elke token is een unieke URI toegewezen.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Bij het aanroepen van deze methode, de URI moet worden opgegeven als `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Voor alle tokens, de URI is identiek zijn, met uitzondering van `PUBLISHER_NAME`, die moeten worden verschillende token uit. In het ideale geval `PUBLISHER_NAME` vertegenwoordigt de ID van het apparaat dat deze token ontvangt.

Deze methode genereert een token met de volgende structuur:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

De token verlooptijd is opgegeven in seconden van 1 januari 1970. Hier volgt een voorbeeld van een token:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

De tokens hebben meestal een levensduur die lijkt op of groter is dan de levensduur van het apparaat. Als het apparaat de functie heeft voor het verkrijgen van een nieuwe token, kan tokens met een korter levensduur kunnen worden gebruikt.

### <a name="devices-sending-data"></a>Apparaten verzenden van gegevens

Zodra de tokens zijn gemaakt, wordt elk apparaat met een eigen unieke token ingericht.

Wanneer het apparaat gegevens op een gebeurtenis-Hub stuurt, labels het apparaat dat de token met het verzoek verzenden. Als u wilt voorkomen dat onbevoegden afluisteren en het token worden gestolen door, jaar de communicatie tussen het apparaat en de gebeurtenis Hub via een versleuteld kanaal.

### <a name="blacklisting-devices"></a>Beschermde apparaten

Als een token wordt gestolen door onbevoegden, kan de hacker zich voordoen als het apparaat waarvan de token is gestolen. Een apparaat blacklisting worden weergegeven door het apparaat onbruikbaar totdat het apparaat wordt een nieuwe token die gebruikmaakt van een andere uitgever gegeven.

## <a name="authentication-of-back-end-applications"></a>Verificatie van back-end-toepassingen

Als u wilt verifiëren back-end-toepassingen die de gegevens die zijn gegenereerd door apparaten opnemen, dienst gebeurtenis Hubs een beveiligingsmodel dat is vergelijkbaar met het model dat wordt gebruikt voor Service Bus onderwerpen. Een gebeurtenis Hubs consumenten groep is gelijk aan een abonnement op een Service Bus-onderwerp. Een client kunt een groep consumenten maken als het verzoek om te maken van de groep consumenten wordt geleverd met een token dat verleent bevoegdheden beheren voor de gebeurtenis Hub of voor de naamruimte die de Hub gebeurtenis hoort. Een client is toegestaan voor het gebruik van gegevens uit een groep consumenten als het verzoek ontvangen wordt geleverd met een token dat verleent ontvangen rechten op consumenten groep, de Hub gebeurtenis of de naamruimte die de Hub gebeurtenis hoort.

De huidige versie van Service Bus biedt geen ondersteuning voor SA's regels voor afzonderlijke abonnementen. Hetzelfde geldt voor gebeurtenis Hubs consumenten groepen. Ondersteuning van SA's wordt in de toekomst voor beide voorzieningen toegevoegd.

Bij gebrek aan SA's verificatie voor individuele consument groepen, kunt u de toetsen SA's alle groepen voor consumenten beveiligen met een gemeenschappelijke sleutel. Deze methode Hiermee wordt een toepassing voor het gebruik van gegevens uit een van de groepen consumenten van de Hub van een gebeurtenis.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de gebeurtenis Hubs, gaat u naar de volgende onderwerpen:

- [Overzicht van de Hubs gebeurtenissen]
- Een [SMS-oplossing in de wachtrij] Service Bus wachtrijen gebruiken.
- Een volledige [steekproef-toepassing die gebruikmaakt van gebeurtenis Hubs].

[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[in de wachtrij beheeroplossing]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
