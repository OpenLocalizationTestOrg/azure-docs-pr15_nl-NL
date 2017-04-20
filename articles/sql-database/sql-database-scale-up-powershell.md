<properties 
    pageTitle="De service laag en prestaties het niveau van een Azure SQL-database via PowerShell wijzigen | Microsoft Azure" 
    description="Wijzig de servicelaag en prestatieniveau van een Azure SQL-database ziet u hoe u de schaal van de SQL-database omhoog of omlaag met PowerShell. De prijzen laag van een Azure SQL-database met PowerShell wijzigen." 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>De service laag en prestaties niveau wijzigen (prijzen laag) van een SQL-database met PowerShell


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-scale-up.md)
- [**PowerShell**](sql-database-scale-up-powershell.md)


Service niveaus en prestaties beschrijven van de functies en bronnen die beschikbaar zijn voor uw SQL-database en kunnen worden bijgewerkt als de behoeften van uw wijziging toepassing. Zie [Servicelagen](sql-database-service-tiers.md)voor meer informatie.

Houd er rekening mee dat de servicelaag wijzigt en/of prestatieniveau van een database Hiermee maakt u een replica van de oorspronkelijke database op het prestatieniveau van de nieuwe en vervolgens wordt overgeschakeld verbindingen naar de replica. Er zijn geen gegevens gaat verloren tijdens dit proces, maar tijdens het kort moment wanneer we naar de replica overzet, verbindingen met de database zijn uitgeschakeld, zodat sommige transacties in flight kunnen worden hersteld. In dit venster varieert, maar is gemiddeld onder 4 seconden en in meer dan 99% van zaken is minder dan 30 seconden. Zeer af en toe zijn met name als er grote aantallen transacties in flight op het moment dat verbindingen zijn uitgeschakeld, dit venster mogelijk langer.  

De duur van het hele schaal-up-proces is afhankelijk van de grootte en service laag van de database voor en na de wijziging. Een database 250 GB dat is wijzigen aan, uit of binnen een laag Standard service moet bijvoorbeeld voltooid binnen 6 uur. Voor een database van dezelfde grootte prestatieniveaus binnen de Premium-servicelaag wordt gewijzigd, moet deze voltooid binnen 3 uur.


- Als u wilt een database downgraden, moet de database kleiner zijn dan de toegestane maximumgrootte van de laag doel-service. 
- Bij het bijwerken van een database met [Geografische-replicatie](sql-database-geo-replication-portal.md) is ingeschakeld, moet u eerst de secundaire databases upgraden naar de gewenste prestaties laag voordat u een upgrade van de hoofddatabase.
- Wanneer een downgrade uitvoeren van een laag Premium-service, moet u eerst alle geografische herhaling relaties beëindigen. U kunt de stappen beschreven in het onderwerp [van een storing herstellen](sql-database-disaster-recovery.md) om het replicatieproces tussen de primaire en de actieve secundaire databases te stoppen.
- De diensten herstellen zijn verschillend voor de verschillende niveaus van de service. Als u zijn Downgrade u mogelijk de mogelijkheid om te zetten naar een punt in tijd kwijtraakt of hebben een lagere back-up bewaarperiode. Zie [Azure SQL Database-back-up en herstellen](sql-database-business-continuity.md)voor meer informatie.
- De nieuwe eigenschappen voor de database worden niet toegepast totdat de wijzigingen voltooid zijn.



**Als u wilt voltooien van dit artikel nodig u het volgende:**

- Een Azure-abonnement. Als u gewoon nodig een Azure-abonnement hebt op **Gratis ACCOUNT** boven aan deze pagina en keert u terug naar het voltooien van dit artikel.
- Een Azure SQL-database. Als u een SQL-database niet hebt, maakt u één de stappen in dit artikel: [uw eerste Azure SQL-Database maken](sql-database-get-started.md).
- Azure PowerShell.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Het serviceniveau laag en prestaties van uw SQL-database wijzigen

De [Set-AzureRmSqlDatabase] uitvoeren (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) cmdlet en instellen-de **-RequestedServiceObjectiveName** de mogelijkheden van de prestaties van de gewenste prijzen laag; bijvoorbeeld *S0*, *S1*, *S2*, *S3*, *P1*, *P2*,...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>Voorbeeld van de PowerShell-script om de service laag en prestaties het niveau van uw SQL-database

Vervang ```{variables}``` met de waarden (niet de accolades omvatten).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Volgende stappen

- [Uit- en schaal](sql-database-elastic-scale-get-started.md)
- [Verbinding maken en een SQL-database met SSMS query](sql-database-connect-query-ssms.md)
- [Een Azure SQL-database exporteren](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Aanvullende informatie

- [Bedrijfscontinuïteit voor bedrijfsoverzichten](sql-database-business-continuity.md)
- [SQL-Database documentatie](http://azure.microsoft.com/documentation/services/sql-database/)
- [Azure SQL Database-Cmdlets] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/bibliotheek/azure/mt619433(v=azure.300\).aspx.aspx)