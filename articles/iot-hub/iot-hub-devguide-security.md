<properties
 pageTitle="Handleiding voor ontwikkelaars - toegangsbeheer voor IoT Hub | Microsoft Azure"
 description="Azure IoT Hub ontwikkelaars begeleiden - hoe u toegang tot IoT Hub en de beveiliging beheer"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>Toegang tot IoT Hub beheren

## <a name="overview"></a>Overzicht

In dit artikel worden de opties voor het beveiligen van uw hub IoT. IoT Hub wordt *machtigingen* om het verlenen van toegang tot elke IoT hub-eindpunten. Machtigingen voor beperken de toegang tot een IoT hub op basis van functionaliteit.

In dit artikel worden beschreven:

- De verschillende machtigingen dat u een apparaat of back-enddatabase-app voor toegang tot uw hub IoT kunt verlenen.
- Het verificatieproces en de tokens die wordt gebruikt om machtigingen te verifiëren.
- Klik hier voor meer informatie over het bereik van referenties als u wilt beperken van toegang tot specifieke resources.
- Ondersteuning voor IoT Hub voor x.509-certificaten.
- Aangepaste apparaat-verificatiemethoden die gebruikmaken van bestaande apparaat identiteit registervermeldingen of verificatiemethoden.

### <a name="when-to-use"></a>Wanneer gebruikt u

U moet de juiste machtigingen voor toegang tot een van de eindpunten IoT Hub. Een apparaat moet bijvoorbeeld een token met beveiligingsreferenties samen met elk bericht dat deze wordt verzonden naar IoT Hub bevatten.

## <a name="access-control-and-permissions"></a>Toegangsbeheer en machtigingen

