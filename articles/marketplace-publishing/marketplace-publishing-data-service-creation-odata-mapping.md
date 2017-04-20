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

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Een bestaande webservice toewijzen aan OData tot en met CSDL

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding eventuele nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet ophalen goedgekeurd voor de aanbieding.** Als u een SaaS bedrijfstoepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u een IaaS-toepassingen hebt of ontwikkelaars-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

In dit artikel bevat een overzicht over het gebruik van een CSDL aan een bestaande service toewijzen aan een OData-compatibele service. Hierin wordt uitgelegd hoe de toewijzing-document (CSDL) maken dat de invoer verzoek van de client via een serviceoproep transformaties en de uitvoer (gegevens) terug naar de client via een compatibele OData-feed. Services voor de eindgebruikers beschrijft van Microsoft Azure Marketplace met behulp van de OData-protocol. Services die worden gebruikt door inhoudsproviders (gegevens eigenaren) worden weergegeven op tal van formulieren, zoals de REST, SOAP, enzovoort.

## <a name="what-is-a-csdl-and-its-structure"></a>Wat is een CSDL en de structuur?
Een CSDL (conceptuele Schema Definition Language) is een specificatie die bepalen op welke manier om te beschrijven webservice of databaseservice in algemene XML-bewoordingen met de Azure Marketplace.

Eenvoudige overzicht van de **aanvragen stroom:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

De **gegevensstroom** is in de tegenovergestelde richting:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Afbeelding 1** diagrammen hoe een client wilt opvragen van een provider van inhoud (uw service) door de Azure Marketplace te doorlopen.  De CSDL wordt gebruikt door de toewijzing/transformatie-component worden afgehandeld van de aanvraag en gegevens tussen de inhoud van de provider (s) en de client doorgeven.

