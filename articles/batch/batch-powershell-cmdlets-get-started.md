<properties
   pageTitle="Aan de slag met Azure Batch PowerShell | Microsoft Azure"
   description="Krijg een snelle introductie over de Azure PowerShell-cmdlets die u kunt de Batch Azure-service beheren"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Aan de slag met Azure Batch PowerShell-cmdlets
U kunt met de Azure Batch PowerShell-cmdlets, uitvoeren en scripts veel van dezelfde taken die is uitgevoerd met de Batch-API's, de Azure-portal en de Azure-Interface met opdrachtregel (CLI). Dit is een snelle introductie over de cmdlets die u gebruiken kunt voor het beheren van uw accounts Batch en werken met uw resources Batch zoals groepen, taken en taken.

Zie voor een volledige lijst van Batch cmdlets en gedetailleerde cmdlet-syntaxis, de [verwijzing naar de cmdlets van Azure Batch](https://msdn.microsoft.com/library/azure/mt125957.aspx).

In dit artikel is gebaseerd op cmdlets in Azure PowerShell versie 3.0.0. Het is raadzaam om uw Azure PowerShell regelmatig om te profiteren van de service-updates en verbeteringen bij te werken.

## <a name="prerequisites"></a>Vereisten voor

De volgende bewerkingen Azure als PowerShell wilt gebruiken voor het beheren van uw resources Batch uitvoeren.

* [Installeren en configureren van Azure PowerShell](../powershell-install-configure.md)

* Voer de cmdlet **Login-AzureRmAccount** verbinding maken met uw abonnement (het Azure Batch cmdlets schip in de module Azure resourcemanager):

    `Login-AzureRmAccount`

* **Met de Batch provider naamruimte registreren**. Deze bewerking moet alleen worden uitgevoerd **eenmaal per abonnement**.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Batchaccounts en toetsen beheren

### <a name="create-a-batch-account"></a>Een Batch-account maken

**Nieuw-AzureRmBatchAccount** Hiermee maakt u een account Batch in een opgegeven resourcegroep. Als u een resourcegroep nog geen hebt, maakt u een door de cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) uit te voeren. Geef een van de Azure regio's met de parameter **locatie** , zoals "Central US". Bijvoorbeeld:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Maak een account Batch vervolgens in de resourcegroep, geven een naam voor het account weergegeven in <*accountnaam*> en de locatie en naam van uw resourcegroep. De Batch-account kan enige tijd duren om te voltooien. Bijvoorbeeld:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] De naam van het Batch-account moet uniek zijn voor de regio van de Azure voor de resourcegroep, tussen 3 en 24 tekens bevatten en kleine letters en cijfers alleen gebruiken.

### <a name="get-account-access-keys"></a>Toegangstoetsen account ophalen
**Get-AzureRmBatchAccountKeys** ziet u de toegangstoetsen die is gekoppeld aan een Batch Azure-account. Voer bijvoorbeeld de volgende handelingen uit als u de primaire en secundaire sleutels van het account dat u hebt gemaakt.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Een nieuwe access-code genereren
**Nieuw-AzureRmBatchAccountKey** genereert een nieuwe primaire of secundaire account-code voor een Batch Azure-account. Typ bijvoorbeeld het volgende als u wilt een nieuwe primaire sleutel voor uw account Batch genereren:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Als u wilt een nieuwe secundaire sleutel genereren, Geef "Secundaire" voor de parameter **KeyType** . U moet de primaire en secundaire sleutels los te genereren.

### <a name="delete-a-batch-account"></a>Een account Batch verwijderen
**Verwijderen-AzureRmBatchAccount** Hiermee verwijdert u een Batch-account. Bijvoorbeeld:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Wanneer u wordt gevraagd, bevestig dat u wilt verwijderen van het account. Account verwijderen kan sommige tijd in beslag nemen.

## <a name="create-a-batchaccountcontext-object"></a>Een object BatchAccountContext maken

Als u wilt verifiëren met behulp van de Batch PowerShell-cmdlets wanneer u maken en beheren van Batch van toepassingen, taken, taken en andere bronnen, moet u eerst een object BatchAccountContext om op te slaan uw accountnaam en toetsen maken:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

U doorgeven het object BatchAccountContext naar cmdlets waarmee de parameter **BatchContext** gebruiken.

