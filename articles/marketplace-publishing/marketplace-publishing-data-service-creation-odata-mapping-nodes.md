<properties
   pageTitle="Handleiding voor het maken van een Service van gegevens voor de Marketplace | Microsoft Azure"
   description="Uitgebreide instructies over het maken, certificeren en implementeren van een Service van de gegevens voor kopen op Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Wat is de knooppunten schema voor het toewijzen van een bestaande webservice met OData tot en met CSDL?

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding eventuele nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet ophalen goedgekeurd voor de aanbieding.** Als u een SaaS bedrijfstoepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u een IaaS-toepassingen hebt of ontwikkelaars-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

In dit document wordt de structuur knooppunt voor het toewijzen van een OData-protocol aan CSDL verduidelijken. Het is belangrijk te weten dat de structuur knooppunt goed is gevormd XML. Hoofdmap, bovenliggende en onderliggende schema kan worden gebruikt bij het ontwerpen van de OData-toewijzing.

## <a name="ignored-elements"></a>Genegeerde elementen
Hier volgen de hoog niveau CSDL elementen (XML-knooppunten) die niet in aanmerking moet worden gebruikt door de Azure Marketplace-end tijdens het importeren van metagegevens van de web-service. Ze kunnen aanwezig zijn, maar worden genegeerd.

| Element | Bereik |
|----|----|
| Met behulp van Element | Het knooppunt, subknooppunten en alle kenmerken |
| Documentatie-Element | Het knooppunt, subknooppunten en alle kenmerken |
| ComplexType | Het knooppunt, subknooppunten en alle kenmerken |
| Koppelingelement | Het knooppunt, subknooppunten en alle kenmerken |
| Uitgebreide eigenschap | Het knooppunt, subknooppunten en alle kenmerken |
| EntityContainer | Alleen de volgende kenmerken worden genegeerd: *breidt* en *AssociationSet* |
| Schema | Alleen de volgende kenmerken worden genegeerd: *Namespace* |
| FunctionImport | Alleen de volgende kenmerken worden genegeerd: *modus* (standaardwaarde van ln wordt gebruikt) |
| EntityType | Alleen de volgende subknooppunten worden genegeerd: *sleutel* en *PropertyRef* |

De volgende beschrijving van de wijzigingen (toegevoegd en elementen genegeerd) naar de verschillende CSDL XML-knooppunten in detail.

## <a name="functionimport-node"></a>FunctionImport knooppunt
Een knooppunt FunctionImport vertegenwoordigt een URL (ingangspunt) dat toegang biedt tot een service aan de eindgebruiker. Het knooppunt toestaat waarin wordt beschreven hoe de URL is gericht, welke parameters die beschikbaar zijn voor de eindgebruiker en hoe deze parameters worden gegeven.

