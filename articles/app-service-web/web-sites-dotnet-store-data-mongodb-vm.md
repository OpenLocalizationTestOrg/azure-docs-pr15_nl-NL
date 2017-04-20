<properties 
    pageTitle="Een WebApp in Azure wordt aangegeven dat verbinding maakt met MongoDB uitgevoerd op een virtuele machine maken" 
    description="Een zelfstudie waarin u leert u hoe u een ASP.NET-app implementeren naar Azure App-Service, met cijfer verbonden met MongoDB op een Azure virtuele machines."
    tags="azure-portal" 
    services="app-service\web, virtual-machines" 
    documentationCenter=".net" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="02/29/2016" 
    ms.author="cephalin"/>


# <a name="create-a-web-app-in-azure-that-connects-to-mongodb-running-on-a-virtual-machine"></a>Een WebApp in Azure wordt aangegeven dat verbinding maakt met MongoDB uitgevoerd op een virtuele machine maken

Cijfer gebruikt, kunt u een ASP.NET-toepassing tot Azure App Service Web Apps implementeren. In deze zelfstudie wordt u een eenvoudige front ASP.NET MVC taak lijst toepassing die is verbonden met een uitgevoerd op een virtuele machine in Azure MongoDB-database maken.  [MongoDB] [ MongoDB] is een bron voor populaire openen, krachtige NoSQL database. Nadat u hebt gebruikt en testen van de ASP.NET-toepassing op de computer, wordt u de toepassing op Service-WebApps voor de App met cijfer uploaden.

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.


## <a name="background-knowledge"></a>Achtergrond knowledge ##

Kennis van de volgende is handig voor deze zelfstudie, maar niet vereist:

* Het C# stuurprogramma voor MongoDB. Zie voor meer informatie over het ontwikkelen van C#-toepassingen tegen MongoDB, het MongoDB [CSharp taal Center][MongoC#LangCenter]. 
* Het ASP .NET framework-toepassing web. U kunt alle informatie over op de [website van ASP.net][ASP.NET].
* ASP .NET MVC web application framework. U kunt alle informatie over op de [website van ASP.NET MVC][MVCWebSite].
* Azure. U kunt aan de slag lezen op [Azure][WindowsAzure].

## <a name="prerequisites"></a>Vereisten voor ##

- [Visual Studio Express 2013 voor Web]  [ VSEWeb] of [Visual Studio 2013] [VSUlt]
- [Azure SDK voor .NET](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
- Een actieve Microsoft Azure-abonnement

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 
## <a name="create-a-virtual-machine-and-install-mongodb"></a>Een virtuele machine maken en MongoDB installeren ##

Deze zelfstudie wordt ervan uitgegaan dat u kunt een virtuele machine hebt gemaakt in Azure wordt aangegeven. Na het maken van de virtuele machine moet u MongoDB installeren op de virtuele machine:

* Als u wilt maken van een virtuele Windows-computer en MongoDB is geïnstalleerd, raadpleegt u [MongoDB installeren op een virtuele machine met Windows Server in Azure wordt aangegeven][InstallMongoOnWindowsVM].


Nadat u de virtuele machine gemaakt in Azure wordt aangegeven en MongoDB geïnstalleerd, moet u de DNS-naam van de virtuele machine ('testlinuxvm.cloudapp.net', bijvoorbeeld) en de externe poort bewaren voor MongoDB die u hebt opgegeven in het eindpunt.  Moet u deze informatie verderop in deze zelfstudie.

<a id="createapp"></a>
## <a name="create-the-application"></a>De toepassing maken ##

In dit gedeelte maakt u een ASP.NET-toepassing 'Mijn takenlijst' genoemd met behulp van Visual Studio en uitvoeren van een eerste installatie tot Azure App Service Web Apps. U de toepassing lokaal wordt uitgevoerd, maar dit wordt verbinding maken met uw VM op Azure en gebruikt u daarin het exemplaar MongoDB die u hebt gemaakt.

1. Visual Studio, klikt u op **Nieuw Project**.

    ![Pagina Nieuw Project starten][StartPageNewProject]

1. Klik in het venster **Nieuw Project** in het linkerdeelvenster Selecteer **Visual C#**en selecteer vervolgens **Web**. Selecteer in het middelste deelvenster **ASP.NET-webtoepassing**. Naam van uw project "MyTaskListApp" onderaan, en klik vervolgens op **OK**.

    ![Dialoogvenster voor de nieuwe Project][NewProjectMyTaskListApp]

1. Klik in het dialoogvenster **Nieuw ASP.NET-Project** **MVC**selecteren en klik vervolgens op **OK**.

    ![Selecteer MVC sjabloon][VS2013SelectMVCTemplate]

1. Als u nog niet hebt aangemeld bij Microsoft Azure, wordt u gevraagd aan te melden. Volg de aanwijzingen u zich aanmeldt bij Azure.
2. Nadat u bent aangemeld, kunt u beginnen met het configureren van uw App Service web-app. Geef de **naam van de Web App**, **App-abonnement**, **resourcegroep**en **regio**en klik op **maken**.

    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)

