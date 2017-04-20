<properties
    pageTitle="Ondersteuning voor meerdere Origin Resource delen (CORS) | Microsoft Azure"
    description="Informatie over het inschakelen van CORS ondersteuning voor de opslag-Services van Microsoft Azure."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Cross-Origin Resource delen (CORS)-ondersteuning voor de van Azure-opslagservices

Begin met versie 2013-08-15, ondersteuning de opslagservices Azure Cross-Origin Resource delen (CORS) voor de services Blob, tabel, wachtrij en bestand. CORS is een HTTP-functie waarmee een webtoepassing uitgevoerd in een domein voor toegang tot resources in een ander domein. Webbrowsers implementeren een beveiligingsbeperking bekend als [dezelfde oorsprong beleid](http://www.w3.org/Security/wiki/Same_Origin_Policy) , waardoor een webpagina bellen API's in een ander domein; CORS biedt een veilige methode voor het toestaan van een domein (het oorspronkelijke domein) API's in een ander domein bellen. Zie de [specificatie van CORS](http://www.w3.org/TR/cors/) voor meer informatie over CORS.

U kunt regels CORS afzonderlijk instellen voor elk van de opslagservices, met door te bellen [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Wachtrij Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx)en [Tabeleigenschappen Service instellen](https://msdn.microsoft.com/library/hh452240.aspx). Als u de regels CORS voor de service hebt ingesteld, wordt klikt u vervolgens een correct geverifieerde aanvraag ten opzichte van de service uit een ander domein geëvalueerd om te bepalen of deze is toegestaan op basis van de regels die u hebt opgegeven.

>[AZURE.NOTE] Houd er rekening mee dat CORS niet een verificatiemethode is. Ieder verzoek ten opzichte van een resource opslag wanneer CORS is ingeschakeld, moet een handtekening geverifieerd, of ten opzichte van een openbare resource moet worden aangebracht.

## <a name="understanding-cors-requests"></a>Lidmaatschap CORS aanvragen

Een verzoek CORS van een domein origin kan twee afzonderlijke aanvragen omvatten:

- Een Preflight-aanvraag, die de CORS-beperkingen die door de service. De Preflight-aanvraag is vereist, tenzij de aanvraag een [eenvoudige methode is](http://www.w3.org/TR/cors/), waarbij de ophalen, hoofd of bericht.

- De werkelijke aanvraag ten opzichte van de gewenste resource.

### <a name="preflight-request"></a>Preflight-aanvraag

De Preflight-verzoek om query's de CORS-beperkingen die zijn ingesteld voor de storage-service door de accounteigenaar van het. De webbrowser (of andere gebruikersagent) stuurt een verzoek opties waarin de kop van de aanvraag, de methode en origin domein. De storage-service evalueert de bewerking op basis van een vooraf geconfigureerde set CORS regels die opgeven welke origin domeinen, verzoek methoden en verzoek kopteksten mag worden opgegeven voor een werkelijke aanvraag ten opzichte van een resource opslag.

Als CORS is ingeschakeld voor de service en er is een regel CORS die overeenkomt met de Preflight-aanvraag, de service reageert met statuscode 200 (OK) en bevat de vereiste koppen van de Access-besturingselementen in de reactie.

Als CORS is niet ingeschakeld voor de service of als er geen regel CORS overeenkomt met het Preflight-verzoek, wordt de service beantwoorden met statuscode 403 (niet toegestaan).

Als het verzoek van de opties niet de vereiste CORS koppen (de oorsprong en Access-besturingselement-verzoek-methode koppen) bevat, wordt de service beantwoorden met statuscode 400 (ongeldige aanvraag).

Houd er rekening mee dat een verzoek voor een preflight tegen de service (Blob, wachtrij en tabel) en niet de gevraagde resource wordt geëvalueerd. De accounteigenaar moet CORS als onderdeel van de service-eigenschappen van account om het verzoek te kunnen uitvoeren hebt ingeschakeld.

### <a name="actual-request"></a>Werkelijke aanvraag

Zodra de Preflight-aanvraag is geaccepteerd en het antwoord wordt geretourneerd, zorgt ervoor dat de browser is de werkelijke aanvraag ten opzichte van de resource opslag. De browser wordt de werkelijke weigeren direct aanvragen als de Preflight-aanvraag is geweigerd.

De werkelijke aanvraag is verwerkt als normale verzoek ten opzichte van de storage-service. De aanwezigheid van de koptekst Origin wordt aangegeven dat de aanvraag een CORS-aanvraag is en de service de overeenkomende CORS regels moeten worden gecontroleerd. Als een overeenkomst wordt gevonden, zijn de kop van het Access-besturingselementen toegevoegd aan het antwoord en terug naar de client verzonden. Als een overeenkomst wordt gevonden, wordt de koppen CORS Access-besturingselementen worden niet geretourneerd.

## <a name="enabling-cors-for-the-azure-storage-services"></a>CORS inschakelen voor de opslag van Azure-services

CORS regels worden ingesteld op het serviceniveau van de, zodat u moet in- of uitschakelen van CORS voor elke service (Blob, wachtrij en tabel) afzonderlijk. CORS is standaard uitgeschakeld voor elke service. Als u wilt inschakelen CORS, moet u de eigenschappen van de desbetreffende service met versie 2013-08-15 instellen of hoger, en CORS regels toevoegen aan de service-eigenschappen. Raadpleeg voor meer informatie over hoe u in- of uitschakelen van CORS voor een service en hoe u CORS regels kunt instellen, [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Wachtrij Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx)en [Tabeleigenschappen Service instellen](https://msdn.microsoft.com/library/hh452240.aspx).

Hier volgt een voorbeeld van een regel voor één CORS, opgegeven via een bewerking Service-eigenschappen instellen:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Elk element is opgenomen in de regel CORS vindt u hieronder:

- **AllowedOrigins**: de domeinen van de oorsprong die een verzoek ten opzichte van de storage-service via CORS zijn toegestaan. Het oorspronkelijke domein is het domein van waaraf de aanvraag is gemaakt. Houd er rekening mee dat het oorspronkelijke hoofdlettergevoelig exact overeen met de oorsprong die de leeftijd van de gebruiker wordt verzonden naar de service moet zijn. U kunt ook het jokerteken ' *' toe te staan dat alle origin domeinen aanvragen via CORS. In het bovenstaande voorbeeld wordt de domeinen [http://www.contoso.com](http://www.contoso.com) en [http://www.fabrikam.com](http://www.fabrikam.com) kunt u aanvragen tegen de service volgens CORS.

- **AllowedMethods**: de methoden (HTTP-verzoek woorden) die het oorspronkelijke domein voor een aanvraag CORS gebruiken mogelijk. In het bovenstaande voorbeeld mogen alleen opslag en GET-aanvragen.

- **AllowedHeaders**: de aanvraagkoppen die het oorspronkelijke domein op het verzoek CORS mogelijk opgeeft. In het bovenstaande voorbeeld zijn alle metagegevens kopteksten beginnen met x-ms-metagegevens, x-ms-metagegevens-doel en x-ms-metagegevens-abc toegestaan. Houd er rekening mee dat het jokerteken ' *' geeft aan dat een koptekst die begint met het opgegeven voorvoegsel is toegestaan.

- **ExposedHeaders**: de reactie koppen die kunnen worden verstuurd in het antwoord op het verzoek CORS en die worden aangeboden door de browser naar de uitgever van de aanvraag. In het bovenstaande voorbeeld krijgt de browser de instructie om weer te geven van een kop die begint met x-ms-metagegevens.

- **MaxAgeInSeconds**: de maximale tijd waarop de Preflight-opties moet worden opgeslagen door een browser aanvragen.

De Azure storage-services ondersteunen voorvoegsels kopteksten voor de **AllowedHeaders** en de **ExposedHeaders** elementen opgeven. Als u een categorie van kopteksten, kunt u een algemene voorvoegsel aan die categorie. Bijvoorbeeld precisie *x-ms-metagegevens** terwijl de kop van een voorvoegsels tot stand brengt een regel die komen overeen met alle kopteksten die met x-ms-metagegevens beginnen.

De volgende beperkingen zijn van toepassing op CORS regels:

- U kunt maximaal vijf CORS regels per storage-service (Blob, tabel en wachtrij).
- De maximale grootte van alle CORS regelinstellingen op het verzoek, met uitzondering van XML-codes, moet niet meer dan 2 KB.
- De lengte van een toegestane koptekst, weergegeven koptekst of toegestane origin moet niet meer dan 256 tekens.
- Toegestane kop- en weergegeven kopteksten kunnen zijn:
  - Letterlijke koppen, waar de exacte kopnaam is opgegeven, zoals de **x-ms-metagegevens-verwerkt**. Maximaal 64 letterlijke kopteksten mag worden opgegeven voor het verzoek.
  - Kopteksten, waar een voorvoegsel van de koptekst is opgegeven, zoals **x-ms-metagegevens**voorafgegaan *. Een voorvoegsel precisie op deze manier kunt of een header die met het opgegeven voorvoegsel begint beschrijft. Maximaal twee voorvoegsels kopteksten mag worden opgegeven voor het verzoek.
- De methoden (of HTTP-woorden) opgegeven in het element **AllowedMethods** moeten voldoen aan de methoden die worden ondersteund door Azure storage-service API's. Ondersteunde methoden zijn verwijderen, ophalen, hoofd, samenvoegen, bericht, opties en plaatsen.

## <a name="understanding-cors-rule-evaluation-logic"></a>Lidmaatschap CORS regel evaluatie logica

Wanneer een opslagservice een preflight of werkelijke aanvraag ontvangt, resulteert dit verzoek op basis van de CORS regels die u hebt ingesteld voor de service via de juiste bewerking voor de Service-eigenschappen instellen. CORS regels worden geëvalueerd in de volgorde waarin ze zijn ingesteld in de hoofdtekst van het verzoek van de Service-eigenschappen instellen-bewerking.

Regels voor CORS worden als volgt geëvalueerd:

1. Eerst wordt het oorspronkelijke domein van de aanvraag vergeleken met de weergegeven voor het element **AllowedOrigins** domeinen. Als het oorspronkelijke domein is opgenomen in de lijst of alle domeinen zijn toegestaan met het jokerteken ' *', klikt u vervolgens regels evaluatie opbrengst. Als het oorspronkelijke domein opgenomen is, mislukt de aanvraag.

2. Vervolgens wordt de methode (of HTTP-woord) van de aanvraag vergeleken met de methoden in het element **AllowedMethods** . Als de methode is opgenomen in de lijst, klikt u vervolgens opbrengst de evaluatie van regels; Als dat niet zo is, mislukt de aanvraag.

3. Als het verzoek overeenkomt met een regel in het oorspronkelijke domein en de methode, wordt deze regel is geselecteerd naar proces die de aanvraag en geen verdere regels worden geëvalueerd. Voordat de aanvraag mislukt kunt, echter een kop in de aanvraag vermeld vergeleken met de koppen in het element **AllowedHeaders** vermeld. Als de koppen verzonden niet overeenkomen met de toegestane koppen, mislukt het verzoek.

Aangezien de regels worden verwerkt in de volgorde waarin dat ze aanwezig zijn in het hoofdgedeelte van de aanvraag, wordt aanbevolen dat u de meeste beperkingen regels met betrekking tot de oorspronkelijke bestanden eerst in de lijst opgeeft, zodat deze eerst worden geëvalueerd. Regels die minder beperkend zijn – bijvoorbeeld een regel toe te staan dat alle oorspronkelijke bestanden – aan het einde van de lijst opgeven.

### <a name="example--cors-rules-evaluation"></a>Voorbeeld – CORS regels evaluatie

Het volgende voorbeeld ziet u de hoofdtekst van een gedeeltelijke verzoek voor een bewerking CORS regels voor de opslagservices in te stellen. Zie [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Wachtrij Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx)en [Tabeleigenschappen Service instellen](https://msdn.microsoft.com/library/hh452240.aspx) voor meer informatie over het bouwen van het verzoek.

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Vervolgens kunt u overwegen de volgende CORS verzoeken om:

Aanvragen||| Antwoord||
---|---|---|---|---
**Methode** |**Origin** |**Kopteksten aanvragen** |**Voorbeeld van de zoekwaarde regel** |**Resultaat**
**PLAATSEN** | http://www.contoso.com |x-ms-blob-inhoudstype | Eerste regel |Succes
**Toevoegen** | http://www.contoso.com| x-ms-blob-inhoudstype | Tweede regel |Succes
**Toevoegen** | http://www.contoso.com| x-ms-blob-inhoudstype | Tweede regel | Is mislukt

De eerste aanvraag overeenkomt met de eerste regel – het oorspronkelijke domein komt overeen met de toegestane oorsprong, de methode komt overeen met de toegestane methoden en de kop overeenkomt met de toegestane kop- en dus is geslaagd.

De tweede aanvraag niet overeenkomen met de eerste regel omdat de methode niet overeenkomen met de toegestane methoden. Dit, echter overeenkomt met de tweede regel, zodat deze is uitgevoerd.

Het derde verzoek komt overeen met de tweede regel in het oorspronkelijke domein en de methode, zodat er geen verdere regels worden geëvalueerd. De *kop van de x-ms-client-aanvraag-id* is echter niet toegestaan door de tweede regel, zodat het verzoek mislukt, ondanks het feit dat de semantiek van de derde regel zou hebben toegestaan deze te kunnen uitvoeren.

>[AZURE.NOTE] Hoewel in dit voorbeeld ziet u een minder beperkend regel vóór een beperktere, is in het algemeen de beste manier voor een overzicht van de meeste beperkingen regels eerst.

## <a name="understanding-how-the-vary-header-is-set"></a>Informatie over hoe de koptekst variëren is ingesteld

De koptekst *variëren* is een HTTP/1.1 Standaardkoptekst dat bestaat uit een set verzoek berichtvelden die het beste contact van de browser of gebruiker agent over de criteria die zijn geselecteerd door de server voor de verwerking van de aanvraag. De koptekst *variëren* wordt vooral gebruikt voor caching door proxy's, browsers en CDN's, waarin deze gebruiken om te bepalen hoe het antwoord mag worden opgeslagen. Zie voor meer informatie de specificatie voor de [koptekst van variëren](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Wanneer de browser of een ander gebruikersagent slaat het antwoord van een aanvraag CORS, het oorspronkelijke domein is in cache opgeslagen als de toegestane oorsprong. Wanneer een tweede domein één verzoek voor een resource opslag problemen terwijl de cache actief is, haalt de gebruikersagent het domein in de cache origin. Het tweede domein niet overeenkomen met het domein in de cache opgeslagen, zodat het verzoek mislukt wanneer deze anders zou slagen. Azure Storage Hiermee in bepaalde gevallen stelt u de koptekst variëren op **Origin** als instructie voor de gebruikersagent naar de volgende CORS-aanvraag naar de service verzenden wanneer het vraagt domein van de in de cache oorsprong verschilt.

Azure opslag Hiermee stelt u de koptekst *variëren* op **Origin** voor werkelijke GET/hoofd-verzoeken in de volgende gevallen:

- Wanneer de aanvraag oorsprong komt exact overeen met de toegestane oorsprong gedefinieerd door een regel CORS. Als u een exacte overeenkomst, de regel CORS mogelijk niet inbegrepen bij een jokerteken ' * ' teken.

- Er is geen regel die overeenkomen met de oorsprong van de aanvraag, maar CORS is ingeschakeld voor de storage-service.

In het geval is waar een verzoek voor een GET/hoofd overeenkomt met een regel CORS waarmee alle oorspronkelijke bestanden, het antwoord geeft aan dat alle oorspronkelijke bestanden zijn toegestaan, kan de cache van de gebruiker-agent de volgende aanvragen van elk domein origin terwijl de cache actief is.

Houd er rekening mee dat voor aanvragen met behulp van methoden dan GET/hoofd, de storage-services wordt niet ingesteld de koptekst variëren Aangezien antwoorden op de volgende manieren niet in de cache door gebruikersagenten opgeslagen worden.

De volgende tabel wordt aangegeven hoe Azure opslag beantwoordt GET/hoofd aanvragen op basis van de hierboven genoemde gevallen:

Aanvragen|Instellingen van een account en het resultaat van de evaluatie van regels|||Antwoord|||
---|---|---|---|---|---|---|---|---
**Origin koptekst presenteren op aanvraag** | **CORS (s) die zijn opgegeven voor deze service** | **Overeenkomende regel bestaat waarmee alle oorspronkelijke bestanden (*)** | **Overeenkomende regel bestaat voor de exacte origin vergelijken** | **Antwoord bevat variëren koptekst ingesteld op Origin** | **Antwoord bestaat uit Access-besturingselement-toegestaan-Origin: "*"** | **Antwoord bevat Access-besturingselement-die worden aangeboden-kopteksten**
Nee|Nee|Nee|Nee|Nee|Nee|Nee
Nee|Ja|Nee|Nee|Ja|Nee|Nee
Nee|Ja|Ja|Nee|Nee|Ja|Ja
Ja|Nee|Nee|Nee|Nee|Nee|Nee
Ja|Ja|Nee|Ja|Ja|Nee|Ja
Ja|Ja|Nee|Nee|Ja|Nee|Nee
Ja|Ja|Ja|Nee|Nee|Ja|Ja


## <a name="billing-for-cors-requests"></a>Facturering voor CORS aanvragen

Voltooide preflight aanvragen zijn gefactureerd als u CORS naar een of meer van de services opslagruimte voor uw account hebt ingeschakeld (door de ondersteuning voor [Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx), [Wachtrij Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx)of [Tabeleigenschappen Service instellen](https://msdn.microsoft.com/library/hh452240.aspx)). Als u wilt minimaliseren kosten, kunt u het element **MaxAgeInSeconds** in uw CORS regels op een hoge waarde instellen, zodat de gebruikersagent het verzoek slaat.

Niet geslaagd preflight aanvragen wordt niet gefactureerd.

## <a name="next-steps"></a>Volgende stappen

[Blob Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452235.aspx)

[Wachtrij Service-eigenschappen instellen](https://msdn.microsoft.com/library/hh452232.aspx)

[Service tabeleigenschappen instellen](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C Cross-Origin Resource specificatie delen](http://www.w3.org/TR/cors/)
