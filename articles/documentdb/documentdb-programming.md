<properties 
    pageTitle="DocumentDB hoeft te programmeren: opgeslagen procedures, databasetriggers en UDF's | Microsoft Azure" 
    description="Informatie over het gebruik van DocumentDB om te schrijven opgeslagen procedures, databasetriggers en door de gebruiker gedefinieerde functies (UDF's) in JavaScript. Krijgen database programing tips en nog veel meer." 
    keywords="Database-triggers, opgeslagen procedure, opgeslagen procedure, databaseprogramma, sproc, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>DocumentDB serverzijde programmeren: opgeslagen procedures, databasetriggers en UDF's

Informatie over hoe de taal van Azure DocumentDB geïntegreerd, transacties uitvoering van JavaScript kan ontwikkelaars schrijven **opgeslagen procedures**, **triggers** en **door gebruiker gedefinieerde functies (UDF)** native in JavaScript. Hiermee kunt u database programma toepassingslogica die kan worden verzonden en uitgevoerd rechtstreeks op de database opslag partities schrijven 

Het is raadzaam om aan de slag door de volgende video, waar Andrew Liu vindt u een korte inleiding tot de DocumentDB serverzijde programming databasemodel te bekijken. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Vervolgens terug naar dit artikel, waar leert u de antwoorden op de volgende vragen:  

- Hoe schrijf ik een een opgeslagen procedure, trigger of UDF JavaScript gebruiken?
- Hoe garandeert DocumentDB aan?
- Hoe werken transacties in DocumentDB?
- Wat zijn vooraf activeert en na de gebeurtenis en hoe schrijf ik een?
- Hoe ik registreren en uitvoeren van een opgeslagen procedure, trigger of UDF RESTful zodanig via HTTP?
- Wat DocumentDB SDK's zijn beschikbaar voor het maken en uitvoeren die zijn opgeslagen procedures, triggers en UDF's?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Inleiding tot de opgeslagen Procedure en UDF hoeft te programmeren

Deze methode van *"JavaScript als moderne dag T-SQL"* maakt softwareontwikkelaars uit de complexiteit van niet-overeenkomende typen systeem en technologieën van object-relationele toewijzing. Er wordt ook een aantal ingebouwde voordelen die kan worden gebruikt om te maken van uitgebreide toepassingen:  

-   **Procedurele logica:** JavaScript als een hoog niveau programmeertaal, biedt een uitgebreide en vertrouwde interface bedrijfslogica Express. U kunt complexe reeksen bewerkingen dichter naar de gegevens uitvoeren.

-   **Atomische transacties:** DocumentDB zorgt ervoor dat databasebewerkingen uitgevoerd binnen één opgeslagen procedure of trigger atomaire zijn. Hiermee kunt een toepassing gerelateerde bewerkingen in één batch combineren zodat ze allemaal slagen of geen van deze mislukt. 

-   **Prestaties:** Het feit dat de JSON intrinsiek is toegewezen aan het systeem Javascript taal type en ook de basiseenheid van opslagruimte in DocumentDB is kunt voor een aantal optimalisaties toe zoals fikse intreding van JSON-documenten in de buffergroep en aanbrengt ze op aanvraag is beschikbaar in het uitvoeren van de code. Er zijn meer prestatievoordelen die is gekoppeld aan verzending bedrijfslogica in de database:
    -   Batchen – ontwikkelaars kunnen bewerkingen, zoals wordt ingevoegd groeperen en deze verzenden in bulk. Het netwerkverkeer latentie van kosten en de realiseren store om te maken van afzonderlijke transacties zijn aanzienlijk verlaagd. 
    -   Oude gecompileerd – DocumentDB precompiles opgeslagen procedures, triggers en de gebruiker gedefinieerde functies (UDF) om te voorkomen dat JavaScript gecompileerd kosten voor elke aanroep. De belasting van het samenstellen van de code bytes voor de procedurele logica is afgelost op een minimale waarde.
    -   Volgordebepaling – dat u een groot aantal bewerkingen gevolg ("trigger") die mogelijk het uitvoeren van een of meer secundaire store bewerkingen. Afgezien van atomiciteit is dit meer zodat wanneer verplaatst naar de server. 
