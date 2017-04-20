<properties
 pageTitle="Handleiding voor ontwikkelaars - querytaal | Microsoft Azure"
 description="Azure IoT Hub handleiding voor ontwikkelaars - beschrijving van de querytaal gebruikt voor het ophalen van informatie over twins, methoden en taken van uw hub IoT"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Referentie - querytaal voor twins en taken

## <a name="overview"></a>Overzicht

IoT Hub biedt een krachtige SQL-achtige taal om op te halen informatie aangaande [apparaat twins] [ lnk-twins] en [taken][lnk-jobs]. In dit artikel bevat:

* Een inleiding tot de belangrijkste functies van de querytaal IoT-Hub, en
* De gedetailleerde beschrijving van de taal.

## <a name="getting-started-with-twin-queries"></a>Aan de slag met dubbele query 's

[Apparaat twins] [ lnk-twins] willekeurige JSON-objecten als zowel labels en eigenschappen kan bevatten. IoT Hub kunt query apparaat twins als één JSON document met alle dubbele gegevens.
Stel, bijvoorbeeld dat uw IoT hub twins de volgende structuur hebben:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

IoT Hub stelt de twins apparaat als een documentverzameling **apparaten**genoemd.
De volgende query haalt zodat de hele reeks apparaat twins:

        SELECT * FROM devices

