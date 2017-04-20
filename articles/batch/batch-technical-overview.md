<properties
    pageTitle="Basisbeginselen van de service Azure Batch | Microsoft Azure"
    description="Informatie over het gebruik van de Batch Azure-service voor grootschalige parallel en HPC werkbelasting"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Basisprincipes van Azure Batch

Azure Batch kunt u het efficiënt uitvoeren van toepassingen grootschalige parallelle en high-performance computing (HPC) in de cloud. Het is een platform-service die computerintensieve werk uit te voeren op een beheerde verzameling virtuele machines gepland en kan automatisch schaal berekenen resources aan de behoeften van uw taken.

Met de Batch-service definieert u Azure berekeningscluster resources voor het uitvoeren van uw toepassingen parallel en bij het op schaal. U kunt uitvoeren op aanvraag of geplande taken en u hoeft te handmatig maken, configureren en beheren een HPC-cluster, afzonderlijke virtuele machines, virtuele netwerken of een complexe taak en planning infrastructuur voor taken.

## <a name="use-cases-for-batch"></a>Gebruik dozen voor Batch

Batch is een beheerde Azure-service die wordt gebruikt voor *het verwerken van de batch* of *batch computing*--uitgevoerd van een grote hoeveelheid vergelijkbare taken om enkele het gewenste resultaat. Batch computing wordt meestal gebruikt door organisaties die regelmatig proces, transformeren en grote hoeveelheden gegevens analyseren.

