<properties 
    pageTitle="Gegevens verplaatsen naar SQL Server op een Azure virtuele machine | Azure" 
    description="Verplaats gegevens van alle bestanden in of uit een lokale SQL Server naar SQL Server op Azure VM." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Verplaats gegevens naar SQL Server op een Azure virtuele machines

Dit onderwerp worden de opties voor het verplaatsen van gegevens van alle bestanden in (CSV- of TSV opmaken) of vanuit een on-premises SQL-Server naar SQL Server op een Azure virtuele machine. Deze taken voor het verplaatsen van gegevens in de cloud maken deel uit van het Team gegevens wetenschap proces.

Voor een onderwerp omtrek van de opties voor het verplaatsen van gegevens naar een Azure SQL-Database voor Machine Learning, raadpleegt u de [gegevens met een Azure SQL-Database voor Azure Machine Learning verplaatsen](machine-learning-data-science-move-sql-azure.md).

Het **menu** onder koppelingen naar onderwerpen waarin wordt uitgelegd hoe u het nemen van gegevens in andere doel-omgevingen waar de gegevens kunnen worden opgeslagen en verwerkt tijdens het Team gegevens wetenschap proces (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


De volgende tabel worden de opties voor het verplaatsen van gegevens naar SQL Server op een Azure virtuele machine.

<b>BRON</b> |<b>DOEL: SQL Server Azure VM</b> |
------------------ |-------------------- |
<b>Plat bestand</b> |1. <a href="#insert-tables-bcp">opdrachtregel bulksgewijs kopiëren hulpprogramma (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">bulksgewijs invoegen SQL-Query</a><br> 3. <a href="#sql-builtin-utilities">grafische ingebouwde hulpprogramma's in SQL Server</a>
<b>On-Premises SQL Server</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Deploy een SQL Server-Database naar de wizard van een Microsoft Azure VM</a><br> 2. <a href="#export-flat-file">met een plat bestand exporteren</a><br> 3. <a href="#sql-migration">Wizard migratie SQL-Database</a> <br> 4. <a href="#sql-backup">database back-en herstellen</a><br>

Houd er rekening mee dat dit document wordt aangenomen dat de SQL-opdrachten uit SQL Server Management Studio of Visual Studio Database Explorer worden uitgevoerd.

> [AZURE.TIP] Als alternatief, kunt u [Azure gegevens Factory](https://azure.microsoft.com/services/data-factory/) maken en plannen van een pijplijn die gegevens naar een SQL Server-VM op Azure verplaatsen. Zie [kopiëren van gegevens met Azure gegevens Factory (kopie activiteit)](../data-factory/data-factory-data-movement-activities.md)voor meer informatie.


## <a name="prereqs"></a>Vereisten voor
Deze zelfstudie wordt ervan uitgegaan dat u hebt:

* Een **Azure-abonnement**. Als u niet een abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een **account van Azure opslag**. Voor het opslaan van de gegevens in deze zelfstudie gebruikt u een account Azure opslag. Als u niet een Azure opslag-account hebt, raadpleegt u het artikel [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) . Nadat u het opslag-account hebt gemaakt, moet u de accountsleutel gebruikt voor toegang tot de opslag verkrijgt. Zie de [weergave, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Ingerichte **SQL Server op een Azure VM**. Zie [een VM Azure SQL Server als een server IPython notitieblok voor geavanceerde analyses instellen](machine-learning-data-science-setup-sql-server-virtual-machine.md)voor instructies.
* Geïnstalleerd en geconfigureerd **Azure PowerShell** lokaal. Zie voor instructies voor het [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Gegevens uit de bron van een plat bestand verplaatsen naar SQL Server op een Azure-VM

Als uw gegevens zich in een plat bestand (gerangschikt in een indeling met rijen/kolommen), kan worden verplaatst naar SQL Server VM op Azure via de volgende methoden:

1. [Opdrachtregel bulksgewijs kopiëren hulpprogramma (BCP)](#insert-tables-bcp) 
2. [Bulksgewijs invoegen SQL-Query](#insert-tables-bulkquery)
3. [Grafische ingebouwde hulpprogramma's in SQL Server (importeren/exporteren, SSIS)](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Opdrachtregel bulksgewijs kopiëren hulpprogramma (BCP)

BCP wordt geïnstalleerd met SQL Server en is een van de snelste manieren om gegevens te verplaatsen. Dit werkt in alle drie SQL Server varianten (On-premises SQL Server, SQL Azure wordt aangegeven en SQL Server VM op Azure). 

> [AZURE.NOTE]**Waar moet mijn gegevens worden voor BCP?**  
> Het is niet verplicht, kan dat bestanden met brongegevens die zich op dezelfde computer als de doel-SQL server voor snellere overdrachten (netwerk snelheid tegenover lokale schijf IO snelheid). U kunt de platte bestanden met de gegevens naar de computer verplaatsen waarop SQL Server is geïnstalleerd met behulp van verschillende bestand hulpprogramma's zoals [AZCopy](../storage/storage-use-azcopy.md)kopiëren, [Azure opslag Explorer](http://storageexplorer.com/) of windows kopiëren en plakken via Remote Desktop Protocol (RDP).

1. Zorg ervoor dat de database en de tabellen zijn gemaakt op de doeldatabase voor SQL Server. Hier volgt een voorbeeld van hoe u de volgende acties uitvoeren die met de `Create Database` en `Create Table` opdrachten:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. Genereer het indelingsbestand waarmee het schema voor de tabel wordt beschreven door de volgende opdracht vanaf de opdrachtregel van de computer waarop bcp is geïnstalleerd.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. De gegevens in de database met de opdracht bcp als volgt invoegen. Dit moet werken vanaf de opdrachtregel ervan uitgaande dat de SQL Server is geïnstalleerd op dezelfde computer:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Hiermee voegt u BCP optimaliseren** Raadpleeg het volgende artikel ['Richtlijnen voor het optimaliseren van bulksgewijs importeren'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) optimaliseren van dergelijke wordt ingevoegd.


### <a name="insert-tables-bulkquery-parallel"></a>Parallelizing wordt ingevoegd voor snellere verplaatsing van gegevens

Als de gegevens die u verplaatst groot is, kunt u versnellen dingen door meerdere BCP-opdrachten tegelijk parallel in een PowerShell-Script uitvoeren.

> [AZURE.NOTE]**Big data opname** Partitioneren om te optimaliseren voor grote en zeer grote gegevenssets het laden van gegevens, de logische en fysieke databasetabellen met behulp van meerdere bestandsgroepen en partition tabellen. Zie voor meer informatie over het maken en het laden van gegevens naar partitietabellen [Parallelle laden SQL partitietabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Parallelle wordt ingevoegd met bcp te zien hoe het voorbeeld PowerShell-script hieronder:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Bulksgewijs invoegen SQL-Query

[Bulksgewijs invoegen SQL-Query](https://msdn.microsoft.com/library/ms188365) kan worden gebruikt om gegevens te importeren vanuit rijen/kolommen op basis van bestanden (de ondersteunde typen, worden beschreven in de[Gegevens voorbereiden voor bulksgewijs exporteren of importeren (SQL Server)](https://msdn.microsoft.com/library/ms188609)) in de database onderwerp. 

Hier volgen enkele steekproef-opdrachten voor het bulksgewijs invoegen zijn zoals hieronder:  

1. Uw gegevens analyseren en stel eventuele opties in de aangepaste voordat u importeert om ervoor te zorgen dat de SQL Server-database wordt ervan uitgegaan dezelfde opmaak voor speciale velden zoals datums. Hier volgt een voorbeeld van het instellen van de datumnotatie als jaar-maand-dag (als uw gegevens de datum in de jaar-maand-dag bevat):

        SET DATEFORMAT ymd; 
    
2. Gegevens importeren met bulksgewijs importeren instructies:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>Ingebouwde hulpprogramma's in SQL Server

U kunt SQL Server integraties Services (SSIS) gebruiken om gegevens in SQL Server VM op Azure vanaf een plat bestand importeren. SSIS is beschikbaar in twee studio-omgevingen. Zie [Integration Services (SSIS) en Studio omgevingen](https://technet.microsoft.com/library/ms140028.aspx)voor meer informatie:

- Zie voor meer informatie over SQL Server Data Tools, [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Zie voor meer informatie over de Wizard importeren en exporteren, de [Wizard SQL Server importeren en exporteren](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Gegevens uit lokale SQL Server verplaatsen naar SQL Server op een Azure VM

U kunt ook de volgende migratiestrategieën gebruiken:

1. [Een SQL Server-Database naar de wizard van een Microsoft Azure VM implementeren](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Exporteren naar een plat bestand](#export-flat-file) 
3. [Wizard migratie SQL-Database](#sql-migration)
4. [Database back-en herstellen](#sql-backup)

We beschrijven elk van deze hieronder:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Een SQL Server-Database naar de wizard van een Microsoft Azure VM implementeren

De **Deploy een SQL Server-Database naar de wizard van een Microsoft Azure VM** is een eenvoudige en aanbevolen manier om gegevens te verplaatsen vanaf een lokale SQL Server-instantie met SQL Server op een VM Azure. Zie voor gedetailleerde stappen, evenals een discussie van andere alternatieven, [migreren van een database met SQL Server op een VM Azure](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Exporteren naar een plat bestand

Verschillende methoden kunnen worden gebruikt voor het bulksgewijs gegevens exporteren vanuit een On-Premises SQL Server, zoals beschreven in het onderwerp met [bulksgewijs importeren en exporteren van gegevens (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) . In dit document wordt uitgelegd hoe de bulksgewijs Copy Program (BCP) als voorbeeld. Nadat de gegevens worden geëxporteerd naar een plat bestand, kan deze worden geïmporteerd naar een andere SQL server met bulksgewijs importeren. 

1. Exporteer de gegevens uit lokale SQL Server naar een bestand met het hulpprogramma bcp als volgt

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. De database en de tabel op SQL Server VM maken over het gebruik van Azure de `create database` en `create table` voor het tabelschema die is geëxporteerd in stap 1.

3. Een opmaakbestand voor de beschrijving van het tabelschema van de gegevens worden geëxporteerd/geïmporteerd maken. Details van het bestand opmaak worden beschreven in [een bestandsindeling (SQL Server) maken](https://msdn.microsoft.com/library/ms191516.aspx).

    Bestandsgeneratie opmaken BCP bij het uitvoeren van de SQL Server-computer 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Bestandsgeneratie opmaken bij het uitvoeren van BCP extern ten opzichte van een SQL Server 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Gebruik een van de methoden beschreven in de sectie [Verplaatsen van gegevens uit de bron bestand](#filesource_to_sqlonazurevm) de gegevens in een platte bestanden verplaatsen naar een SQL Server.

### <a name="sql-migration"></a>Wizard migratie SQL-Database

[Wizard migratie SQL Server-Database](http://sqlazuremw.codeplex.com/) bevat een gebruiksvriendelijke manier om gegevens te verplaatsen tussen twee instanties van SQL server. Dit kan de gebruiker het gegevensschema tussen bronnen en doeltabellen toewijzen, kies kolomtypen en verschillende andere functies. Bulksgewijs kopiëren (BCP) achter de wordt gebruikt. Hieronder vindt u een schermafbeelding van het scherm Welkom voor de wizard SQL-Database migreren.  

![Wizard SQL Server-migratie][2]

### <a name="sql-backup"></a>Database back-en herstellen

SQL Server worden ondersteund in: 

1. [Database back-en herstellen van functionaliteit](https://msdn.microsoft.com/library/ms187048.aspx) (beide naar een lokaal bestand of Bacpac-exporteren naar blob) en [Gegevens laag toepassingen](https://msdn.microsoft.com/library/ee210546.aspx) (via bacpac). 
2. Mogelijkheid om te SQL Server VMs op Azure rechtstreeks met een gekopieerde database of kopiëren naar een bestaande SQL Azure-database maken. Zie [Gebruik de Wizard van de Database kopiëren](https://msdn.microsoft.com/library/ms188664.aspx)voor meer informatie. 

Schermafbeelding van de Database-back-up/terugzetten vanuit SQL Server Management Studio voor hieronder wordt weergegeven.

![SQL-hulpprogramma voor het importeren van Server][1]

## <a name="resources"></a>Resources

[Een Database migreren met SQL Server op een Azure VM](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server Azure virtuele Machines overzicht](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
