<properties 
    pageTitle="WebApp met tabelopslag (Node.js) | Microsoft Azure" 
    description="Een zelfstudie die gebaseerd op de Web-App met Express tutorial door toe te voegen Azure Storage-services en de Azure-module." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>De webtoepassing node.js opslag

## <a name="overview"></a>Overzicht

In deze zelfstudie gaat u de toepassing in deze zelfstudie [Node.js webtoepassing met Express] gemaakt met behulp van de Microsoft Azure-clientbibliotheken voor Node.js voor gebruik met data management-services uitbreiden. U kunt uw toepassing te maken van een web gebaseerde takenlijst-toepassing die u naar Azure implementeren kunt wordt uitbreiden. De takenlijst kan een gebruiker ophalen van taken, nieuwe taken toevoegen en taken markeren als voltooid.

De taakitems worden opgeslagen in Azure Storage. Azure opslagruimte biedt ongestructureerde gegevensopslag die fouttolerantie en ten zeerste beschikbaar is. Azure opslag bevat verschillende gegevensstructuren waar u kunt opslaan en access-gegevens en u van de opslagservices van de API's opgenomen in de SDK Azure voor Node.js of via een REST API's gebruikmaken kunt. Zie [Opslaan en toegang krijgen tot gegevens in Azure wordt aangegeven]voor meer informatie.

Deze zelfstudie wordt ervan uitgegaan dat u de [Webtoepassing Node.js] en [Node.js met Express][Node.js webtoepassing gebruiken Express] zelfstudies hebt voltooid.

U leert:

-   Het werken met de engine Jade sjabloon
-   Het werken met services van Azure gegevensbeheer

Schermafbeelding van de voltooide toepassing is hieronder:

