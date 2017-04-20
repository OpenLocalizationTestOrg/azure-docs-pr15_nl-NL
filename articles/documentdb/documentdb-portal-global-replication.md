<properties
    pageTitle="De globale databasereplicatie DocumentDB | Microsoft Azure"
    description="Informatie over het beheren van de globale replicatie van uw account DocumentDB via de portal van Azure."
    services="documentdb"
    keywords="globale database, herhaling"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Het uitvoeren van algemene databasereplicatie DocumentDB met behulp van de Azure portal

Informatie over het gebruik van de Azure portal gegevens in meerdere regio's voor algemene beschikbaarheid van gegevens in Azure DocumentDB worden gerepliceerd.

Voor informatie over hoe globale database herhaling in DocumentDB, gaat [verdelen gegevens globaal met DocumentDB](documentdb-distribute-data-globally.md). Zie voor informatie over het uitvoeren van algemene databasereplicatie via een programma [ontwikkelen met meerdere landen/regio DocumentDB-accounts](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Globale verdeling van DocumentDB databases is algemeen beschikbaar en is automatisch ingeschakeld voor uw nieuwe DocumentDB-accounts. We werken om in te schakelen globale verdeling voor alle bestaande accounts, maar in de tussentijd, indien globale verdeling ingeschakeld voor uw account gewenst, neem [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) en we wordt inschakelen voor u nu.

## <a id="addregion"></a>Globale database regio's toevoegen

DocumentDB is beschikbaar in de meeste [Azure regio's] [azureregions]. Nadat u het niveau van de consistentie standaard voor uw databaseaccount selecteert, kunt u een of meer regio's koppelen (afhankelijk van uw keuze van standaard consistentie niveau en globale verdeling behoeften).

1. Klik in de [portal van Azure](https://portal.azure.com/)in de Jumpbar, klikt u op **DocumentDB-Accounts**.
2. Selecteer in het blad **DocumentDB Account** het databaseaccount te wijzigen.
3. Klik in het blad account op **gegevens globaal repliceren** in het menu.
4. Klik in het blad **gegevens globaal repliceren** , selecteert u de regio's toevoegen of verwijderen en klik vervolgens op **Opslaan**. Er is een kosten voor het toevoegen van regio's, raadpleegt u de [prijzen van de pagina](https://azure.microsoft.com/pricing/details/documentdb/) of het [verdelen gegevens globaal met DocumentDB](documentdb-distribute-data-globally.md) -artikel voor meer informatie.

    ![Klik op de regio's in de kaart wilt toevoegen of deze verwijderen][1]

### <a name="selecting-global-database-regions"></a>Globale database regio's selecteren

Tijdens het configureren van twee of meer regio's, het wordt aanbevolen dat regio's zijn ingeschakeld op basis van de regio paren die worden beschreven in de [bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR): Azure gepaarde t-toets regio's]  [ bcdr] artikel.

Tijdens het configureren van aan meerdere landen, controleert u met name om hetzelfde aantal regio's (+/-1 voor even/oneven) van elk van de gepaarde regio kolommen te selecteren. Bijvoorbeeld, als u implementeren naar de vier gebieden van de VS wilt, selecteert u twee Amerikaans delen vanaf de linkerkolom en twee vanaf de rechterkant. Zo is, de volgende handelingen uit in de gewenste set zou: West Amerikaans, Oost Amerikaans Noord centraal ons en Zuid centraal ons.

Deze instructies is belangrijk dat u volgt wanneer slechts twee regio's zijn geconfigureerd voor noodgevallen herstel scenario's. Voor meer dan twee regio's, na dit richtlijnen is een goed idee, maar niet kritieke zo lang maken als enkele van de geselecteerde regio's aan deze koppelen voldoen.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Volgende stappen

Informatie over het beheer van de consistentie van uw account globaal gerepliceerde [consistentie niveaus in DocumentDB](documentdb-consistency-levels.md)lezen.

Voor informatie over hoe globale database herhaling in DocumentDB werkt, raadpleegt u [de gegevens verdelen globaal met DocumentDB](documentdb-distribute-data-globally.md). Zie voor informatie over het via programmacode repliceren gegevens in meerdere gebieden, [ontwikkelen met meerdere landen/regio DocumentDB-accounts](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
