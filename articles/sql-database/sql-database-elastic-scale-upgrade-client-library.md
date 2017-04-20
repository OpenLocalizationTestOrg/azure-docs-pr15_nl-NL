< eigenschappen
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Een app voor het gebruik van de meest recente elastische database clientbibliotheek upgraden

Nieuwe versies van de [elastische Database client-bibliotheek](sql-database-elastic-database-client-library.md) zijn beschikbaar via de interface NuGetPackage Manager in Visual Studio NuGetand. Upgrades correcties bevatten en ondersteuning voor nieuwe mogelijkheden van de clientbibliotheek.

**Voor de meest recente versie:** Ga naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Bouw die tabellen opnieuw uw toepassing met de nieuwe bibliotheek, evenals de metagegevens van uw bestaande Shard kaart Manager die zijn opgeslagen in uw Azure SQL-Databases ter ondersteuning van nieuwe functies wijzigen.

Deze stappen uitvoert in volgorde zorgt ervoor dat oude versies van de clientbibliotheek niet meer aanwezig in uw omgeving zijn wanneer metagegevensobjecten worden bijgewerkt, wat betekent dat oude versies metagegevensobjecten na een upgrade niet worden gemaakt.   

## <a name="upgrade-steps"></a>Upgradestappen

**1. upgrade uitvoeren van uw toepassingen.** Klik in Visual Studio, downloaden en verwijzen naar de meest recente versie van de client-bibliotheek in al uw ontwikkelingsprojecten die de bibliotheek; gebruiken vervolgens opnieuw opbouwen en implementeren. 

 * Selecteer in de Visual Studio-oplossing **Extra** --> **NuGet Package Manager** -->  **NuGet-pakketten beheren voor oplossing**. 
 * (Visual Studio 2013) In het linkerdeelvenster, selecteert u **Updates**en selecteer vervolgens de knop **bijwerken** op de verpakking **Azure SQL Database elastische schalen clientbibliotheek** die wordt weergegeven in het venster.
 * (Visual Studio-2015) Stel het Filter upgrade **beschikbaar**. Selecteer het pakket wilt bijwerken en klik op de knop **bijwerken** .
    
 
 * Bouw en implementeer. 

**2. upgrade uitvoeren van scripts.** Als u **PowerShell** -scripts gebruikt voor het beheren van shards, [de nieuwe versie van de bibliotheek downloaden](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) en kopieer deze naar de map waaruit u uitvoeren van scripts. 

**3. upgrade uw service gesplitste samenvoegen.** Als u het databasehulpmiddel voor elastische gesplitste samenvoegen met een laptopgeheugen gegevens, [downloaden en implementeren van de nieuwste versie van het hulpmiddel](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)reorganiseren. Gedetailleerde Upgradestappen voor de Service vindt u [hier](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. upgrade van uw databases Shard kaart Manager**. De metagegevens ondersteuning van uw Shard kaarten in Azure SQL-Database bijwerken.  Er zijn twee manieren u kunt dit uitvoeren met PowerShell of C#. Beide opties worden weergegeven onder.

***Optie 1: De Upgrade metagegevens via PowerShell***

1. Download de meest recente hulpprogramma voor de opdrachtregel voor NuGet via [hier](http://nuget.org/nuget.exe) en opslaan in een map. 

2. Open een opdrachtprompt, navigeer naar de map en de opdracht:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Ga naar de submap met de nieuwe client-DLL versie die u hebt zojuist gedownload, bijvoorbeeld:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. De upgrade scriptlet voor elastische database-client downloaden van het [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)en sla het bestand in dezelfde map met het DLL-bestand.

5. Uit de map, "PowerShell.\upgrade.ps1" onderdelen uitvoeren vanaf de opdrachtprompt en volg de aanwijzingen.
 
***Optie 2: De Upgrade metagegevens met C#***

U kunt ook een Visual Studio-toepassing die wordt geopend uw ShardMapManager en kunt u de upgrade van metagegevens uitvoeren door de ondersteuning voor de methoden [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) en [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) zoals in dit voorbeeld wordt herhaald over alle shards maken: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Deze technieken voor metagegevens upgrades kunnen meerdere keren zonder schade worden toegepast. Bijvoorbeeld als een oudere clientversie per ongeluk een shard wordt gemaakt nadat u al hebt bijgewerkt, kunt u upgrade opnieuw uitvoeren in alle shards om ervoor te zorgen dat de meest recente metagegevensversie in uw infrastructuur aanwezig is. 

**Notitie:**  Nieuwe versies van de clientbibliotheek tot-datum gepubliceerd blijven werken met eerdere versies van de metagegevens Shard kaart Manager op Azure SQL-DB en vice versa.   Maar om te profiteren van enkele van de nieuwe functies in de meest recente client, metagegevens moet worden bijgewerkt.   Houd er rekening mee dat metagegevens upgrades heeft geen invloed op alle gebruikersgegevens of toepassingsspecifieke gegevens, alleen objecten die zijn gemaakt en die wordt gebruikt door de beheerder van de Map Shard.  En toepassingen blijven werken via het upgradeproces hierboven is beschreven. 

## <a name="elastic-database-client-version-history"></a>Versiegeschiedenis van elastische database-client 

Voor versiegeschiedenis, gaat u naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 