-   **Inkapseling:** Opgeslagen procedures kunnen worden gebruikt om te groeperen bedrijfslogica op één plaats. Dit is het verstandig:
    -   Een laag abstraction boven aan de onbewerkte gegevens, waarmee gegevens architecten ontwikkelen van hun toepassingen onafhankelijk van de gegevens worden toegevoegd. Dit is met name nuttig wanneer u de gegevens die zich schema-minder, vanwege de brokkelig hypothesen die u in de toepassing worden warm moet mogelijk als zij hebben tot gegevens rechtstreeks verwerken.  
    -   Deze abstraction kunt ondernemingen hun gegevens veilig te houden door te stroomlijnen de toegang via de scripts.  

Het maken en de uitvoering van de databasetriggers, opgeslagen procedure en aangepaste query's wordt ondersteund door de [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx), [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)en [client SDK's](documentdb-sdk-dotnet.md) in veel platforms inclusief .NET, Node.js en JavaScript.

**Dat in deze zelfstudie wordt de [Node.js SDK met Q](http://azure.github.io/azure-documentdb-node-q/) ** om te laten zien van de syntaxis en het gebruik van opgeslagen procedures, triggers en UDF's.   

## <a name="stored-procedures"></a>Opgeslagen procedures

### <a name="example-write-a-simple-stored-procedure"></a>Voorbeeld: Een eenvoudige opgeslagen procedure schrijven 
Laten we beginnen met een eenvoudige opgeslagen procedure waarmee een reactie "Hallo wereld" wordt geretourneerd.

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Opgeslagen procedures per siteverzameling zijn geregistreerd en kunnen worden uitgevoerd op een document en de bijlage die aanwezig zijn in die verzameling. Het codefragment van de volgende ziet hoe u de procedure Hallo wereld die zijn opgeslagen met een verzameling registreren. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Zodra de opgeslagen procedure is geregistreerd, kunnen we uitgevoerd op de verzameling en de resultaten weer op de client gelezen. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Het contextobject biedt toegang tot alle bewerkingen die kan worden uitgevoerd op DocumentDB opslag, maar ook toegang tot de objecten vergaderverzoeken en antwoorden. In dit geval we het antwoordobject gebruikt voor het instellen van de hoofdtekst van het antwoord dat is verzonden naar de client. Raadpleeg de [DocumentDB JavaScript-server SDK-documentatie](http://azure.github.io/azure-documentdb-js-server/)voor meer informatie.  

Laat ons in dit voorbeeld uitvouwen en meer gerelateerde functionaliteit aan de opgeslagen procedure database toevoegen. Opgeslagen procedures kunnen maken, bijwerken, lezen, query en verwijderen van documenten en bijlagen binnen de verzameling.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Voorbeeld: Schrijven een opgeslagen procedure om een document te maken 
Het codefragment van de volgende ziet hoe u het contextobject gebruiken om te communiceren met DocumentDB resources.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Deze opgeslagen procedure neemt als invoer documentToCreate, de hoofdtekst van een document moet worden gemaakt in de huidige verzameling. Alle deze bewerkingen zijn asynchroon en JavaScript functie terugbellen afhankelijk. De functie terugbellen heeft twee parameters, één voor het foutobject in geval die de bewerking is mislukt en één voor het gemaakte object. Binnen het terugbellen, kunnen gebruikers de uitzondering verwerken of een fout. Wanneer een terugbellen niet wordt opgegeven en er een fout treedt, genereert de runtime DocumentDB een fout.   

De terugbellen genereert een fout in het bovenstaande voorbeeld als u de bewerking is mislukt. Anders wordt de id van het gemaakte document ingesteld als de hoofdtekst van het antwoord op de client. Hier ziet u hoe u deze opgeslagen procedure uitvoeren met invoerparameters weergegeven.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Houd er rekening mee dat deze opgeslagen procedure kan worden gewijzigd voor het uitvoeren van een matrix van document instanties als invoer en maakt deze allemaal op dezelfde opgeslagen procedure uitvoering in plaats van meerdere netwerkaanvragen voor elk van deze afzonderlijk maken. Dit kan worden gebruikt voor de uitvoering van een efficiënte bulksgewijs importeur voor DocumentDB (Zie verderop in deze zelfstudie).   

Het beschreven voorbeeld gedemonstreerd hoe opgeslagen procedures te gebruiken. Aan bod komen triggers en de gebruiker gedefinieerde functies (UDF) verderop in deze zelfstudie.

## <a name="database-program-transactions"></a>Programma databasetransacties
Transactie in een normale database kan worden gedefinieerd als een reeks bewerkingen die zijn uitgevoerd als een logische eenheid van werk. Elke transactie biedt **aan garandeert**. EN is een bekende acroniem dat voor vier eigenschappen - atomiciteit, consistentie, moeten worden geïsoleerd en levensduur staat.  

Kortom, atomiciteit zorgt ervoor dat alle het werk gedaan binnen een transactie wordt behandeld als één eenheid waar beide alles is erop gebrand of geen. Consistentie zorgt ervoor dat de gegevens zich altijd in een goede interne staat over transacties. Moeten worden geïsoleerd zorgt ervoor dat geen twee transacties storen elkaar – over het algemeen, de meeste commerciële systemen bieden meerdere moeten worden geïsoleerd niveaus die kunnen worden gebruikt op basis van de behoeften van toepassing. Levensduur zorgt ervoor dat er een wijziging die doorgevoerd in de database wordt altijd aanwezig zijn.   

In DocumentDB, wordt JavaScript gehost in dezelfde geheugenruimte als de database. Daarom aanvragen gedaan binnen opgeslagen procedures en triggers uitvoeren in hetzelfde bereik van een databasesessie. Hiermee worden DocumentDB om te garanderen aan voor alle bewerkingen die deel uitmaken van één opgeslagen procedure/trigger. Houd rekening met de volgende definitie van de opgeslagen procedure:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Deze opgeslagen procedure transacties binnen een app spellen tot trade items tussen twee spelers in één bewerking gebruikt. De opgeslagen procedure probeert te lezen van twee documenten als argument in elke overeenkomt met de speler-id's doorgegeven. Beide documenten speler worden gevonden, klikt u vervolgens de opgeslagen procedure wordt bijgewerkt als de documenten door hun items verwisselen. Als er fouten zijn aangetroffen daarbij, genereert deze een JavaScript-uitzondering die impliciet afbreken van de transactie.

Als de verzameling de opgeslagen procedure is geregistreerd ten opzichte van een siteverzameling één partition en dan de transactie is beperkt tot alle docuemnts binnen de verzameling. Als de verzameling is partitioneren, worden opgeslagen procedures in het transactiebereik van een partitiesleutel één uitgevoerd. Elk opgeslagen procedure execution moet een partition-sleutelwaarde die overeenkomt met het bereik dat de transactie moet worden uitgevoerd onder vervolgens bevatten. Zie [DocumentDB partitioneren](documentdb-partition-data.md)voor meer informatie.

### <a name="commit-and-rollback"></a>Doorvoeren en ongedaan maken
Transacties zijn nauw en native geïntegreerd in de DocumentDB JavaScript programming model. Binnen een JavaScript-functie, worden alle bewerkingen automatisch samengevoegd met onder één transactie. Als de JavaScript is voltooid zonder een uitzondering, zijn de bewerkingen in de database vastgelegde. De instructies 'Transactie BEGINT' en "Transactie doorvoeren" in relationele databases zijn in feite impliciet in DocumentDB.  
 
Als er een uitzondering die uit het script wordt doorgegeven, wordt van DocumentDB JavaScript runtime hersteld de gehele transactie. Zoals u in het eerdere voorbeeld, uitzondering opgetreden effectief equivalent is aan een 'TERUGDRAAIEN transactie"in DocumentDB.
 
### <a name="data-consistency"></a>Consistentie van de gegevens
Opgeslagen procedures en triggers worden altijd uitgevoerd op de primaire replica van de verzameling DocumentDB. Dit zorgt ervoor dat leest uit binnen procedures aanbieding sterke consistentie opgeslagen. Query's met de gebruiker gedefinieerde functies kunnen worden uitgevoerd op de primaire of met een secundaire replica, maar we ervoor zorgen dat het niveau van de gevraagde consistentie te voldoen aan de juiste replica te kiezen.

## <a name="bounded-execution"></a>Gebonden worden uitgevoerd
Alle DocumentDB bewerkingen moeten uitvoeren binnen de opgegeven server duur time-out aanvragen. Deze beperking geldt ook voor JavaScript-functies (opgeslagen procedures, triggers en de gebruiker gedefinieerde functies). Als u een bewerking niet wordt voltooid met deze termijn, de transactie hersteld. JavaScript-functies moeten voltooien binnen de termijn of een model voortgezet op basis om te worden uitgevoerd batch/cv implementeren.  

Alle functies van onder het object siteverzameling (voor maken, lezen, vervangen en verwijderen van documenten en bijlagen) retourneren om te vereenvoudigen ontwikkeling van opgeslagen procedures en triggers worden afgehandeld tijdslimieten, een Booleaanse waarde die aangeeft of deze bewerking wordt voltooid. Als deze waarde ONWAAR is, wordt deze aangegeven dat de termijn bijna is verlopen en dat de procedure omhoog kan worden uitgevoerd teruglopen moet.  Bewerkingen in de wachtrij vóór de eerste niet-geaccepteerde store bewerking voltooid als de opgeslagen procedure is in tijd zijn voltooid en niet meer verzoeken wachtrij zijn gegarandeerd.  

JavaScript-functies zijn ook beperkt tot op verbruik. DocumentDB behoudt doorvoer per siteverzameling op basis van de ingerichte grootte van een databaseaccount. Doorvoer wordt uitgedrukt in een genormaliseerde maateenheid CPU, geheugen en IO verbruik verzoek eenheden of RUs genoemd. JavaScript-functies kunnen gebruiken van een groot aantal RUs binnen de kortste en kunnen worden weergegeven tarief beperkte als de limiet van de siteverzameling is bereikt. Mogelijk ook in quarantaine geplaatst resource intensief opgeslagen procedures om ervoor te zorgen beschikbaarheid van primitieve databasebewerkingen.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Voorbeeld: Bulksgewijs importeren van gegevens in een databaseprogramma
Hieronder volgt een voorbeeld van een opgeslagen procedure die is geschreven naar bulksgewijs-invoer documenten in een siteverzameling. Houd er rekening mee hoe de opgeslagen procedure gebonden execution verwerkt door te schakelen van de Booleaanse waarde retourneren op basis van createDocument, en gebruikt u vervolgens het aantal documenten die zijn ingevoegd in elke aanroep van de opgeslagen procedure voor het bijhouden en de voortgang in batches hervatten.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Databasetriggers
### <a name="database-pre-triggers"></a>Oude databasetriggers
DocumentDB biedt triggers die zijn uitgevoerd of geactiveerd door een bewerking op een document. Wanneer u een document maakt – deze oude trigger, worden uitgevoerd voordat het document is gemaakt, kunt u bijvoorbeeld een oude trigger opgeven. Hier volgt een voorbeeld van hoe oude triggers kunnen worden gebruikt voor het valideren van de eigenschappen van een document dat wordt gemaakt:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


En de bijbehorende Node.js aan de clientzijde registratiecode voor de trigger:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Oude triggers geen eventuele invoerparameters weergegeven. Het verzoekobject kan worden gebruikt voor het bewerken van het bericht dat is gekoppeld aan de bewerking. Hier de oude trigger wordt uitgevoerd met het maken van een document en de berichttekst verzoek om het document mag worden gemaakt in de indeling van JSON bevat.   

Wanneer triggers zijn geregistreerd, kunnen gebruikers de bewerkingen die deze afgespeeld met worden kan opgeven. Deze trigger is gemaakt met TriggerOperation.Create, wat betekent dat het volgende niet is toegestaan.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>-Na-databasetriggers
Post triggers, zoals oude triggers, zijn gekoppeld aan een bewerking op een document en eventuele invoerparameters weergegeven niet uitvoeren. Ze worden uitgevoerd **nadat** die de bewerking is voltooid, en toegang hebben tot het antwoordbericht die wordt verzonden naar de klant.   

Het volgende voorbeeld ziet post triggers in actie:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


De trigger kan worden geregistreerd, zoals wordt weergegeven in het onderstaande voorbeeld.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Deze trigger wordt gevraagd om het metagegevensdocument en bijwerkt met meer informatie over het gemaakte document.  

Wat die is belangrijk te weten is de **transacties** uitvoering van triggers in DocumentDB. Deze post trigger wordt uitgevoerd als onderdeel van de dezelfde transactie als het maken van het oorspronkelijke document. Als we een uitzondering genereren van de post trigger (zeg als kan niet worden verwerkt in het metagegevensdocument), de hele transactie, mislukt en worden hersteld. Geen document wordt gemaakt en een uitzondering wordt geretourneerd.  

##<a id="udf"></a>De gebruiker gedefinieerde functies
De gebruiker gedefinieerde functies (UDF's) worden gebruikt voor het uitbreiden van het DocumentDB SQL-query taal grammatica controleren en implementeren van aangepaste bedrijfslogica. Ze kunnen alleen worden aangeroepen vanuit in query's. Ze hebben geen toegang tot het contextobject en zijn bedoeld om te worden gebruikt als alleen-berekeningscluster JavaScript. Daarom kunnen UDF's worden uitgevoerd op secundaire kopieën van de service DocumentDB.  
 
In het onderstaande voorbeeld een UDF als u wilt berekenen op basis van tarieven voor verschillende inkomsten haken belasting heeft gemaakt en klik in een query gebruikt om te zoeken naar alle personen die meer dan 20.000 betaald over belastingen.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


De UDF kan vervolgens worden gebruikt in query's zoals in het onderstaande voorbeeld:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>JavaScript taal geïntegreerde query API
Naast uitgegeven query's met de DocumentDB SQL grammatica, kunt de serverzijde SDK u geoptimaliseerde query's met een fluent JavaScript-interface zonder dat u kennis van SQL kan uitvoeren. De JavaScript-query die API u query's via programmacode maken kunt door het doorgeven van predikaatfunctie functies in chainable functie u belt, met een syntaxis voor de matrix dient te worden en populaire JavaScript-bibliotheken zoals lodash van ECMAScript5. Query's worden afgeleid door de JavaScript-runtime moet worden uitgevoerd efficiënt gebruik maakt van DocumentDB indexen.

> [AZURE.NOTE]`__` (dubbel-onderstrepingsteken) is een alias voor `getContext().getCollection()`.
> <br/>
> Met andere woorden, kunt u `__` of `getContext().getCollection()` voor toegang tot de API voor JavaScript-query.

Ondersteunde functies zijn:
<ul>
<li>
<b>... chain(). waarde ([terugbellen] [, opties])</b>
<ul>
<li>
Hiermee start u een gekoppelde aanroep die moet worden beëindigd met value().
</li>
</ul>
</li>
<li>
<b>filter (predicateFunction [, opties] [, terugbellen])</b>
<ul>
<li>
De invoer met een predikaatfunctie functie die resulteert in waar/onwaar om te filteren in of uit invoer documenten in de resulterende set worden gefilterd. Dit lijkt veel lijkt op een WHERE-component in SQL.
</li>
</ul>
</li>
<li>
<b>toewijzing (transformationFunction [, opties] [, terugbellen])</b>
<ul>
<li>
Dit geldt een raming gegeven van een transformatiefunctie dat wordt elke invoer item aan een JavaScript-object of de waarde toegewezen. Dit lijkt veel lijkt op een SELECT-component in SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([naameigenschap] [, opties] [, terugbellen])</b>
<ul>
<li>
Dit is een snelkoppeling voor een map die de waarde van één eigenschap uit elke invoer item haalt.
</li>
</ul>
</li>
<li>
<b>samenvoegen ([isShallow] [, opties] [, terugbellen])</b>
<ul>
<li>
Combineert en effent matrices uit elke invoer item in een één matrix. Dit lijkt veel lijkt op SelectMany in LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predicate] [, opties] [, terugbellen])</b>
<ul>
<li>
Een nieuwe set van documenten door te sorteren van de documenten in de stream invoer document in oplopende volgorde met het opgegeven predikaat produceren. Dit lijkt veel lijkt op een ORDER BY-component in SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predicate] [, opties] [, terugbellen])</b>
<ul>
<li>
Een nieuwe set van documenten door te sorteren van de documenten in de stream invoer document in aflopende volgorde met het opgegeven predikaat produceren. Dit lijkt veel lijkt op een x DESC ORDER BY-component in SQL.
</li>
</ul>
</li>
</ul>