1. Nadat het project maken is voltooid, wacht totdat de web-app moet worden gemaakt in Azure App-Service, zoals aangegeven in het venster **Azure App serviceactiviteit** . Klik vervolgens op **MyTaskListApp voor deze Web-App nu publiceren**.

1. Klik op **publiceren**.

    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)

    Als uw standaard ASP.NET-toepassing is gepubliceerd naar Azure App Service Web Apps, wordt deze gestart in de browser.

## <a name="install-the-mongodb-c-driver"></a>Installeer het MongoDB C#-stuurprogramma

MongoDB biedt aan de clientzijde ondersteuning voor C#-toepassingen via een stuurprogramma, moet u op de lokale computer installeren. Het C#-stuurprogramma is beschikbaar via de NuGet.

De MongoDB C#-stuurprogramma installeren:

1. In **Solution Explorer**met de rechtermuisknop op het project **MyTaskListApp** en selecteer **NuGetPackages beheren**.

    ![NuGet-pakketten beheren][VS2013ManageNuGetPackages]

2. Klik in het venster **NuGet-pakketten beheren** in het linkerdeelvenster, klikt u op **Online**. Typ in het vak **Zoeken op Online** aan de rechterkant "mongodb.driver".  Klik op **installeren** om het stuurprogramma te installeren.

    ![Zoeken naar MongoDB C#-stuurprogramma][SearchforMongoDBCSharpDriver]

3. Klik op **ik ga akkoord** accepteer de 10gen, Inc. licentievoorwaarden.

4. Klik op **sluiten** nadat het stuurprogramma heeft geïnstalleerd.
    ![MongoDB C# stuurprogramma geïnstalleerd][MongoDBCsharpDriverInstalled]


Het MongoDB C#-stuurprogramma is nu geïnstalleerd.  Verwijzingen naar de **MongoDB.Bson**, **MongoDB.Driver**en **MongoDB.Driver.Core** bibliotheken zijn toegevoegd aan het project.

![MongoDB C# stuurprogramma verwijzingen][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a>Een model toevoegen ##
Klik in **Solution Explorer**met de rechtermuisknop op de map *modellen* en **toevoegen** van een nieuwe **Class** en noem deze *TaskModel.cs*.  In *TaskModel.cs*, kunt u de bestaande code vervangen door de volgende code:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MongoDB.Bson.Serialization.Attributes;
    using MongoDB.Bson.Serialization.IdGenerators;
    using MongoDB.Bson;
    
    namespace MyTaskListApp.Models
    {
        public class MyTask
        {
            [BsonId(IdGenerator = typeof(CombGuidGenerator))]
            public Guid Id { get; set; }
    
            [BsonElement("Name")]
            public string Name { get; set; }
    
            [BsonElement("Category")]
            public string Category { get; set; }
    
            [BsonElement("Date")]
            public DateTime Date { get; set; }
    
            [BsonElement("CreatedDate")]
            public DateTime CreatedDate { get; set; }
    
        }
    }

## <a name="add-the-data-access-layer"></a>De data access-laag toevoegen ##
Met de rechtermuisknop op de *MyTaskListApp* project en het **toevoegen** van een **Nieuwe map** met de naam *DAL*in **Solution Explorer**.  Met de rechtermuisknop op de map *DAL* en **toevoegen** van een nieuwe **Class**. De naam van het klassebestand *Dal.cs*.  In *Dal.cs*, kunt u de bestaande code vervangen door de volgende code:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;
    
    
    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            private MongoServer mongoServer = null;
            private bool disposed = false;
    
            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net"
            private string connectionString = "mongodb://mongodbsrv20151211.cloudapp.net";
    
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  The database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
    
            // Default constructor.        
            public Dal()
            {
            }
    
            // Gets all Task items from the MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }
    
            // Creates a Task and inserts it into the collection in MongoDB.
            public void CreateTask(MyTask task)
            {
                var collection = GetTasksCollectionForEdit();
                try
                {
                    collection.InsertOne(task);
                }
                catch (MongoCommandException ex)
                {
                    string msg = ex.Message;
                }
            }
    
            private IMongoCollection<MyTask> GetTasksCollection()
            {
                MongoClient client = new MongoClient(connectionString);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
    
            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
                MongoClient client = new MongoClient(connectionString);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
    
            # region IDisposable
    
            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }
    
            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                        if (mongoServer != null)
                        {
                            this.mongoServer.Disconnect();
                        }
                    }
                }
    
                this.disposed = true;
            }
    
            # endregion
        }
    }