Meer informatie over dit knooppunt zijn gevonden bij [hier] [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Hieronder vindt u de extra kenmerken (of toevoegingen aan kenmerken) die zijn die worden aangeboden door het knooppunt FunctionImport:

**d:BaseUri** -
het URI-sjabloon voor de REST resource die beschikbaar zijn voor Marketplace. De sjabloon Marketplace gebruikt om te maken van query's voor de REST-webservice. De sjabloon URI bevat tijdelijke aanduidingen voor de parameters in de vorm van {parameterName}, waar parameterName is de naam van de parameter. Bijvoorbeeld apiVersion = {apiVersion}.
Parameters zijn worden weergegeven als URI parameters of als onderdeel van het pad URI toegestaan. Als de weergave in het pad zijn ze altijd verplicht (kan niet worden gemarkeerd als nullable). *Voorbeeld:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**De naam van** -de naam van de geïmporteerde functie.  Mogen niet hetzelfde als de namen van andere gedefinieerd in de CSDL.  Bijvoorbeeld Naam = "GetModelUsageFile"

**EntitySet** *(optioneel)* - als de functie een verzameling Entiteitstypen retourneert, de waarde van de **EntitySet** moet de entiteit instellen waartoe de verzameling behoort. Het kenmerk **EntitySet** moet anders niet worden gebruikt. *Voorbeeld:*`EntitySet="GetUsageStatisticsEntitySet"`

**Retourtype** *(Optioneel)* - geeft het type van elementen die het resultaat van de URI.  Gebruik dit kenmerk niet als de functie geen waarde retourneert. Hier volgen de ondersteunde typen:

 - **Siteverzameling (<Entity type name>)**: Hiermee geeft u een verzameling gedefinieerde Entiteitstypen. De naam is in het kenmerk naam van het knooppunt EntityType. Een voorbeeld is een verzameling (WXC. HourlyResult).
 - **Onbewerkte (<mime type>)**: Hiermee geeft u een onbewerkte document/blob die aan de gebruiker wordt geretourneerd. Een voorbeeld is Raw(image/jpeg) andere voorbeelden:

  - ReturnType="Raw(text/plain)"
  - Retourtype = "siteverzameling (sage. DeleteAllUsageFilesEntity) "*

**d:Paging** - geeft aan hoe paginering is verwerkt door de REST resource. De parameterwaarden worden gebruikt in gekrulde braches, bijvoorbeeld pagina {$page} = & itemsperpage = {$size} de beschikbare opties zijn:

- **Geen:** geen paginering beschikbaar is
- **Overslaan:** paginering wordt uitgedrukt via een logische "overslaan" en "nemen" (bovenaan). Overslaan vroom M elementen en nemen, retourneert de volgende N-elementen. Parameterwaarde: $skip
- **Nemen:** Geeft als resultaat de volgende N elementen duren. Parameterwaarde: $take
- **PageSize:** paginering wordt uitgedrukt via een logische pagina en de grootte (items per pagina). Pagina staat voor de huidige pagina die wordt geretourneerd. Parameterwaarde: $page
- **Grootte:** grootte Hiermee geeft u het aantal items geretourneerd voor elke pagina. Parameterwaarde: $size

**d:AllowedHttpMethods** *(Optioneel)* - geeft aan welke werkwoord is verwerkt door de REST resource. Ook wordt beperkt geaccepteerde werkwoord op de opgegeven waarde.  Standaard = posten.  *Voorbeeld:* `d:AllowedHttpMethods="GET"` De beschikbare opties zijn:

- **Ophalen:** meestal gebruikt om te retourneren van gegevens
- **Bericht:** meestal gebruikt voor het invoegen van nieuwe gegevens
- **Plaatsen:** meestal gebruikt voor het bijwerken van gegevens
- **Verwijderen:** gebruikt om gegevens te verwijderen

Extra onderliggende knooppunten (niet de documentatie CSDL vallen) in het knooppunt FunctionImport zijn:

**d:RequestBody** *(Optioneel)* - het hoofdgedeelte van de aanvraag wordt gebruikt om aan te geven dat het verzoek een hoofdtekst verwacht te kunnen verzenden. Parameters kunnen worden opgegeven in de hoofdtekst van het verzoek. Ze zijn uitgedrukt binnen accolades, bijvoorbeeld {parameterName}. Deze parameters zijn van de invoer in de hoofdtekst die worden overgebracht naar de service van de provider van de inhoud van de gebruiker toegewezen. Het element requestBody heeft een kenmerk met naam httpMethod. Het kenmerk kan twee waarden:

- **Bericht:** Wordt gebruikt als de aanvraag een HTTP-bericht is
- **Ophalen:** Als de aanvraag een HTTP GET is gebruikt

    Voorbeeld:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** en **d:Namespace** - beschrijving van dit knooppunt de naamruimten die zijn gedefinieerd in het XML-bestand die wordt geretourneerd door de functie importeren (URI eindpunt). De XML-code die wordt geretourneerd door de backend-service mogelijk niet het getal van naamruimten om te onderscheiden van de inhoud die wordt geretourneerd. **Alle deze naamruimten, als gebruikt in d:Map of d:Match XPath-query's moet worden vermeld.** Het knooppunt d:Namespaces bevat een set/lijst met d:Namespace knooppunten. Elk van deze bevat één naamruimte gebruikt in het antwoord van de end-service. Hierna ziet u het kenmerk van het knooppunt d:Namespace:

-   **d:Prefix:** Het voorvoegsel voor de naamruimte, zoals gezien in de XML-resultaten geretourneerd door de service, bijvoorbeeld f:FirstName, f:LastName, waarin f het voorvoegsel is.
- **d:Uri:** De volledige URI van de naamruimte gebruikt in het document resultaat. Hiermee geeft u de waarde die het voorvoegsel wordt omgezet in gedurende runtime.

**d:ErrorHandling** *(Optioneel)* - dit knooppunt bevat voorwaarden voor foutafhandeling. Elk van de voorwaarden is gevalideerd op basis van het resultaat die door de service van de provider van de inhoud wordt geretourneerd. Als een voorwaarde komt overeen met de voorgestelde HTTP-foutcode die een foutbericht wordt weergegeven aan de eindgebruiker wordt geretourneerd.

**d:ErrorHandling** *(Optioneel)* en **d:Condition** bevat *(optioneel)* - voorwaarde knooppunt één voorwaarde die het resultaat van de provider van de inhoud-service is ingecheckt. Hier volgen de **vereiste** kenmerken:

- **d:Match:** Een XPath-expressie die is gevalideerd met of een opgegeven knooppunt/waarde in van de provider van de inhoud uitvoer XML-aanwezig is. Het XPath voor de uitvoer is uitgevoerd en moet worden geretourneerd waar als de voorwaarde onwaar of een overeenkomende anders is.
- **d:HttpStatusCode:** De HTTP-statuscode die moet worden geretourneerd door Marketplace in het geval de voorwaarde komt overeen met. Marketplace signalizes fouten aan de gebruiker via HTTP-statuscodes. Een lijst met HTTP-statuscodes zijn beschikbaar op http://en.wikipedia.org/wiki/HTTP_status_code
- **d:ErrorMessage:** Het foutbericht dat wordt geretourneerd – met de statuscode HTTP – aan de eindgebruiker. Dit moet een beschrijvende foutbericht wordt weergegeven die geen geheimen niet bevatten.

**d:title** *(Optioneel)* - kunt met een beschrijving van de titel van de functie. De waarde voor de titel is die afkomstig zijn uit

- Het optionele map-kenmerk (een xpath) waarin wordt aangegeven waar vind ik de titel in het antwoord dat uit het serviceaanvraag geretourneerd.
- - Of - de titel die is opgegeven als de waarde van het knooppunt.

**d:Rights** *(Optioneel)* - (bijvoorbeeld copyright) van de rechten die is gekoppeld aan de functie. De waarde voor de rechten is die afkomstig zijn uit:

- Het optionele map-kenmerk (een xpath) waarin wordt aangegeven waar vind ik de rechten in het antwoord dat uit het serviceaanvraag geretourneerd.
-   - Of - de rechten die is opgegeven als de waarde van het knooppunt.

**d:Description** *(Optioneel)* - een korte beschrijving voor de functie. De waarde voor de beschrijving die is afkomstig zijn uit

- Het optionele map-kenmerk (een xpath) waarin wordt aangegeven waar vind ik de beschrijving in het antwoord dat uit het serviceaanvraag geretourneerd.
- - Of – de beschrijving van de opgegeven als de waarde van het knooppunt.

**d:EmitSelfLink** - *Zie bovenstaande voorbeeld "FunctionImport voor 'Paginering' tot en met resultaatgegevens"*

**d:EncodeParameterValue** - optionele extensie met OData

**d:QueryResourceCost** - optionele extensie met OData

**d:Map** - optionele extensie met OData

**d:Headers** - optionele extensie met OData

**d:Headers** - optionele extensie met OData

**d:Value** - optionele extensie met OData

**d:HttpStatusCode** - optionele extensie met OData

**d:ErrorMessage** - optionele extensie met OData

## <a name="parameter-node"></a>Parameterknooppunt

In dit knooppunt vertegenwoordigt een parameter dat wordt weergegeven als onderdeel van de sjabloon URI / hoofdtekst die is opgegeven in het knooppunt FunctionImport aanvragen.

Een pagina van het document zeer handig details over het knooppunt "Parameter Element" wordt aangetroffen op [hier](http://msdn.microsoft.com/library/ee473431.aspx) (de vervolgkeuzelijst van de **Andere versie** gebruiken om te selecteren van een andere versie zo nodig aan bekijkt u de documentatie). *Voorbeeld:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Parameter kenmerk | Is vereist | Waarde |
|----|----|----|
| Naam | Ja | De naam van de parameter. Hoofdlettergevoelig!  Overeenkomen met de BaseUri hoofdletters/kleine letters. **Voorbeeld:**`<Property Name="IsDormant" Type="Byte" />` |
| Type | Ja | Het parametertype. De waarde moet een **EDMSimpleType** of een complexe type dat is binnen het bereik van het model. Zie voor meer informatie "6 ondersteunde eigenschap-Parameter grafiektypen".  (Hoofdlettergevoelig! Eerste teken is een hoofdletter, rest zijn kleine letters)  Zie ook, [conceptueel Model typen (CSDL)] [MSDNParameterLink]. **Voorbeeld:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Modus | Nee | **In**, of InOut afhankelijk of de parameter zich een invoer, uitvoer of invoer/uitvoer-parameter. (Alleen 'IN' is beschikbaar in Azure Marketplace.) **Voorbeeld:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | Nee | Het maximale aantal toegestane lengte van de parameter. **Voorbeeld:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Precisie van formules | Nee | De precisie van de parameter. **Voorbeeld:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Schaal | Nee | De schaal van de parameter. **Voorbeeld:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Hierna ziet u de kenmerken die zijn toegevoegd aan de specificatie CSDL:

| Parameter kenmerk | Beschrijving |
|----|----|
| **d:Regex** *(Optioneel)* | Een regex-instructie die wordt gebruikt voor het valideren van de invoerwaarde voor de parameter. Als de invoerwaarde niet overeenkomen met de instructie wordt de waarde genegeerd. In deze sectie geeft ook een set opgeven van waarden in, bijvoorbeeld ^ [0-9] +? $ dat alleen getallen zijn toegestaan. **Voorbeeld:** ' < parameternaam = "naam" modus = 'In' Type = "Tekenreeks" d: Nullable = "false" d:Regex = "^ [a-zA-Z] * $" d:Description = "een naam die geen spaties of niet-alfabetische niet-Engelstalige tekens bevatten kan' d:SampleValues ="George|John|Thomas|James "/ >' |
| **d:Enum** *(Optioneel)* | Een recht streepje gescheiden lijst met waarden voor de parameter geldig. Het type van de waarden moet overeenkomen met de gedefinieerde type van de parameter. Voorbeeld: ' Engels|metrisch|onbewerkte`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< parameternaam = "Duur" Type 'Tekenreeks' modus = = 'In' Nullable = "true" d:Enum = "1 jaar|5years|10years "/ >' |
| **d: Nullable** *(Optioneel)* | U kunt definiëren of een parameter null worden kan. De standaardinstelling is: waar. Parameters die beschikbaar zijn als onderdeel van het pad in de sjabloon URI kunnen echter niet null. Als het kenmerk is ingesteld op ONWAAR voor deze parameters – wordt de invoer van de gebruiker overschreven. **Voorbeeld:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Optioneel)* | De Voorbeeldwaarde van een weergeven als een notitie naar de klant in de gebruikersinterface.  Het is mogelijk verschillende om waarden te tellen met behulp van een recht streepje gescheiden lijst, dat wil zeggen ' een|b|c` **Example:** `< parameternaam = "BikeOwner" Type 'Tekenreeks' modus = = 'In' d:SampleValues = "George|John|Thomas|James "/ >' |

## <a name="entitytype-node"></a>EntityType knooppunt

Dit knooppunt vertegenwoordigt een van de typen die worden geretourneerd uit Marketplace naar de eindgebruiker. Deze bevat ook de toewijzingen van de uitvoer die wordt geretourneerd door de service van de provider van de inhoud op de waarden die aan de eindgebruiker worden geretourneerd.

Meer informatie over dit knooppunt bevinden zich op [hier](http://msdn.microsoft.com/library/bb399206.aspx) (Gebruik de vervolgkeuzelijst van de **Andere versie** naar een andere versie Selecteer zo nodig aan bekijkt u de documentatie.)

| Kenmerknaam | Is vereist | Waarde |
|----|----|----|
| Naam | Ja | De naam van het entiteitstype. **Voorbeeld:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType | Nee | De naam van een andere entiteit Typ dat wil zeggen het grondtal type het entiteitstype die wordt gedefinieerd. **Voorbeeld:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Hierna ziet u de kenmerken die zijn toegevoegd aan de specificatie CSDL:

**d:Map** - uitgevoerd voor de service-uitvoer van een XPath-expressie. Aangenomen hier is dat de service-uitvoer een aantal elementen die, bevat herhalen zoals een ATOM-feed waar er is een verzameling vermelding knooppunten die herhalen. Elk van deze herhalende knooppunten bevat één record. Het XPath ten slotte ingesteld zodat deze verwijzen voor de afzonderlijke herhalende knooppunt in van de provider van de inhoud service resultaat dat de waarden voor een afzonderlijke record bevat. Voorbeeld van de service-uitvoer:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

XPath-expressie zou worden /foo/staaf omdat elk van de staaf knooppunt is het herhalende knooppunt in de uitvoer en bevat de feitelijke inhoud die naar de eindgebruiker wordt geretourneerd.

**Toets** - dit kenmerk wordt genegeerd door Marketplace. REST op basis van webservices, in het algemeen niet laten zien een primaire sleutel.


## <a name="property-node"></a>Eigenschapsknooppunt

Dit knooppunt bevat één eigenschap van de record.

Meer informatie over dit knooppunt zijn gevonden bij [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (Gebruik de vervolgkeuzelijst van de **Andere versie** naar een andere versie Selecteer zo nodig aan bekijkt u de documentatie.) *Voorbeeld:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| Kenmerknaam | Vereist | Waarde |
|----|----|----|
| Naam | Ja | De naam van de eigenschap. |
| Type | Ja | Het type van de eigenschapswaarde. Het waardetype eigenschap moet een **EDMSimpleType** of een complex type (aangegeven met een volledig gekwalificeerde naam) die binnen bereik van het model zijn. Zie conceptueel Model typen (CSDL) voor meer informatie. |
| Nullable | Nee | **Waar** (dit is de standaardwaarde) of **False** afhankelijk van of de eigenschap een null-waarde kan hebben. Opmerking: In de versie van CSDL aangegeven door de naamruimte [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) , een complexe eigenschap moet bevatten Nullable = "False". |
| Standaardwaarde | Nee | De standaardwaarde van de eigenschap. |
|MaxLength | Nee | De maximale lengte van de eigenschapswaarde. |
| FixedLength | Nee | **Waar** of **Onwaar** afhankelijk van of de waarde van de eigenschap wordt opgeslagen als een tekenreeks met lengte fiexed. |
| Precisie van formules | Nee | Verwijst naar het maximum aantal cijfers moeten worden bewaard in de numerieke waarde. |
| Schaal | Nee | Maximum aantal cijfers achter de komma moeten worden bewaard in de numerieke waarde. |
| Unicode | Nee | **Waar** of **Onwaar** afhankelijk van of de waarde van de eigenschap worden opgeslagen als een Unicode-tekenreeks. |
| Sortering | Nee | Een tekenreeks die de sorteervolgorde moet worden gebruikt in de gegevensbron. |
| ConcurrencyMode | Nee | **Geen** (dit is de standaardwaarde) of **vast**. Als de waarde is ingesteld op **vast**, wordt de waarde van de eigenschap worden gebruikt in optimistische gelijktijdigheid controles. |

Hier volgen de extra kenmerken die zijn toegevoegd aan de specificatie CSDL:

**d:Map** - XPath-expressie die zijn uitgevoerd voor de service uitgevoerd en haalt één eigenschap van de uitvoer. Het opgegeven XPath is ten opzichte van het herhalende knooppunt dat is geselecteerd in van het knooppunt EntityType XPath. Het is ook mogelijk om op te geven van een absolute XPath zodat een statische resource op te nemen in elk van de knooppunten uitvoer, zoals bijvoorbeeld een copyright-instructie slechts één keer in de oorspronkelijke service gevonden is uitvoer maar aanwezig zijn in elk van de rijen in de OData-uitvoer moet zijn. Voorbeeld van de service:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

XPath-expressie hier zou ./bar/baz0 het knooppunt baz0 ophalen van de inhoud van de provider-service.

**d:CharMaxLength** - voor het tekenreekstype, kunt u de maximale lengte. Zie DataService CSDL voorbeeld

**d:IsPrimaryKey** - geeft aan of de kolom de primaire sleutel in de tabel/weergave. Zie DataService CSDL voorbeeld.

**d:isExposed** - bepaalt als het tabelschema worden weergegeven (doorgaans true). Zie DataService CSDL voorbeeld

**d:IsView** *(Optioneel)* - waar als dit is gebaseerd op een weergave in plaats van een tabel.  Zie DataService CSDL voorbeeld

**d:Tableschema** - Zie DataService CSDL voorbeeld

**d:ColumnName** - Is de naam van de kolom in de tabelweergave.  Zie DataService CSDL voorbeeld

**d:IsReturned** - Is de Booleaanse waarde die bepaalt als de Service deze waarde naar de klant beschrijft.  Zie DataService CSDL voorbeeld

**d:IsQueryable** - Is de Booleaanse waarde die bepaalt als de kolom in een databasequery's kan worden gebruikt.   Zie DataService CSDL voorbeeld

**d:OrdinalPosition** - Is van de kolom numerieke positie van vormgeving x, in de tabel of de weergave, waarbij x afkomstig is van 1 voor het aantal kolommen in de tabel.  Zie DataService CSDL voorbeeld

**d:DatabaseDataType** - Is het gegevenstype van de kolom in de database, dat wil zeggen SQL-gegevenstype. Zie DataService CSDL voorbeeld

## <a name="supported-parametersproperty-types"></a>Typen ondersteunde Parameters/eigenschappen
Hier volgen de ondersteunde typen voor parameters en eigenschappen. (Hoofdlettergevoelig)

| Primitieve typen | Beschrijving |
|----|----|
| Null-waarden | Gebrek aan een waarde aangeeft |
| Booleaanse waarde | Hiermee geeft u de wiskundige concept van de binaire waarde logica|
| Byte | Niet-ondertekende 8 bits geheel getal|
|Datum /| Datum en tijd vertegenwoordigt met waarden die variëren van 12:00:00 uur januari 1, 1753 N.C. tot en met 11:59:59 uur, December 9999 N.C.|
|Decimaal | Hiermee geeft u numerieke waarden met een vaste precisie en schaal. Dit type kan worden beschreven een numerieke waarde die variëren van negatieve 10 ^ 255 + 1 tot en met positieve 10 ^ 255 -1|
| Dubbeltik | Een getal met een precisie van 15 cijfers die in waarden met niet-geheel exacte bereik van 2.23E +-308 tot en met + 1.79E resulteert zwevende punt voorstelt +308. **Decimaal getal vanwege Exel exporteren probleem gebruiken**|
| Eén | Een getal met een precisie van 7 cijfers die in waarden met niet-geheel exacte bereik van + 1.18E-38 tot en met + 3.40E resulteert zwevende punt voorstelt +38|
|GUID |Hiermee geeft u de waarde van een unieke id van 16 bytes (128-bits) |
|Int16|Hiermee geeft u een ondertekend 16-bits geheel getal |
|Int32|Hiermee geeft u een ondertekend 32-bits geheel getal |
|Int64|Hiermee geeft u een ondertekend 64-bits geheel getal |
|Tekenreeks | Hiermee geeft u vaste of variabele-lengte tekengegevens |


## <a name="see-also"></a>Zie ook
- Als u de algehele OData toewijzingsproces en doel begrijpen geïnteresseerd bent, leest u in dit artikel [Gegevens Service OData toewijzing](marketplace-publishing-data-service-creation-odata-mapping.md) moet worden gereviseerd definities, structuren en instructies.
- Als u wilt beoordelen voorbeelden, Lees dit artikel [Gegevens Service OData toewijzing voorbeelden](marketplace-publishing-data-service-creation-odata-mapping-examples.md) om te zien van de steekproef code en meer informatie over de codesyntaxis van de en context.
- Lees dit artikel [Data Service publicerende Guide](marketplace-publishing-data-service-creation.md)om terug te keren naar het vooraf aangegeven pad voor het publiceren van een Service van gegevens met de Azure Marketplace.
