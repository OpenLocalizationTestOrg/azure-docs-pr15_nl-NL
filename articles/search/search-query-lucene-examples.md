<properties
    pageTitle="Lucene query voorbeelden voor het zoeken van Azure | Microsoft Azure zoeken"
    description="Lucene syntaxis van de query voor onduidelijk zoeken, nabijheid zoeken, waarbij de term, reguliere expressies zoeken en zoeken met jokertekens."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Syntaxisvoorbeelden van Lucene query voor het samenstellen van query's in Azure zoeken

Als het bouwen van query's voor Azure zoeken, kunt u de [syntaxis van de eenvoudige query](https://msdn.microsoft.com/library/azure/dn798920.aspx) wilt instellen of de alternatieve [Lucene Query Parser in Azure zoeken](https://msdn.microsoft.com/library/azure/mt589323.aspx). De Query-Parser Lucene ondersteunt meer complexe query constructies, zoals veld beperkt query's, bij benadering zoeken, nabijheid zoeken, waarbij de term en reqular expressie zoeken.

In dit artikel, kunt u stapsgewijs door voorbeelden die de syntaxis van de query Lucene en resultaten naast elkaar worden weergegeven. Voorbeelden voor een vooraf geladen zoekindex in [JSFiddle](https://jsfiddle.net/), een online code-editor voor het testen script en HTML-code worden uitgevoerd. 

Klik met de rechtermuisknop op de query voorbeeld-URL's JSFiddle openen in een afzonderlijk browservenster.

> [AZURE.NOTE] De volgende voorbeelden gebruikmaken van een zoekindex die bestaat uit de functies die beschikbaar zijn op basis van een gegevensset die is verstrekt door de [Plaats van New York OpenData](https://nycopendata.socrata.com/) initiatief. Deze gegevens niet beschouwd huidige of voltooid. De index is in een sandbox-service van Microsoft. U hoeft niet een Azure-abonnement of Azure zoeken om te proberen deze query's.

## <a name="viewing-the-examples-in-this-article"></a>Weergeven van de voorbeelden in dit artikel

Alle voorbeelden in dit artikel opgeven de Lucene Query Parser via de parameter van de zoekopdracht**queryType** . Wanneer u met de Lucene Query Parser vanuit uw code, moet u het **queryType** op elk verzoek.  Geldige waarden zijn **eenvoudige**|**volledige**, met **eenvoudige** als de standaard- en **volledige** voor de Query-Parser Lucene. Zie [Zoeken-documenten (Azure zoeken Service REST API)](https://msdn.microsoft.com/library/azure/dn798927.aspx) voor meer informatie over het opgeven van queryparameters.

**Voorbeeld 1** --met de rechtermuisknop op de volgende query fragment om dit te openen in een nieuwe browserpagina die wordt geladen JSFiddle en de query wordt uitgevoerd:
- [& queryType = het volledige & zoeken = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Deze query retourneert documenten van onze taken index (geladen in een sandboxservice)

In het nieuwe browservenster ziet u de JavaScript-bron- en HTML-uitvoer naast elkaar worden geïnstalleerd. Het script verwijst naar een query die is verstrekt door de voorbeeld-URL's in dit artikel. Klik in het **Voorbeeld 1**is de onderliggende query bijvoorbeeld als volgt:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Kennisgeving de query wordt gebruikt voor een vooraf geconfigureerde Azure zoekindex nycjobs genoemd. De parameter **searchFields** de zoekopdracht tot alleen het titelveld bedrijven wordt beperkt. Het **queryType** is ingesteld op **volledige**, zorgt u ervoor dat Azure zoeken naar de Lucene Query-Parser gebruiken voor deze query.

### <a name="fielded-query-operation"></a>Fielded querybewerking

U kunt de voorbeelden in dit artikel wijzigen door op te geven van een constructie **fieldname:searchterm** als een querybewerking fielded, waarbij het veld is één woord en de zoekterm ook één woord of een woordgroep, (optioneel) met Booleaans operatoren wilt definiëren. Enkele voorbeelden het volgende:

- business_title:(senior NOT junior)
- de staat: ("Amsterdam" en "Nieuwe Jersey")

Zorg ervoor dat meerdere tekenreeksen tussen aanhalingstekens plaatsen als u wilt dat beide tekenreeksen moet worden geëvalueerd als een eenheid, zoals in dit geval twee afzonderlijke steden in het locatieveld zoekt. Zorg er ook voor de operator is een hoofdletter terwijl u niet ziet en and

Het veld dat is opgegeven in **fieldname:searchterm** moet een doorzoekbare veld. Zie [Create Index (Azure zoeken Service REST API)](https://msdn.microsoft.com/library/azure/dn798941.aspx) voor meer informatie over hoe indexkenmerken worden gebruikt in velddefinities.

## <a name="fuzzy-search"></a>Bij benadering zoeken

Een bij benadering gezocht overeenkomsten in termen die vergelijkbaar gebouwd zijn. Per [Lucene documentatie](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), bij benadering zoekopdrachten op basis van [Damerau-Levenshtein afstand](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Als u wilt onduidelijk zoeken, gebruikt u de tilde ' ~ "symbool aan het einde van een woord met een optionele parameter, een waarde tussen 0 en 2, waarmee de afstand bewerken. Bijvoorbeeld: "blauwe ~" of "blauwe ~ 1" het resultaat blauw, blauwe en lijm.

**Voorbeeld 2** --met de rechtermuisknop op de volgende query-fragment naar zelf proberen. Deze query zoekt naar zakelijke titels met het hoofd term in deze, maar niet leerling:

- [& queryType = het volledige & zoeken = business_title:senior niet leerling](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Nabijheid zoeken

Zoekacties in nabijheid worden gebruikt om te zoeken van termen die zich dicht bij elkaar in een document. Invoegen van een tilde ' ~ "symbool aan het einde van een zin gevolgd door het aantal woorden die de grens nabijheid maakt. Bijvoorbeeld: "hotel airport" ~ 5 vindt u de termen hotel en airport binnen 5 woorden van elkaar in een document.

**Voorbeeld 3** : met de rechtermuisknop op de volgende query-fragment. Deze query zoekt naar taken met de term koppelen (waar deze onjuist is gespeld):

- [& queryType = het volledige & zoeken = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Voorbeeld 4** --met de rechtermuisknop op de query. Zoeken naar taken met de term "hoge analisten" waarin deze worden gescheiden door niet meer dan één woord:

- [& queryType = het volledige & zoeken = business_title: "hoge analisten" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Voorbeeld 5** --probeer het opnieuw verwijderen de woorden tussen de term "hoge analisten".

- [& queryType = het volledige & zoeken = business_title: "hoge analisten" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Termijn prestatieverbetering

Term verhogen verwijst naar een hoger als dit de gestimuleerd term, ten opzichte van de documenten die geen de term bevatten bevat document classificeren. Dit verschilt van profielen scoren in dat score profielen bepaalde velden, in plaats van bepaalde termen verhogen. Het volgende voorbeeld kunt de verschillen illustreren.

Houd rekening met een score profiel dat verhoogt komt overeen met in een bepaald veld, zoals **shape** in het voorbeeld musicstoreindex. Term verhogen kan worden gebruikt voor verdere termen die hoger zijn dan anderen verhogen bepaalde zoeken. Bijvoorbeeld: "uit ^ 2 elektronische" documenten met de zoektermen in het **nieuwe** veld die hoger zijn dan andere doorzoekbare velden in de index wordt verhogen. Bovendien worden documenten met de zoekterm 'uit' hoger dan de andere 'elektronische"grond van de term Microfoonversterking waarde (2) zoekterm worden gerangschikt.

Gebruik het caret-teken, om te verhogen van een term ' ^ ', symbool met een factor Microfoonversterking (getal) aan het einde van de term die u zoekt. Hoe hoger de Microfoonversterking factor, de nog relevanter wordt de term is ten opzichte van andere zoektermen. Standaard is de factor Microfoonversterking 1. Hoewel de factor Microfoonversterking positief zijn moet, kan het zijn minder dan 1 (bijvoorbeeld 0,2).

**Voorbeeld 6** --met de rechtermuisknop op de query. Zoekt u taken met de term "computer analisten" waar ziet er zijn geen resultaten met woorden computer zowel analisten, maar analisten taken zijn aan de bovenkant van de resultaten.

- [& queryType = het volledige & zoeken = business_title:computer analisten](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Voorbeeld 7** --probeer het opnieuw, met de computer term via de term analisten deze tijd verhogen resultaten als beide woorden niet bestaan.

- [& queryType = het volledige & zoeken = business_title:computer ^ 2 analisten](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Reguliere expressies

Een zoekopdracht reguliere expressies vindt u een overeenkomst op basis van de inhoud tussen slash "/", als beschreven in de [klasse RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Voorbeeld 8** --met de rechtermuisknop op de query. Taken met een zoekt u de term hoofd of Junior.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

De URL voor dit voorbeeld worden niet correct weergegeven op de pagina. Als een tijdelijke oplossing de onderstaande URL kopiëren en plak deze in de browser URL-adres:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Zoeken met jokertekens

U kunt de syntaxis van de algemeen erkende gebruiken voor meerdere (\*) of één zoekopdrachten met jokertekens teken (?). Opmerking de Lucene query-parser ondersteunt het gebruik van deze symbolen met één term en niet een zin.

**Voorbeeld 9** --met de rechtermuisknop op de query. Zoek voor taken die het voorvoegsel 'prog', waaronder zakelijke titels met de voorwaarden programming en het programma in deze zou bevatten.

- [& queryType = volledige & $select = business_title & zoeken = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

U kunt niet gebruiken een * of? symbool als het eerste teken van een zoekopdracht.


## <a name="next-steps"></a>Volgende stappen

Probeer aan te geven van de Query-Parser Lucene in uw code. De volgende koppelingen wordt uitgelegd hoe u zoekopdrachten gaat uitvoeren configureert voor zowel .NET en de REST API. De koppelingen Gebruik eenvoudige standaardsyntaxis wilt instellen, zodat u moet toepassen wat u hebt geleerd van dit artikel om op te geven van de **queryType**.

- [De zoekindex van Azure met de SDK .NET query](search-query-dotnet.md)
- [Query de zoekindex van Azure de REST API gebruiken](search-query-rest-api.md)


 