Batch werkt ook met intrinsiek parallelle (ook wel ' embarrassingly parallel")-toepassingen en werkbelasting. Intrinsiek parallelle werkbelasting zijn eenvoudig splitsen in meerdere taken tegelijkertijd werken uitvoeren op meerdere computers.

![Parallelle taken][1]<br/>

Enkele voorbeelden van werkbelasting die vaak worden verwerkt met deze techniek zijn:

* Financiële risico modellering
* Klimaat en hydrologische gegevensanalyse
* Weergave van afbeeldingen, analyse en verwerking
* Media-codering en transcodering
* Genetische sequentie-analyse
* Technische functie belasting analyse
* Software testen

Batch kan ook parallelle berekeningen met een stap verkleinen aan het einde, en meer complexe HPC-werkbelastingen, zoals [Bericht Passing Interface (MPI)](batch-mpi.md) toepassingen uitvoeren.

Zie voor een vergelijking tussen de Batch en andere opties van de oplossing HPC in Azure wordt aangegeven, [Batch en HPC oplossingen](batch-hpc-solutions.md).

## <a name="developing-with-batch"></a>Ontwikkelen met Batch

Verwerking van parallelle werkbelasting met Batch wordt meestal gedaan via een programma met behulp van een van de [Batch API's](#batch-development-apis). Met de Batch API's, u maken en beheren van toepassingen van berekeningscluster knooppunten (virtuele machines) en taken plannen en taken uit te voeren op die knooppunten. Een clienttoepassing of de service die u creëert gebruikt de Batch-API's om te communiceren met de Batch-service.

U kunt efficiënt verwerken grootschalige werkbelasting voor uw organisatie of de front-end van een service bieden naar uw klanten, zodat ze kunnen worden uitgevoerd projecten en taken--op aanvraag of volgens een schema--op een, honderden of zelfs duizenden knooppunten. U kunt ook Batch gebruiken als onderdeel van een grotere werkstroom, beheerd door hulpprogramma's zoals [Factory van Azure-gegevens](../data-factory/data-factory-data-processing-using-batch.md).

> [AZURE.TIP] Wanneer u klaar om het verloop tot de API Batch voor een verder begrip van de mogelijkheden van het bent, raadpleegt u het [overzicht van de functie Batch voor ontwikkelaars](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Azure accounts die u nodig hebt

Wanneer u Batch oplossingen ontwikkelt, gebruikt u de volgende accounts in Microsoft Azure.

- **Azure-account en abonnementen** - als u al een Azure-abonnement niet hebt, kunt u uw [MSDN-abonnee profiteren]activeren[msdn_benefits], of zich registreren voor een [gratis Azure-account][free_account]. Wanneer u een account maakt, is een standaardabonnement voor u gemaakt.

- **Batch account** - wanneer uw toepassingen met de Batch-service, de naam van het account, de URL van het account en een toegangstoets worden gebruikt als referenties. Alle uw Batch resources zoals groepen, knooppunten, taken, berekenen en taken zijn gekoppeld aan een account Batch. U kunt de [Batch-account maken](batch-account-create-portal.md) in de portal van Azure.

- **Opslag account** - Batch beschikt over ingebouwde ondersteuning voor het werken met bestanden in de [Opslag van Azure][azure_storage]. Bijna elke Batch scenario gebruikt Azure opslag--waarin u de programma's die uw taken uitvoeren en de gegevens die verwerking tijdelijk opslaat en voor de opslag van uitvoergegevens die ze genereren. Zie [over Azure opslag-accounts](./../storage/storage-create-storage-account.md)maken van een opslag-account.

### <a name="batch-development-apis"></a>Batch ontwikkeling API 's

Uw toepassingen en -services kunnen oproepen direct REST API, gebruik een of meer van de volgende clientbibliotheken of een combinatie van beide voor het beheren van bronnen en uitvoeren parallelle taken bij het op schaal met de service Batch berekenen.

| API    | API-verwijzing | Downloaden | Voorbeelden van programmacode |
| ----------------- | ------------- | -------- | ------------ |
| **Batch REST** | [MSDN][batch_rest] | N/B | [MSDN][batch_rest] |
| **Batch .NET**    | [MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Batch Python**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Batch Node.js** | [github.IO][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Batch Java** (preview) | [github.IO][api_java] | [Maven][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Resourcebeheer batch

U kunt ook de volgende handelingen uit voor het beheren van resources binnen uw account Batch naast de client-API gebruiken.

- [PowerShell-cmdlets batch][batch_ps]: het Azure Batch cmdlets in de [Azure PowerShell](../powershell-install-configure.md) -module kunt u Batch resources met PowerShell beheren.

- [Azure CLI](../xplat-cli-install.md): het Azure opdrachtregel-Interface (Azure CLI) is een platforms set hulpmiddelen waarmee shell-opdrachten voor de communicatie met veel Azure services, met inbegrip van de Batch.

- Bibliotheek voor [Batch Management.NET](batch-management-dotnet.md) -client: ook verkrijgbaar via [NuGet][api_net_mgmt_nuget], kunt u de Batch Management .NET-client-bibliotheek voor het beheren van via programmacode Batch accounts, quota en toepassingspakketten. Overzicht van voor de bibliotheek management op [MSDN is][api_net_mgmt].

### <a name="batch-tools"></a>Hulpmiddelen voor batch

Terwijl u niet hoeft te maken van oplossingen met Batch, vindt u hier enkele zeer handige hulpmiddelen gebruiken terwijl bouwen en voor foutopsporing in de Batch toepassingen en services.

 - [Azure-portal][portal]: U kunt maken, controleren en verwijderen van toepassingen, taken en taken in de Azure portal Batch bladen Batch. U kunt de statusinformatie voor deze weergeven en andere resources terwijl u uw taken uitvoeren, en zelfs downloaden van bestanden van de knooppunten berekenen in uw toepassingen (downloaden van een mislukte taak `stderr.txt` tijdens het oplossen van, bijvoorbeeld). U kunt ook extern bureaublad-bestanden die u gebruiken kunt voor aanmelding bij het berekenen van knooppunten downloaden.

 - [Azure Batch Explorer][batch_explorer]: Batch Explorer biedt vergelijkbare Batch resource management functionaliteit biedt als de Azure-portal, maar in een zelfstandig product clienttoepassing Windows Presentation Foundation (WPF). Een van de Batch .NET steekproef toepassingen die beschikbaar zijn op [GitHub][github_samples], u kunt ook met de Visual Studio-2015 of hoger samenstellen en deze gebruiken om te bladeren en beheren van de resources in uw account Batch terwijl u ontwikkelen en fouten opsporen in uw Batch-oplossingen. Taak weergeven, toepassingen en taakgegevens, bestanden downloaden van knooppunten berekenen en verbinding maken met knooppunten op afstand via Remote Desktop (RDP)-bestanden die u kunt downloaden met Batch Verkenner.

 - [Microsoft Azure opslag Explorer][storage_explorer]: terwijl strikt een Azure Batch hulpmiddel voor de opslag-Explorer is nog een zeer handige functie om te laten terwijl u ontwikkelt en voor foutopsporing in uw Batch-oplossingen.

## <a name="scenario-scale-out-a-parallel-workload"></a>Scenario: Schaal van een parallelle werkbelasting

Een algemene oplossing op basis van de Batch-API's om te communiceren met de Batch-service heeft betrekking op intrinsiek parallelle werk, zoals de weergave van afbeeldingen voor 3D-schermen--op een groep berekeningscluster knooppunten schalen. Deze groep berekeningscluster knooppunten uw "render-farm" kan zijn dat vindt u tientallen, honderden of zelfs duizenden cores aan uw taak weergave, bijvoorbeeld.

In het volgende diagram ziet u een algemene Batch werkstroom, met een clienttoepassing of een gehoste service Batch met voor het uitvoeren van een parallelle werkbelasting.

![Batch oplossing werkstroom][2]

In dit scenario algemene wordt een rekenkundige werkbelasting in Azure Batch in uw toepassing of service verwerkt door de volgende stappen uit te voeren:

1. De **invoer bestanden** en de **toepassing** die worden verwerkt door de bestanden die bij uw account Azure Storage uploaden. De invoer bestanden kunnen worden alle gegevens die worden verwerkt door uw toepassing, zoals financiële modellen gegevens of videobestanden moeten getranscodeerd. Bestanden voor de toepassing kunnen een toepassing die wordt gebruikt voor het verwerken van de gegevens, zoals een 3D-weergave-toepassing of media transcoder zijn.

2. Een Batch- **groep** maken van berekeningscluster knooppunten in uw account Batch--deze knooppunten zijn de virtuele machines die wordt uitgevoerd van uw taken. U opgeven eigenschappen zoals het [knooppunt grootte](./../cloud-services/cloud-services-sizes-specs.md), het besturingssysteem en de locatie in Azure-opslag van de toepassing te installeren wanneer de knooppunten deelneemt aan de groep (de toepassing die u hebt geüpload in stap 1 #). U kunt ook de toepassingen wilt [schalen dat automatisch](batch-automatic-scaling.md)--dynamisch aanpassen van het aantal knooppunten berekenen in de groep--in antwoord op de werklast die uw taken genereren.

3. Maak een Batch **taak** om uit te voeren van de werklast op berekeningscluster knooppunten in de groep. Wanneer u een taak maakt, kunt u deze koppelen aan een groep Batch.

4. **Taken** toevoegen aan de taak. Wanneer u taken aan een taak toevoegen, de Batch-service automatisch de taken worden gepland voor uitvoering op de knooppunten berekenen in de groep. Elke taak gebruikt de toepassing die u hebt geüpload te verwerken van de invoer bestanden.

    - 4a. Voordat een taak is uitgevoerd, kan deze de gegevens (de invoer-bestanden) die aan de proces voor het berekenen knooppunt dat is toegewezen aan downloaden. Als de toepassing niet al is geïnstalleerd op het knooppunt (Zie stap #2), kan worden gedownload hier in plaats daarvan. Wanneer de downloads die voltooid zijn, wordt de taken uitvoeren op hun toegewezen knooppunten.

5. Als de taken worden uitgevoerd, kunt u zoeken Batch om de voortgang van de taak en de bijbehorende taken. Uw clienttoepassing of service communiceert met de Batch-service via HTTPS en omdat u duizenden taken uitvoeren op duizenden berekeningscluster knooppunten controleren mogelijk, zorg ervoor dat u [de service Batch efficiënt query](batch-efficient-list-queries.md).

6. Als de taken is voltooid, kunnen ze hun resultaatgegevens uploaden naar Azure Storage. U kunt ook bestanden rechtstreeks vanuit berekeningscluster knooppunten ophalen.

7. Wanneer uw monitoring vastgesteld dat de taken in uw project hebt voltooid, kunt u de uitvoergegevens voor nadere verwerking of evaluatie met uw clienttoepassing of service downloaden.

Houd er rekening mee dit alleen een manier om te gebruiken van de Batch is en dit scenario wordt slechts een paar van de beschikbare functies beschreven. Bijvoorbeeld, kunt u [meerdere taken parallel](batch-parallel-node-tasks.md) uitvoeren op elk knooppunt berekenen en u kunt [voorbereiden en de voltooiing van projecttaken](batch-job-prep-release.md) gebruiken om de knooppunten voorbereiden voor uw taken en klik daarna opschonen.

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht van de Batch-service hebt, is het tijd aan de afdruk om te leren hoe u deze verwerkingstijd van uw computerintensieve parallelle werkbelasting kunt gebruiken.

- Lees de [Batch Functieoverzicht voor ontwikkelaars](batch-api-basics.md), alle noodzakelijke informatie voor iedereen voorbereiden voor gebruik van de Batch. Het artikel bevat meer gedetailleerde informatie over de Batch serviceresources zoals groepen, knooppunten, projecten en taken en de vele API-functies die u gebruiken kunt tijdens het maken van uw Batch-toepassing.

- [Aan de slag met de bibliotheek Azure Batch voor .NET](batch-dotnet-get-started.md) voor meer informatie over het gebruik van C# en de Batch .NET-bibliotheek voor het uitvoeren van een eenvoudige werkbelasting met een algemene Batch-werkstroom. In dit artikel moet een van uw eerste stopt terwijl het leren hoe u het gebruik van de Batch-service. Er is ook een [Python versie](batch-python-tutorial.md) van de zelfstudie.

- Download de [voorbeelden van de code op GitHub] [ github_samples] om te zien hoe zowel C# en Python kan verbinding maken met Batch naar planning en proces steekproef werkbelasting.

- Bekijk de [Batch leerpad] [ learning_path] direct na een idee van de bronnen beschikbaar voor u u leren werken met Batch.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
