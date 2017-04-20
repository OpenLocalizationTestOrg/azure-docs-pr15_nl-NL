<properties
    pageTitle="Eenvoudig toepassingsinstallatie en beheer in Azure Batch | Microsoft Azure"
    description="Gebruik de functie van de toepassing-pakketten van Azure Batch gemakkelijk meerdere toepassingen en versies voor installatie op Batch beheren berekenen knooppunten."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="application-deployment-with-azure-batch-application-packages"></a>Toepassingsimplementatie waarbij Azure Batch toepassing-pakketten

De functie van de toepassing-pakketten van Azure Batch biedt eenvoudig beheer van de taak-toepassingen en hun implementatie van de knooppunten berekenen in uw groep. U kunt met de toepassingspakketten, uploaden en meerdere versies van de toepassingen die uw taken uitvoeren, inclusief hun ondersteunende bestanden beheren. U kunt vervolgens automatisch implementeren een of meer van deze toepassingen naar de knooppunten berekenen in uw groep.

In dit artikel leert u hoe u uploadt en beheren van toepassingspakketten in de portal van Azure. Vervolgens leert u hoe u ze installeert op een groep van toepassingen berekeningscluster knooppunten met de [Batch.NET] [ api_net] bibliotheek.

> [AZURE.NOTE] De toepassing-pakketten functie die hier worden beschreven, vervangt de functie 'Batch Apps' beschikbaar in eerdere versies van de service.

## <a name="application-package-requirements"></a>Vereisten voor het pakket van toepassing