*Afbeelding 1: Gedetailleerde doorlopen van vraagt client naar de provider van de inhoud via Azure Marketplace*

  ![tekenen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Achtergrondinformatie over Atom, Atom-publicatie en de OData-protocol waarop de Azure Marketplace-extensies samenstelt, moeten u controleren: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Fragment boven koppeling:     *"het doel van het gegevensbestand-protocol (hierna OData) is een protocol REST gebaseerde verstrekken voor stijl van een CRUD-bewerkingen (maken, lezen, bijwerken en verwijderen) op resources die worden aangeboden als gegevensservices. Een service"gegevens" is een eindpunt waarbij bevat gegevens die worden aangeboden uit een of meer "verzamelingen" elke met nul of meer 'vermeldingen', die bestaan uit met de naam-waardeparen getypt. OData wordt gepubliceerd door Microsoft onder standaarden OASIS (organisatie voor de ontwikkeling van gestructureerde informatie standaarden) zodat iedereen die wil op servers, klanten of hulpmiddelen voor zonder royalty's of beperkingen maken kunt."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Er zijn drie belangrijke gegevens die moeten worden gedefinieerd door de CSDL:

- Het **eindpunt** van de Service Provider het Web adres URI () van de Service
- De **gegevensparameters** die worden doorgegeven als invoer voor de Service-Provider de definities van de parameters die wordt verzonden naar de service van de Provider van de inhoud naar beneden af op het gegevenstype.
- **Schema** van de gegevens die worden geretourneerd met de Service voor het aanvragen van het schema van de gegevens die wordt geleverd door de service van de Provider van de inhoud, waaronder Container, verzamelingen/tabellen, variabelen/kolommen en gegevenstypen.

In het volgende diagram ziet u een overzicht van de stroom van waarbij de client de OData-instructie (aanroep van de provider van de inhoud webservice) invoert voor de resultaten/gegevens terug.

  ![tekenen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Stappen:

1. Client wordt aanvraag verzonden via Service-oproep voltooid met invoerparameters die zijn gedefinieerd in XML-bestand met de Azure Marketplace
2. CSDL wordt gebruikt voor het valideren van de Service-oproep.
    - De opgemaakt Service bellen wordt vervolgens verzonden naar de inhoud Providers-Service door de Azure Marketplace
3. De Webservice wordt uitgevoerd en de actie van de Http-bewerking tekstknooppunten wordt uitgevoerd (dat wil zeggen krijgt) de resultaatgegevens met Azure Marketplace waar de gevraagde gegevens (indien aanwezig) gegarandeerd in XML-indeling naar de klant is met behulp van de toewijzing die is gedefinieerd in de CSDL.
4. De Client wordt verzonden de gegevens (indien aanwezig) in de XML- of JSON-indeling

## <a name="definitions"></a>Definities

### <a name="odata-atom-pub"></a>OData ATOM-publicatie

Een uitbreiding van de ATOM-publicatie waarin elk item één rij met een resultaat vertegenwoordigt instellen. De inhoud deel van de invoer is verbeterd aanduiding van de waarden van de rij – als belangrijke waardeparen. Meer informatie hier wordt aangetroffen: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - conceptueel Schema Definition Language

Kunt u functies (SPROCs) definiëren en entiteiten die worden weergegeven via een database. Meer informatie hier vinden: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] Klik op de **andere versies** vervolgkeuzelijst en selecteer een versie als u het artikel niet ziet.

### <a name="edm---entry-data-model"></a>EDM - vermelding-gegevensmodel
- Overzicht: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Voorbeeld: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Gegevenstypen: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

De volgende ziet u de gedetailleerde van links naar rechts flow van waarbij de klant invoert voor de OData-instructie (aanroep van de provider van de inhoud webservice) om te krijgen van de resultaten/gegevens weer:

  ![tekenen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>Basisbeginselen van CSDL

Een CSDL (conceptuele Schema Definition Language) is een specificatie die bepalen op welke manier om te beschrijven webservice of databaseservice in algemene XML-bewoordingen met de Azure Marketplace. CSDL beschrijving van de kritieke stuks die **stelt u in het doorgeven van gegevens uit de gegevensbron met de Azure Marketplace.** De belangrijkste onderdelen worden hier beschreven:

- Interface voor informatie over alle functies van openbaar (FunctionImport knooppunt)
- Informatie over gegevenstypen voor alle bericht requests(input) en bericht responses(outputs) (EntityContainer/EntitySet/EntityType knooppunten)
- Informatie over het transportprotocol moeten binden gebruikt (kop knooppunt)
- Adresgegevens voor het zoeken naar de opgegeven service (BaseURI kenmerk)

Kortom, vertegenwoordigt de CSDL een contract platform - en taal-onafhankelijke tussen de service die persoon en de serviceprovider. De CSDL gebruikt, kan een client Zoek een webservice service/database en een van de openbaar beschikbare functies roepen.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Met een CSDL betrekking tot een Database of een siteverzameling
**De specificatie CSDL**

CSDL is XML-grammatica voor de beschrijving van een webservice. De specificatie zelf in 4 belangrijkste elementen is verdeeld: EntitySet, FunctionImport; Naamruimte en EntityType.

Om dit abstraction gemakkelijker te begrijpen kunt een CSDL koppelen aan een tabel.

Onthouden.

  CSDL vertegenwoordigt een contract platform - en taal-onafhankelijke tussen de **service die persoon** en de **serviceprovider**. CSDL gebruikt, een **client** kunt Zoek een **webservice service/database** en een van de openbaar roepen **functies.**

Voor een Data-Service kunnen de vier onderdelen van een CSDL in een Database, tabel, kolom en Store Procedure worden beschouwd.

Die betrekking hebben op deze als volgt voor een Service van gegevens:

- EntityContainer ~ = Database
- EntitySet ~ = tabel
- EntityType ~ = kolommen
- FunctionImport ~ = opgeslagen Procedure

**HTTP-woorden die zijn toegestaan**
- GET-berekent de waarden uit de database (geeft als resultaat van een siteverzameling)
- BERICHT – gebruikt voor het doorgeven van gegevens en optioneel retourwaarden uit de database (maken een nieuwe vermelding in de verzameling, retour-id-URI)
- VERWIJDEREN: Hiermee verwijdert u gegevens uit de DB (Hiermee verwijdert u een siteverzameling)
- Zet – tabel bijwerken met gegevens naar een DB (vervangen van een siteverzameling of een account maakt)

## <a name="metadatamapping-document"></a>Metagegevens/toewijzing Document

Het document metagegevens/toewijzing wordt gebruikt voor het toewijzen van een aanbieder bestaande webservices zodat deze kan worden weergegeven als een OData-webservice door het systeem Azure Marketplace. Dit is gebaseerd op CSDL en implementeert webservices weergegeven via Azure Marketplace op basis van een paar uitbreidingen voor CSDL voor de behoeften van de REST. Het selectievakje extensies zijn gevonden in de naamruimte [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) .

Een voorbeeld van de CSDL wordt gevolgd: (kopiëren en plakken het onderstaande voorbeeld CSDL in een XML-editor en wijzigen zodat deze overeenkomt met uw Service.  Plak in CSDL toewijzing onder DataService tabblad bij het maken van uw service in de [Portal voor publiceren Azure Marketplace](https://publish.windowsazure.com)).

**Termen:** De bepalingen van de CSDL betreffende de bepalingen van de [Portal voor publiceren](https://publish.windowsazure.com) UI (PPUI).
- Bieden 'Titel' in de PPUI zich verhoudt tot MyWebOffer
- Mijnbedrijf in de PPUI verband houden met de **Naam van Publisher weer te geven** in het [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI
- Uw API betrekking heeft op een webpagina of gegevensservice (een abonnement in de PPUI)

**Hiërarchie:** 
 een bedrijf (inhoudsprovider) eigenaar is van voorstellen waarvoor Plan(s), namelijk service(s), welke regel omhoog met een API.

### <a name="webservice-csdl-example"></a>Voorbeeld van de WebService CSDL

Maakt verbinding met een service die een eindpunt van de web-toepassing (zoals een C#-toepassing weergeeft)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] Als u meer voorbeelden van CSDL-webservice in het artikel [voorbeelden van het toewijzen van een bestaande webservice met OData tot en met CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md) weergeven

###<a name="dataservice-csdl-example"></a>Voorbeeld van DataService CSDL

Maakt verbinding met een service die weergeeft een databasetabel of -weergave als een eindpunt onder voorbeeld ziet u dat twee API's voor gegevens grondtal basis API CSDL (de beschikking over weergaven in plaats van tabellen).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Zie ook
- Als u geïnteresseerd bent in zal het leren van informatie over de specifieke knooppunten en parameters, Lees dit artikel [Gegevens Service OData toewijzing knooppunten](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) voor definities en uitleg, voorbeelden en gebruik van hoofdletters/kleine letters context.
- Als u wilt beoordelen voorbeelden, Lees dit artikel [Gegevens Service OData toewijzing voorbeelden](marketplace-publishing-data-service-creation-odata-mapping-examples.md) om te zien van de steekproef code en meer informatie over de codesyntaxis van de en context.
- Lees dit artikel [Data Service publicerende Guide](marketplace-publishing-data-service-creation.md)om terug te keren naar het vooraf aangegeven pad voor het publiceren van een Service van gegevens met de Azure Marketplace.