> [AZURE.NOTE] Standaard de primaire sleutel van het account wordt gebruikt voor verificatie, maar u kunt de toets gebruiken door het wijzigen van uw BatchAccountContext **KeyInUse** eigenschap expliciet selecteren: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Maken en wijzigen van de Batch resources
Gebruik de cmdlets zoals **New-AzureBatchPool**, **Nieuw-AzureBatchJob**en **Nieuw-AzureBatchTask** resources onder een Batch-account maken. Er zijn die **Get -** en **Set -** cmdlets om de eigenschappen van bestaande resources bijwerken en **verwijderen -** cmdlets als u wilt verwijderen van resources onder een Batch-account.

Wanneer u veel van deze cmdlets, naast het doorgeven van een object BatchContext gebruikt die u wilt maken of objecten die gedetailleerde broninstellingen bevatten, doorgeeft, zoals wordt weergegeven in het volgende voorbeeld. Zie de gedetailleerde help voor elke cmdlet voor meer voorbeelden.

### <a name="create-a-batch-pool"></a>Een resourcegroep die Batch maken

Bij het maken of bijwerken van een resourcegroep die Batch, selecteert u de configuratie van een cloud-service of een virtuele machineconfiguratie voor het besturingssysteem op de berekeningscluster knooppunten (Zie [overzicht van de functie Batch](batch-api-basics.md#pool)). Uw keuze bepaalt of uw knooppunten berekeningscluster image moet worden gemaakt met een van de [Azure Gast OS loslaat](../cloud-services/cloud-services-guestos-update-matrix.md#releases) of met een van de ondersteunde Linux of Windows-VM afbeeldingen in de Azure Marketplace.

Wanneer u een **Nieuw-AzureBatchPool**uitvoert, geeft u de instellingen van het besturingssysteem gebruikt in een PSCloudServiceConfiguration of PSVirtualMachineConfiguration object. Bijvoorbeeld de volgende cmdlet Hiermee maakt u een nieuwe Batch toepassingen grootte kleine berekeningscluster knooppunten in de cloud service te configureren, image gemaakt met de nieuwste versie van het besturingssysteem van familie 3 (Windows Server 2012). Hier geeft de parameter **CloudServiceConfiguration** de variabele *$configuration* als het object PSCloudServiceConfiguration. De parameter **BatchContext** geeft een eerder gedefinieerde variabele *$context* als het object BatchAccountContext.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Het doel aantal berekeningscluster knooppunten in de nieuwe groep wordt bepaald door een formule autoscaling. In dit geval de formule is gewoon **$TargetDedicated = 4**, waarin wordt aangegeven dat het aantal knooppunten berekenen in de groep is 4 maximaal.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Query voor groepen, taken, taken en andere details

Gebruik de cmdlets zoals **Get-AzureBatchPool**, **Get-AzureBatchJob**en **Get-AzureBatchTask** op query voor entiteiten onder een Batch-account hebt gemaakt.

### <a name="query-for-data"></a>Query voor gegevens

Gebruik bijvoorbeeld **Get-AzureBatchPools** om uw toepassingen. Standaard wordt in deze query's voor alle toepassingen bij uw account, ervan uitgaande dat u al het object BatchAccountContext opgeslagen in *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Een OData-filter gebruiken

U kunt een OData-filter de parameter **Filter** gebruiken om te vinden van alleen de objecten waarin u geïnteresseerd bent opgeven. U kunt bijvoorbeeld alle toepassingen vinden met id's beginnen met 'myPool':

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Deze methode is niet zo soepel met behulp van 'Waar-Object' in een lokale pijplijn. Echter wordt de query verzonden naar de service Batch rechtstreeks zodat alle filters gebeurt er op de server, internetbandbreedte op te slaan.

### <a name="use-the-id-parameter"></a>Gebruik de parameter Id

Een alternatief voor een OData-filter is via de parameter **Id** . Om query's voor een specifieke groep met id "myPool":

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

De parameter **Id** ondersteunt alleen volledige-id-zoekactie, niet jokertekens of stijl van een OData-filters.

### <a name="use-the-maxcount-parameter"></a>Gebruik van de parameter MaxCount

Elke cmdlet retourneert standaard maximaal 1000 objecten. Als u deze limiet is bereikt, Verfijn uw filter weer terug minder objecten of met de parameter **MaxCount** maximaal expliciet zijn ingesteld. Bijvoorbeeld:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Als u wilt verwijderen de bovengrens, stel **MaxCount** 0 of minder.

### <a name="use-the-powershell-pipeline"></a>Gebruik van de pijplijn PowerShell

Batch cmdlets kunt gebruikmaken van de pijplijn PowerShell om gegevens tussen cmdlets te verzenden. Dit heeft hetzelfde resultaat als een parameter opgeven, maar het werken met meerdere entiteiten vergemakkelijkt.

Bijvoorbeeld, zoeken en alle taken onder uw account weer te geven:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Opnieuw starten (opnieuw opstarten) elk berekeningscluster knooppunt in een groep:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Pakket Toepassingsbeheer

Toepassingspakketten om er zelf een vereenvoudigd toe om te implementeren toepassingen naar de knooppunten berekenen in uw toepassingen. Met de Batch PowerShell-cmdlets, kunt u uploaden en beheren van toepassingspakketten in uw account Batch en pakket versies om te berekenen van knooppunten implementeren.

**Maken** van een toepassing:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Toevoegen** het toepassingspakket van een:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Stel de **standaardversie** voor de toepassing:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Lijst** van een toepassing-pakketten

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Verwijder** het toepassingspakket van een

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Verwijder** een toepassing

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Voordat u de toepassing verwijderen, moet u alle van een toepassing toepassing pakket versies verwijderen. U ontvangt een foutbericht als u probeert te verwijderen van een toepassing die momenteel is van toepassingspakketten.

### <a name="deploy-an-application-package"></a>Het toepassingspakket van een implementeren

Wanneer u een groep maakt, kunt u een of meer toepassingspakketten voor implementatie opgeven. Wanneer u een pakket bij het maken van toepassingen opgeeft, wordt deze geïmplementeerd op elk knooppunt als het knooppunt joins van toepassingen. Pakketten zijn ook geïmplementeerd wanneer een knooppunt wordt opnieuw gestart of reimaged.

Geef de `-ApplicationPackageReference` option bij het maken van een groep om het toepassingspakket van een implementeren naar van de groep knooppunten, terwijl ze deelnemen aan de groep. Maak eerst een object **PSApplicationPackageReference** , en configureert u deze met de toepassing-Id en pakket versie die u wilt implementeren naar een van de groep berekeningscluster knooppunten:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Nu de groep maken en geef het pakket reference-object als het argument voor de `ApplicationPackageReferences` optie:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

U vindt meer informatie over de toepassingspakketten in [toepassing-implementatie waarbij Azure Batch toepassing-pakketten](batch-application-packages.md).

>[AZURE.IMPORTANT] [Koppeling een Azure Storage-account](#linked-storage-account-autostorage) moet u bij uw account Batch toepassingspakketten gebruiken.

### <a name="update-a-pools-application-packages"></a>Een resourcegroep die van toepassing-pakketten bijwerken

Als u de toepassingen die zijn toegewezen aan een bestaande toepassingen bijwerken, moet u eerst een PSApplicationPackageReference-object maken met de gewenste eigenschappen (toepassing-Id en pakket versie):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Vervolgens krijgen van de groep van Batch, alle bestaande-pakketten ruimen onze nieuwe pakket verwijzing toevoegen en bijwerken van de Batch-service met de instellingen voor de nieuwe:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

U hebt nu de eigenschappen van de groep in de Batch-service bijgewerkt. Als u wilt dat is wel het nieuwe toepassingspakket om te berekenen van knooppunten in de groep implementeert, echter moet u opnieuw opstart of reimage die knooppunten. U kunt elk knooppunt opnieuw starten in een groep met deze opdracht:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] U kunt meerdere toepassingspakketten implementeren naar de berekeningscluster knooppunten in een groep. Als u *toevoegen* het toepassingspakket van een in plaats van de bestaande pakketten vervangen wilt, weglaten de `$pool.ApplicationPackageReferences.Clear()` regel erboven.

## <a name="next-steps"></a>Volgende stappen

* Zie [Azure Batch cmdlet-naslaginformatie](https://msdn.microsoft.com/library/azure/mt125957.aspx)voor de syntaxis van de gedetailleerde cmdlet en voorbeelden.

* Zie voor meer informatie over toepassingen en -toepassingspakketten in de Batch, [toepassing-implementatie waarbij Azure Batch toepassing-pakketten](batch-application-packages.md).
