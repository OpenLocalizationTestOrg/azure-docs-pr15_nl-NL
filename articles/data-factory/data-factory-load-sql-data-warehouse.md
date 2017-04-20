<properties 
    pageTitle="TB aan gegevens laden in SQL Data Warehouse | Microsoft Azure" 
    description="Laat zien hoe 1 TB aan gegevens in kan worden geladen Azure SQL Data Warehouse onder 15 minuten met Azure gegevens Factory" 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure gegevens Factory [kopie Wizard]
[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is een cloudgebaseerde, schalen database kunnen grote hoeveelheden gegevens, relationele en niet-relationele verwerken.  Gebaseerd op massively parallelle verwerking (MPP) architectuur en is SQL Data Warehouse geoptimaliseerd voor enterprise data warehouse werkbelasting.  Deze optie cloud elasticiteit biedt de flexibiliteit om te schalen opslag en onafhankelijk berekenen.

Aan de slag met Azure SQL Data Warehouse is nu gemakkelijker dan ooit **Factory van Azure-gegevens**.  Azure gegevens Factory is een volledig beheerde cloudgebaseerde integratie-service van gegevens kan worden gebruikt om het vullen van een SQL Data Warehouse met de gegevens van uw bestaande systeem en zodat u waardevolle tijd tijdens de evaluatie van SQL Data Warehouse en het bouwen van uw analytics-oplossingen bevindt.  Dit zijn de belangrijkste voordelen van het laden van gegevens in Azure SQL Data Warehouse Azure gegevens Factory gebruiken:

- **Gemakkelijk om in te stellen**: 5 stappen intuïtieve wizard met geen scripts vereist.
- **Uitgebreide gegevens opslaan ondersteuning**: ingebouwde ondersteuning voor een uitgebreide set on-premises en cloud gebaseerde gegevensopslag.
- **Beveiligde en compatibele**: gegevens is overgedragen HTTPS- of ExpressRoute en globale service aanwezigheid zorgt ervoor dat uw gegevens verlaat nooit de geografische rand
- **Ongeëvenaarde prestaties met behulp van PolyBase** : Polybase gebruiken is de efficiëntste manier om gegevens te verplaatsen naar Azure SQL Data Warehouse. Het tijdelijk opslaan blob-functie gebruikt, kunt u hoge belasting snelheden van alle typen gegevens winkels naast Azure-blobopslag, waarin de Polybase al dan niet standaard ondersteunt realiseren.

In dit artikel leest u hoe u gegevens Factory kopie Wizard 1 TB gegevens van Azure-blobopslag in Azure SQL Data Warehouse laden in onder 15 minuten, op meer dan 1,2 GB/s doorvoer.

In dit artikel vindt u stapsgewijze instructies voor het verplaatsen van gegevens in Azure SQL Data Warehouse met behulp van de Wizard kopiëren. 

> [AZURE.NOTE] Zie [gegevens van en naar Azure SQL Data Warehouse met Azure gegevens Factory verplaatsen](data-factory-azure-sql-data-warehouse-connector.md) artikel voor algemene informatie over de mogelijkheden van de gegevens fabriek bij het verplaatsen van gegevens naar/van Azure SQL Data Warehouse. 
> 
> U kunt ook samenstellen pijpleidingen met behulp van Azure portal, Visual Studio, PowerShell, enzovoort. Zie [Zelfstudie: gegevens van Azure Blob kopiëren naar Azure SQL-Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor een snelle rondleiding met stapsgewijze instructies voor het gebruik van de activiteit kopiëren in Factory van Azure-gegevens.  

## <a name="prerequisites"></a>Vereisten voor
- Azure-blobopslag: dit experiment gebruikt Azure Blob Storage (GRS) voor het opslaan van TPC-H testen gegevensset.  Als u nog geen een Azure opslag-account, informatie over [het maken van een opslag-account](../storage/storage-create-storage-account.md#create-a-storage-account).
- Gegevens van de [TPC-H](http://www.tpc.org/tpch/) : gaan we TPC-H gebruiken als de testen gegevensset.  Als u wilt dat doet, moet u gebruiken `dbgen` uit TPC-H toolkit, die u helpt bij de gegevensset te genereren.  Kunt u broncode voor downloaden `dbgen` [TPC](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) voor's en uzelf compileren of downloaden van de gecompileerd binair getal van [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Dbgen.exe uitvoeren met de volgende opdrachten voor het genereren van 1 TB plat bestand voor `lineitem` verspreiding van een tabel in 10 bestanden:
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    Nu de gegenereerde bestanden naar Azure Blob kopiëren.  Verwijzen naar [gegevens verplaatsen naar en vanuit een on-premises implementatie-bestandssysteem met behulp van Azure gegevens Factory](data-factory-onprem-file-system-connector.md) voor hoe u dat doen via ADF kopiëren.   
- Azure SQL datawarehouse: dit experiment laadt gegevens in Azure SQL Data Warehouse die zijn gemaakt met 6.000 DWUs

    Raadpleeg [een Azure SQL Data Warehouse maken](../sql-data-warehouse/sql-data-warehouse-get-started-provision/) voor gedetailleerde instructies voor het maken van een database van SQL Data Warehouse.  Als u de beste prestaties van mogelijke laden in SQL Data Warehouse Polybase gebruiken, kiest u maximum aantal Data Warehouse eenheden (DWUs) in de prestatie-instelling, dat wil 6.000 DWUs zeggen toegestaan.

    > [AZURE.NOTE] 
    > Bij het laden van Azure Blob, is het laden van de prestaties van de gegevens rechtstreeks evenredig aan het aantal DWUs u op de SQL-Data Warehouse configureren:
    > 
    > 1 TB laden in 1.000 DWU SQL Data Warehouse duurt 87min (~ 200MBps doorvoer) 1 TB laden in 2.000 DWU SQL Data Warehouse duurt 46min (~ 380MBps doorvoer) 1 TB laden in 6.000 DWU SQL Data Warehouse duurt 14min (~1.2GBps doorvoer) 

    Als u wilt maken van een SQL-Data Warehouse met 6.000 DWUs, verplaatst u de schuifregelaar prestaties helemaal naar rechts:

    ![Prestaties van schuifregelaar](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Voor een bestaande database die niet is geconfigureerd met 6.000 DWUs, kunt u deze schalen met behulp van Azure-portal.  Navigeer naar de database in Azure-portal en er is een knop **schaal** in het deelvenster **Overzicht** is weergegeven in de volgende afbeelding:

    ![Knop schaal](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Klik op de knop **schaal** om het volgende deelvenster verplaatst u de schuifregelaar naar de maximumwaarde en klik op de knop **Opslaan** .

    ![Dialoogvenster tekenschaal](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    Dit experiment gegevens laadt in het gebruik van Azure SQL Data Warehouse `xlargerc` resource class.

    Als u wilt bereiken best mogelijke doorvoer, kopiëren, moet worden uitgevoerd met een SQL Data Warehouse-gebruiker die deel uitmaken van `xlargerc` resource class.  Leer hoe u dat doen door de volgende [wijziging een voorbeeld van een gebruiker resource klassen](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).  

- Bestemming tabelschema in Data Warehouse van Azure SQL-database maken door de volgende DDL-instructie uit te voeren:

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

Met de vereiste stappen voltooid, gaan we nu de kopie activiteit met de Wizard kopie configureren.

## <a name="launch-copy-wizard"></a>Start de Wizard kopiëren

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com).
2.  Klik op **+ Nieuw** in de linkerbovenhoek op **bedrijfsinformatie + analytics**en klik op **Gegevens fabriek**. 
6. Klik in het **nieuwe gegevens factory** -blad:
    1. Voer **LoadIntoSQLDWDataFactory** voor de **naam**.
        De naam van de fabriek Azure gegevens moet uniek zijn. Als het foutbericht: **gegevens factory naam "LoadIntoSQLDWDataFactory" is niet beschikbaar**, wijzig de naam van de gegevens fabriek (bijvoorbeeld yournameLoadIntoSQLDWDataFactory) en probeert opnieuw te maken. Zie [Gegevens Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naming regels voor Data Factory-onderdelen.  
     
    2. Selecteer uw Azure- **abonnement**.
    3. Voer een van de volgende stappen uit voor de groep Resource: 
        1. Selecteer **bestaande gebruiken** om een bestaande resourcegroep te selecteren.
        2. Selecteer **Nieuw** om in te voeren een naam voor een resourcegroep.
    3. Selecteer een **locatie** voor de fabriek gegevens.
    4. Schakel het selectievakje **vastmaken aan dashboard** onderaan in het blad.  
    5. Klik op **maken**.
10. Nadat het is gemaakt, ziet u het blad **Gegevens Factory** zoals wordt weergegeven in de volgende afbeelding:

    ![Gegevens factory-startpagina](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. Op de startpagina van de gegevens Factory, klikt u op de tegel **gegevens kopiëren** om **De Wizard kopiëren**te starten. 

    > [AZURE.NOTE] Als u ziet dat de webbrowser blijft hangen bij "Machtigen...", uitschakelen/Schakel **cookies van derden blokkeren en sitegegevens** instelling (of) behouden ingeschakeld en maken van een uitzondering voor **login.microsoftonline.com** en probeer het opnieuw starten van de wizard.


## <a name="step-1-configure-data-loading-schedule"></a>Stap 1: Gegevens laden planning configureren
De eerste stap is het laden van de planning van gegevens configureren.  

Op de pagina **Eigenschappen** :
1. Voer **CopyFromBlobToAzureSqlDataWarehouse** voor **de naam van de taak**
2. Selecteer optie **eenmaal nu uitvoeren** .   
3. Klik op **volgende**.  

![Wizard - de pagina Eigenschappen kopiëren](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Stap 2: Configureer bron
In dit gedeelte vindt u de stappen voor het configureren van de bron: Azure Blob met de 1 TB TPC-H regel item bestanden.

Selecteer de **Azure-blobopslag** als de gegevens opslaan en klik op **volgende**.

![Wizard - de pagina Select bron kopiëren](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

Vul de verbindingsgegevens voor het Azure Blob storage-account en klik op **volgende**.

![Wizard - de verbindingsgegevens kopiëren](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

Kies de **map** met de bestanden van de regel item TPC-H en klik op **volgende**.

![Wizard kopiëren: Selecteer invoer map](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

Na het klikken op **volgende**, worden de instellingen van de notatie bestand automatisch gedetecteerd.  Selectievakje om ervoor te zorgen dat kolomscheidingsteken is ' | 'in plaats van de standaard-komma','.  Klik op **volgende** nadat u de gegevens hebt bekeken.

![Wizard - instellingen van de notatie bestand kopiëren](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Stap 3: De bestemming configureren
In dit gedeelte ziet u hoe u het configureren van de bestemming: `lineitem` tabel in de Data Warehouse van Azure SQL-database.

Kies **Azure SQL Data Warehouse** als de waarde van doel-store en klik op **volgende**.

![Wizard - select bestemming gegevensopslag kopiëren](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

Vul de verbindingsgegevens voor Azure SQL Data Warehouse.  Zorg ervoor dat u opgeeft dat de gebruiker die deel uitmaakt van de rol `xlargerc` (Zie het gedeelte **vereisten** voor gedetailleerde instructies), en klik op **volgende**. 

![Wizard - de verbindingsgegevens bestemming kopiëren](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

Kies de doeltabel en klik op **volgende**.

![Wizard - de pagina van de tabel-koppeling kopiëren](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

Accepteer de standaardinstellingen voor kolommen toewijzen en klik op **volgende**.

![Wizard - de pagina schema koppeling kopiëren](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>Stap 4: Prestatie-instellingen

**Toestaan polybase** is standaard ingeschakeld.  Klik op **volgende**.

![Wizard - de pagina schema koppeling kopiëren](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Stap 5: Implementeren en laden resultaten controleren
Klik op **Voltooien** om te implementeren. 

![Wizard - overzichtspagina kopiëren](media/data-factory-load-sql-data-warehouse/summary-page.png)

Nadat de installatie voltooid is, klikt u op `Click here to monitor copy pipeline` om te controleren van de kopie voortgang uitvoeren.

Selecteer de kopie pijplijn die u hebt gemaakt in de lijst met **Activiteit Windows** .

![Wizard - overzichtspagina kopiëren](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

U kunt de details worden uitgevoerd in de **Activiteit venster Verkenner** in het rechterdeelvenster, inclusief het gegevensvolume lezen uit de bron en naar de bestemming, duur en de gemiddelde doorvoer voor de verwerking geschreven kopie weergeven.

Zoals u in de volgende schermafbeelding die u zien kunt, duurde 1 TB van Azure-blobopslag te kopiëren naar SQL Data Warehouse 14 minuten, effectief bereiken 1,22 GB/s doorvoer!

![Wizard - geslaagd dialoogvenster kopiëren](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Aanbevolen procedures
Hier volgen enkele aanbevolen procedures voor het uitvoeren van uw Data Warehouse van Azure SQL-database:

- Gebruik een grotere resource klasse bij het laden in een COLUMNSTORE GEGROEPEERDE INDEX.
- Voor een efficiënter joins af wijzen, kunt u overwegen hash verdeling door een kolom selecteren in plaats van standaard afronden robin distributie.
- Voor sneller laden snelheden, kunt u overwegen opslagruimte voor tijdelijke gegevens.
- Statistieken maken als u klaar bent met het laden van Azure SQL Data Warehouse.

Zie [Aanbevolen procedures voor Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) voor meer informatie. 

## <a name="next-steps"></a>Volgende stappen
- [Wizard kopiëren van gegevens Factory](data-factory-copy-wizard.md) - in dit artikel bevat informatie over de Wizard kopiëren. 
- [Kopie activiteit prestaties en afstemmen handleiding](data-factory-copy-activity-performance.md) - in dit artikel bevat de verwijzing prestatiemetingen en afstemmen handleiding.