## <a name="add-a-controller"></a>Een controller toevoegen ##
Open het bestand *Controllers\HomeController.cs* in **Solution Explorer** en de bestaande code vervangen door het volgende:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using MyTaskListApp.Models;
    using System.Configuration;
    
    namespace MyTaskListApp.Controllers
    {
        public class HomeController : Controller, IDisposable
        {
            private Dal dal = new Dal();
            private bool disposed = false;
            //
            // GET: /MyTask/
    
            public ActionResult Index()
            {
                return View(dal.GetAllTasks());
            }
    
            //
            // GET: /MyTask/Create
    
            public ActionResult Create()
            {
                return View();
            }
    
            //
            // POST: /MyTask/Create
    
            [HttpPost]
            public ActionResult Create(MyTask task)
            {
                try
                {
                    dal.CreateTask(task);
                    return RedirectToAction("Index");
                }
                catch
                {
                    return View();
                }
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            # region IDisposable
    
            new protected void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }
    
            new protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                        this.dal.Dispose();
                    }
                }
    
                this.disposed = true;
            }
    
            # endregion
    
        }
    }

## <a name="set-up-the-styles"></a>De stijlen instellen ##
De titel boven aan de pagina wilt wijzigen, opent u de *Views\Shared\\_Layout.cshtml* bestand in **Solution Explorer** en vervangen door 'Toepassingsnaam' in de kop van de navigatiebalk "Mijn taak lijst toepassing" zodat deze ziet er zo uit:

    @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

Open het bestand *\Views\Home\Index.cshtml* wilt instellen op het menu takenlijst, en de bestaande code vervangen door de volgende code:
    
    @model IEnumerable<MyTaskListApp.Models.MyTask>
    
    @{
        ViewBag.Title = "My Task List";
    }
    
    <h2>My Task List</h2>
    
    <table border="1">
        <tr>
            <th>Task</th>
            <th>Category</th>
            <th>Date</th>
            
        </tr>
    
    @foreach (var item in Model) {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Name)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Category)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Date)
            </td>
            
        </tr>
    }
    
    </table>
    <div>  @Html.Partial("Create", new MyTaskListApp.Models.MyTask())</div>


Als u wilt toevoegen de mogelijkheid een nieuwe taak wilt maken, met de rechtermuisknop op de *Views\Home\\ * map en een **weergave** **toevoegen** .  De naam van de weergave *maken*. De code vervangen door het volgende:

    @model MyTaskListApp.Models.MyTask
    
    <script src="@Url.Content("~/Scripts/jquery-1.10.2.min.js")" type="text/javascript"></script>
    <script src="@Url.Content("~/Scripts/jquery.validate.min.js")" type="text/javascript"></script>
    <script src="@Url.Content("~/Scripts/jquery.validate.unobtrusive.min.js")" type="text/javascript"></script>
    
    @using (Html.BeginForm("Create", "Home")) {
        @Html.ValidationSummary(true)
        <fieldset>
            <legend>New Task</legend>
    
            <div class="editor-label">
                @Html.LabelFor(model => model.Name)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Name)
                @Html.ValidationMessageFor(model => model.Name)
            </div>
    
            <div class="editor-label">
                @Html.LabelFor(model => model.Category)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Category)
                @Html.ValidationMessageFor(model => model.Category)
            </div>
    
            <div class="editor-label">
                @Html.LabelFor(model => model.Date)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Date)
                @Html.ValidationMessageFor(model => model.Date)
            </div>
    
            <p>
                <input type="submit" value="Create" />
            </p>
        </fieldset>
    }

**Oplossing Explorer** ziet er als volgt:

![Oplossing Explorer][SolutionExplorerMyTaskListApp]

## <a name="set-the-mongodb-connection-string"></a>De verbindingsreeks MongoDB instellen ##
Open het bestand *DAL/Dal.cs* in **Solution Explorer**. De volgende regel met code vindt:

    private string connectionString = "mongodb://<vm-dns-name>";

Vervang `<vm-dns-name>` met de DNS-naam van de virtuele machine MongoDB uitgevoerd die u in de stap voor het [maken van een virtuele machine en MongoDB installeren][] van deze zelfstudie hebt gemaakt.  Als u de DNS-naam van uw virtuele machine zoekt, gaat u naar de Portal Azure **virtuele Machines**, en selecteer **De naam van de DNS-**zoeken.

