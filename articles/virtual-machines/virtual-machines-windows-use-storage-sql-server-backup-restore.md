<properties
    pageTitle="Het gebruik van Azure opslag voor SQL Server back-up en herstellen | Microsoft Azure"
    description="Leer hoe u de back-up van SQL Server met Azure Storage. Dit artikel wordt uitgelegd de voordelen van een back-up SQL-databases met Azure Storage."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Azure opslagruimte gebruiken voor SQL Server-back-up en herstellen

## <a name="overview"></a>Overzicht

Beginnen met SQL Server 2012 SP1 CU2, kunt u SQL Server-back-ups schrijven rechtstreeks naar de Azure Blob storage-service. U kunt deze functionaliteit maximaal maken en terugzetten van de service Azure Blob met een lokale SQL Server-database of een SQL Server-database op een Azure virtuele machine. Back-up naar cloud biedt voordelen van beschikbaarheid, onbeperkte geografische gerepliceerd buiten het bedrijf opslag- en gebruiksgemak migratie van gegevens naar en vanuit de cloud. Back-up of herstellen-instructies kunt u met behulp van Transact-SQL of SMO uitgeven.

SQL Server-2016 maakt u kennis met nieuwe mogelijkheden; u kunt [bestand-momentopname back-up](http://msdn.microsoft.com/library/mt169363.aspx) uitvoeren bijna onmiddellijk opgeleverd back-ups en zeer snel Hiermee herstelt.

In dit onderwerp wordt uitgelegd waarom u zou ervoor kan kiezen Azure opslag gebruiken voor SQL-back-ups en vervolgens beschrijving van de onderdelen die nodig zijn. U kunt de informatiebronnen die aan het einde van het artikel gebruiken voor toegang tot walkthroughs en aanvullende informatie aan de slag gaan deze service de back-ups van uw SQL Server.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Voordelen van het gebruik van de Azure Blob-Service voor back-ups van SQL Server

Er zijn verschillende uitdagingen die u betrokken back-ups van SQL Server. Deze uitdagingen opnemen opslagbeheer kans op pesterijen fout bij opslag, toegang tot externe opslag- en hardwareconfiguratie. Veel van deze uitdagingen worden besproken met behulp van de Azure Blob store-service voor back-ups van SQL Server. Houd rekening met de volgende voordelen:

- **Gebruiksgemak**: opslaan van uw back-ups op Azure BLOB's is een handige, flexibele en eenvoudig toegang tot externe optie. Maken van buiten het bedrijf opslagruimte voor uw back-ups van SQL Server is zo eenvoudig als het wijzigen van uw bestaande scripts/taken voor het gebruik van de syntaxis van de **Back-up-URL** . Externe opslag moet meestal ver genoeg van de locatie van de productie database om te voorkomen dat een enkel noodgevallen die van invloed op beide de buiten het bedrijf en productie database-locaties zijn mogelijk. U [geografische repliceren uw Azure BLOB's](../storage/storage-redundancy.md), hebt u een extra niveau van de beveiliging van een storing die kan invloed hebben op de hele regio.
- **Back-up archief**: het Azure Blob Storage-service biedt een beter alternatief voor de optie vaak gebruikte tape back-ups archiveren. Tapeopslag mogelijk fysieke transport aan een externe faciliteit en maateenheden wilt beveiligen van de media. Het opslaan van uw back-ups op Azure-blobopslag vindt u een chatgesprek, beschikbaar zijn, en een duurzame optie archiveren.
- **Beheerde hardware**: Er is geen realiseren van hardware management met Azure-services. Azure services beheren de hardware en redundantie en bescherming tegen hardwarefouten in geografische herhaling voorzien.
- **Onbeperkte opslag**: doordat een directe back-up naar Azure BLOB's die u hebt toegang tot vrijwel onbeperkte opslag. U kunt ook heeft maximaal een back-up een schijf Azure virtuele machines limieten op basis van machine grootte. Er is een beperking voor het aantal schijven die u aan een Azure virtuele machines back-ups kunt koppelen. Deze limiet is 16 schijven voor een extra groot exemplaar en minder voor kleinere exemplaren.
- **Beschikbaarheid van de back-up**: back-ups die zijn opgeslagen in Azure BLOB's zijn beschikbaar vanaf elke locatie en op elk gewenst moment en kunnen gemakkelijk worden geopend voor terugzetten naar de on-premises implementatie SQL Server of in een andere SQL Server uitgevoerd in een Azure Virtual Machine, zonder dat u een database koppelen/loskoppelen of downloaden en de VHD koppelen.
- **Kosten**: betalen alleen voor de service die wordt gebruikt. Kan zijn efficiënt als optie buiten het bedrijf en back-archief. Zie de [Azure prijzen Rekenmachine](http://go.microsoft.com/fwlink/?LinkId=277060 "Rekenmachine prijzen")en het [artikel Azure prijzen](http://go.microsoft.com/fwlink/?LinkId=277059 "prijzen artikel") voor meer informatie.
- **Opslag momentopnamen**: wanneer databasebestanden zijn opgeslagen in een Azure blob en SQL Server-2016 dat u gebruikt, kunt u de [back-up momentopname van de bestanden](http://msdn.microsoft.com/library/mt169363.aspx) gebruiken om uit te voeren bijna onmiddellijk opgeleverd back-ups en zeer snel Hiermee herstelt.

Zie [SQL Server-back-up en herstellen met Azure Blob Storage-Service](http://go.microsoft.com/fwlink/?LinkId=271617)voor meer informatie.

De volgende twee secties introduceren de Azure Blob storage-service, met inbegrip van de vereiste SQL Server-onderdelen. Het is belangrijk is voor meer informatie over de onderdelen en hun interactie voor het gebruiken van back-up en herstellen van de Azure Blob storage-service.

## <a name="azure-blob-storage-service-components"></a>De onderdelen van een Azure Blob Storage-Service

De volgende onderdelen van de Azure worden gebruikt wanneer een back-up met de Azure Blob storage-service.

| Onderdeel               | Beschrijving                          |
|---------------------|-------------------------------|
| **Opslag-Account** | De opslag-account is het beginpunt voor alle opslagservices. Voor toegang tot een Azure Blob Storage-service, moet u eerst een Azure Storage-account maken. Zie voor meer informatie over Azure Blob storage-service, [het gebruik van de Azure Blob Storage-Service](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Container** | Een container biedt een groepering van een set BLOB's, en een onbeperkt aantal BLOB's kunt opslaan. U schrijft een SQL Server back-up op een service Azure Blob moet ten minste de container hoofdmap is gemaakt. |
| **BLOB** | Een bestand van elk type en het formaat. BLOB's worden gebruikt met de volgende URL-notatie: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Zie [lidmaatschap blokkeren en BLOB van pagina's](http://msdn.microsoft.com/library/azure/ee691964.aspx) voor meer informatie over pagina BLOB's, |

## <a name="sql-server-components"></a>SQL Server-onderdelen

De volgende SQL Server-onderdelen worden gebruikt wanneer een back-up met de Azure Blob storage-service.

| Onderdeel               | Beschrijving                          |
|---------------------|-------------------------------|
| **URL** | Een URL bevat een id URI (Uniform Resource) aan een unieke back-upbestand. De URL wordt gebruikt voor de locatie en naam van de back-upbestand van SQL Server. De URL moet verwijzen naar een werkelijke blob, niet alleen een container. Als de blob niet bestaat, wordt deze is gemaakt. Als een bestaande blob is opgegeven, back-up is mislukt, tenzij de > met opmaakoptie is opgegeven. De volgende is een voorbeeld van de URL die u in de back-up-opdracht opgeven wilt: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS wordt aanbevolen maar niet vereist. |
| **Referentie** | De informatie die is vereist voor het verbinding maken en te verifiëren met Azure Blob storage-service wordt opgeslagen als een referentie.  In de volgorde voor SQL Server back-ups naar een Azure Blob of het herstellen van het schrijven, moet een referentie voor SQL Server worden gemaakt. Voor meer informatie raadpleegt u [Referentie voor SQL Server](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Als u wilt kopiëren en een back-bestand uploaden naar de Azure Blob storage-service kiest, moet u een pagina blob type gebruiken als uw opslagoptie voor de als u van plan bent dit bestand te gebruiken voor het herstellen van gegevens. TERUGZETTEN vanuit een blok blob type, mislukt met een fout.

## <a name="next-steps"></a>Volgende stappen

1. Maak een Azure-account als u er nog geen hebt. Als u Azure beoordeelt, kunt u de [gratis proefversie](https://azure.microsoft.com/free/).

1. Ga via een van de volgende zelfstudies waarmee u via een opslag-account maken en uitvoeren van een herstellen.

    - **SQL Server 2014**: [Zelfstudie: SQL Server 2014 back-up maken en deze herstellen naar Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **SQL Server-2016**: [Zelfstudie: met de Microsoft Azure Blob storage-service met SQL Server-2016 databases](https://msdn.microsoft.com/library/dn466438.aspx)

1. Raadpleeg de extra documentatie beginnen met [SQL Server-back-up en herstellen met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx).

Als er sprake is van problemen, raadpleegt u het onderwerp [SQL Server back-up URL aanbevolen procedures en probleemoplossing](https://msdn.microsoft.com/library/jj919149.aspx).

Voor andere SQL Server back-up maken en opties voor terugzetten, Zie [back-up en herstellen voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
