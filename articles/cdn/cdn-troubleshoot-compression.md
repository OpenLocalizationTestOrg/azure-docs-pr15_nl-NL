<properties
    pageTitle="Problemen oplossen comprimeren in Azure CDN | Microsoft Azure"
    description="Problemen oplossen met comprimeren Azure CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>Probleemoplossing CDN bestandscompressie

In dit artikel vindt u problemen oplossen met [comprimeren CDN](cdn-improve-performance.md).

Als u meer hulp op een willekeurige plaats in dit artikel nodig hebt, kunt u contact opnemen met de Azure experts op [het MSDN-Azure en de stapel overloop-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident Azure ondersteuning indienen. Ga naar de [site van Azure-ondersteuning](https://azure.microsoft.com/support/options/) en **Ondersteuning krijgen**op.

## <a name="symptom"></a>Symptoom

Compressie voor uw eindpunt is ingeschakeld, maar bestanden worden geretourneerd niet gecomprimeerd.

>[AZURE.TIP] Als u wilt controleren of uw bestanden worden geretourneerd gecomprimeerd, moet u een hulpmiddel zoals [Fiddler](http://www.telerik.com/fiddler) of van uw browser [speciale tools voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)gebruiken.  Selectievakje de HTTP antwoord kopteksten geretourneerd met uw in de cache CDN inhoud.  Als er een koptekst met de naam is `Content-Encoding` met een waarde van **gzip**, **bzip2**of **verkleinen**, uw inhoud is gecomprimeerd.
>
>![Koptekst inhoud-codering](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>Oorzaak

Er zijn verschillende mogelijke oorzaken, waaronder:

- De gevraagde inhoud is niet in aanmerking komen voor compressie.
- Compressie is niet ingeschakeld voor het gewenste bestandstype.
- De HTTP-aanvraag bevat geen een koptekst aanvragen van een geldige compressietype.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

> [AZURE.TIP] Met een nieuwe eindpunten wordt geïmplementeerd, configuratiewijzigingen CDN enige tijd om te doorvoeren via het netwerk.  Meestal worden wijzigingen toegepast binnen 90 minuten.  Als dit de eerste keer dat u de compressie voor uw eindpunt CDN hebt ingesteld, kunt u overwegen 1-2 uur wachten om er zeker van te zijn dat de instellingen hebt doorgegeven aan de POP's compressie. 

### <a name="verify-the-request"></a>Controleer of de aanvraag

Eerst moeten we een snelle controle op het verzoek doen.  U kunt van uw browser [speciale tools voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) gebruiken om weer te geven van de aanvragen plaatsvindt.

- Controleer of het verzoek wordt verzonden naar de URL eindpunt, `<endpointname>.azureedge.net`, en niet uw origin.
- Controleer of de aanvraag bevat een koptekst **Accepteren-codering** en de waarde voor die header **gzip**, **verkleinen**of **bzip2**bevat.

> [AZURE.NOTE] **Azure CDN van Akamai** profielen wordt alleen ondersteund **gzip** -codering.

![CDN verzoek kopteksten](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Controleer of compressie-instellingen (standaard CDN profile)

> [AZURE.NOTE] Deze stap is alleen van toepassing als uw CDN-profiel een **Azure CDN standaard uit Verizon** of **Azure CDN standaard uit Akamai** -profiel is. 

Ga naar uw eindpunt in de [portal van Azure](https://portal.azure.com) en klik op de knop **configureren** .

- Controleer of compressie is ingeschakeld.
- Controleer of het MIME-type voor de inhoud wordt gecomprimeerd is opgenomen in de lijst met gecomprimeerde indelingen.

![CDN compressie-instellingen](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Controleer of compressie-instellingen (Premium CDN profile)

> [AZURE.NOTE] Deze stap is alleen van toepassing als uw CDN-profiel een **Azure CDN Premium uit Verizon** -profiel is.

Ga naar uw eindpunt in de [portal van Azure](https://portal.azure.com) en klik op de knop **beheren** .  De aanvullende portal wordt geopend.  Plaats de muisaanwijzer boven de **HTTP grote** tab en vervolgens de muis boven de **Cache-instellingen** : flyout.  Klik op **compressie**. 

- Controleer of compressie is ingeschakeld.
- Controleer of dat de lijst **Bestandstypen** bevat een lijst door komma's gescheiden (geen spaties) met MIME-typen.
- Controleer of het MIME-type voor de inhoud wordt gecomprimeerd is opgenomen in de lijst met gecomprimeerde indelingen.

![CDN premium compressie-instellingen](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Controleer of dat de inhoud wordt opgeslagen in de cache

> [AZURE.NOTE] Deze stap is alleen van toepassing als uw CDN-profiel een profiel **Azure CDN uit Verizon** (Standard of Premium is).

Controleer de koppen antwoord om ervoor te zorgen dat het bestand is opgeslagen in de cache in de regio waar deze wordt aangevraagd met speciale tools voor ontwikkelaars van uw browser.

- Controleer de tekenarcering **Server** .  De koptekst moet de notatie **Platform (POP/Server-ID)**, zoals gezien in het volgende voorbeeld.
- Controleer de **X-Cache** tekenarcering.  De koptekst moet lezen **RAKEN**.  

![CDN antwoord kopteksten](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Controleer of het bestand voldoet aan de vereisten grootte

> [AZURE.NOTE] Deze stap is alleen van toepassing als uw profiel CDN een **CDN Azure uit Verizon** -profiel (Standard of Premium is).

Als u in aanmerking komen voor compressie, een bestand moet voldoen aan de volgende vereisten voor de grootte:

- Groter zijn dan 128 bytes.
- Kleiner is dan 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Het verzoek op de server origin voor de kop van een **Via** controleren

De koptekst **Via** HTTP wordt naar de webserver wordt aangegeven dat het verzoek worden doorgestuurd door een proxyserver.  Microsoft IIS-endwebservers al dan niet standaard comprimeren niet antwoorden wanneer de aanvraag een kop **Via bevat** .  Als u wilt overschrijven dit gedrag, volgt u als volgt te werk:

- **IIS 6**: [instellen HcNoCompressionForProxies = "FALSE" in de eigenschappen IIS-metagegevens](https://msdn.microsoft.com/library/ms525390.aspx)
- **IIS-7 en hoger**: [zowel **noCompressionForHttp10** en **noCompressionForProxies** in ONWAAR in de serverconfiguratie instellen](http://www.iis.net/configreference/system.webserver/httpcompression)