![De voltooide pagina met webonderdelen in internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Instellen opslag referenties in Web.Config

Voor toegang tot Azure opslag, moet u in de opslagruimte referenties doorgeven. Dit doet u web.config toepassingsinstellingen gebruiken.
Deze instellingen wordt doorgegeven als omgevingsvariabelen naar knooppunt, die vervolgens worden voorgelezen door de SDK Azure.

> [AZURE.NOTE] Opslag referenties worden alleen gebruikt wanneer de toepassing wordt geïmplementeerd op Azure. Wanneer u zich in de emulator, wordt de toepassing de emulator opslag gebruikt.

Voer de volgende stappen voor de opslag-accountreferenties ophalen en deze toevoegen aan de web.config-instellingen:

1.  Als dit nog niet is geopend, de PowerShell Azure starten vanuit het menu **Start** door uit te vouwen, **Alle programma's, Azure**met de rechtermuisknop op **Azure PowerShell**en selecteer **Als Administrator uitvoeren**.

2.  Mappen naar de map met uw toepassing wijzigen. Bijvoorbeeld C:\\knooppunt\\tasklist\\WebRole1.

3.  Voer de volgende cmdlet voor het ophalen van de accountgegevens opslag vanuit het venster Azure Powershell:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    Hiermee haalt de lijst met accounts voor opslagruimte en toetsen die is gekoppeld aan uw gehoste service-account.

    > [AZURE.NOTE] Aangezien de SDK Azure Hiermee maakt u een opslag-account wanneer u een service implementeert, wordt een opslag-account moet al bestaan uit uw toepassing in de vorige hulplijnen implementeren.

4.  Open het **ServiceDefinition.csdef** -bestand met de omgevingsinstellingen die worden gebruikt wanneer de toepassing wordt geïmplementeerd op Azure:

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Het volgende blok onder **omgeving** element, wordt vervangen door {opslag ACCOUNT} en {toegangstoets opslag} invoegen met de naam van het account en de primaire sleutel voor de opslag-account dat u wilt gebruiken voor implementatie:

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![De inhoud van het web.cloud.config-bestand](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Sla het bestand en sluit Kladblok af.

### <a name="install-additional-modules"></a>Aanvullende modules installeren

2. Gebruik van de volgende opdracht uit om te installeren de [azure], [knooppunt-uuid], [nconf] en [asynchrone] modules lokaal ook om op te slaan van een vermelding voor hen naar het bestand **package.json** :

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    De uitvoer van deze opdracht ziet er ongeveer als volgt uit:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>Gebruik van de service van de tabel in een toepassing voor knooppunt.

In deze sectie wordt u de eenvoudige toepassing gemaakt met de opdracht **express** door het toevoegen van een **task.js** -bestand geïmporteerd met het model voor uw taken uitbreiden. U wordt ook de bestaande **app.js** wijzigen en maak een nieuw **tasklist.js** -bestand dat het objectmodel gebruikt.

### <a name="create-the-model"></a>Het model maken

1. Maak een nieuwe map met de naam **modellen**in de adreslijst **WebRole1** .

2. Maak een nieuw bestand met de naam **task.js**in de adreslijst **modellen** . Dit bestand bevat het model voor de taken die door uw toepassing is gemaakt.

3. Toevoegen aan het begin van het bestand **task.js** , de volgende code om te verwijzen naar vereiste bibliotheken:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Vervolgens voegt u code om te definiëren en exporteren van het object taak. Dit object is verantwoordelijk voor het verbinding maken met de tabel.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Voeg nu de volgende code om extra methoden definiëren op het object taak, waardoor interacties met de gegevens die zijn opgeslagen in de tabel:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Opslaan en sluit het bestand **task.js** .

### <a name="create-the-controller"></a>De domeincontroller maakt

1. In de map **WebRole1/routes** , een nieuw bestand met de naam **tasklist.js** maken en deze in een teksteditor te openen.

2. Voeg de volgende code toe **tasklist.js**. Hiermee wordt de azure en asynchrone modules, die worden gebruikt door **tasklist.js**geladen. Hiermee definieert u ook de functie **TaskList** , die wordt doorgegeven een exemplaar van de **taak** -object die we eerder hebt gedefinieerd:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Gaat u verder toe te voegen aan het bestand **tasklist.js** door toe te voegen van de gebruikte **showTasks**, **addTask**en **completeTasks**methoden:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Sla het bestand **tasklist.js** .

### <a name="modify-appjs"></a>App.js wijzigen

1. In de map **WebRole1** , open het bestand **app.js** in een teksteditor. 

2. Toevoegen aan het begin van het bestand, de volgende handelingen uit als u wilt laden van de azure module en de tabel naam en partition sleutel instellen:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. Het bestand app.js, schuif omlaag naar waar u de volgende regel zien:

        app.use('/', routes);
        app.use('/users', users);

    De bovenstaande regels vervangen door de code die hieronder wordt weergegeven. Hiermee wordt een exemplaar van de <strong>taak</strong> met een verbinding met uw account opslag geïnitialiseerd. Hiermee wordt doorgegeven aan de <strong>TaskList</strong>, waarin gebruiken gaan om te communiceren met de service tabel:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Sla het bestand **app.js** .

### <a name="modify-the-index-view"></a>De weergave van de index wijzigen

1. Mappen wijzigen in de map **weergaven** en open het bestand **index.jade** in een teksteditor.

2. De inhoud van het bestand **index.jade** vervangen door de onderstaande code. Hiermee definieert u de weergave voor het weergeven van bestaande taken, evenals een formulier voor het toevoegen van nieuwe taken en bestaande ideeën als voltooid markeren.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Opslaan en sluit **index.jade** -bestand.

### <a name="modify-the-global-layout"></a>De algemene indeling wijzigen

Het bestand **layout.jade** in de map **weergaven** wordt gebruikt als een globale sjabloon voor andere **.jade** -bestanden. In deze stap wijzigt u, zodat het gebruiken van [Twitter-Bootstrap](https://github.com/twbs/bootstrap), namelijk een toolkit die kunt u heel gemakkelijk een nette ogende website te ontwerpen.

1. Download en extraheren van de bestanden voor [Twitter-Bootstrap](http://getbootstrap.com/). Kopieer het bestand **bootstrap.min.css** uit de **bootstrap\\verdeling\\css** map naar de **openbare\\opmaakmodellen** map van uw tasklist-toepassing.

2. Open de **layout.jade** in uw teksteditor uit de map **weergaven** en de inhoud vervangen door het volgende:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Sla het bestand **layout.jade** .

### <a name="running-the-application-in-the-emulator"></a>De toepassing wordt uitgevoerd in de Emulator

Gebruik de volgende opdracht uit de toepassing wilt starten in de emulator.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

De browser wordt geopend en de volgende pagina wordt weergegeven:

![Een web geheugen: getiteld mijn takenlijst met een tabel met taken en velden naar een nieuwe taak toevoegen.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Het formulier met items toevoegen of bestaande items verwijderen door deze te markeren als voltooid.

## <a name="publishing-the-application-to-azure"></a>De toepassing Azure publiceren


Klik in het venster Windows PowerShell belt u de volgende cmdlet als u wilt implementeren van uw gehoste Azure-service.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

**Myuniquename** vervangen door een unieke naam voor deze toepassing. **Datacentername** vervangen door de naam van een Azure Datacenter, dat zoals **West VS**.

Nadat de installatie voltooid is, ziet u een reactie ongeveer als volgt uit:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Als voorheen, omdat u hebt opgegeven de **-starten** de optie de browser geopend en uw toepassing in Azure wordt aangegeven moet worden uitgevoerd wanneer de publicatie is voltooid weergegeven.

![Een browservenster weergeven van de pagina Mijn takenlijst. De URL wordt aangegeven op dat de pagina nu wordt gehost op Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Stoppen en verwijderen van uw toepassing.

Nadat u hebt uw toepassing wordt geïmplementeerd, wilt u mogelijk uitgeschakeld zodat u kunt kosten voorkomen of bouwen en implementeren van andere toepassingen binnen de gratis proefabonnement periode.

Azure facturen web exemplaren van de rol per uur van de servertijd verbruikt.
Servertijd verbruikt zodra uw toepassing wordt geïmplementeerd, zelfs als de exemplaren die niet worden uitgevoerd en gestopt worden.

De volgende stappen hoe u stoppen en verwijderen van uw toepassing.

1.  Klik in het venster Windows PowerShell stop de service-implementatie gemaakt in de vorige sectie met de volgende cmdlet:

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    De service wordt gestopt, kan enkele minuten duren. Wanneer de service is gestopt, ontvangt u een bericht aangegeven dat dit is gestopt.

3.  Als u wilt de service verwijderen, belt u de volgende cmdlet:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Wanneer hierom wordt gevraagd, typt u **Y** als u wilt verwijderen van de service.

    Verwijderen van de service kan enkele minuten duren. Nadat de service is verwijderd, ontvangt u een bericht aangegeven dat de service is verwijderd.

  [De webtoepassing node.js Express gebruiken]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Opslaan en toegang tot gegevens in Azure wordt aangegeven]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Node.js webtoepassing]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 
