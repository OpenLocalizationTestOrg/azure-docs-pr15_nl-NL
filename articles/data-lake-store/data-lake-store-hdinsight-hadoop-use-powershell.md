<properties
   pageTitle="HDInsight clusters maken met Azure Lake gegevensopslag via PowerShell | Azure"
   description="Azure-PowerShell gebruiken om te maken en gebruiken van HDInsight clusters met Azure gegevens Lake"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Een HDInsight cluster met Lake gegevensopslag via Azure PowerShell maken

> [AZURE.SELECTOR]
- [Met behulp van Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Via PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Resourcemanager gebruiken](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Leer hoe u Azure PowerShell gebruiken voor het configureren van een cluster HDInsight (Hadoop, HBase of Storm) met toegang tot Azure Lake gegevensopslag. Enkele belangrijke overwegingen voor deze release:

* **Voor een clusters (Linux), en Hadoop/Storm clusters (Windows en Linux)**, de Lake gegevensopslag kunnen alleen worden gebruikt als een extra opslagruimte-account. Het standaardaccount voor de opslag voor de dergelijke clusters nog steeds Azure opslag BLOB's (WASB).

* **Voor HBase clusters (Windows en Linux)**, de Lake gegevensopslag kunnen worden gebruikt als een standaard-opslag of extra opslagruimte.

> [AZURE.NOTE] Enkele belangrijke punten met. 
> 
> * Optie voor het maken van HDInsight clusters met toegang tot Lake gegevensopslag is alleen beschikbaar voor HDInsight versie 3,2 en 3.4 (voor Hadoop, HBase en Storm clusters op Windows, evenals Linux). Deze optie is alleen beschikbaar op HDInsight 3.4 clusters voor een kolomgroepen op Linux.
>
> * Bovengenoemde, is Lake gegevensopslag is beschikbaar als de standaard-opslag voor bepaalde clustertypen (HBase) en extra opslagruimte voor andere clustertypen (Hadoop, een, Storm). Gegevensopslag Lake gebruiken als een extra opslagruimte-account heeft geen gevolgen voor prestaties of de mogelijkheid om te lezen/schrijven voor de opslag van het cluster. In een scenario voor waar Lake gegevensopslag wordt gebruikt als extra opslagruimte worden cluster-bestanden (zoals Logboeken, enzovoort) naar de standaard-opslag (Azure BLOB's), geschreven, terwijl de gegevens die u wilt verwerken in een account voor gegevensopslag Lake kan worden opgeslagen.


In dit artikel inrichten we een Hadoop-cluster met Lake gegevensopslag als extra opslagruimte.

HDInsight voor gebruik met Lake gegevensopslag configureren omvat via PowerShell de volgende stappen:

* Een Azure-gegevensopslag Lake maken
* Verificatie voor rollen gebaseerde toegang tot Lake gegevensopslag instellen
* HDInsight cluster met verificatie in Lake gegevensopslag maken
* Een testtaak uitvoeren op het cluster

## <a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell 1.0 of groter**. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

- **Windows SDK**. U kunt deze installeren vanaf [hier](https://dev.windows.com/en-us/downloads). Hiermee u maakt een beveiligingscertificaat.

- **Azure Active Directory Service Principal**. Stappen in deze zelfstudie bieden instructies voor het maken van een service principal in Azure AD. U moet wel een Azure AD-beheerder om te kunnen maken van een service principal. Als u een Azure AD-beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.
    
    **Als u niet een Azure AD-beheerder bent**, is het niet mogelijk om uit te voeren de benodigde stappen voor het maken van een service principal. In dat geval moet de beheerder van uw Azure AD eerst een service principal maken voordat u een HDInsight cluster met Lake gegevensopslag kunt maken. Bovendien moet de hoofdsom service worden gemaakt met behulp van een certificaat, zoals beschreven op [een service principal met certificaat maken](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate). 


## <a name="create-an-azure-data-lake-store"></a>Een Azure-gegevensopslag Lake maken

Volg deze stappen om het maken van een Lake gegevensopslag.

1. Een nieuw Azure PowerShell-venster openen vanaf uw bureaublad, en voer de volgende fragment. Wanneer u wordt gevraagd aan te melden, zorg er dan voor dat u zich hebt aangemeld als een van de admininistrators/eigenaar van het abonnement:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Als er een vergelijkbaar met foutbericht `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` wanneer u de provider van de resource Lake gegevensopslag registreert, is het mogelijk dat uw subsrcription niet whitelisted voor Azure Lake gegevensopslag is. Zorg ervoor dat u uw Azure-abonnement voor gegevensopslag Lake openbare preview inschakelen door deze [instructies](data-lake-store-get-started-portal.md#signup)te volgen.

3. Een account Azure Lake gegevensopslag is gekoppeld aan een resourcegroep Azure. Beginnen met het maken van een resourcegroep Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Een resourcegroep Azure maken] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Een resourcegroep Azure maken")

2. Maak een account Azure Lake gegevensopslag. De naam van het account dat u opgeeft moet alleen kleine letters en cijfers bevatten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Een account Lake van Azure-gegevens maken] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Een account Lake van Azure-gegevens maken")

3. Controleer of het account is gemaakt.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    De uitvoer hiervoor moet **waar**zijn.

4. Upload enkele voorbeeldgegevens naar Lake van Azure-gegevens. We gebruiken deze verderop in dit artikel om te bevestigen dat de gegevens toegankelijk zijn vanuit een cluster HDInsight zijn. Als u enkele voorbeeldgegevens uploaden zoekt, kunt u de map **Ambulances gegevens** krijgen van de [Azure gegevens Lake cijfer opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Verificatie voor rollen gebaseerde toegang tot Lake gegevensopslag instellen

Elke Azure-abonnement is gekoppeld aan een Azure Active Directory. Gebruikers en services die bronnen van het abonnement via de klassieke Azure-Portal of Azure resourcemanager API moeten eerst worden geverifieerd bij die Azure Active Directory. Toegang wordt verleend bij Azure abonnementen en -services door toe te wijzen deze de juiste rol op een Azure bron.  Voor services geeft een service principal de service in de Azure Active Directory (AAD). In dit gedeelte ziet u hoe u een toepassingsservice, zoals HDInsight, toegang tot een Azure resource (het account voor gegevensopslag voor Lake Azure u eerder hebt gemaakt) verlenen door te maken van een service principal voor de toepassing en rollen toewijzen aan die via Azure PowerShell.

Als u wilt Active Directory-verificatie voor Azure gegevens meer instellen, moet u de volgende taken uitvoeren.

* Een zelfondertekend certificaat maken
* Maken van een toepassing in Azure Active Directory en de hoofdsom van een Service

### <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

Zorg ervoor dat u hebt [Windows SDK](https://dev.windows.com/en-us/downloads) geïnstalleerd voordat u verder gaat met de stappen in deze sectie. U moet een adreslijst, zoals **C:\mycertdir**, waar het certificaat worden gemaakt ook hebt gemaakt.

1. Ga vanaf de PowerShell-venster naar de locatie waar u Windows SDK hebt geïnstalleerd (meestal `C:\Program Files (x86)\Windows Kits\10\bin\x86` en gebruikt u de [MakeCert] [ makecert] hulpprogramma voor het maken van een zelfondertekend certificaat en een persoonlijke sleutel. Gebruik de volgende opdrachten.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    U wordt gevraagd het wachtwoord voor persoonlijke sleutel invoeren. Nadat de opdracht met succes wordt uitgevoerd, ziet u een **CertFile.cer** en **mykey.pvk** in de certificaat-map die u hebt opgegeven.

4. Gebruik de [Pvk2Pfx] [ pvk2pfx] hulpprogramma voor het converteren van de PVK en .cer-bestanden die MakeCert gemaakt naar een .pfx-bestand. Voer de volgende opdracht.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Wanneer u wordt gevraagd het wachtwoord voor persoonlijke sleutel u eerder hebt opgegeven invoeren. De waarde die u voor de parameter **-IO opgeeft** is het wachtwoord dat is gekoppeld aan de .pfx-bestand. Nadat de opdracht is uitgevoerd, ziet u ook een CertFile.pfx in de certificaat-map die u hebt opgegeven.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Een Azure Active Directory en de hoofdsom van een service maken

In deze sectie voert u de stappen voor het maken van een service hoofdsom voor een Azure Active Directory-toepassing, een rol toewijzen aan de service principal en verifiëren als de service-principal doordat een certificaat. Voer de volgende opdrachten aan het maken van een toepassing in Azure Active Directory.

1. Plak de volgende cmdlets uit in het venster PowerShell-console. Controleer of de waarde die u voor de eigenschap **- Weergavenaam opgeeft** uniek is. Daarnaast de waarden voor **- Startpagina** en **-IdentiferUris** tijdelijke aanduiding voor waarden zijn en niet worden gecontroleerd.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Maken van een service principal met de toepassings-ID.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. De service principal toegang verlenen tot de gegevensopslag Lake bestand/tijdelijke map die u toegang uit het cluster HDInsight tot. Het codefragment van de onderstaande biedt toegang tot de hoofdmap van het account voor gegevensopslag Lake.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    Voer bij de prompt **Y** om te bevestigen.

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Maken van een HDInsight cluster met verificatie in Lake gegevensopslag

In dit gedeelte maken we een HDInsight Hadoop-cluster. Voor deze release, het cluster HDInsight en de gegevensopslag Lake moeten op dezelfde locatie (Oost VS 2).

1. Beginnen met het ophalen van het abonnement tenant-ID. Moet u die later.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Voor deze release, kunnen voor een cluster Hadoop Lake gegevensopslag alleen worden gebruikt als een extra opslagruimte voor het cluster. De standaard-opslag nog steeds niet voor de opslag van Azure BLOB's (WASB). Ja, gaan we het opslag-account en Opslagcontainers die zijn vereist voor het cluster eerst maken.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. Maak het cluster HDInsight. Gebruik de volgende cmdlets uit.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Nadat de cmdlet goed is voltooid, ziet u een uitvoer als volgt:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Testtaken uitvoeren op het cluster HDInsight gebruik van de Lake gegevensopslag

Nadat u een cluster HDInsight hebt geconfigureerd, kunt u testtaken kunt uitvoeren op het cluster om te testen of het cluster HDInsight toegang hebt tot Lake gegevensopslag. Klik hiervoor uitgevoerd we een steekproef component taak die een tabel met de voorbeeldgegevens die u eerder hebt geüpload naar uw Lake gegevensopslag maakt.

### <a name="for-a-linux-cluster"></a>Voor een cluster Linux

In deze sectie wordt u SSH in de cluster en uitvoeren de een voorbeeldquery component. Windows beschikt niet over een ingebouwde SSH-client. We raden u aan **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight van Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Zodra u verbinding hebt, start u de component CLI met behulp van de volgende opdracht uit:

        hive

2. De CLI gebruikt, voert u de volgende instructies om een nieuwe tabel met de naam **voertuigen** met behulp van de voorbeeldgegevens in de gegevensopslag Lake maken:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Hier ziet u een uitvoer ongeveer als volgt uit:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>Voor een Windows-cluster

Gebruik de volgende cmdlets uit de component query uit te voeren. In deze query we een tabel maken van de gegevens in de gegevensopslag Lake en voer een selectiequery op de tabel.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Dit heeft de volgende uitvoer. **ExitValue** 0 in de uitvoer stappen worden voorgesteld dat de taak is voltooid.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

De uitvoer van de taak ophalen met behulp van de volgende cmdlet:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

De taak-uitvoer lijkt op het volgende:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Access Lake gegevensopslag HDFS opdrachten gebruiken

Wanneer u het cluster HDInsight als u wilt gebruiken Lake gegevensopslag hebt geconfigureerd, kunt u de HDFS shell-opdrachten gebruiken voor toegang tot de store.

### <a name="for-a-linux-cluster"></a>Voor een cluster Linux

In dit gedeelte u SSH wordt in het cluster en voer de HDFS-opdrachten. Windows beschikt niet over een ingebouwde SSH-client. We raden u aan **stopverf**, dat kan worden gedownload van [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)gebruiken.

Zie voor meer informatie over het gebruik van stopverf, [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight van Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Zodra u verbinding hebt, gebruikt u de volgende opdracht uit HDFS bestandssysteem voor een overzicht van de bestanden in de Lake gegevensopslag.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Dit moet het bestand dat u eerder hebt geüpload naar de gegevensopslag Lake vermeld.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

U kunt ook de `hdfs dfs -put` aan sommige bestanden uploaden naar de gegevensopslag Lake en gebruikt u de opdracht `hdfs dfs -ls` om te controleren of de bestanden die zijn geüpload.


### <a name="for-a-windows-cluster"></a>Voor een Windows-cluster

1. Aanmelden bij de nieuwe [Azure-Portal](https://portal.azure.com).

2. Klik op **Bladeren**, klikt u op **HDInsight clusters**en klik vervolgens op het HDInsight cluster die u hebt gemaakt.

3. Klik in het blad cluster **Extern bureaublad**op en klik in het blad **Extern bureaublad** op **verbinding maken**.

    ![Externe HDI cluster] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Een resourcegroep Azure maken")

    Wanneer u wordt gevraagd, voert u de referenties die u hebt opgegeven voor de gebruiker van het externe bureaublad.

4. In de externe sessie, start van Windows PowerShell en gebruikt u de HDFS bestandssysteem-opdrachten voor een overzicht van de bestanden in de Azure Lake gegevensopslag.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    Dit moet het bestand dat u eerder hebt geüpload naar de gegevensopslag Lake vermeld.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    U kunt ook de `hdfs dfs -put` aan sommige bestanden uploaden naar de gegevensopslag Lake en gebruikt u de opdracht `hdfs dfs -ls` om te controleren of de bestanden die zijn geüpload.

## <a name="see-also"></a>Zie ook

* [Portal: Maak een cluster HDInsight als u wilt gebruiken Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