Als de DNS-naam van de virtuele machine is 'testlinuxvm.cloudapp.net' en MongoDB op de standaardpoort naar 27017 luistert, wordt de verbinding tekenreeks regel met code eruitziet zoals:

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

Als het eindpunt VM Hiermee geeft u een andere externe poort voor MongoDB, kunt u Geef de poort in de verbindingstekenreeks:

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

Zie voor meer informatie over MongoDB verbindingstekenreeksen, [verbindingen][MongoConnectionStrings].

## <a name="test-the-local-deployment"></a>De lokale implementatie testen ##

Als u wilt uitvoeren van de toepassing op de computer, selecteer **Starten foutopsporing** in het menu **Foutopsporing** of druk op **F5**. Hiermee start u IIS Express en een browser geopend en wordt de startpagina van de toepassing wordt gestart.  U kunt een nieuwe taak is, worden toegevoegd aan de MongoDB-database op uw computer virtuele in Azure toevoegen.

![Mijn taak lijst-toepassing][TaskListAppBlank]

## <a name="publish-to-azure-app-service-web-apps"></a>Publiceren naar Azure App Service WebApps

In deze sectie publiceert u uw wijzigingen op Azure App Service Web Apps.

1. Klik in Solution Explorer opnieuw met de rechtermuisknop op **MyTaskListApp** en klik op **publiceren**.
2. Klik op **publiceren**.

    U ziet nu uw web-app uitvoeren in Azure App-Service en toegang tot de database MongoDB in Azure virtuele Machines.

## <a name="summary"></a>Overzicht ##

U hebt nu geïmplementeerd ASP.NET-toepassingen naar Azure App Service Web Apps. De WebApp weergeven:

1. Meld u aan bij de Portal van Azure.
2. Klik op **WebApps**. 
3. Selecteer uw web-app in de lijst met **Web Apps** .

Zie voor meer informatie over het ontwikkelen van C#-toepassingen tegen MongoDB, [CSharp taal Center][MongoC#LangCenter]. 

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 

<!-- HYPERLINKS -->

[AzurePortal]: http://manage.windowsazure.com
[WindowsAzure]: http://www.windowsazure.com
[MongoC#LangCenter]: http://docs.mongodb.org/ecosystem/drivers/csharp/
[MVCWebSite]: http://www.asp.net/mvc
[ASP.NET]: http://www.asp.net/
[MongoConnectionStrings]: http://www.mongodb.org/display/DOCS/Connections
[MongoDB]: http://www.mongodb.org
[InstallMongoOnWindowsVM]: ../virtual-machines/virtual-machines-windows-classic-install-mongodb.md
[VSEWeb]: http://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express
[VSUlt]: http://www.microsoft.com/visualstudio/eng/2013-downloads

<!-- IMAGES -->


[StartPageNewProject]: ./media/web-sites-dotnet-store-data-mongodb-vm/NewProject.png
[NewProjectMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/NewProjectMyTaskListApp.png
[VS2013SelectMVCTemplate]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013SelectMVCTemplate.png
[VS2013DefaultMVCApplication]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013DefaultMVCApplication.png
[VS2013ManageNuGetPackages]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013ManageNuGetPackages.png
[SearchforMongoDBCSharpDriver]: ./media/web-sites-dotnet-store-data-mongodb-vm/SearchforMongoDBCSharpDriver.png
[MongoDBCsharpDriverInstalled]: ./media/web-sites-dotnet-store-data-mongodb-vm/MongoDBCsharpDriverInstalled.png
[MongoDBCSharpDriverReferences]: ./media/web-sites-dotnet-store-data-mongodb-vm/MongoDBCSharpDriverReferences.png
[SolutionExplorerMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/SolutionExplorerMyTaskListApp.png
[TaskListAppBlank]: ./media/web-sites-dotnet-store-data-mongodb-vm/TaskListAppBlank.png
[WAWSCreateWebSite]: ./media/web-sites-dotnet-store-data-mongodb-vm/WAWSCreateWebSite.png
[WAWSDashboardMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/WAWSDashboardMyTaskListApp.png
[Image9]: ./media/web-sites-dotnet-store-data-mongodb-vm/RepoReady.png
[Image10]: ./media/web-sites-dotnet-store-data-mongodb-vm/GitInstructions.png
[Image11]: ./media/web-sites-dotnet-store-data-mongodb-vm/GitDeploymentComplete.png

<!-- TOC BOOKMARKS -->
[Een virtuele machine maken en MongoDB installeren]: #virtualmachine
[Create and run the My Task List ASP.NET application on your development computer]: #createapp
[Create an Azure web site]: #createwebsite
[Deploy the ASP.NET application to the web site using Git]: #deployapp
 