Als binnen predikaat en/of Gegevenskiezer functies opgenomen, krijgen automatisch de volgende JavaScript-constructies geoptimaliseerd om rechtstreeks op DocumentDB indexen te voeren:

* Eenvoudige operatoren: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Letterlijke waarden, met inbegrip van het object letterlijke: {}
* var, afzender

De volgende JavaScript-constructies doen niet ophalen geoptimaliseerd voor DocumentDB indexen:

* Stroom bepalen (bijvoorbeeld als voor, terwijl)
* Functie oproepen

Voor meer informatie raadpleegt u onze [Aan de clientzijde JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Voorbeeld: Een opgeslagen procedure met behulp van de query JavaScript API schrijven

Het volgende voorbeeld is een voorbeeld van hoe de JavaScript-API Query kan worden gebruikt in de context van een opgeslagen procedure. De opgeslagen procedure Hiermee voegt u een document, een invoerparameter gekregen en updates van een metagegevens document, met de `__.filter()` methode, met minimaal, maxSize en totalSize op basis van de eigenschap van de grootte van de invoer document.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL naar Javascript bedriegen blad
De volgende tabel vindt verschillende SQL-query's en de bijbehorende JavaScript-query's.

Als het document met SQL-query's, sleutels (bijvoorbeeld `doc.id`) zijn hoofdlettergevoelig.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>JavaScript-Query API</th>
<th>Meer informatie</th>
</tr>
<tr>
<td>
<pre>
Selecteer * uit documenten
</pre>
</td>
<td>
<pre>
__.map(Function(doc) {afzender doc;});
</pre>
</td>
<td>Resultaten in alle documenten (gepagineerd met voortgezet token) als is.</td>
</tr>
<tr>
<td>
<pre>
Selecteer docs.id, docs.message als msg, docs.actions van documenten
</pre>
</td>
<td>
<pre>
__.map(Function(doc) {retourneren {id: doc.id, msg: doc.message, acties: doc.actions};});
</pre>
</td>
<td>De id, bericht (alias naar msg) en actie uit alle documenten projecteert.</td>
</tr>
<tr>
<td>
<pre>
Selecteer * uit documenten WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.filter(Function(doc) {doc.id retourneren === "X998_Y998;"});
</pre>
</td>
<td>Query's voor documenten met het predikaat: id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
Selecteer * uit documenten waar ARRAY_CONTAINS(docs. Labels, 123)
</pre>
</td>
<td>
<pre>
__.filter(Function(x) {x.Tags terug & & x.Tags.indexOf(123) > -1;});
</pre>
</td>
<td>Query's voor documenten die u een eigenschap labels en Tags moet is een matrix met de waarde 123.</td>
</tr>
<tr>
<td>
<pre>
Selecteer docs.id, docs.message als msg van documenten WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {doc.id retourneren === "X998_Y998;"}) .map(function(doc) {retourneren {id: doc.id, msg: doc.message};}) .value();
</pre>
</td>
<td>Query's voor documenten met een predikaat, id = "X998_Y998" en klik vervolgens op de projecten de-id en het bericht (alias naar msg).</td>
</tr>
<tr>
<td>
<pre>
Selecteer waarde-code uit documenten JOIN tag IN documenten. Tags ORDER BY docs._ts
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {afzender doc. Labels & & Array.isArray (document. Tags). }) .sortBy(function(doc) {afzender doc._ts;}) .pluck("tags") .flatten() .value()
</pre>
</td>
<td>Filters voor documenten die de eigenschap van een matrix, Tags en de resulterende documenten sorteren op de eigenschap _ts tijdstempel system, en vervolgens projecten + effent de labels-matrix.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Runtime-ondersteuning
[DocumentDB JavaScript-serverzijde SDK](http://azure.github.io/azure-documentdb-js-server/) biedt ondersteuning voor optimaal gebruik van de algemene JavaScript-taalfuncties als gestandaardiseerde door [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Beveiliging
JavaScript opgeslagen procedures en triggers zijn sandbox zodat de effecten van een script niet naar de andere lekken doen zonder via de overgeslagen transactie op het databaseniveau van de. De runtimeomgevingen zijn gebundelde maar na elke uitvoeren van de context is verwijderd. Daarom zijn ze gegarandeerd veilige van onbedoelde nadelen van elkaar verschillen.

### <a name="pre-compilation"></a>Oude gecompileerd
Opgeslagen procedures, triggers en UDF's zijn om te voorkomen gecompileerd kosten op het moment van elke aanroep script impliciet gecompileerde naar de indeling van de code bytes. Dit zorgt ervoor dat aanroepen van opgeslagen procedures, snel en hebben een laag op het milieu.

## <a name="client-sdk-support"></a>Ondersteuning voor SDK-clients
Naast de client [Node.js](documentdb-sdk-node.md) ondersteunt DocumentDB [.NET](documentdb-sdk-dotnet.md), [Java](documentdb-sdk-java.md) [JavaScript](http://azure.github.io/azure-documentdb-js/)en [Python SDK's](documentdb-sdk-python.md). Opgeslagen procedures, triggers en UDF's kunnen worden gemaakt en uitgevoerd met een van deze SDK's ook. Het volgende voorbeeld wordt getoond hoe maken en uitvoeren van een opgeslagen procedure via de .NET-client. Houd er rekening mee hoe de typen .NET worden doorgegeven naar de opgeslagen procedure als JSON en voorlezen.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


In dit voorbeeld ziet u hoe u met de [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) oude trigger maken en een document maken met de trigger ingeschakeld. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


En het volgende voorbeeld ziet hoe u een door de gebruiker gedefinieerde functie (UDF) maken en deze gebruiken in een [DocumentDB SQL-query](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST API
Alle DocumentDB bewerkingen kunnen worden uitgevoerd op een wijze die RESTful. Opgeslagen procedures, triggers en de gebruiker gedefinieerde functies kunnen worden geregistreerd onder een siteverzameling met behulp van HTTP POST. Hier volgt een voorbeeld van het registreren van een opgeslagen procedure:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


De opgeslagen procedure is geregistreerd bij het uitvoeren van een POST-aanvraag ten opzichte van de URI dbs/testdb/colls/testColl/sprocs met de instantie met de opgeslagen procedure om te maken. Triggers en UDF's kunnen ook worden geregistreerd door een bericht tegen/triggers en /udfs respectievelijk.
Dit opgeslagen procedure kunnen en worden uitgevoerd door een bericht verzenden ten opzichte van de resource-koppeling:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Hier wordt de invoer voor de opgeslagen procedure doorgegeven in de hoofdtekst van het verzoek. Houd er rekening mee dat de invoer wordt doorgegeven als een matrix JSON van invoerparameters weergegeven. De opgeslagen procedure haalt de eerste invoer als een document dat is de hoofdtekst van een antwoord. Het antwoord dat we ontvangen is als volgt:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Triggers, in tegenstelling tot opgeslagen procedures, kunnen niet rechtstreeks worden uitgevoerd. In plaats daarvan worden uitgevoerd als onderdeel van een bewerking op een document. We kunt de triggers om uit te voeren met een verzoek om met HTTP-headers opgeven. Hierna volgt een verzoek om een document maken.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Hier is de oude trigger moet worden uitgevoerd met het verzoek opgegeven in de kop van x-ms-documentdb-pre-trigger-include. Eventuele post triggers zijn dienovereenkomstig, uitgedrukt in de x-ms-documentdb-post-trigger-include koptekst. Houd er rekening mee dat zowel oude en post triggers kunnen worden ingesteld voor een bepaalde aanvraag.

## <a name="sample-code"></a>Voorbeeld van een code

In ons [Github opslagplaats](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples)vindt u meer voorbeelden van de code van de serverzijde (inclusief [bulk-verwijderen](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)en [bijwerken](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)).

Wilt u uw prima opgeslagen procedure delen? Stuur ons een aanvraag voor een halen! 

## <a name="next-steps"></a>Volgende stappen

Nadat u hebt een of meer opgeslagen procedures, triggers en de gebruiker gedefinieerde functies die zijn gemaakt, kunt u ze laden en deze weergeven in de Azure-Portal met Script Verkenner. Zie de [weergave die is opgeslagen procedures, triggers, en de gebruiker gedefinieerde functies DocumentDB Script Verkenner](documentdb-view-scripts.md)voor meer informatie.

Ook kan nuttig voor u de volgende verwijzingen en bronnen in het pad voor meer informatie over DocumentDB serverzijde programmeren:

- [Azure DocumentDB SDK 's](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScript-ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript-JSON typesysteem](http://www.json.org/js.html) 
- [Veilige en draagbare Database uitbreiden](http://dl.acm.org/citation.cfm?id=276339) 
- [Service oriënteren Database-architectuur](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [De .NET Runtime in Microsoft SQL server hostingprovider](http://dl.acm.org/citation.cfm?id=1007669)
