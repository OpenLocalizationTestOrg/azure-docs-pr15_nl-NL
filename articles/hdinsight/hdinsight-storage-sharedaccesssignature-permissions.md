<properties
pageTitle="HDInsight toegang tot gegevens met behulp van Access handtekeningen gedeeld beperken"
description="Informatie over het gebruik van Access handtekeningen gedeeld HDInsight toegang tot gegevens die zijn opgeslagen in de opslag van Azure BLOB's te beperken."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Azure opslag gedeeld Access handtekeningen gebruiken voor het beperken van toegang tot gegevens met HDInsight

HDInsight opslag Azure BLOB's voor gegevensopslag wordt gebruikt. Terwijl HDInsight volledige toegang tot de blob gebruikt als standaard opslag voor het cluster hebt moet, kunt u machtigingen om de gegevens die zijn opgeslagen in andere BLOB's die worden gebruikt door het cluster kunt beperken. U wilt bijvoorbeeld dat sommige gegevens alleen-lezen. U kunt dit doen gedeeld Access handtekeningen gebruiken.

Gedeelde Access handtekeningen (SA's) zijn een functie van Azure opslag accounts waarmee u beperkingen instellen voor toegang tot gegevens. Bijvoorbeeld, zodat alleen-lezen toegang hebt tot gegevens. In dit document leert u hoe u met SA's lezen en lijst-lezentoegang aan een container blob inschakelen uit HDInsight.

##<a name="requirements"></a>Vereisten

* Een Azure-abonnement

* C# of Python. C#-voorbeeldcode wordt weergegeven als een Visual Studio-oplossing.

    * Visual Studio moet versie 2013 of 2015
    
    * Python moet versie 2,7 of hoger

* Een HDInsight Linux gebaseerde cluster of [Azure PowerShell] [ powershell] -als u een bestaand Linux gebaseerde cluster hebt, kunt u Ambari naar een Access gedeeld handtekening toevoegen aan het cluster. Als dat niet zo is, kunt u Azure PowerShell een nieuw cluster maken en toevoegen van een handtekening gedeeld Access gemaakt.

* De voorbeeldbestanden van [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Deze opslagplaats heeft de volgende:

    * Een Visual Studio-project dat een container opslag, opgeslagen beleid en SA's voor gebruik met HDInsight maken kunt
    
    * Een script Python die een container opslag, opgeslagen beleid en SA's voor gebruik met HDInsight maken kunt
    
    * Een PowerShell-script die u kunt een nieuw HDInsight cluster maken en configureren, zodat de SA's kan gebruiken.

##<a name="shared-access-signatures"></a>Gedeelde toegang handtekeningen

Er zijn twee soorten Access handtekeningen gedeeld:

* Ad hoc: de begintijd, verlooptijd en machtigingen voor de SA's worden alle opgegeven naar de SAS-URI (of impliciet, in de zaak waar begintijd wordt weggelaten, wordt).

* Opgeslagen-beleid: een opgeslagen-beleid is gedefinieerd op een container resource - blob container, tabel, wachtrij of bestandsshare - en kan worden gebruikt voor het beheren van de beperkingen voor een of meer gedeelde toegang handtekeningen. Wanneer u een SA's aan een opgeslagen-beleid koppelen, neemt de SA's over de beperkingen - de begintijd, verlooptijd en machtigingen - gedefinieerd voor het opgeslagen-beleid.

Het verschil tussen de twee formulieren belangrijk is voor één belangrijke scenario: intrekkingsreferenties. Een SA's is een URL, zodat iedereen die de SA's ontvangt, ongeacht het wie allereerst aangevraagd kunt gebruiken. Als een SA's openbaar is gepubliceerd, kan deze door iedereen in de wereld worden gebruikt. Een SA's dat is verdeeld is geldig tot vier dingen gebeurt:

1. De verlooptijd is opgegeven op de SA's is bereikt.

2. De verlooptijd is opgegeven in het opgeslagen--beleid waarnaar wordt verwezen door de SA's is bereikt (als een opgeslagen-beleid wordt verwezen en als de opgegeven een verlooptijd). Dit kan een optreden omdat het interval verstrijkt of omdat u het toegangsbeleid opgeslagen als u wilt dat een verlooptijd in het verleden, dat wil zeggen een manier om af te trekken van de SA's hebt gewijzigd.

3. Het opgeslagen access-beleid waarnaar wordt verwezen door de SA's wordt verwijderd, namelijk een andere manier om de SA's intrekken. Houd er rekening mee dat als u het opgeslagen-beleid met precies dezelfde naam opnieuw moet maken, alle bestaande SA's tokens wordt opnieuw geldig op basis van de machtigingen die zijn gekoppeld aan dat opgeslagen access-beleid (ervan uitgaande dat de verlooptijd op de SA's niet is verstreken). Als u de SA's intrekken zijn willen, moet u een andere naam te gebruiken als u het beleid van access met een verlooptijd in de toekomst opnieuw moet maken.

4. De accountsleutel die is gebruikt voor het maken van de SA's is gegenereerd. Houd er rekening mee dat dit doet, alle toepassingsonderdelen met dat account-toets wordt totdat ze worden bijgewerkt gebruik van de andere geldige accountsleutel of de zojuist geregenereerde accountsleutel verificatie mislukt.

> [AZURE.IMPORTANT] Een gedeelde access-handtekening URI is gekoppeld aan de accountsleutel waarmee u de handtekening hebt gemaakt en het bijbehorende opgeslagen-beleid (indien aanwezig). Als er geen opgeslagen-beleid is opgegeven, is de enige manier om in te trekken van een handtekening gedeelde toegang de accountsleutel wilt wijzigen. 

Het wordt aanbevolen dat u altijd gebruikmaken van het opgeslagen clienttoegangsbeleid, zodat u kunt handtekeningen intrekken of de vervaldatum uitbreiden naar wens. De stappen in dit document opgeslagen clienttoegangsbeleid met semiselectie SA's wilt maken.

Zie [informatie over het model SA's](../storage/storage-dotnet-shared-access-signature-part-1.md)voor meer informatie over gedeelde Access handtekeningen.

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Een opgeslagen-beleid maken en een SA's genereren

U moet een opgeslagen beleid momenteel via programmacode maken. U kunt zowel de C# en Python voorbeeld van het maken van een opgeslagen beleid en SA's bij [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature)vinden.

###<a name="create-a-stored-policy-and-sas-using-c"></a>Een opgeslagen beleid en SA's met behulp van C maken\#

1. Open de oplossing in Visual Studio.

2. Klik in Solution Explorer met de rechtermuisknop op het project __SASToken__ en selecteer __Eigenschappen__.

3. Selecteer __Instellingen__ en waarden toevoegen voor de volgende items:

    * StorageConnectionString: De verbindingsreeks voor de opslag-account dat u wilt maken van een opgeslagen beleid en SA's voor. De notatie moet `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` waar `myaccount` is de naam van uw account voor opslagruimte en `mykey` is de sleutel voor de opslag-account.
    
    * ContainerName: De container in het opslag-account dat u wilt beperken van toegang tot.
    
    * SASPolicyName: De naam wilt gebruiken voor het opgeslagen beleid die worden gemaakt.
    
    * FileToUpload: Het pad naar een bestand dat aan de container worden geüpload.
    
4. Voer het project. Een consolevenster wordt weergegeven en de volgende strekking informatie wordt weergegeven wanneer de SA's is gegenereerd:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Sla de SA's beleid token, zoals u dit moet bij de opslag-account koppelen aan uw cluster HDInsight. U moet ook de naam van het opslag-account en de containernaam van de.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Een opgeslagen beleid en SA's met Python maken

1. Open het bestand SASToken.py en wijzigen van de volgende waarden:

    * beleid\_naam: de naam voor het opgeslagen beleid die worden gemaakt.
    
    * opslag\_account\_naam: de naam van uw account opslag.
    
    * opslag\_account\_sleutel: de toets voor de opslag-account.
    
    * opslag\_container\_naam: de container in het opslag-account dat u wilt beperken van toegang tot.
    
    * voorbeeld\_bestand\_pad: het pad naar een bestand dat aan de container worden geüpload

2. Het script uitvoeren. De SA's van de volgende strekking token wordt weergegeven wanneer het script is voltooid:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Sla de SA's beleid token, zoals u dit moet bij de opslag-account koppelen aan uw cluster HDInsight. U moet ook de naam van het opslag-account en de containernaam van de.
    
##<a name="use-the-sas-with-hdinsight"></a>De SA's gebruiken met HDInsight

Wanneer u een cluster HDInsight maakt, moet u een primaire opslag-account en kunt u desgewenst extra opslagruimte accounts opgeven. Beide methoden van het toevoegen van opslag is vereist voor volledige toegang tot de opslag en containers die worden gebruikt.

Om te kunnen gebruiken een handtekening Access gedeeld toegang tot een container wilt beperken, moet u een aangepaste vermelding toevoegen aan de configuratie van de __core-site__ voor het cluster.

* Voor __Windows-__ of __Linux-gebaseerde__ HDInsight kolomgroepen kunt u dit doen tijdens het maken van het cluster via PowerShell.

* Voor __Linux gebaseerde__ HDInsight kolomgroepen kunt u de configuratie na het maken van het cluster met Ambari wijzigen.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Maak een nieuw cluster waarin de SA 's

Een voorbeeld van het maken van een HDInsight cluster waarin de SA's is opgenomen in de `CreateCluster` map van de bibliotheek. Als u wilt gebruiken, gebruikt u de volgende stappen:

1. Open de `CreateCluster\HDInsightSAS.ps1` bestand in een teksteditor en wijzigt u de volgende waarden aan het begin van het document.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Bijvoorbeeld wijzigen `'mycluster'` op de naam van het cluster die u wilt maken. De waarden SA's heeft moeten overeenkomen met de waarden uit de vorige stappen bij het maken van een account voor opslagruimte en SA's token.
    
    Nadat u de waarden hebt gewijzigd, moet u het bestand opslaan.

1. Open een nieuwe Azure PowerShell-prompt. Als u niet bekend met Azure PowerShell bent of niet hebt geïnstalleerd, raadpleegt u [installeren en configureren van Azure PowerShell][powershell].

2. Gebruik van de prompt de volgende handelingen uit om te verifiëren bij uw Azure-abonnement:

        Login-AzureRmAccount
    
    Wanneer u wordt gevraagd, meld u aan met het account voor uw Azure-abonnement.
    
    Als uw aanmelding gekoppeld aan meerdere Azure abonnementen is, moet u mogelijk gebruiken `Select-AzureRmSubscription` om te selecteren van het abonnement dat u wilt gebruiken.

2. Wijzigen van de prompt mappen naar de `CreateCluster` map waarin het bestand HDInsightSAS.ps1. Gebruikt u de volgende handelingen uit het script uitvoeren
        
        .\HDInsightSAS.ps1
    
    Als het script wordt uitgevoerd, wordt deze uitvoer Meld u aan de PowerShell-prompt zoals dat groeperen en opslag accounts wordt de resource gemaakt. Dit wordt u gevraagd om in te voeren van de HTTP-gebruiker voor het cluster HDInsight. Dit is het gebruikersaccount gebruikt om te beveiligen HTTP/s toegang tot het cluster.
    
    Als u een cluster Linux gebaseerde maakt, wordt u ook gevraagd om een SSH gebruikersnaam en wachtwoord. Dit wordt gebruikt om extern aanmeldt bij het cluster.
    
    > [AZURE.IMPORTANT] Wanneer u hierom wordt gevraagd om de HTTP/s of SSH-gebruikersnaam en wachtwoord, moet u een wachtwoord dat voldoet aan de volgende criteria voldoet opgeven:
    >
    > - Moet ten minste 10 tekens
    > - Minimaal één cijfer moet bevatten
    > - Moet ten minste één niet-alfanumerieke tekens bevatten
    > - Moet ten minste één hoofdletters of kleine letter bevatten


Het duurt lang voordat deze script om te voltooien, meestal ongeveer 15 minuten. Als het script is voltooid zonder fouten, is het cluster gemaakt.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Bijwerken van een bestaand cluster als u wilt de SA's gebruiken

Als u een bestaand Linux gebaseerde cluster hebt, kunt u de SA's toevoegen aan de configuratie __core-site__ met behulp van de volgende stappen uit:

1. Open het web Ambari UI voor uw cluster. Het adres voor deze pagina is https://YOURCLUSTERNAME.azurehdinsight.net. Wanneer u wordt gevraagd, worden geverifieerd bij het gebruik van de naam van de beheerder (admin) en het wachtwoord die u hebt gebruikt bij het maken van het cluster cluster.

2. Vanaf de linkerkant van het web Ambari UI, selecteer __HDFS__ en selecteer vervolgens het tabblad __configuraties__ in het midden van de pagina.

3. Selecteer het tabblad __Geavanceerd__ en schuif vervolgens totdat u de sectie __aangepast core-site__ .

4. Vouw de sectie __aangepast core-site__ en Ga naar het einde en selecteer de koppeling __... eigenschap toevoegen__ . Gebruik de volgende waarden voor de velden __sleutel__ en __waarde__ :

    * __Sleutel__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Waarde__: de SA's die het resultaat van de C# of Python-toepassing die u eerder hebt uitgevoerd.
    
    __CONTAINERNAME__ vervangen door de containernaam die u met de C# of SA's toepassing gebruikt. __STORAGEACCOUNTNAME__ vervangen door de naam van het opslag-account die u gebruikt.

5. Klik op de knop __toevoegen__ om op te slaan deze sleutel en de waarde en klik op de knop __Opslaan__ als de configuratiewijzigingen wilt opslaan. Wanneer u wordt gevraagd, een beschrijving van de wijziging ('toe te voegen SA's opslag access' bijvoorbeeld) toevoegen en klik vervolgens op __Opslaan__.

    Klik op __OK__ wanneer u de wijzigingen zijn voltooid.

    > [AZURE.IMPORTANT] Hiermee wordt de configuratiewijzigingen opgeslagen, maar u moet meerdere services opnieuw starten voordat de wijziging van kracht.

6. In de webversie van het Ambari UI, __HDFS__ Selecteer in de lijst aan de linkerkant en selecteer vervolgens __Alle opnieuw__ in de __Service-acties__ vervolgkeuzelijst aan de rechterkant. Wanneer u wordt gevraagd, selecteer __Onderhoudsmodus inschakelen__ en vervolgens Selecteer alle opnieuw __Conform".

    Herhaal deze procedure voor de MapReduce2 en garens posten in de lijst aan de linkerkant van de pagina.

7. Nadat deze opnieuw hebt opgestart, elk item afzonderlijk selecteren en schakel vervolgkeuzelijst voor aggregaatfuncties onderhoudsmodus uit de __Acties van de Service__ .

##<a name="test-restricted-access"></a>Beperkte toegang testen

Gebruik de volgende methoden om te bevestigen dat u toegang hebt beperkt:

* Voor __Windows gebaseerde__ HDInsight kolomgroepen extern bureaublad verbinding maken met het cluster te gebruiken. Zie [verbinding met het gebruik van RDP HDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) voor meer informatie.

    Wanneer u verbinding hebt, open een opdrachtprompt via de __opdrachtregel Hadoop__ -pictogram op het bureaublad.

* Gebruik voor __Linux gebaseerde__ HDInsight kolomgroepen SSH verbinding maken met de cluster. Zie een van de volgende onderwerpen voor informatie over het gebruik van SSH met Linux gebaseerde clusters:

    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight uit Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Zodra u verbinding hebt met het cluster, gebruikt u de volgende stappen uit om te bevestigen dat u alleen lezen en lijst met items op het account van de opslagruimte SA's kunt:

1. Typ het volgende uit de prompt. __SASCONTAINER__ vervangen door de naam van de container gemaakt voor het account van de opslagruimte SA's. __SASACCOUNTNAME__ vervangen door de naam van de opslag-account gebruikt voor de SA's:

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    Hiermee wordt een lijst met de inhoud van de container, waaronder het bestand dat is geüpload moet waarop de container en SA's is gemaakt.
    
2. Gebruik de volgende handelingen uit om te bevestigen dat u de inhoud van het bestand kunt lezen. Vervang de __SASCONTAINER__ en __SASACCOUNTNAME__ zoals in de vorige stap. __Bestandsnaam__ vervangen door de naam van het bestand dat wordt weergegeven in de vorige opdracht:

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    Hiermee wordt een lijst met de inhoud van het bestand.
    
3. Gebruik de volgende handelingen uit om te downloaden van het bestand naar het lokale bestandssysteem:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    Hiermee wordt het bestand naar een lokaal bestand met de naam __testbestand.txt__downloaden.

4. Gebruik de volgende handelingen uit het lokale bestand uploaden naar een nieuw bestand met de naam __testupload.txt__ op de opslag SA's:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    Ontvangt u een bericht ongeveer als volgt uit:
    
        put: java.io.IOException
        
    Deze fout treedt op omdat de opslaglocatie van gelezen + alleen lijst. Gebruik de volgende handelingen uit in de gegevens op de standaard-opslag voor het cluster, dat wil schrijfbare zeggen plaatsen:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Schakel ditmaal voltooid de bewerking.
    
##<a name="troubleshooting"></a>Problemen oplossen

###<a name="a-task-was-canceled"></a>Een taak is geannuleerd

__Symptomen__: wanneer u een cluster met het PowerShell-script maakt, verschijnt het volgende foutbericht weergegeven:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__Oorzaak__: deze fout kan optreden als u een wachtwoord voor de beheerder/HTTP-gebruiker voor het cluster of (voor Linux gebaseerde kolomgroepen gebruikt) de gebruiker SSH.

__Resolutie__: Typ een wachtwoord dat voldoet aan de volgende criteria voldoet:

- Moet ten minste 10 tekens
- Minimaal één cijfer moet bevatten
- Moet ten minste één niet-alfanumerieke tekens bevatten
- Moet ten minste één hoofdletters of kleine letter bevatten

##<a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe beperkte toegang opslag toevoegen aan uw cluster HDInsight, informatie over andere manieren om te werken met gegevens in uw cluster:

* [Component gebruiken met HDInsight](hdinsight-use-hive.md)

* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md
