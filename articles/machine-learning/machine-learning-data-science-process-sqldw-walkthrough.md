<properties
    pageTitle="Het proces van het Team gegevens wetenschappelijke in actie: SQL Data Warehouse met | Microsoft Azure"
    description="Proces van geavanceerde analyses en technologie in actie"  
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Het proces van het Team gegevens wetenschappelijke in actie: SQL Data Warehouse gebruiken

In deze zelfstudie begeleiden we u bij het maken en implementeren van een machine learning-model met SQL Data Warehouse (SQL DW) voor een openbaar gegevensset--de gegevensset [Tevens Taxi reizen](http://www.andresmh.com/nyctaxitrips/) . De binaire indeling model opgesteld worden voorspeld al dan niet een tip voor een reis is betaald, en de modellen voor multiclass classificatie en regressie worden ook besproken die voorspellen van de verdeling voor de tip bedragen dat is betaald.

De procedure volgt de werkstroom [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) . U leert hoe een gegevens wetenschappelijke-omgeving, instellen hoe u de gegevens laadt in SQL DW, en hoe gebruiken SQL DW of een notitieblok IPython Verken de gegevens en engineeren functies voor het model. Vervolgens leert hoe voor bouwen en implementeren van een model met Azure Machine Learning.


## <a name="dataset"></a>De gegevensset tevens Taxi reizen

De gegevens tevens Taxi reis bestaat uit ongeveer 20GB gecomprimeerde CSV-bestanden (~ 48GB niet gecomprimeerd), vastleggen van meer dan 173 miljoen afzonderlijke reizen en de tarieven betaald voor elke reis. Elke record reis bevat de locaties ophalen en inleverbibliotheek en tijden, anoniem hack (stuurprogramma van) licentie getal en het getal straten (taxi van unieke id). De gegevens behandelt alle reizen in het jaar 2013 en is opgegeven in de volgende twee gegevenssets voor elke maand:

1. Het bestand **trip_data.csv** bevat reis details, zoals het aantal personen, ophalen en dropoff punten reis duur en duur van reis. Hier volgen een paar steekproef records:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Het bestand **trip_fare.csv** bevat details van het tarief dat is betaald voor elke reis zoals betalingstype, tarief bedrag, extra kosten en belastingen, tips en tolgelden, en het totale bedrag dat is betaald. Hier volgen een paar steekproef records:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De **unieke sleutel** gebruikt voor het samenvoegen van reis\_gegevens en reis\_tarief bestaat uit de volgende drie velden:

- straten,
- inbreken\_licentie en
- ophalen\_datetime.

## <a name="mltasks"></a>Adres drie soorten taken die voorspellen

We opstellen drie tekstvoorspelling problemen op basis van de *tip\_bedrag* om te laten zien drie soorten modeling taken:

1. **Binaire indeling**: te voorspellen al dan niet een tip is betaald voor een reis, dat wil zeggen een *tip\_bedrag* die groter is dan $0 een positief voorbeeld is, wanneer een *tip\_bedrag* van €0 is een negatieve voorbeeld.

2. **Multiclass classificatie**: te voorspellen van het bereik van tip betaald voor de reis. U deelt de *tip\_bedrag* in vijf opslaglocaties of klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressie taak**: aan de hoeveelheid tip betaald voor een reis voorspellen.  


## <a name="setup"></a>Stel de omgeving Azure gegevens wetenschappelijke voor geavanceerde analyses

Voer de volgende stappen uit om het instellen van uw omgeving Azure gegevens wetenschappelijke.

**Uw eigen Azure blob storage-account maken**

- Wanneer u uw eigen Azure-blobopslag inrichten, kies een geografische-locatie voor uw Azure-blobopslag in of zo dicht mogelijk naar **Centraal Zuid-Amerikaanse**, dat wil zeggen waarin de gegevens tevens Taxi is opgeslagen. De gegevens worden gekopieerd AzCopy vanuit de openbare blob storage container aan een container gebruiken in uw eigen opslag-account. Hoe dichter uw Azure-blobopslag Zuid centraal ons, des te sneller deze taak (stap 4) wordt voltooid.
- Als u uw eigen Azure opslag-account hebt gemaakt, wilt u de stappen op [over Azure opslag-accounts](../storage/storage-create-storage-account.md). Zorg ervoor dat u notities over de waarden voor de volgende opslag-accountreferenties terwijl ze nodig hebben verderop in dit scenario.

  - **Opslagaccountnaam**
  - **Accountsleutel opslag**
  - **De containernaam van de** (dat u wilt dat de gegevens die moeten worden opgeslagen in de Azure-blobopslag)

**Uw exemplaar van de Azure SQL-DW inrichten.**
Ga als volgt de documentatie op [een SQL-Data Warehouse maken](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) voor het inrichten van een exemplaar van SQL Data Warehouse. Zorg ervoor dat u maakt de notatie van de volgende SQL Data Warehouse referenties die worden gebruikt in de volgende stappen.

  - **Servernaam**: <server Name>. database.windows.net
  - **De naam van de SQLDW (Database)**
  - **Gebruikersnaam**
  - **Wachtwoord**

**Installeer Visual Studio-2015 en SQL Server Data Tools.** Zie [Visual Studio-2015 installeren en/of SSDT (SQL Server Data Tools) voor SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md)voor instructies.

**Verbinding maken met uw Azure SQL-DW met Visual Studio.** Zie stap 1 en 2 in [verbinding maken met Azure SQL Data Warehouse met Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md)voor instructies.

>[AZURE.NOTE] De volgende SQL-query uitgevoerd op de database die u hebt gemaakt in uw SQL Data Warehouse (in plaats van de query die beschikbaar zijn in stap 3 van het onderwerp connect) om een sleutel basispagina te **maken**.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Maak een werkruimte Azure Machine Learning onder uw Azure-abonnement.** Zie [een Azure Machine Learning-werkruimte maken](machine-learning-create-workspace.md)voor instructies.

## <a name="getdata"></a>De gegevens laadt in SQL Data Warehouse

Open een Windows PowerShell-opdracht-console. Opdrachten voor het downloaden van het voorbeeld SQL script uitvoeren van de volgende PowerShell bestanden die we deelt met u op Github naar een lokale map die u met de parameter opgeeft *-DestDir*. U kunt de waarde van parameter wijzigen *-DestDir* naar een lokale map. Als *- DestDir* niet bestaat, wordt deze door de PowerShell-script gemaakt.

>[AZURE.NOTE] Mogelijk moet u **als Administrator uitvoeren** bij het uitvoeren van de volgende PowerShell-script als het telefoonboek van uw *DestDir* moet beheerdersbevoegdheden om te maken of om ernaar te schrijven.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Nadat u hebt uitgevoerd, wordt de huidige werkmap gewijzigd in *- DestDir*. U moet kunnen aan het scherm ziet met de zoals hieronder:

![][19]

In uw *-DestDir*, moet u het volgende PowerShell-script beheerdersaccount uitvoeren:

    ./SQLDW_Data_Import.ps1

Wanneer de PowerShell-script wordt uitgevoerd voor de eerste keer, wordt u gevraagd om in te voeren van de gegevens uit uw Azure SQL-DW en uw Azure blob storage-account. Wanneer deze PowerShell-script is voltooid wordt uitgevoerd voor het eerst, met de referenties u invoer zijn geschreven naar een configuratiebestand SQLDW.conf in de huidige werkmap. De toekomstige uitvoeren van deze PowerShell-script-bestand bevat de optie voor het lezen van dat alle benodigde parameters uit dit configuratiebestand. Als u enkele parameters veranderen moet, kunt u de parameters in het scherm bij de prompt invoert met het verwijderen van dit configuratiebestand en de parameterwaarden invoeren als u hierom wordt gevraagd of de betreffende parameterwaarden wijzigen door het bestand SQLDW.conf in uw adreslijst *- DestDir* te bewerken.

>[AZURE.NOTE] Om te vermijden schema naamconflicten met artikelen die al in uw DW van de SQL Azure tijdens het lezen van parameters rechtstreeks vanuit het bestand SQLDW.conf, wordt een willekeurig getal met 3 cijfers toegevoegd aan de naam van het schema uit het bestand SQLDW.conf als de naam van het standaard schema voor elke uitvoeren. De PowerShell-script u mogelijk gevraagd een schema in te voeren: de naam van de gebruiker naar keuze van kan worden opgegeven.

Dit **PowerShell-script** -bestand is voltooid de volgende taken uitvoeren:

- **Downloadt en installeert AzCopy**, als AzCopy niet al is geïnstalleerd

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Kopieën-gegevens in uw privé blob storage-account** uit de openbare blob met AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- De **laadtijd gegevens Polybase (door uit te voeren LoadDataToSQLDW.sql) naar uw Azure SQL-DW gebruiken** op uw account privé blob storage met de volgende opdrachten.

    - Een schema maken

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Een database beperkt referentie maken

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Een externe gegevensbron maken voor een blob Azure opslag

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Maak een externe bestandsindeling voor een CSV-bestand. Gegevens is niet gecomprimeerd en velden worden gescheiden door het sluisteken.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Externe tarief en reis tabellen voor tevens taxi gegevensset maken in Azure-blobopslag.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Gegevens uit externe tabellen in Azure-blobopslag laden naar SQL Data Warehouse

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Maken van een steekproef-gegevenstabel (NYCTaxi_Sample) en gegevens ernaar uit het selecteren van de SQL-query's op de tabellen reis en tarief invoegen. (Enkele stappen uitvoeren van deze procedure moet gebruiken deze voorbeeldtabel.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

De geografische locatie van uw accounts voor de opslag van invloed is op worden geladen.

>[AZURE.NOTE] Afhankelijk van de geografische locatie van uw persoonlijke blob storage-account, het kopiëren van gegevens uit een openbare blob aan uw persoonlijke opslag-account kan duren ongeveer 15 minuten, of zelfs langer, en het proces van het laden van gegevens uit uw account opslagruimte aan uw Azure SQL-DW kan duren 20 minuten of langer.  

U moet bepalen welke Doe als er dubbele bron- en doeltabellen bestanden.

>[AZURE.NOTE] Als het .csv-bestanden worden gekopieerd van de openbare-blobopslag bij uw privé blob storage-account al bestaat in uw persoonlijke blob storage-account, AzCopy u wordt gevraagd of u wilt dat deze te overschrijven. Als u niet wilt overschrijven, invoerwaarden **n** wanneer hierom wordt gevraagd. Als u overschrijven **alle** van ze, invoer **per wilt** wanneer hierom wordt gevraagd. U kunt ook invoer **y** om te overschrijven CSV-bestanden afzonderlijk.

![#21 uitzetten][21]

U kunt uw eigen gegevens. Als uw gegevens zich in de on-premises computer in de praktijk-toepassing, kunt u AzCopy on-premises gegevens uploaden naar uw persoonlijke Azure-blobopslag. U hoeft alleen te wijzigen van **de bronlocatie** `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, in de opdracht AzCopy van de PowerShell-script-bestand naar de lokale map die de gegevens bevat.


>[AZURE.TIP] Als uw gegevens zich al in uw persoonlijke Azure-blobopslag in de praktijk-toepassing, kunt u het AzCopy overslaan in de PowerShell-script en de gegevens rechtstreeks naar Azure SQL-DW uploaden. Dit vereist extra bewerkingen van het script aan te passen deze naar de indeling van uw gegevens.


Deze Powershell-script ook aansluit in de gegevens van Azure SQL-DW de gegevensbestanden uitleg in het voorbeeld SQLDW_Explorations.sql, SQLDW_Explorations.ipynb en SQLDW_Explorations_Scripts.py zodat deze drie bestanden worden getest direct kunnen nadat de PowerShell-script is voltooid.

Na een geslaagde uitvoering, ziet u scherm zoals hieronder:

![][20]

## <a name="dbexplore"></a>Gegevens verkennen en functie engineering in Azure SQL Data Warehouse

In dit gedeelte uitvoeren we verkennen en functie generatie door SQL-query's ten opzichte van Azure SQL-DW rechtstreeks met **Visual Studio gegevens Tools**uit te voeren. Alle SQL-query's die worden gebruikt in deze sectie vindt u in het voorbeeldscript met de naam *SQLDW_Explorations.sql*. Dit bestand heeft al zijn gedownload naar uw lokale map door de PowerShell-script. U kunt dit ook ophalen uit [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). Maar het bestand in Github niet over de gegevens van de Azure SQL-DW aangesloten.

Verbinding maken met uw Azure SQL-DW Visual Studio gebruikt met de SQL-DW aanmeldingsnaam en het wachtwoord op en open het **SQL Object Explorer** om te bevestigen dat de database en de tabellen zijn geïmporteerd. Hiermee kunt u het bestand *SQLDW_Explorations.sql* ophalen.

>[AZURE.NOTE] Als u wilt openen in een query-editor van Parallel Data Warehouse (PDW), gebruikt u de opdracht **Nieuwe Query** terwijl uw PDW is geselecteerd in de **SQL Object Explorer**. De standaard SQL-query-editor wordt niet ondersteund door PDW.

Hier vindt u het type gegevens verkennen en functie generatie uitgevoerd in deze sectie:

- Gegevens onderzoeken met een paar velden in verschillende tijd windows verkennen.
- Onderzoek kwaliteit van de gegevens van de velden lengte en breedte.
- Genereren binaire en multiclass classificatie etiketten op basis van de **tip\_bedrag**.
- Functies genereren en berekeningscluster/vergelijken reis afstanden.
- Deelnemen aan de twee tabellen en extraheren van een steekproef die wordt gebruikt om modellen te maken.

### <a name="data-import-verification"></a>Gegevens importeren verificatie

Deze query's bieden een snelle verificatie van het aantal rijen en kolommen in de tabellen die zijn gevuld eerder met Polybase van parallelle bulksgewijs importeren,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Uitvoer:** U moet 173,179,759 rijen en kolommen 14.

### <a name="exploration-trip-distribution-by-medallion"></a>Uitleg: Reis verdeling door straten

In dit voorbeeldquery geeft aan wat het medallions (taxi getallen) die meer dan 100 reizen voltooid binnen een opgegeven periode. De query wilt gebruikmaken van de gepartitioneerde tabel omdat deze zijn afhankelijk door het partitioneringsschema van **ophalen\_datetime**. Query's uitvoeren van de volledige gegevensset ook maakt gebruik van de gepartitioneerde tabel en/of indexeren gescande afbeelding.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Uitvoer:** De query moet worden een tabel geretourneerd met rijen waarbij het 13,369 medallions (taxi's) en het aantal reis uitgevoerd door ze in 2013 opgeeft. De laatste kolom bevat het aantal het aantal reizen voltooid.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Uitleg: Reis verdeling door straten en hack_license

In dit voorbeeld geeft aan wat het medallions (taxi getallen) en hack_license getallen (stuurprogramma's) dat voltooid meer dan 100 reizen binnen een opgegeven periode.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Uitvoer:** De query moet worden een tabel geretourneerd met 13,369 rijen geven het 13,369 auto/stuurprogramma-id's die meer die 100 transacties in 2013 hebt voltooid. De laatste kolom bevat het aantal het aantal reizen voltooid.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Kwaliteitsbeoordeling van gegevens: Controleer de records met onjuiste lengtegegevens en/of breedtegraad

In dit voorbeeld gekeken naar als een van de velden lengtegegevens en/of breedtegraad een ongeldige waarde bevatten (radiaal graden moeten liggen tussen-90 en 90), of als zich (0, 0) coördinaten.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Uitvoer:** De query retourneert 837,467 reizen met ongeldige lengtegraad en/of breedtegraad velden.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Uitleg: Gekantelde versus niet Gekantelde reizen verdeling

Dit voorbeeld wordt het aantal reizen die zijn Gekantelde versus het getal die niet zijn Gekantelde in een opgegeven periode (of in de volledige gegevensset als het volledige jaar bedekt zoals deze hier is ingesteld). Deze verdeling weerspiegelt de binaire label verdeling moet worden later gebruikt voor binaire classificatie-modellen.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Uitvoer:** De frequentie van de volgende tip voor het jaar 2013: 90,447,622 Gekantelde en 82,264,709 niet-Gekantelde, moet de query worden geretourneerd.

### <a name="exploration-tip-classrange-distribution"></a>Uitleg: Tip class/bereik verdeling

In dit voorbeeld berekent de verdeling van tip bereiken in een bepaalde periode (of in de volledige gegevensset als die betrekking hebben op het volledige jaar). Dit is de verdeling van het label klassen die later worden gebruikt voor multiclass classificatie-modellen.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Uitvoer:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Uitleg: Berekenen en vergelijken reis-afstand

In dit voorbeeld de lengtegraad ophalen en inleverbibliotheek converteert en breedtegraad naar SQL Geografie verwijst, de reis afstand SQL Geografie punten verschil met berekent en geeft als resultaat een steekproef van de resultaten voor vergelijking. Het voorbeeld beperkt de resultaten naar geldige coördinaten alleen met de kwaliteit assessment gegevensquery eerder vallen.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Functie engineering met SQL-functies

Soms kan het SQL-functies zijn een efficiënte optie voor de functie engineering. In dit scenario, zoals een SQL-functie als u wilt berekenen van de directe afstand tussen de locaties ophalen en dropoff gedefinieerd. U kunt de volgende SQL-scripts uitvoeren in **Visual Studio Tools-gegevens**.

Hier ziet u de SQL-script waarin de functie afstand.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Hier volgt een voorbeeld om te bellen van deze functie om te genereren van functies in uw SQL-query:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Uitvoer:** Deze query genereert een tabel (met 2,803,538 rijen) met ophalen en dropoff minuten en aantal graden en de bijbehorende directe afstanden in mijl een. Hier volgen de resultaten voor eerste 3 rijen:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Gegevens voorbereiden voor model bouwen

De volgende query joins de **nyctaxi\_reis** en **nyctaxi\_tarief** tabellen, genereert een binaire indeling label **Gekantelde**, een label meerdere class classificatie **tip\_class**, en haalt van een steekproef uit de volledige gekoppelde gegevensset. De steekproeven wordt uitgevoerd door een subset van de reizen op basis van ophalen tijd ophalen.  Deze query kan worden gekopieerd en geplakt rechtstreeks in de [Azure Machine Learning Studio](https://studio.azureml.net) - [Gegevens importeren] [ import-data] -module voor directe gegevens opname vanuit het exemplaar van de SQL-database in Azure wordt aangegeven. De query worden records met onjuiste uitgesloten (0, 0) coördinaten.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Wanneer u gaat u verder met Azure Machine Learning, kan u:  

1. Sla de uiteindelijke SQL-query als u wilt extraheren en de voorbeeldgegevens en kopiëren plakken de query rechtstreeks in een [Importgegevens] [ import-data] module in Azure Machine Learning, of
2. De steekproef en ontworpen gegevens die u wilt gebruiken voor het samenstellen van een nieuwe tabel in de SQL-DW model en het gebruik van de nieuwe tabel in de [Gegevens importeren] [ import-data] module Azure Machine weten. De PowerShell-script in eerdere stap heeft dit voor u gedaan. U kunt lezen rechtstreeks vanuit deze tabel in de module gegevens importeren.


## <a name="ipnb"></a>Gegevens verkennen en functie engineering in IPython notitieblok

In dit gedeelte we verkennen en functie generatie met beide Python wordt uitgevoerd en SQL-query's ten opzichte van de SQL-DW eerder hebt gemaakt. Een voorbeeld IPython notitieblok met de naam **SQLDW_Explorations.ipynb** en een scriptbestand Python **SQLDW_Explorations_Scripts.py** zijn gedownload naar uw lokale map. Deze zijn ook beschikbaar op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Deze twee bestanden zijn identiek in Python scripts. De Python script-bestand is voor u beschikbaar als er geen een server IPython notitieblok. Deze twee Python voorbeeldbestanden zijn ontworpen onder **Python 2.7**.

De benodigde informatie aan Azure SQL-DW in de steekproef IPython notitieblok en de Python script-bestand dan ook gedownload naar uw lokale computer is aangesloten door de PowerShell-script eerder. Ze zijn uitvoerbare zonder wijzigingen.

Als u al een werkruimte AzureML hebt ingesteld, kunt u rechtstreeks in het voorbeeld IPython notitieblok uploaden naar de service AzureML IPython notitieblok en waarop deze wordt gestart. Hier volgen de stappen voor het uploaden naar de service AzureML IPython notitieblok:

1. Meld u aan bij uw werkruimte AzureML, klikt u boven "Studio" en "NOTITIEBLOKKEN" aan de linkerkant van de pagina met webonderdelen op.

    ![#22 uitzetten][22]

2. Klik op de linker benedenhoek van de pagina met webonderdelen op 'Nieuw' en selecteer "Python 2". Vervolgens Geef een naam voor het notitieblok en klik op het vinkje om de nieuwe lege IPython notitieblok te maken.

    ![#23 uitzetten][23]

3. Klik op het symbool "Jupyter" op de linker bovenhoek van het nieuwe IPython-notitieblok.

    ![#24 uitzetten][24]

4. Slepen en neerzetten van de steekproef IPython notitieblok naar de pagina **structuur** van uw service AzureML IPython notitieblok en klik op **uploaden**. Klik in het voorbeeld IPython notitieblok worden geüpload naar de service AzureML IPython notitieblok.

    ![#25 uitzetten][25]

Script-bestand, de volgende Python pakketten nodig zijn om te kunnen uitvoeren van de steekproef IPython notitieblok of de Python. Als u de service AzureML IPython notitieblok gebruikt, zijn deze pakketten vooraf geïnstalleerd.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

De aanbevolen volgorde bij het maken van geavanceerde analytische oplossingen op AzureML met grote gegevens is het volgende:

- Lees meer in een kleine steekproef van de gegevens in een gegevensframe in het geheugen.
- Sommige visualisaties en het gebruik van de steekproef gegevens explorations uitvoeren.
- Experimenteren met de functie engineering gebruik van de steekproef gegevens.
- Gebruik voor grotere gegevens verkennen, gegevens bewerken en functie engineering, Python SQL-query's rechtstreeks ten opzichte van de SQL-DW uitgeven.
- Bepaal de grootte van de steekproef geschikt is voor het bouwen van Azure Machine Learning-model.

De volgende elementen zijn een paar gegevens verkennen, gegevensvisualisatie en functie engineering voorbeelden. Meer gegevens explorations vindt u in de steekproef IPython notitieblok en de steekproef Python script-bestand.

### <a name="initialize-database-credentials"></a>Initialisatie van de databasereferenties

De instellingen voor de database-verbinding in de volgende variabelen geïnitialiseerd:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Database-verbinding maken

Hier ziet u de verbindingsreeks waarmee de verbinding met de database.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Rapport aantal rijen en kolommen in tabel < nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Totale aantal rijen = 173179759  
- Totaal aantal kolommen = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Rapport aantal rijen en kolommen in tabel < nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Totale aantal rijen = 173179759  
- Totaal aantal kolommen = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Lezen in een steekproef kleine gegevens uit de SQL-Database voor het BTW-records van gegevens

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Tijd om te lezen dat op de voorbeeldtabel is 14.096495 seconden.  
Aantal rijen en kolommen opgehaald = (1000, 21).

### <a name="descriptive-statistics"></a>Beschrijvende statistiek

U bent nu klaar om te verkennen van de steekproef gegevens. We beginnen met het bekijken via sommige Beschrijvende statistiek voor de **reis\_afstand** (of desgewenst andere velden die u kiest om op te geven).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Visualisatie: Voorbeeld van tekeningen

Nu we eens kijken naar de BoxPlot voor de afstand reis kunt visualiseren de quantiles.

    df1.boxplot(column='trip_distance',return_type='dict')

![Uitzetten #1][1]

### <a name="visualization-distribution-plot-example"></a>Visualisatie: Voorbeeld verdeling tekenen

Waarnemingspunten die visualiseren van de verdeling en een histogram voor de steekproef reis-afstand.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Uitzetten #2][2]

### <a name="visualization-bar-and-line-plots"></a>Visualisatie: Staaf- en lijn wordt getekend

In dit voorbeeld wordt de afstand reis in vijf opslaglocaties bin en de binning resultaten visualiseren.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

We kunnen de bovenstaande opslaglocatie verdeling in een balk uitzetten of tekeningen met regel:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 uitzetten][3]

en

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 uitzetten][4]

### <a name="visualization-scatterplot-examples"></a>Visualisatie: Voorbeelden Scatterplot

We geven spreidings-tekeningen tussen **reis\_tijd\_in\_sec** en **reis\_afstand** om te zien of er momenteel een correlatie is

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 uitzetten][6]

Op dezelfde manier we de relatie tussen kunt controleren **tarief\_code** en **reis\_afstand**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 uitzetten][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Gegevens verkennen op steekproef gegevens met SQL-query's in IPython notitieblok

In dit gedeelte besproken gegevens onderzoeken met behulp van de steekproef gegevens die worden behouden in de nieuwe tabel die u hebt gemaakt hierboven. Houd er rekening mee dat vergelijkbaar explorations kunnen worden uitgevoerd met behulp van de oorspronkelijke tabellen.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Uitleg: Aantal rijen en kolommen in de steekproef tabel rapporteren

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Uitleg: Gekantelde/niet omzeild verdeling

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Uitleg: Tip class verdeling

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Uitleg: De verdeling tip door klasse uitzetten

    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 uitzetten][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Uitleg: Dagelijkse verdeling van reizen

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Uitleg: Reis verdeling per straten

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Uitleg: Reis verdeling door straten en hack licentie

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Uitleg: Reis tijd verdeling

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Uitleg: Reis afstand verdeling

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Uitleg: Betaling type verdeling

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Controleer of de uiteindelijke vorm van de tabel featurized

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Modellen in Azure Machine Learning maken

We gaan nu gaat u verder met het model bouwen en implementeren van modellen in [Azure Machine Learning](https://studio.azureml.net). De gegevens is klaar voor gebruik in een van de voorspelling problemen geïdentificeerd eerder, namelijk:

1. **Binaire indeling**: te voorspellen al dan niet een tip is betaald voor een reis.

2. **Multiclass classificatie**: het cellenbereik dat is betaald, op basis van de eerder gedefinieerde klassen tip voorspellen.

3. **Regressie taak**: aan de hoeveelheid tip betaald voor een reis voorspellen.  



Om te beginnen met de oefening modellering, meld u aan bij uw **Azure Machine Learning** -werkruimte. Als u een machine learning-werkruimte nog niet hebt gemaakt, raadpleegt u [een werkruimte Azure ML maken](machine-learning-create-workspace.md).

1. Zie aan de slag met Azure Machine Learning [Wat Azure Machine Learning Studio is?](machine-learning-what-is-ml-studio.md)

2. Meld u aan bij [Azure Machine Learning Studio](https://studio.azureml.net).

3. De startpagina van Studio biedt een enorme hoeveelheid gegevens, video's, zelfstudies en koppelingen naar de verwijzing Modules en andere resources. Raadpleeg voor meer informatie over Azure Machine Learning, het [Azure Machine Learning documentatie Center](https://azure.microsoft.com/documentation/services/machine-learning/).

Een experiment typische training bestaat uit de volgende stappen uit:

1. Maak een experiment **+ Nieuw** .
2. De gegevens ophalen van Azure ML.
3. Vooraf worden verwerkt, transformeren en werken met de gegevens naar wens.
4. Genereer functies naar wens.
5. De gegevens splitsen in gegevenssets training/validatie/testen (of als zich afzonderlijk gegevenssets voor elk).
6. Selecteer een of meer machine learning-algoritmen afhankelijk van het probleem learning om op te lossen. Bijvoorbeeld binaire indeling, multiclass classificatie en regressie.
7. Een of meer modellen gebruik van de gegevensset training trainen.
8. De validatie gegevensset met de ervaren modellen score.
9. De modellen om te berekenen van de doelstellingen van de relevante voor het onderwijs probleem evalueren.
10. Probleemloos de modellen af te stellen en selecteert u het beste model om te implementeren.

In deze oefening hebben we al verkend en de gegevens in SQL Data Warehouse engineering en besloten op de grootte van de steekproef te nemen in Azure ML. Hier volgt de procedure voor het maken van een of meer van de voorspelling-modellen:

1. De gegevens ophalen met de [Gegevens importeren] Azure-ML[ import-data] module, beschikbaar in de sectie **invoer van gegevens en uitvoer** . Voor meer informatie raadpleegt u de [Gegevens importeren] [ import-data] module verwijzing pagina.

    ![Azure ML importgegevens][17]

2. Selecteer **Azure SQL-Database** als de **gegevensbron** in het deelvenster **Eigenschappen** .

3. Voer de naam van de DNS-database in het veld **naam van de Database-server** . Indeling:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Voer de **naam van de Database** in het corresponderende veld.

5. Voer de *gebruikersnaam van de SQL* in de **Server-account gebruikersnaam**en het *wachtwoord* in het **wachtwoord voor een gebruikersaccount Server**.

6. Schakel de optie voor het **accepteren van het certificaat van een server** .

7. In het tekstgebied **databasequery's** bewerken voorbeelden plakken de query die de benodigde haalt databasevelden (inclusief berekende velden zoals de etiketten) en naar beneden de gegevens naar de gewenste steekproefgrootte.

Een voorbeeld van een binaire indeling experiment het lezen van gegevens rechtstreeks vanuit de database van SQL Data Warehouse is in de onderstaande afbeelding (inclusief de tabel namen nyctaxi_trip en nyctaxi_fare vervangen door de naam van het schema en de tabelnamen die u in uw scenario gebruikt). Vergelijkbare experimenten kunnen worden samengesteld voor multiclass classificatie en regressie problemen.

![Azure ML trein][10]

> [AZURE.IMPORTANT] In de gegevens modellering extractie van en meting voorbeelden van de query die beschikbaar zijn in vorige gedeelten, **alle labels voor de oefeningen drie modellen zijn opgenomen in de query**. Een belangrijke (vereist) stap in elk van de oefeningen modellering is **uitsluiten** de onnodige labels voor de andere twee problemen en eventuele andere **doel lekt**. Bijvoorbeeld wanneer u met de binaire indeling, gebruikt u het label **Gekantelde** en uitsluiten van de velden **tip\_class**, **tip\_bedrag**, en **totale\_bedrag**. De laatste zijn doel lekkage omdat ze betekent de tip dat dat is betaald.
>
> Als u wilt een onnodige kolommen uitsluiten of lekkage doelgerichte, u kunt de [Kolommen selecteren in de gegevensset] [ select-columns] module of de [Metagegevens bewerken][edit-metadata]. Zie voor meer informatie [Kolommen selecteren in de gegevensset] [ select-columns] en [Metagegevens bewerken] [ edit-metadata] verwijzen naar pagina's.

## <a name="mldeploy"></a>Modellen in Azure Machine Learning implementeren

Wanneer uw model klaar is, kunt u deze gemakkelijk als een webservice rechtstreeks vanuit het experiment implementeren. Zie voor meer informatie over de implementatie van Azure ML webservices [Deploy een Azure Machine Learning-webservice](machine-learning-publish-a-machine-learning-web-service.md).

Als u wilt een nieuwe webservice implementeren, moet u:

1. Maak een score experiment.
2. De webservice implementeren.

Klik op **Maken SCOREN EXPERIMENTEREN** op de onderste balk van de actie om een score experiment van een experiment **voltooid** -training.

![Azure scoren][18]

Azure Machine Learning probeert te maken van een score experiment op basis van de onderdelen van het experiment training. Dit zal met name het volgende doen:

1. Sla het model ervaren en verwijder het model trainingsmodules.
2. Een logische **invoer poort** bevatten het schema verwachte invoergegevens te identificeren.
3. Een logische **uitvoerpoort** bevatten van het verwachte web service uitvoerschema te identificeren.

Wanneer het score experiment is gemaakt, bekijken en maken pas zo nodig. Een typische aanpassing is voor het vervangen van de invoer gegevensset en/of de query met een exclusief kolomlabelvelden, als deze niet meer beschikbaar wanneer de service wordt genoemd. Het is ook verstandig om te verkleinen van de grootte van de invoer gegevensset en/of de query aan een paar records alleen voldoende om aan te geven van de invoer schema. Voor de uitvoerpoort meestal wordt alleen de **Labels van een score voorzien** en **Kansen behaald** opnemen in de uitvoer met behulp van de [Kolommen selecteren in de gegevensset] en uitsluiten van alle invoervelden[ select-columns] module.

Een steekproef scoren experiment is opgegeven in de onderstaande afbeelding. Klik op de knop **WEBSERVICE publiceren** in de onderste actiebalk als u klaar bent om te implementeren.

![Azure ML publiceren][11]


## <a name="summary"></a>Overzicht
Als u wilt samenvatting wat we hebben gedaan in deze zelfstudie Stapsgewijze instructies, hebt u gemaakt een Azure gegevens wetenschappelijke omgeving, gewerkt met een grote openbare gegevensset, laat dit door het Team gegevens wetenschap proces-, helemaal uit de overname van de gegevens naar het modelleren van training en vervolgens naar de implementatie van een Azure Machine Learning-webservice.

### <a name="license-information"></a>Licentie-informatie

In dit voorbeeld Stapsgewijze instructies en de bijbehorende scripts en IPython notebook(s) worden gedeeld door Microsoft onder de MIT-licentie. Controleer of u het bestand LICENSE.txt, in de adreslijst van de steekproef-code op GitHub voor meer informatie.

## <a name="references"></a>Verwijzingen

• [Andrés Monroy tevens Taxi reizen downloadpagina](http://www.andresmh.com/nyctaxitrips/)  
• [Van tevens Taxi reis gegevens door Chris Whong fOILing](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Tevens Taxi en Limousine provisie onderzoek en statistieken worden aangepast](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