> [AZURE.NOTE] [IoT Hub SDK's] [ lnk-hub-sdks] ondersteuning voor paginering met grote resultaten.

Hub voor IoT beschikken om op te halen twins filteren met willekeurige voorwaarden. Bijvoorbeeld:

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

haalt de twins met het label **location.region** is ingesteld op **ons**.
Booleaans operatoren en rekenkundige vergelijkingen worden ondersteund, bijvoorbeeld

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

Hiermee haalt u alle twins zich bevindt in de VS die is geconfigureerd voor het telemetrielogboek minder vaak dan elke minuut verzenden. Gemakkelijker te maken is het ook mogelijk om te matrixconstanten gebruiken in combinatie met de **IN** - en operatoren **NIN** (niet in). Bijvoorbeeld:

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

Hiermee haalt u alle twins die gerapporteerd wifi of wired connectivity. Verwijzen naar de [WHERE-component] [ lnk-query-where] sectie voor de volledige verwijzing van het filtermogelijkheden.

Groeperen en aggregaties worden ook ondersteund. Bijvoorbeeld:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Geeft als resultaat het aantal van de apparaten in elke status van de configuratie telemetrielogboek.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

Met het bovenstaande voorbeeld ziet u een situatie waar drie apparaten gerapporteerd geslaagde configuratie, twee nog steeds de configuratie toepast en een een foutmelding.

### <a name="c-example"></a>C#-voorbeeld

De queryfunctionaliteit worden weergegeven door de [C#-service SDK] [ lnk-hub-sdks] in de de klas **RegistryManager** .
Hier volgt een voorbeeld van een eenvoudige query:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Houd er rekening mee hoe de **query** -object wordt gestart met een paginaformaat (maximaal 1000) en klik vervolgens meerdere pagina's kunnen worden opgehaald door de ondersteuning voor de methoden **GetNextAsTwinAsync** meerdere keren.
Het is belangrijk te weten dat het queryobject veelvoud bevat **volgende\***, afhankelijk van de optie deserialisatie is vereist door de query, dat wil zeggen twin of taak objecten of gewone Json moet worden gebruikt wanneer u een uitstekende delen gebruikt.

### <a name="node-example"></a>Voorbeeld knooppunten

De queryfunctionaliteit worden weergegeven door de [service knooppunt SDK] [ lnk-hub-sdks] in de het object **register** .
Hier volgt een voorbeeld van een eenvoudige query:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Houd er rekening mee hoe de **query** -object wordt gestart met een paginaformaat (maximaal 1000) en klik vervolgens meerdere pagina's kunnen worden opgehaald door de ondersteuning voor de methoden **nextAsTwin** meerdere keren.
Het is belangrijk te weten dat het queryobject veelvoud bevat **volgende\***, afhankelijk van de optie deserialisatie is vereist door de query, dat wil zeggen twin of taak objecten of gewone Json moet worden gebruikt wanneer u een uitstekende delen gebruikt.

### <a name="limitations"></a>Beperkingen

Op dit moment prognoses worden alleen ondersteund als gebruik aggregaties, dat wil zeggen niet is samengeteld query's alleen de beschikking over `SELECT *`. Aggregatie worden ook alleen ondersteund in combinatie met groeperen.

## <a name="getting-started-with-jobs-queries"></a>Aan de slag met taken query 's

[Taken] [ lnk-jobs] zelf een formule uitvoeren op sets met apparaten. Elk apparaat dubbele bevat de gegevens van de taken van de plaats waar deze in een verzameling **taken**genoemd hoort.
Logisch,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Deze verzameling is momenteel queriable als **devices.jobs** in de querytaal IoT Hub.

U kunt bijvoorbeeld de volgende query gebruiken als u alle taken (afgelopen en geplande) die betrekking hebben op één apparaat:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Houd er rekening mee hoe deze query biedt de status van het apparaat-specifieke (en eventueel ook het antwoord directe methode) van elke taak als resultaat gegeven.
Het is ook mogelijk om te filteren met willekeurige Booleaanse voorwaarden op alle eigenschappen van de objecten in de verzameling **devices.jobs** .
Bijvoorbeeld de volgende query:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

Hiermee haalt u alle voltooide dubbele update taken voor apparaat **myDeviceId** die na September 2016 zijn gemaakt.

Het is ook mogelijk om op te halen van de resultaten per apparaat van één taak.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Beperkingen
Op dit moment ondersteund query's op **devices.jobs** niet:

* Prognoses, dat wil zeggen alleen `SELECT *` mogelijk is;
* Voorwaarden die naar de dubbele apparaat naast taakeigenschappen verwijzen zoals hierboven;
* Peforming aggregaties, bijvoorbeeld aantal, gemiddelde, groeperen op.

## <a name="basics-of-an-iot-hub-query"></a>Basisbeginselen van een query IoT Hub

Elke query IoT Hub bestaat van een selecteren en van componenten en door optionele WHERE en GROUP BY componenten. Elke query wordt uitgevoerd op een verzameling JSON-documenten, bijvoorbeeld apparaat twins. De FROM-component wordt aangegeven dat de documentenverzameling om te worden herhaald in (**apparaten** of **devices.jobs**). Kies vervolgens wordt het filter in de WHERE-component toegepast. In het geval van aggregaties, de resultaten van deze stap zijn gegroepeerd als opgegeven in de GROUP BY-component en, voor elke groep, een rij wordt gegenereerd zoals is opgegeven in de SELECT-component.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>FROM-component

De component **uit < from_specification >** kunt wordt ervan uitgegaan dat slechts twee waarden: **FROM apparaten**om over te query apparaat twins of **FROM devices.jobs**naar Querydetails taak per apparaat.

## <a name="where-clause"></a>WHERE-component

De **waar < filter_condition >** component is optioneel. Hiermee geeft u de voorwaarden die u dat de JSON-documenten in de FROM-verzameling voldoen moeten om te worden opgenomen als onderdeel van het resultaat. Elk document JSON moet als resultaat de opgegeven voorwaarden op "true" in het resultaat moet worden opgenomen.

De toegestane voorwaarden worden beschreven in de sectie [expressies en voorwaarden][lnk-query-expressions].

## <a name="select-clause"></a>SELECT-component

De SELECT-component (**< select_list > Selecteer**) is verplicht en geeft aan welke waarden worden opgehaald door de query. Hiermee geeft u de JSON-waarden worden gebruikt voor het nieuwe JSON objecten genereren voor elk element van de gefilterde (en desgewenst gegroepeerde) subset van de siteverzameling van de fase raming genereert een nieuw JSON-object, gemaakt met de waarden die zijn opgegeven in de SELECT-component.

Dit is de grammatica van de SELECT-component:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

waarbij **attribute_name** verwijst naar een eigenschap van het document JSON in de FROM-verzameling. Enkele voorbeelden van SELECT-component vindt u in de [aan de slag met dubbele query's] [ lnk-query-getstarted] sectie.

Op dit moment selectie componenten verschilt **Selecteer \* ** alleen worden ondersteund in de samenvoegquery op twins.

## <a name="group-by-clause"></a>GROUP BY-component

De component **GROUP BY < group_specification >** is een optionele stap die kan worden uitgevoerd nadat het filter dat is opgegeven in de WHERE-component en vóór de opgegeven in de Selecteer raming. Deze documenten op basis van de waarde van een kenmerk van groepen. Deze groepen worden gebruikt om samengevoegde waarden zoals opgegeven in de SELECT-component te genereren.

Een voorbeeld van een query maken met de GROUP BY is:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

De syntaxis van de formele voor GROEPEREN op luidt als volgt:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

waarbij **attribute_name** verwijst naar een eigenschap van het document JSON in de FROM-verzameling. 

De GROUP BY-component is momenteel alleen ondersteund als query's uitvoeren twins.

## <a name="expressions-and-conditions"></a>Expressies en voorwaarden

Op een hoog niveau, een *expressie*:

* Resulteert in een exemplaar van een type JSON (dat wil zeggen Booleaanse waarde, getal, tekenreeks, matrix of object), en
* Is gedefinieerd door het bewerken van gegevens die afkomstig zijn uit het apparaat JSON document en -constanten met ingebouwde operatoren en functies.

*Voorwaarden* zijn expressies die resulteert in een Booleaanse waarde, dat wil zeggen elke constante anders dan Booleaanse **waarde true** wordt beschouwd als **Onwaar** (inclusief **null**, **ongedefinieerd**elk exemplaar van het object of een matrix, een willekeurige tekenreeks en duidelijk de Booleaanse **Onwaar**).

De syntaxis voor expressies is:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

waarbij geldt:

| Symbool | Definitie |
| -------------- | -----------------|
| attribute_name | Een eigenschap van het document JSON in de FROM-verzameling. |
| unary_operator | Een unaire operator aan de hand van de sectie operatoren. |
| binary_operator | Een binaire operator aan de hand van de sectie operatoren. |
| decimal_literal | Een toegestane achterstand uitgedrukt in decimale notatie. |
| hexadecimal_literal | Een getal uitgedrukt door de tekenreeks '0 x' gevolgd door een reeks hexadecimale cijfers. |
| string_literal | Letterlijke tekenreeksen zijn Unicode-tekenreeksen dat wordt aangeduid met een opeenvolging van nul of meer Unicode-tekens of escape-reeksen. Letterlijke tekenreeksen zijn tussen enkele aanhalingstekens staan (apostrof: ') of dubbele aanhalingstekens (aanhalingsteken: "). Hiermee heft u toegestaan: `\'`, `\"`, `\\`, `\uXXXX` voor Unicode-tekens die zijn gedefinieerd door 4 hexadecimale cijfers. |

### <a name="operators"></a>Operatoren

De volgende operators voorwaarden wordt voldaan:

| Familie | Operatoren |
| -------------- | -----------------|
| Rekenkundig | +,-,*,/,% |
| Logische | EN, OF NIET |
| Vergelijking |  =,! =, <>,, <, > =, <> |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het uitvoeren van query's in uw-apps met behulp van [IoT Hub SDK's][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md