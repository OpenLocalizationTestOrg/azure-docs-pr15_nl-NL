<properties
    pageTitle="Offline synchroniseren inschakelen voor uw Azure Mobile-App (iOS)"
    description="Informatie over het gebruik van App-Service Mobile-Apps naar offline gegevens in uw iOS-toepassing cache en synchroniseren"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Offline synchronisatie voor de mobiele app van iOS inschakelen

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie behandelt de functie offlinesynchronisatie van Azure Mobile-Apps voor iOS. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding. Wijzigingen worden opgeslagen in een lokale database. Als u het apparaat weer online hebt, worden deze wijzigingen worden gesynchroniseerd met de externe-end.

Als dit uw eerste ervaring met Azure Mobile-Apps, moet u eerst de zelfstudie [maken van een iOS-App]uitvoeren. Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u de gegevens access extensie-pakketten toevoegen aan uw project. Voor meer informatie over de server extensie-pakketten, raadpleegt u [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Zie het onderwerp [Offline gegevens synchroniseren in de Mobile-Apps Azure]meer informatie over de functie offlinesynchronisatie.

## <a name="review-sync"></a>Controleert u de synchronisatie-code van client

De client-project dat u al voor het [maken van een App voor iOS] zelfstudie hebt gedownload bevat code ondersteunende offlinesynchronisatie met een lokale database Core gegevens gebaseerde. Hieronder volgt een overzicht van wat al in de Zelfstudievideo code opgenomen is. Zie voor een overzicht van de functie, [Offline gegevens synchroniseren in Azure Mobile-Apps].

De synchronisatiefunctie voor synchronisatie van offline gegevens van Azure Mobile-Apps kan eindgebruikers om te communiceren met een lokale database als het netwerk niet toegankelijk is. Als u wilt deze functies gebruiken in uw app, u de context van de synchronisatie van geïnitialiseerd `MSClient` en verwijzen naar een lokale archief. Vervolgens verwijzen naar de tabel tot en met de `MSSyncTable` interface.

1. In **QSTodoService.m** (doel-C) of **ToDoTableViewController.swift** (Swift), ziet u het type van het lid `syncTable` is `MSSyncTable`. Offline synchronisatie gebruikt deze interface van de tabel synchroniseren in plaats van `MSTable`. Wanneer een synchronisatietabel wordt gebruikt, worden alle bewerkingen gaat u naar het lokale archief en alleen worden gesynchroniseerd met de externe end met expliciete push en halen bewerkingen.

    Als u een verwijzing naar een tabel synchroniseren, gebruikt u de methode `syncTableWithName` op `MSClient`. Als u wilt verwijderen offline synchroniseren functionaliteit, gebruikt u `tableWithName` in plaats daarvan.

2. Voordat de tabelbewerkingen van een kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd. Hier ziet u de betreffende code. 
    
    **Doelstelling-C**:
    
    In de `QSTodoService.init` methode:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    In de `ToDoTableViewController.viewDidLoad` methode:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Hiermee maakt u een lokale archief met de interface `MSCoreDataStore`, die is opgegeven in de Mobile-Apps SDK. U kunt in plaats daarvan een opgeven een verschillende lokale archief door het implementeren van de `MSSyncContextDataSource` protocol. 
    
    Bovendien de eerste parameter van `MSSyncContext` wordt gebruikt om op te geven van een conflict-handler. Aangezien we hebt doorgegeven `nil`, wordt de standaard conflict handler, die niet bij een conflict krijgt.
    
3. Nu kunnen we de werkelijke synchronisatiebewerking uitvoeren en gegevens ophalen uit de externe-end.

    **Doelstelling-C**:
    
    `syncData`eerst worden nieuwe wijzigingen, klikt u vervolgens belt `pullData` toegang tot gegevens van de externe end. In inschakelt, de methode `pullData` krijgt u nieuwe gegevens die overeenkomt met een query:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    In de doel-C-versie in `syncData`, verwijst naar eerst `pushWithCompletion` van de context synchroniseren. Deze methode is een lid van `MSSyncContext` (in plaats van de synchronisatie van een tabel zelf) omdat deze wijzigingen worden push op alle tabellen. Alleen de records die zijn gewijzigd in een andere manier lokaal (via CUD bewerkingen) wordt verzonden naar de server. Klik op de helper `pullData` wordt genoemd, welke oproepen `MSSyncTable.pullWithQuery` voor externe gegevens ophalen en bewaren in de lokale database.
    
    In de snelle-versie is geen doorschakelen naar `pushWithCompletion`. Dit komt omdat de push-bewerking niet strikt noodzakelijk is. Als er een wachtende wijzigingen in de synchronisatie-context voor de tabel die een push-bewerking doet, ophalen altijd problemen een push eerst. Als er meer dan één synchronisatietabel, is het echter push om ervoor te zorgen dat alles consistent is voor alle gerelateerde tabellen best expliciet in te bellen.
    
    In de doel-C en de Swift versies, de methode `pullWithQuery` kunt u opgeven van een query de records wilt filteren die u wilt ontvangen. In dit voorbeeld de query alleen haalt alle records in de externe `TodoItem` tabel.
    
    De tweede parameter voor `pullWithQuery` is van een query-ID die wordt gebruikt voor *incrementele synchroniseren*. Incrementele synchronisatie haalt alleen de records die zijn gewijzigd sinds de laatste synchronisatie, met behulp van de record `UpdatedAt` tijdstempel (genoemd `updatedAt` in het lokale archief.) De query-ID moet een beschrijvende tekenreeks die uniek is voor beide logische query's in uw app. Als u wilt opt-out van incrementele synchronisatie, doorgeven `nil` als de query-ID. Houd er rekening mee dat dit mogelijk niet efficiënt, is omdat alle records op elke bewerking halen worden opgehaald.

5. Het doel-C-app wordt gesynchroniseerd wanneer we wijzigen of gegevens toevoegen, een gebruiker voert de penbeweging vernieuwen en klik op starten. De snelle app worden gesynchroniseerd wanneer een gebruiker de beweging vernieuwen uitvoert en klik op starten. 

Omdat het app-gesynchroniseerd wanneer u gegevens zijn gewijzigd (doel-C) of als de app wordt gestart (doel-C & Swift), de app wordt ervan uitgegaan dat de gebruiker online is. We zullen de app in een andere sectie bijwerken zodat gebruikers bewerken kunnen, zelfs als ze offline bent.

## <a name="review-core-data"></a>Controleer het Core-gegevensmodel

Wanneer u met de Core offline gegevensopslag, moet u bepaalde tabellen en velden in uw gegevensmodel definiëren. De app voorbeeld bevat een gegevensmodel met de juiste indeling. In deze sectie wordt we begeleiden via deze tabellen en hoe deze worden gebruikt.

- Open **QSDataModel.xcdatamodeld**. Er zijn vier tabellen die zijn gedefinieerd--drie die worden gebruikt door de SDK, en één tabel voor de taak items zelf:     *MS_TableOperations: voor het bijhouden van de items die moeten worden gesynchroniseerd met de server    * MS_TableOperationErrors: voor het bijhouden van eventuele fouten die tijdens offlinesynchronisatie optreden     *MS_TableConfig: bijgewerkt voor het bijhouden van de laatste tijd voor de laatste bewerking synchroniseren voor alle halen bewerkingen    * TodoItem : Voor het opslaan van de items die moeten worden. Het systeem kolommen **createdAt**, **updatedAt**en **versie** zijn optioneel Systeemeigenschappen.

>[AZURE.NOTE] De SDK van Azure Mobile-Apps behoudt kolomnamen die wordt met '**``**". U moet dit voorvoegsel niet gebruiken op een andere naam heeft dan systeemkolommen, anders uw kolomnamen wordt gewijzigd wanneer u de externe backend gebruikt.

- Wanneer u met de functie offline synchroniseren, kunt u de systeemtabellen moet opgeven, zoals hieronder wordt weergegeven.

    ### <a name="system-tables"></a>Systeemtabellen

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Kenmerk  |    Type     |
  	|----------- |   ------    |
  	| ID         | Geheel getal van 64  |
  	| itemId     | Tekenreeks      |
  	| Eigenschappen | Binaire gegevens |
  	| tabel      | Tekenreeks      |
  	| tableKind  | Geheel getal 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Kenmerk  |    Type     |
  	|----------- |   ------    |
  	| ID         | Tekenreeks      |
  	| Bewerkingsnummer | Geheel getal van 64 |
  	| Eigenschappen | Binaire gegevens |
  	| tableKind  | Geheel getal 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Kenmerk  |    Type     |
  	|----------- |   ------    |
  	| ID         | Tekenreeks      |
  	| toets        | Tekenreeks      |
  	| keyType    | Geheel getal van 64  |
  	| tabel      | Tekenreeks      |
  	| waarde      | Tekenreeks      |

    ### <a name="data-table"></a>Gegevenstabel

    **TodoItem**

  	| Kenmerk    |  Type   | Opmerking                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| ID           | Tekenreeks, gemarkeerd als vereist  | primaire sleutel in de externe store                            |
  	| voltooien     | Booleaanse waarde | taak-itemveld                                        |
  	| tekst         | Tekenreeks  | taak-itemveld                                        |
  	| createdAt | Datum    | (optioneel) kaarten aan createdAt systeemeigenschap         |
  	| updatedAt | Datum    | (optioneel) kaarten aan updatedAt systeemeigenschap         |
  	| Versie   | Tekenreeks  | (optioneel) gebruikt om te bepalen conflicten, kaarten naar versie |


## <a name="setup-sync"></a>Het gedrag van de synchronisatie van de app wijzigen

In deze sectie wijzigt u de app zodat deze wordt niet gesynchroniseerd op app starten of bij het invoegen en bijwerken van items, maar alleen als de knop Vernieuwen beweging wordt uitgevoerd.

**Doelstelling-C**:

1. In **QSTodoListViewController.m**, wijzigt u de methode **viewDidLoad** als u wilt verwijderen van de oproep door naar `[self refresh]` aan het einde van de methode. Nu de gegevens worden niet worden gesynchroniseerd met de server op app starten, maar in plaats hiervan is de inhoud van lokale archief.

2. In **QSTodoService.m**, wijzigt u de definitie van `addItem` zodat deze kan niet worden gesynchroniseerd nadat het item is ingevoegd. Verwijder de `self syncData` blokkeren en vervangen door de volgende handelingen uit:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. De definitie van `completeItem` als hierboven; de blokkering voor verwijderen `self syncData` en vervangen door de volgende handelingen uit:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. In `viewDidLoad` in **ToDoTableViewController.swift**, opmerkingen toevoegen om deze twee regels, klikt u op app starten synchroniseren stoppen. Op het moment van dit artikel schrijven, wordt de app Swift taak niet de service bijgewerkt wanneer iemand wordt toegevoegd of is voltooid van een item, alleen op app starten.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>De app testen

In dit gedeelte maakt u verbinding met een ongeldige URL kunt u een offline mogelijkheid simuleren. Wanneer u gegevens toevoegt, worden ze gehouden in de lokale Core-gegevensopslag, maar niet gesynchroniseerd met de mobiele backend.

1. De URL van de Mobile-App in **QSTodoService.m** wijzigen in een ongeldige URL en de app opnieuw uitvoeren:

    **Doelstelling-C** in QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Swift** in ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Sommige items die moeten worden toevoegen. Sluit de simulator (of de app automatisch sluiten) en opnieuw starten. Controleer of dat de wijzigingen hebt behouden is gebleven.

3. De inhoud van de tabel met externe TodoItem bekijken:

    + Klik voor een backend Node.js, Ga naar de [Azure-portal](https://portal.azure.com/)en klik in de Mobile-App backend op **Eenvoudig tabellen** > **TodoItem** om weer te geven van de inhoud van de `TodoItem` tabel.
    + Voor een backend .NET de tabelinhoud met een SQL-hulpmiddel zoals SQL Server Management Studio, of een REST-client zoals Fiddler of Postman te bekijken.

    Controleer de nieuwe items *niet* zijn gesynchroniseerd met de server:

4. De URL terug naar de juiste op wijzigen in **QSTodoService.m** en de app opnieuw. Voer de beweging vernieuwen trekken omlaag in de lijst met items. Hier ziet u een kringveld voortgang.

5. De gegevens TodoItem weer. De nieuwe en gewijzigde TodoItems wordt nu weergegeven.

## <a name="summary"></a>Overzicht

Ter ondersteuning van de functie offlinesynchronisatie we gebruikt de `MSSyncTable` gebruikersinterface en geïnitialiseerd `MSClient.syncContext` met een lokale archief. In dit geval is het lokale archief een database op basis van Core gegevens.

Wanneer u met een lokale Core gegevensopslag, moet u meerdere tabellen met de [juiste Systeemeigenschappen](#review-core-data)definiëren.

De normale CRUD-bewerkingen voor Azure Mobile-Apps werken als wanneer de app nog steeds is verbonden, maar alle bewerkingen vindt plaats op het lokale archief.

Als wij het lokale archief synchroniseren met de server willen, we gebruikt de `MSSyncTable.pullWithQuery`methode.


## <a name="additional-resources"></a>Aanvullende informatie

* [Offline gegevens synchroniseren in Azure mobiele Apps]

* [Begeleidende cloud: Offline synchroniseren in Azure mobiele Services] \(Opmerking: de video in Mobile-Services, maar offline synchroniseren op een soortgelijke manier in Azure Mobile-Apps werkt\)

<!-- URLs. -->


[Een iOS-App maken]: app-service-mobile-ios-get-started.md
[Offline gegevens synchroniseren in Azure mobiele Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Omslag van de cloud: Offline synchroniseren in Azure mobiele Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