[Koppeling een Azure Storage-account](#link-a-storage-account) moet u bij uw account Batch toepassingspakketten gebruiken.

De toepassing-pakketten-functie in dit artikel beschreven is compatibel *alleen* met Batch van toepassingen die na 10 maart 2016 zijn gemaakt. Toepassingspakketten wordt niet geïmplementeerd om te berekenen van knooppunten in toepassingen die zijn gemaakt voor deze datum.

Deze functie is geïntroduceerd in [Batch REST API] [ api_rest] versie 2015-12-01.2.2 en de bijbehorende [Batch.NET] [ api_net] bibliotheek versie 3.1.0. Het is raadzaam dat u de meest recente versie van de API altijd gebruiken tijdens het werken met Batch.

> [AZURE.IMPORTANT] Op dit moment ondersteuning alleen *CloudServiceConfiguration* groepen voor toepassingspakketten. U kunt toepassing-pakketten niet gebruiken in toepassingen die zijn gemaakt met behulp van VirtualMachineConfiguration afbeeldingen. Zie het gedeelte [VM configuratie](batch-linux-nodes.md#virtual-machine-configuration) van de [voorziening Linux berekenen knooppunten in Azure Batch groepen](batch-linux-nodes.md) voor meer informatie over de twee verschillende configuraties.

## <a name="about-applications-and-application-packages"></a>Over toepassingen en -toepassingspakketten

In de Batch Azure, wordt een *toepassing* verwijst naar een reeks versienummer binaire bestanden die automatisch kan worden gedownload naar de knooppunten berekenen in uw groep. Een *toepassingspakket* verwijst naar een *bepaalde set* van de binaire getallen en staat voor een bepaalde *versie* van de toepassing.

![Hoog niveau diagram van toepassingen en -toepassingspakketten][1]

### <a name="applications"></a>Toepassingen

Een toepassing in de Batch bevat een of meer toepassing verpakt en Hiermee geeft u configuratieopties voor de toepassing. Een toepassing kunt bijvoorbeeld opgeven dat de standaardversie voor het pakket van toepassing te installeren op een berekeningscluster knooppunten en of de pakketten kunnen worden bijgewerkt of verwijderd.

### <a name="application-packages"></a>Toepassingspakketten

Het toepassingspakket van een is een ZIP-bestand met de toepassing binaire bestanden en ondersteunende bestanden die vereist voor de uitvoering van door uw taken zijn. Elke toepassingspakket vertegenwoordigt een specifieke versie van de toepassing.

Toepassingspakketten op het niveau van de groep en de taak, kunt u opgeven. U kunt een of meer van deze pakketten en (optioneel) een versie opgeven wanneer u een groep of een taak maken.

* **Groep toepassing-pakketten** zijn geïmplementeerd in *elk* knooppunt in de groep. Toepassingen zijn geïmplementeerd wanneer een knooppunt deelneemt aan een groep en wanneer deze wordt gestart of reimaged.

    Groep toepassing-pakketten zijn geschikt wanneer alle knooppunten in een groep van een taak taken uitvoeren. Wanneer u een groep maken en u kunt toevoegen of bijwerken van een bestaande toepassingen-pakketten, kunt u een of meer toepassingspakketten opgeven. Als u een bestaande toepassingen toepassing-pakketten bijwerkt, moet u de knooppunten om te kunnen installeren van het nieuwe pakket opnieuw opstarten.

* **Taak-toepassing-pakketten** zijn geïmplementeerd alleen naar een berekeningscluster knooppunt dat is gepland voor het uitvoeren van een taak, vlak voor de opdrachtregel van de taak wordt uitgevoerd. Als de opgegeven toepassing-pakket en versie is al op het knooppunt, wordt deze niet opnieuw is geïmplementeerd en wordt het bestaande pakket wordt gebruikt.

    Taak-toepassing-pakketten zijn handig in gedeelde toepassingen omgevingen, waar andere taken worden uitgevoerd op een groep en de groep wordt niet verwijderd wanneer een taak is voltooid. Als uw project minder taken dan knooppunten in de groep heeft, kunnen taak toepassing-pakketten overdracht van gegevens minimaliseren omdat uw toepassing wordt geïmplementeerd alleen op de knooppunten die taken uitvoeren.

    Andere scenario's die u van de taak toepassing-pakketten profiteren kunnen zijn functies die gebruikmaken van een bijzonder grote-toepassing, maar voor slechts een klein aantal taken. Bijvoorbeeld, een vooraf verwerking fase of een taak samenvoegen, waar de toepassing oude verwerking of samenvoegen heavyweight is.

> [AZURE.IMPORTANT] Er zijn beperkingen van het aantal toepassingen en -toepassingspakketten binnen een Batch-account, evenals de maximale grootte van pakket. Zie [quota en beperkingen voor de Batch Azure-service](batch-quota-limit.md) voor meer informatie over deze limieten.

### <a name="benefits-of-application-packages"></a>Voordelen van toepassingspakketten

Toepassingspakketten kunnen de code in de Batch-oplossing te vereenvoudigen en verlaag het realiseren dat is vereist voor het beheren van de toepassingen die uw taken uitgevoerd.

Van uw groep starttaak heeft geen om op te geven van een lijst met individuele resource-bestanden te installeren op de knooppunten lang. U hoeft niet te meerdere versies van uw toepassingsbestanden in Azure Storage of op uw knooppunten handmatig beheren. En, moet u niet bang [SA's URL's](../storage/storage-dotnet-shared-access-signature-part-1.md) voor toegang tot de bestanden in uw account opslag genereren. Batch werkt op de achtergrond met Azure-opslag voor het opslaan van toepassingspakketten en implementeren om te berekenen van knooppunten.

## <a name="upload-and-manage-applications"></a>Uploaden en -toepassingen beheren

U kunt de [portal van Azure] [ portal] of de [Batch Management.NET](batch-management-dotnet.md) -bibliotheek voor het beheren van de toepassingspakketten in uw account Batch. In de volgende secties, we eerst een account opslag koppelen en klik bespreken toe te voegen toepassingen en -pakketten en beheren ze met de portal.

### <a name="link-a-storage-account"></a>Een opslag-account koppelen

Als u wilt gebruiken toepassingspakketten, moet u eerst een Azure Storage-account koppelen aan uw account Batch. Als u niet een account opslagruimte voor uw Batch-account hebt geconfigureerd, wordt een waarschuwing dat de eerste keer dat u de **toepassingen** -tegel in het blad **Batch account** weergegeven in de portal van Azure.

> [AZURE.IMPORTANT] Momenteel ondersteunt *alleen* het accounttype voor **algemene** opslag batch volgens de beschrijving in stap 5, [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account), van [over Azure opslag-accounts](../storage/storage-create-storage-account.md). Wanneer u een opslag van Azure-account koppelen aan uw Batch-account, koppeling *alleen* een **algemene** opslag-account.

![Geen waarschuwing opslag account is geconfigureerd in Azure-portal][9]

De Batch-service wordt de bijbehorende opslag-account gebruikt voor de opslag en voor het ophalen van toepassingspakketten. Nadat u een koppeling van de twee accounts hebt gemaakt, kunt Batch automatisch de pakketten die zijn opgeslagen in het gekoppelde account opslagruimte aan uw knooppunten berekeningscluster implementeren. Klik op **Accountinstellingen opslagruimte** op het blad **Waarschuwing** en klik vervolgens op **Opslag Account** op het blad **Opslag Account** naar een opslag-account koppelen aan uw account Batch.

![Kies opslag account blade in Azure-portal][10]

Het is raadzaam om die u een opslag account *specifiek* voor gebruik met uw account Batch maakt en selecteert u deze hier. Zie "een opslag-account maken' in [over Azure opslag mailaccounts](../storage/storage-create-storage-account.md)voor meer informatie over het maken van een opslag-account. Nadat u een opslag-account hebt gemaakt, kunt u vervolgens deze koppelen aan uw account Batch met behulp van het blad **Opslag-Account** .

> [AZURE.WARNING] Omdat de Batch wordt Azure-opslag voor de opslag van uw toepassingspakketten, u bent [in rekening gebracht als normale] [ storage_pricing] voor de blob-gegevens blokkeren. Zorg ervoor dat u rekening houden met de grootte en het nummer van uw toepassingspakketten en vervallen-pakketten om te beperken kosten regelmatig te verwijderen.

### <a name="view-current-applications"></a>Huidige weergave-toepassingen

Als u wilt de toepassingen in uw account Batch bekijkt, klikt u op de **toepassingen** menu-item in het linkermenu tijdens het bekijken van het blad **Batch-account** .

![Tegel voor toepassingen][2]

Hiermee opent u het blad **toepassingen** :

![Lijst met toepassingen][3]

Het blad **toepassingen** bevat de ID van elke toepassing in uw account en de volgende eigenschappen:

* **Pakketten**--het aantal versies dat is gekoppeld aan deze toepassing.
* **Standaardversie**--de versie die wordt geïnstalleerd als u niet een versie opgeeft wanneer u de toepassing voor een groep hebt ingesteld. Deze instelling is optioneel.
* **Updates toestaan**--de waarde die aangeeft of pakket wordt bijgewerkt, verwijderingen en toevoegingen zijn toegestaan. Als dit is ingesteld op **Nee**, zijn pakket bijwerken en verwijderen uitgeschakeld voor de toepassing. Alleen nieuwe toepassing pakket versies kunnen worden toegevoegd. De standaardinstelling is **Ja**.

### <a name="view-application-details"></a>Details van de toepassing weergeven

Klik op een toepassing in het blad **toepassingen** het blad met de gegevens voor de toepassing te openen.

![Toepassingdetails van][4]

In het blad toepassing details, kunt u de volgende instellingen configureren voor uw toepassing.

* **Updates toestaan**--opgeven of de toepassingspakketten kunnen worden bijgewerkt of verwijderd. Zie 'Bijwerken of verwijderen van een toepassingspakket' verderop in dit artikel.
* **Standaardversie**--een standaard-toepassingspakket om te implementeren als u wilt berekenen van knooppunten opgeven.
* **Weergavenaam**--een 'vriendelijke' een naam die uw oplossing kunt gebruiken wanneer informatie over het gebruik van de toepassing, wordt zoals in de gebruikersinterface van een service die u opgeeft dat uw klanten via Batch Batch opgeven.

### <a name="add-a-new-application"></a>Een nieuwe toepassing toevoegen

Maak een nieuwe toepassing, het toevoegen van een toepassingspakket en het opgeven van een nieuwe, unieke id De eerste toepassingspakket dat u met de nieuwe toepassings-ID toevoegen wordt ook de nieuwe toepassing maken.

Klik op **toevoegen** op het blad **toepassingen** het blad **nieuwe toepassing** te openen.

![Nieuwe toepassing blade in Azure-portal][5]

Het **nieuwe toepassing** blad biedt de volgende velden om op te geven van de instellingen van uw nieuwe toepassing en de toepassingspakket.

**Toepassings-id**

Dit veld bevat de ID van uw nieuwe-toepassing, dat wil onderhevig aan de standaard validatieregels van Azure Batch-ID zeggen:

* Kunnen elke combinatie van alfanumerieke tekens, inclusief afbreekstreepjes en onderstrepingstekens bevatten.
* Kan meer dan 64 tekens bevatten.
* Moet uniek zijn binnen de Batch-account.
* Is het bevriezen van hoofdletters/kleine letters en hoofdletters/kleine letters letters.

**Versie**

Hiermee geeft u de versie van de toepassingspakket dat u wilt uploaden. Versietekenreeksen zijn onderhevig aan de volgende regels voor gegevensvalidatie:

* Kunnen elke combinatie van alfanumerieke tekens, inclusief afbreekstreepjes, onderstrepingstekens en punten bevatten.
* Kan meer dan 64 tekens bevatten.
* Moet uniek zijn binnen de toepassing.
* Hoofdletters/kleine letters behouden en hoofdlettergevoelig.

**Toepassingspakket**

Dit veld bevat de ZIP-bestand met de toepassing binaire bestanden en ondersteunende bestanden die zijn vereist voor het uitvoeren van de toepassing. Klik op het vak **Selecteer een bestand** of het mappictogram om te bladeren naar en selecteer een ZIP-bestand met de bestanden van uw toepassing.

Nadat u een bestand hebt geselecteerd, klikt u op **OK** om te beginnen met het uploaden met Azure Storage. Wanneer de uploadbewerking voltooid is, krijgt u een melding en het blad wordt gesloten. Afhankelijk van de grootte van het bestand dat u wilt uploaden en de snelheid van uw netwerkverbinding, kan deze bewerking kan enige tijd duren.

> [AZURE.WARNING] Sluit het blad **nieuwe toepassing** niet voordat de uploadbewerking voltooid is. Hiermee wordt het uploadproces beëindigen.

### <a name="add-a-new-application-package"></a>Een nieuwe toepassingspakket toevoegen

Als u wilt een nieuwe versie van toepassing-pakket voor een bestaande toepassing toevoegen, selecteert u een toepassing in het blad **toepassingen** , **pakketten**, klik op **toevoegen** om te openen van het blad **toevoegen-pakket** .

![Het toevoegen van toepassing-pakket blade in Azure-portal][8]

Zoals u ziet, wordt de velden overeenkomen met die van het blad **nieuwe toepassing** , maar het vak **toepassings-id** is uitgeschakeld. Als u voor de nieuwe toepassing hebt, geef de **versie** voor uw nieuwe pakket, bladert u naar uw ZIP- **pakket toepassing** bestand en klik op **OK** om het pakket uploaden.

### <a name="update-or-delete-an-application-package"></a>Bijwerken of verwijderen van een toepassingspakket

Als u wilt bijwerken of verwijderen van een bestaand toepassingspakket, opent u het blad details voor de toepassing, klikt u op **pakketten** het blad **pakketten** openen en klik op het **beletselteken** in de rij van de toepassingspakket dat u wilt wijzigen, selecteert u de actie die u wilt uitvoeren.

![Bijwerken of verwijderen van pakket in Azure-portal][7]

**Update**

Als u op **bijwerken**klikt, wordt het blad *Update-pakket* wordt weergegeven. Deze blade is vergelijkbaar met het blad *nieuwe toepassingspakket* echter alleen het pakket selectieveld is ingeschakeld, zodat u kunt een nieuwe ZIP-bestand wilt uploaden.

![Update-pakket blade in Azure-portal][11]

**Verwijderen**

Wanneer u op **verwijderen**hebt geklikt, u wordt gevraagd het verwijderen van de Pakketversie te bevestigen en Batch Hiermee verwijdert u het pakket van Azure-opslag. Als u de standaardversie van een toepassing verwijdert, wordt de **standaardversie** instelling voor de toepassing verwijderd.

![Toepassing verwijderen][12]

## <a name="install-applications-on-compute-nodes"></a>-Toepassingen installeren op knooppunten berekenen

U kunt het beheren van toepassingspakketten in de portal van Azure hebt gezien, kan het implementeren van deze om te berekenen van knooppunten en laat u ze met batchtaken besproken.

### <a name="install-pool-application-packages"></a>Groep toepassing-pakketten installeren

Als u wilt installeren het toepassingspakket van een op alle berekeningscluster knooppunten in een groep, Geef een of meer toepassing pakket *verwijzingen* voor de toepassingen. De toepassingspakketten die u voor een resourcegroep die opgeeft zijn geïnstalleerd op elk knooppunt berekeningscluster wanneer dat knooppunt deelneemt aan de groep en wanneer het knooppunt wordt opnieuw gestart of reimaged.

Geef in de Batch .NET, een of meer [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] wanneer u een nieuwe groep maakt, of voor een bestaande toepassingen. De [ApplicationPackageReference] [ net_pkgref] class Hiermee geeft u een toepassings-ID en versie te installeren op een resourcegroep die van knooppunten berekenen.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Als een pakket distributie om welke reden dan ook is mislukt, markeert u de Batch-service het knooppunt [onbruikbaar][net_nodestate], en geen taken worden gepland voor uitvoering op het knooppunt. In dit geval moet u **opnieuw** het knooppunt maximaal toegestane aantal pogingen de pakket-implementatie. Opnieuw starten van het knooppunt kunnen ook opnieuw plannen van de taken op het knooppunt.

### <a name="install-task-application-packages"></a>Taak-toepassing-pakketten installeren

Net als u naar een groep, u toepassing pakket *verwijzingen* voor een taak opgeven. Wanneer een taak is gepland op een knooppunt worden uitgevoerd, is het pakket gedownload en uitgepakt net voordat de opdrachtregel van de taak wordt uitgevoerd. Als een opgegeven pakket en versie is al geïnstalleerd op het knooppunt, wordt het pakket niet is gedownload en wordt het bestaande pakket wordt gebruikt.

Een taak toepassing als pakket wilt installeren, configureren van de taak [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] eigenschap:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>De geïnstalleerde toepassingen uitvoert

De pakketten die u hebt opgegeven voor een groep of een taak worden gedownload en uitgepakt naar een map met een naam in de `AZ_BATCH_ROOT_DIR` van het knooppunt. Batch maakt ook een omgevingsvariabele met het pad naar de map met de naam. Uw taakregels gebruikt deze omgevingsvariabele wanneer u verwijst naar de toepassing op het knooppunt. De variabele heeft de volgende indeling:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`en `version` zijn waarden die overeenkomen met de toepassing en pakket versie die u hebt opgegeven voor implementatie. Stel dat u hebt opgegeven dat versie 2.7 van toepassing *blender* moet worden geïnstalleerd, zou uw taakregels deze omgevingsvariabele voor toegang tot de bestanden gebruiken:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Als u een standaardversie voor een toepassing opgeeft, kunt u het versieachtervoegsel weglaten. Als u "2,7" ingesteld als de standaardversie voor toepassing *blender*, bijvoorbeeld uw taken kunnen verwijzen naar de volgende omgevingsvariabele en ze versie 2.7 wordt uitgevoerd:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Het volgende codefragment toont een voorbeeld van de taak-opdrachtregel waarmee wordt geopend de standaardversie van de toepassing *blender* :

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] Zie [omgevingsinstellingen voor taken](batch-api-basics.md#environment-settings-for-tasks) in het [overzicht van de functie Batch](batch-api-basics.md) voor meer informatie over berekeningscluster knooppunt omgevingsinstellingen.

## <a name="update-a-pools-application-packages"></a>Een resourcegroep die van toepassing-pakketten bijwerken

Als u een bestaande toepassingen al met een toepassingspakket is geconfigureerd, kunt u een nieuw pakket voor de toepassingen. Als u een nieuw pakket verwijzing voor een groep, de volgende gevallen opgeven:

* Alle nieuwe knooppunten die deelnemen aan de groep en een bestaande knooppunt dat is gestart of reimaged worden nu geïnstalleerd het zojuist opgegeven pakket.
* Berekent het nieuwe toepassingspakket kunnen niet automatisch installeren door knooppunten die zich al in de groep wanneer u de verwijzingen pakket bijwerkt. Deze berekenen knooppunten moeten opnieuw gestart of als u wilt ontvangen van het nieuwe pakket reimaged.
* Als een nieuw pakket wordt geïmplementeerd, geven de omgevingsvariabelen gemaakte de nieuwe toepassing pakket verwijzingen.

In dit voorbeeld bevat de bestaande toepassingen versie 2.7 van de toepassing *blender* is geconfigureerd als een van de [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Als u wilt bijwerken van de groep knooppunten met versie 2.76b, Geef een nieuwe [ApplicationPackageReference] [ net_pkgref] met de nieuwe versie en doorvoeren de wijziging.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Nu dat de nieuwe versie is geconfigureerd, heeft een *nieuwe* knooppunt dat u deelneemt aan de groep 2.76b geïmplementeerd in deze versie. Als u wilt installeren 2.76b op de knooppunten die *al* in de groep, opnieuw starten of ze reimage. Houd er rekening mee dat opnieuw opgestarte knooppunten de bestanden uit de vorige pakket implementaties behoudt.

## <a name="list-the-applications-in-a-batch-account"></a>Lijst van de toepassingen in een Batch-account

U kunt de toepassingen en hun pakketten in een account Batch weergeven met behulp van de [ApplicationOperations][net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] methode.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Laten teruglopen omhoog

Met de toepassingspakketten kunt u uw klanten selecteert u de toepassingen voor hun taken en geef de exacte versie wilt gebruiken wanneer u taken met uw service Batch ingeschakelde verwerken. U kunt ook de mogelijkheid om uw klanten om te uploaden en bijhouden van hun eigen toepassingen in uw service opgeven.

## <a name="next-steps"></a>Volgende stappen

* De [Batch REST API] [ api_rest] ook biedt ondersteuning voor gebruik met de toepassingspakketten. Zie bijvoorbeeld de [applicationPackageReferences] [ rest_add_pool_with_packages] element in [een resourcegroep die bij een account toevoegen] [ rest_add_pool] voor informatie over het opgeven van pakketten installeren met behulp van de REST API. Zie [toepassingen] [ rest_applications] voor meer informatie over hoe u de toepassingsinformatie opvragen met behulp van de Batch REST API.

* Leer hoe u via programmacode [Azure Batch accounts en quota met Batch Management .NET beheren](batch-management-dotnet.md). De [Batch Management.NET] [ api_net_mgmt] bibliotheek kunt account maken en verwijderen functies inschakelen voor uw toepassing Batch of de service.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Toepassing-pakketten op hoog niveau diagram"
[2]: ./media/batch-application-packages/app_pkg_02.png "Toepassingen-tegel in Azure-portal"
[3]: ./media/batch-application-packages/app_pkg_03.png "Toepassingen blade in Azure-portal"
[4]: ./media/batch-application-packages/app_pkg_04.png "Toepassing details blade in Azure-portal"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nieuwe toepassing blade in Azure-portal"
[7]: ./media/batch-application-packages/app_pkg_07.png "Bijwerken of verwijderen van pakketten vervolgkeuzelijst in Azure-portal"
[8]: ./media/batch-application-packages/app_pkg_08.png "Nieuwe toepassing pakket blade in Azure-portal"
[9]: ./media/batch-application-packages/app_pkg_09.png "Geen gekoppelde melding voor de opslag-account"
[10]: ./media/batch-application-packages/app_pkg_10.png "Kies opslag account blade in Azure-portal"
[11]: ./media/batch-application-packages/app_pkg_11.png "Update-pakket blade in Azure-portal"
[12]: ./media/batch-application-packages/app_pkg_12.png "Dialoogvenster voor het bevestigen van pakket in Azure-portal verwijderen"