U kunt [machtigingen](#iot-hub-permissions) verlenen op de volgende manieren:

* **Hub-niveau gedeelde-beleid**. Gedeelde clienttoegangsbeleid kunnen elke combinatie van [machtigingen](#iot-hub-permissions)verlenen. U kunt beleidsregels definiëren in de [portal van Azure][lnk-management-portal], of via een programma met behulp van de [provider van de resource IoT Hub REST API's][lnk-resource-provider-apis]. Een nieuw gemaakte IoT hub heeft de volgende standaardbeleidsregels:

    - **iothubowner**: beleid voor alle machtigingen.
    - **service**: beleid met ServiceConnect machtigingen.
    - **apparaat**: beleid met DeviceConnect machtigingen.
    - **registryRead**: beleid met RegistryRead machtigingen.
    - **registryReadWrite**: beleid met RegistryRead en RegistryWrite machtigingen.


* **Beveiligingsreferenties per apparaat**. Elke IoT Hub bevat een [apparaat identiteit register][lnk-identity-registry]. Voor elk apparaat in het register, kunt u beveiligingsreferenties die **DeviceConnect** beperkt tot de bijbehorende apparaat-eindpunten machtigen.

Bijvoorbeeld in een normale IoT-oplossing:

- Het apparaat management onderdeel gebruikmaakt van het beleid *registryReadWrite* .
- De gebeurtenis processor-component gebruikt de *service* -beleid.
- De runtime apparaat bedrijven logica component gebruikt de *service* -beleid.
- Afzonderlijke apparaten verbinding maken met referenties die zijn opgeslagen in de hub IoT identiteit register.

## <a name="authentication"></a>Verificatie

Azure IoT Hub verleent toegang tot de eindpunten door te verifiëren van een token ten opzichte van de gedeelde clienttoegangsbeleid en het apparaat identiteit register beveiligingsreferenties.

Beveiligingsreferenties, zoals symmetric toetsaanslagen worden nooit verzonden via het netwerk.

> [AZURE.NOTE] De provider van de resource Azure IoT Hub is beveiligd via uw Azure-abonnement als, alle providers in de [Resourcemanager Azure worden][lnk-azure-resource-manager].

Zie voor meer informatie over het maken en gebruiken van beveiligingstokens [IoT Hub beveiligingstokens][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protocol details

Elke ondersteunde protocol, zoals MQTT, AMQP en HTTP-transacties tokens op verschillende manieren.

Wanneer u met MQTT, het pakket verbinding maken met apparaat-id heeft als de ClientId, {iothubhostname} / {deviceId} in het veld Username en een token SA's in het veld voor wachtwoord. {iothubhostname} moet de volledige CName van de hub IoT (bijvoorbeeld contoso.azure-devices.net).

Bij gebruik van [AMQP][lnk-amqp], IoT Hub [SASL zonder opmaak] ondersteunt[ lnk-sasl-plain] en [AMQP Claims-beveiliging op basis van-][lnk-cbs].

Als u AMQP claims-beveiliging op basis van-gebruikt, is de standaard geeft aan hoe deze tokens te verzenden.

Voor SASL zonder opmaak, kan de **gebruikersnaam** zijn:

* `{policyName}@sas.root.{iothubName}`als hub niveau tokens gebruiken.
* `{deviceId}@sas.{iothubname}`Als het apparaat beperkt tokens gebruiken.

In beide gevallen het wachtwoordveld bevat de token, zoals wordt beschreven in [de IoT Hub beveiligingstokens][lnk-sas-tokens].

HTTP implementeert verificatie door een geldig token op te nemen in de koptekst van de aanvraag **autorisatie** .

#### <a name="example"></a>Voorbeeld

Gebruikersnaam (DeviceId is hoofdlettergevoelig):`iothubname.azure-devices.net/DeviceId`

Wachtwoord (genereren SA's met apparaat Explorer):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] De [Azure IoT Hub SDK's] [ lnk-sdks] tokens selectievakje Automatisch schrijfinstructies wanneer u verbinding maakt met de service. In sommige gevallen ondersteunen de SDK's niet alle protocollen of alle verificatiemethoden.

### <a name="special-considerations-for-sasl-plain"></a>Aandachtspunten voor het SASL zonder opmaak

Wanneer u SASL zonder opmaak met AMQP, kunt een client verbinding maakt met een hub IoT een token één voor elke TCP-verbinding gebruiken. Als het token verloopt, wordt de TCP-verbinding verbroken van de service en een opnieuw activeert. Dit probleem, is terwijl niet problematisch voor een onderdeel van de back-enddatabase toepassing zeer schadelijk voor een apparaat aan de clientzijde-toepassing voor de volgende oorzaken hebben:

*  Gateways verbinding meestal namens veel apparaten. Wanneer u met SASL zonder opmaak, hebt worden u een afzonderlijke TCP-verbinding voor elk apparaat dat verbinding maakt met een hub IoT te maken. Dit scenario aanzienlijk verhogen van het verbruik van kracht en netwerken resources, en de latentie van elk apparaatverbinding verhogen.
* Beperkte capaciteit apparaten nadelig worden beïnvloed door het verbeterde gebruik van resources na elke token verlooptijd opnieuw verbinding te maken.

## <a name="scope-hub-level-credentials"></a>Bereik hub niveau referenties

U kunt de hub niveau beveiligingsbeleid voor apparaten beperken door te tokens maken met een beperkte bron-URI. Het eindpunt apparaat naar cloud om berichten te verzenden vanaf een apparaat is bijvoorbeeld **/devices/ {deviceId} / berichten/gebeurtenissen**. U kunt een hub niveau gedeelde-beleid ook gebruiken met **DeviceConnect** machtigingen aan te melden een token waarvan resourceURI **/devices/ {deviceId is}**. Deze methode Hiermee maakt u een token die alleen kan worden gebruikt om berichten namens apparaat **deviceId**te verzenden.

Deze methode is vergelijkbaar met het [beleid van gebeurtenis Hubs publisher][lnk-event-hubs-publisher-policy], en kunt u aangepaste verificatiemethoden implementeren.

## <a name="security-tokens"></a>Beveiligingstokens

Beveiligingstokens IoT Hub gebruikt om apparaten en services om te voorkomen toetsen verzenden op het netwerk te verifiëren. Daarnaast kunnen beveiligingstokens zijn geldigheid en beperkt bereik. [Azure IoT Hub SDK's] [ lnk-sdks] selectievakje Automatisch schrijfinstructies tokens zonder een speciale configuratie. Sommige gevallen vereisen echter de gebruiker te genereren en beveiligingstokens rechtstreeks gebruiken. Hierbij het directe gebruik van de itemweergavesjabloon geeft MQTT, AMQP of HTTP, of voor de uitvoering van het patroon dat token-service, zoals wordt uitgelegd in [aangepaste apparaat verificatie][lnk-custom-auth].

IoT Hub ook kunt apparaten om te verifiëren met IoT Hub met behulp van [x.509-certificaten][lnk-x509]. 

### <a name="security-token-structure"></a>Beveiliging token-structuur
Beveiligingstokens kunt u tijd gebonden toegang tot apparaten en services voor specifieke functionaliteit in IoT Hub verlenen. Om ervoor te zorgen dat alleen geautoriseerde apparaten en services verbinding kunnen maken, moeten worden beveiligingstokens ondertekend met een gedeelde toegangstoets beleid of een symmetric sleutel die zijn opgeslagen met een apparaat-id in het register identiteit.

Een token die zijn aangemeld met een gedeelde toegang beleid belangrijke verleent toegang tot alle functies die zijn gekoppeld aan de gedeelde toegangsmachtigingen beleid. Een ondertekend met een apparaat van de identiteit symmetric sleutel token verleent aan de andere kant, alleen de machtiging **DeviceConnect** voor de identiteit van de bijbehorende apparaat.

Het beveiligingstoken heeft de volgende indeling:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Dit zijn de verwachte waarden:

| Waarde | Beschrijving |
| ----- | ----------- |
| {handtekening} | Een tekenreeks van de handtekening HMAC-SHA256 van het formulier: `{URL-encoded-resourceURI} + "\n" + expiry`. **Belangrijk**: de sleutel is verkregen door base64 en als gebruiken om te berekenen van de HMAC-SHA256. |
| {resourceURI} | URI voorvoegsel (per segment) van de eindpunten die toegankelijk is met deze token, beginnend met hostname van de hub IoT (geen protocol). Bijvoorbeeld:`myHub.azure-devices.net/devices/device1` |
| {verstrijken} | UTF8 tekenreeksen voor het aantal seconden sinds de epoche 00:00:00 UTC op 1 januari 1970. |
| {URL-codering-resourceURI} | Lager zaak URL-codering van de bron-URI voor de kleine letters |
| {policyName} | De naam van het gedeelde access-beleid waarnaar deze token verwijst. Afwezig in het geval van tokens die verwijzen naar apparaat-register referenties. |

**Opmerking over voorvoegsel**: het URI voorvoegsel wordt berekend door segment en niet door teken. Bijvoorbeeld `/a/b` is een voorvoegsel voor `/a/b/c` maar niet voor `/a/bc`.

Het codefragment van de volgende Node.js ziet u een functie genaamd **generateSasToken** waarin wordt berekend het token van de invoer `resourceUri, signingKey, policyName, expiresInMins`. De volgende secties bevatten informatie over hoe u de verschillende ingangen voor de verschillende token gebruik gevallen geïnitialiseerd.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Als een vergelijking is de overeenkomstige Python-code genereren een beveiligingstoken:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Aangezien de geldigheid van het token is gevalideerd op IoT Hub computers, is het belangrijk dat de drift van de klok van de computer waarop de token genereert minimale.

### <a name="use-sas-tokens-in-a-device-client"></a>Tokens SA's gebruiken in een apparaat-client

Er zijn twee manieren om te verkrijgen **DeviceConnect** machtigingen afstemmen op IoT Hub met beveiligingstokens: een [symmetric apparaatsleutel uit het register van de identiteit apparaat](#use-a-symmetric-key-in-the-identity-registry)gebruikt of een [beleid van gedeelde toegangstoets](#use-a-shared-access-policy).

Houd er rekening mee dat alle functies die toegankelijk zijn vanuit apparaten wordt blootgesteld aan het ontwerp op eindpunten met voorvoegsel `/devices/{deviceId}`.

> [AZURE.IMPORTANT] De enige manier dat IoT Hub wordt geverifieerd door een specifieke apparaat gebruikt de symmetric apparaat identiteit-toets. In gevallen wanneer een gedeelde-beleid wordt gebruikt voor toegang tot apparaatfunctionaliteit de oplossing moet rekening houden met het onderdeel het beveiligingstoken als een vertrouwde subonderdelen verlenen.

Er zijn de eindpunten apparaat-omlaag (ongeacht het protocol):

| Eindpunt | Functionaliteit |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Apparaat-naar-cloud-berichten verzenden. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Cloud-to-device berichten ontvangen. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Gebruik van een sleutel symmetric in het register identiteit

Wanneer u de symmetric sleutel van de identiteit van een apparaat met een token de policyName genereren (`skn`) element van het token wordt weggelaten.

Een token gemaakt voor toegang tot alle functies van het apparaat moet bijvoorbeeld de volgende parameters hebben:

* bron-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* ondertekening sleutel: een symmetric toets voor de `{device id}` id
* geen beleidsnaam
* elk gewenst moment verlooptijd.

Een voorbeeld met de bovenstaande knooppunt-functie is:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Het resultaat toegang tot alle functies voor device1 krijgen, zou zijn:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Is het mogelijk te genereren van een beveiligde token met de functie .NET [Apparaat Explorer][lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Een gedeelde-beleid gebruiken

Wanneer u een token maakt van een gedeelde-beleid, het beleid het veld naam `skn` moet zijn ingesteld op de naam van het beleid wordt gebruikt. Het is ook vereist dat het beleid de machtiging **DeviceConnect** verleent.

De twee belangrijkste scenario's voor het gebruik van gedeelde-beleid voor toegang tot apparaatfunctionaliteit zijn:

* [cloud protocol gateways][lnk-endpoints],
* [services token] [ lnk-custom-auth] gebruikt voor de implementatie van aangepaste verificatiemethoden.

Aangezien het gedeelde-beleid potentieel toegang verlenen kunt tot verbinden als elk apparaat, is het belangrijk dat u de juiste bron-URI gebruikt bij het maken van beveiligingstokens. Dit is vooral belangrijk is voor token-services, dat om te bepalen de token naar een specifieke apparaat met de bron-URI. Dit punt is minder relevant is voor protocol gateways zoals ze zijn verkeer voor alle apparaten al op de volgende manier regelt.

Als u bijvoorbeeld maakt een token service het vooraf gemaakte gedeelde toegangsbeleid **apparaat** met een token met de volgende parameters:

* bron-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* ondertekening sleutel: een van de toetsen van het `device` beleid,
* Beleidsnaam: `device`,
* elk gewenst moment verlooptijd.

Een voorbeeld met de bovenstaande knooppunt-functie is:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Het resultaat toegang tot alle functies voor device1 krijgen, zou zijn:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Een gateway protocol hetzelfde kunt gebruiken voor alle apparaten gewoon instellen de bron-URI naar `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Gebruik van beveiligingstokens uit de onderdelen van service

Onderdelen van de service kunnen alleen genereren voor beveiligingstokens met gedeelde clienttoegangsbeleid de juiste machtigingen verlenen zoals eerder is beschreven.

Hierna ziet u de servicefuncties die worden aangeboden over de eindpunten:

| Eindpunt | Functionaliteit |
| ----- | ----------- |
| `{iot hub host name}/devices` | Maken, bijwerken, ophalen en identiteiten apparaat verwijderen. |
| `{iot hub host name}/messages/events` | Apparaat-naar-cloud berichten ontvangen. |
| `{iot hub host name}/servicebound/feedback` | Feedback voor cloud-to-device berichten ontvangen. |
| `{iot hub host name}/devicebound` | Cloud-to-device berichten verzenden. |

Als u bijvoorbeeld maakt een service genereren met het vooraf gemaakte gedeelde toegangsbeleid **registryRead** een token met de volgende parameters:

* bron-URI: `{IoT hub name}.azure-devices.net/devices`,
* ondertekening sleutel: een van de toetsen van het `registryRead` beleid,
* Beleidsnaam: `registryRead`,
* elk gewenst moment verlooptijd.

    var eindpunt = "myhub.azure-devices.net/devices";   var policyName = 'apparaat';   var policyKey = '...';

    var token = generateSasToken (eindpunt, policyKey, policyName, 60);

Het resultaat, die toegang zou doen als u wilt lezen van alle apparaat identiteiten, zou zijn:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Ondersteunde x.509-certificaten

U kunt een X.509-certificaat voor de verificatie van een apparaat met IoT Hub. Deze groep omvat:

-   **Een bestaand X.509-certificaat**. Een apparaat mogelijk al een x.509-certificaat dat is gekoppeld. Het apparaat kunt u dit certificaat gebruiken om te verifiëren met IoT Hub.

-   **Een X-509 selfservice gegenereerde en zelfondertekend certificaat**. Een fabrikant of een interne implementatie kunt genereren van deze certificaten en de bijbehorende persoonlijke sleutel (en het certificaat) opslaan op het apparaat. U kunt hulpprogramma's zoals [OpenSSL] [ lnk-openssl] en [Windows SelfSignedCertificate] [ lnk-selfsigned] hulpprogramma voor dit doel.

-   **CA ondertekend x.509-certificaat**. U kunt ook een x.509-certificaat gegenereerd en ondertekend door een certificeringsinstantie (CA) gebruiken voor het identificeren van een apparaat en een apparaat met IoT Hub verifiëren.

Een apparaat kan gebruiken een x.509-certificaat of een beveiligingstoken voor verificatie, maar niet beide.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Een client x.509-certificaat voor een apparaat registreren

De [Azure IoT Service SDK voor C#] [ lnk-service-sdk] (versie 1.0.8+) ondersteunt registreren van een apparaat waarbij een client x.509-certificaat voor verificatie. Andere API's zoals importeren/exporteren van apparaten ondersteunen ook x.509-clientcertificaten.

### <a name="c-support"></a>C\# ondersteuning

De klasse **RegistryManager** biedt een programma manier om u te registreren van een apparaat. Met name inschakelen de methoden **AddDeviceAsync** en **UpdateDeviceAsync** voor een gebruiker registreren en bijwerken van een apparaat in het register Iot Hub apparaat identiteit. Deze twee methoden werken met een exemplaar van het **apparaat** als invoer. De klasse **apparaat** bevat een eigenschap voor **verificatie** waarmee de gebruiker om op te geven van primaire en secundaire x.509-certificaat vingerafdrukken. De vingerafdruk vertegenwoordigt een hash SHA-1 van de X.509-certificaat (opgeslagen met binaire DER-codering). Gebruikers kunnen de optie voor het opgeven van een primaire vingerafdruk of een secundaire vingerafdruk of beide. Primaire en secundaire vingerafdrukken worden wilt afhandelen certificaat rollover scenario's ondersteund.

> [AZURE.NOTE] IoT Hub niet vereisen of hele x.509-certificaat, alleen de vingerafdruk opslaan.

Hier volgt een voorbeeld C\# codefragment registreren van een apparaat met een client x.509-certificaat:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Gebruik van een client x.509-certificaat tijdens runtime bewerkingen

Het [apparaat van de Azure IoT SDK voor .NET] [ lnk-client-sdk] (versie 1.0.11+) ondersteunt het gebruik van x.509-clientcertificaten.

### <a name="c-support"></a>C\# ondersteuning

De klasse **DeviceAuthenticationWithX509Certificate** ondersteunt het maken van de  **DeviceClient** exemplaren die met een client x.509-certificaat.

Hier volgt een voorbeeld-codefragment:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Aangepaste apparaat verificatie

U kunt het IoT Hub [apparaat identiteit register] [ lnk-identity-registry] beveiligingsreferenties per apparaat configureren en toegangsbeheer [tokens]gebruiken[lnk-sas-tokens]. Echter als een oplossing IoT al geïnvesteerd in een aangepast apparaat identiteit register en/of verificatie kleurenschema, kunt u integreren deze bestaande infrastructuur met IoT Hub door te maken van een *token-service*. Op deze manier kunt u andere IoT-functies gebruiken in uw oplossing.

Een token service is een aangepaste cloudservice. Wordt een IoT Hub *gedeelde-beleid* bij **DeviceConnect** machtigingen om tokens *binnen bereik van het apparaat* te maken. Deze tokens inschakelen een apparaat verbinding maken met uw hub IoT.

  ![Stappen van het patroon dat token-service][img-tokenservice]

Dit zijn de belangrijkste stappen die van het patroon dat token service:

1. Een access-beleid IoT Hub gedeeld met **DeviceConnect** machtigingen voor uw hub IoT maken. U kunt dit beleid maken in de [portal van Azure] [ lnk-management-portal] of via een programma. De token-service gebruikt dit beleid de tokens dat wordt gemaakt zich aan te melden.
2. Wanneer een apparaat moet voor toegang tot uw IoT-hub, opgevraagd een ondertekend token uit uw token-service. Het apparaat kan verifiëren met uw aangepaste apparaat identiteit register/verificatiemethode om te bepalen de identiteit apparaat waarin de token-service voor het maken van het token wordt gebruikt.
3. De token-service retourneert een token. Het token wordt gemaakt met `/devices/{deviceId}` als `resourceURI`, met `deviceId` als het apparaat wordt geverifieerd. Het gedeelde-beleid de token service gebruikt om het token te maken.
4. Het apparaat gebruikt de token rechtstreeks met de hub IoT.

> [AZURE.NOTE] U kunt de klas .NET [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] of de Java-klasse [IotHubServiceSasToken] [ lnk-java-sas] een token maken in uw token-service.

De token-service kunt u de token verlooptijd instellen naar wens. Als het token verloopt, wordt in de hub IoT de apparaatverbinding servers. Het apparaat moet vervolgens een nieuwe token aanvragen van de token service. Als u een korte verlooptijd gebruikt, is dit verhoogt de belasting op het apparaat en de token-service.

Voor een apparaat wilt aansluiten op uw hub, moet u nog steeds deze toevoegen aan het register IoT Hub apparaat identiteit, zelfs als het apparaat is een token en niet de apparaatsleutel van een om verbinding te maken. U kunt daarom blijven per apparaat toegangsbeheer gebruiken door in- of uitschakelen van apparaat identiteiten in het [register voor IoT Hub-identiteit] [ lnk-identity-registry] wanneer het apparaat met een token worden geverifieerd. Dit vermindert de risico's van het gebruik van tokens met lange verstrijken tijden.

### <a name="comparison-with-a-custom-gateway"></a>Vergelijking met een aangepaste gateway

Het patroon token-service is de aanbevolen wijze willen implementeren van de Register-/ verificatiemethode voor een aangepaste identiteit met IoT Hub. Het wordt aanbevolen omdat IoT Hub blijft het meeste oplossing verkeer verwerken. Er zijn echter gevallen waarin de aangepaste verificatiemethode dus is verweven met het protocol dat een service voor het verwerken van al het verkeer (*aangepaste gateway*) vereist is. Een voorbeeld hiervan is [beveiliging TLS (Transport Layer) en vooraf gedeelde sleutels (PSKs)][lnk-tls-psk]. Zie voor meer informatie het [protocol gateway] [ lnk-protocols] onderwerp.

## <a name="reference-topics"></a>Naslaginformatie:

De volgende onderwerpen voor de verwijzing bieden u meer informatie over toegang tot uw hub IoT beheren.

## <a name="iot-hub-permissions"></a>IoT Hub-machtigingen

De volgende tabel bevat de machtigingen die u gebruiken kunt voor toegang tot uw hub IoT.

| Machtiging            | Notities |
| --------------------- | ----- |
| **RegistryRead**      | Verleent lezen toegang tot het apparaat identiteit register. Zie voor meer informatie [apparaat identiteit register][lnk-identity-registry]. |
| **RegistryReadWrite** | Verleent lezen en schrijven toegang tot het apparaat identiteit register. Zie voor meer informatie [apparaat identiteit register][lnk-identity-registry]. |
| **ServiceConnect**    | Er toegang verleent tot naar cloud service-omlaag communicatie en eindpunten cmdlets voor controle. Zo geeft het recht met back-enddatabase cloudservices apparaat naar cloud berichten ontvangt, cloud-to-device berichten verzenden en de bijbehorende bezorging bevestigingen ophalen. |
| **DeviceConnect**     | Biedt toegang tot apparaat-omlaag communicatie eindpunten. Zo geeft het recht apparaat naar cloud-berichten verzenden en ontvangen van berichten van de cloud-naar-apparaat. Met deze machtiging wordt gebruikt door apparaten. |

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Andere onderwerpen verwijzing in de handleiding voor ontwikkelaars omvatten:

- [IoT Hub eindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke hub IoT voor runtime en beheer bewerkingen beschrijft.
- [Beperken en quota] [ lnk-quotas] beschrijving van de quota die van toepassing zijn op de IoT Hub-service en het bandbreedteregeling gedrag verwacht wanneer u de service gebruiken.
- [IoT Hub apparaat en service SDK's] [ lnk-sdks] lijsten van de verschillende taal SDK's u een gebruiken bij het ontwikkelen van apparaat zowel service-toepassingen die samenwerken met IoT Hub.
- [Taal voor twins, methoden en taken query] [ lnk-query] beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat twins, methoden en taken.
- [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] vindt u meer informatie over de ondersteuning van IoT Hub voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe toegang IoT Hub te beheren, kunt u in de volgende onderwerpen voor de handleiding voor ontwikkelaars interessant kunnen zijn:

- [Apparaat twins gebruiken voor het synchroniseren van staat en configuraties][lnk-devguide-device-twins]
- [Directe methode op een apparaat][lnk-devguide-directmethods]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u uitproberen enkele van de concepten die in dit artikel worden beschreven wilt, kunt u in de volgende zelfstudies voor IoT Hub interessant kunnen zijn:

- [Aan de slag met Azure IoT Hub][lnk-getstarted-tutorial]
- [Het verzenden van berichten van de cloud-naar-apparaat met IoT Hub][lnk-c2d-tutorial]
- [Het proces IoT Hub apparaat-naar-cloud-berichten][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
