<properties
    pageTitle="Azure CDN gebruikt met CORS | Microsoft Azure"
    description="Informatie over het gebruik van de Azure inhoud bezorging netwerk (CDN) naar met Cross-Origin Resource delen (CORS)."
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
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Azure CDN gebruikt met CORS     

## <a name="what-is-cors"></a>Wat is CORS?

CORS (Cross Origin resources delen) is een HTTP-functie waarmee een webtoepassing uitgevoerd in een domein voor toegang tot resources in een ander domein. Alle moderne webbrowsers implementeren om te verkleinen de mogelijkheid van meerdere sites uitvoeren van scripts aanvallen, een beveiligingsbeperking [dezelfde oorsprong beleid](http://www.w3.org/Security/wiki/Same_Origin_Policy)genoemd.  Hiermee voorkomt u dat een webpagina bellen API's in een ander domein.  CORS biedt een veilige methode voor het toestaan van een domein (het oorspronkelijke domein) API's in een ander domein bellen.
 
## <a name="how-it-works"></a>Werkwijze
1.  Het verzoek van de opties met een koptekst **Origin** HTTP verzendt in de browser. De waarde van deze kop is het domein dat de bovenliggende pagina served. Als een pagina uit https://www.contoso.com probeert te krijgen van de gegevens van een gebruiker in het domein fabrikam.com, zou de kop van de volgende aanvraag worden verzonden naar fabrikam.com: 
    
    `Origin: https://www.contoso.com`
 
2.  De server reageert mogelijk met de volgende items:
    - Een **Access-besturingselement-toestaan-Origin** kop in het antwoord dat aangeeft welke sites origin zijn toegestaan. Bijvoorbeeld:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - De foutpagina van een als de server mag niet het verzoek cross-origin
    - Een **Access-besturingselement-toestaan-Origin** koptekst met een jokerteken waarmee alle domeinen:
        
        `Access-Control-Allow-Origin: *`
 
Voor complexe HTTP-aanvragen, er is een "preflight" aanvraag eerste klaar om te bepalen of er toestemming voordat u de gehele aanvraag verzendt.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Jokertekens of één origin scenario 's

CORS op Azure CDN werkt automatisch met geen aanvullende configuratie wanneer de koptekst van de **Access-besturingselement-toestaan-Origin** jokerteken (*) of een enkel origin is ingesteld.  De CDN wordt het eerste antwoord in cache en volgende aanvragen dezelfde kop wordt gebruikt.
 
Als aanvragen al zijn doorgevoerd in de CDN vóór CORS wordt ingesteld op de uw origin, moet u de inhoud van uw inhoud eindpunt om het laden van de inhoud met de **Access-besturingselement-toestaan-Origin** koptekst verwijderen.
 
## <a name="multiple-origin-scenarios"></a>Meerdere origin-scenario 's

Als u toestaan dat een specifieke lijst met oorspronkelijke bestanden wilt toegestaan tot CORS, kunnen u dingen iets ingewikkelder. Het probleem treedt op wanneer de CDN de **Access-besturingselement-toestaan-Origin** koptekst voor de eerste CORS oorsprong slaat.  Wanneer een andere CORS origin een volgende aanvraag maakt, wordt de CDN de in de cache kop van het **Access-besturingselement-toestaan-Origin** , die niet overeenkomen met served.  Er zijn verschillende manieren om dit te corrigeren.
 
### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium uit Verizon

De beste manier om in te schakelen dit is het gebruik van **Azure CDN Premium uit Verizon**, waarmee sommige geavanceerde functionaliteit. 
 
U moet [een regel maken](cdn-rules-engine.md) om te controleren van de kop **Origin** op het verzoek.  Als dit een geldige origin is, wordt de regel de kop van de **Access-besturingselement-toestaan-Origin** configureren met de oorsprong die beschikbaar zijn in het verzoek.  Als het oorspronkelijke opgegeven in de header **Origin** is niet toegestaan, moet de **Access-besturingselement-toestaan-Origin** koptekst, waardoor de browser naar het verzoek negeren weglaten in de regel. 
 
Er zijn twee manieren hiervoor met de engine regels.  In beide gevallen de koptekst van de **Access-besturingselement-toestaan-oorsprong** van bronserver van het bestand wordt volledig genegeerd, de toegestane CORS oorsprong volledig worden beheerd in van de CDN regels engine.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Een normale expressie met alle geldige oorspronkelijke bestanden
 
In dit geval maakt u een reguliere expressie waarin alle van de oorspronkelijke bestanden die u wilt toestaan dat: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] [Perl compatibele reguliere expressies](http://pcre.org/) **Azure CDN uit Verizon** gebruikt als de engine voor reguliere expressies.  U kunt een hulpprogramma zoals [reguliere expressies 101](https://regex101.com/) gebruiken voor het valideren van uw reguliere expressie.  Houd er rekening mee dat het teken "/" in reguliere expressies geldig is en niet moet worden voorafgegaan, echter ontsnappen dat teken wordt een aanbevolen en door sommige validatiefuncties regex naar verwachting.

Als de reguliere expressie overeenkomt met, de regel wordt vervangen door de **Access-besturingselement-toestaan-Origin** koptekst (indien aanwezig) van de oorsprong met de oorsprong die de aanvraag heeft verzonden.  U kunt ook extra CORS kopteksten, zoals **Besturingselement-toestaan-methoden voor clienttoegang**toevoegen.

![Voorbeeld van de regels met reguliere expressies](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Kop-regel voor elke origin aanvragen.

U kunt in plaats daarvan een afzonderlijke regel voor elke origin die u wilt toestaan de **Koptekst jokertekens aanvragen** [overeenkomen met voorwaarde](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1)gebruiken in plaats van reguliere expressies maken. Net als met de methode reguliere expressies Hiermee stelt u de regels engine alleen de koppen CORS. 
  
![Voorbeeld van de regels zonder reguliere expressies](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] In het voorbeeld hierboven, het gebruik van de jokerteken * de regels-engine om aan te passen HTTP- en HTTPS worden vermeld.
 
### <a name="azure-cdn-standard"></a>Azure CDN standaard

Op Azure CDN standaard profielen is de enige om toe te staan dat voor meerdere oorsprong zonder het gebruik van de oorsprong jokertekens via [caching van query-tekenreeks](cdn-query-string.md).  U moet query tekenreeks is ingeschakeld voor het eindpunt CDN en gebruik een unieke queryreeks aanmeldingsaanvragen van elk toegestane domein. Hierdoor resulteert in de cache opslaan van een afzonderlijk object voor elke unieke queryreeks CDN. Deze benadering is echter niet ideaal, zoals deze zijn ingevoegd om meerdere exemplaren van hetzelfde bestand in de cache op de CDN.  

