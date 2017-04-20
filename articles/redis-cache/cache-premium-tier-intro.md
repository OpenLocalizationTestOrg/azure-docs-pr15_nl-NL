<properties 
    pageTitle="Inleiding tot de laag Azure bestand Vgx. Cache Premium | Microsoft Azure" 
    description="Informatie over het maken en beheren van bestand Vgx. permanente, bestand Vgx. cluster en VNET ondersteuning voor uw Premium laag Azure bestand Vgx. Cache-sessies" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Inleiding tot de laag Premium van Azure Cache bestand Vgx.
Azure bestand Vgx. Cache is een gedistribueerde, beheerde cache waarmee u ten zeerste scalable en heeft gereageerd toepassingen doordat zeer snel toegang tot uw gegevens samen. 

De nieuwe Premium-laag is een Enterprise klaar laag, waaronder alle functies van de standaard lagen en meer, zoals betere prestaties, groter-werkbelastingen, herstel, importeren/exporteren en verbeterde beveiliging. Doorgaan met lezen voor meer informatie over extra functies van het niveau van de cache Premium.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Betere prestaties in vergelijking met standaard of Basic laag
**Betere prestaties via standaard of eenvoudige laag.** Cache in de Premium-laag zijn geïmplementeerd op hardware die sneller processors en biedt betere prestaties vergeleken naar de Basic of standaard laag. Premium laag cache hebben sneller worden verwerkt en lagere vertragingstijden. 

**Doorvoer voor de Cache voor dezelfde grootte is hoger in Premium ten opzichte van de standaard laag.** Bijvoorbeeld de doorvoer van een 53 GB P4 (Premium) cache is 250K aanvragen per seconde ten opzichte van 150 K voor C6 (standaard).

Zie voor meer informatie over de grootte, doorvoer en bandbreedte met premium cache, [Azure bestand Vgx. Cache Veelgestelde vragen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Bestand gegevens permanente Vgx.
De Premium-laag kunt u de cache-gegevens in een opslag van Azure-account. In de cache van een Basic/standaard alle gegevens alleen in het geheugen opgeslagen. Onderliggende infrastructuur voor het geval zijn er problemen potentieel gegevensverlies. We raden u aan de functie gegevens permanente bestand Vgx. in de Premium-laag gebruiken om uit te breiden tolerantie tegen verlies van gegevens. Azure bestand Vgx. Cache biedt RDB en AOF (binnenkort) opties in het [bestand Vgx. permanente](http://redis.io/topics/persistence). 

Zie voor instructies over het configureren van permanente [permanente voor een Premium Azure bestand Vgx. Cache configureren](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Bestand Vgx cluster.
Als u wilt maken cache groter zijn dan 53 GB of shard gegevens op verschillende bestand Vgx. knooppunten wilt, kunt u bestand Vgx. cluster die beschikbaar is in de Premium-laag. Elk knooppunt bestaat uit een primaire/replica cache paar beheerd door Azure beschikbaarheid. 

**Bestand Vgx. cluster kunt u maximale schaal en doorvoer.** Doorvoer Hiermee lineair verhoogt u het aantal shards (knooppunten) in het cluster te vergroten. Bv. Als u een P4 cluster van 10 shards maakt, is de beschikbare doorvoer 250K * 10 = 2,5 miljoen aanvragen per seconde. Raadpleeg de [Azure bestand Vgx. Cache Veelgestelde vragen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) voor meer informatie over de grootte, doorvoer en bandbreedte met premium cache.

Lees [hoe u het cluster voor een Premium Azure bestand Vgx. Cache configureren](cache-how-to-premium-clustering.md)om te beginnen met cluster.

##<a name="enhanced-security-and-isolation"></a>Verbeterde beveiliging en moeten worden geïsoleerd

Cache gemaakt in de laag Basic of standaard zijn toegankelijk op openbare internet. Toegang tot de Cache is beperkt op basis van de access-toets. Met de Premium-laag kunt u verder ervoor zorgen dat alleen voor clients in een netwerk met een opgegeven toegang hebben tot de Cache. U kunt implementeren bestand Vgx. Cache in een [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/). Kunt u alle functies van VNet zoals subnetten, clienttoegangsbeleid besturingselement, en andere functies die u kunt verdere toegang tot het bestand Vgx. beperken.

Lees [hoe u het virtuele netwerkondersteuning voor een Premium Azure bestand Vgx. Cache configureren](cache-how-to-premium-vnet.md)voor meer informatie.

## <a name="importexport"></a>Importeren/exporteren

Importeren/exporteren is een Azure bestand Vgx. Cache data management bewerking waarmee u gegevens importeren in Azure bestand Vgx. Cache of gegevens exporteren vanuit Azure bestand Vgx. Cache door te importeren en exporteren van een momentopname van het bestand Vgx. Cache Database (RDB) uit een premium-cache naar een blob pagina in een Azure opslag-Account. Hiermee kunt u migreren tussen verschillende exemplaren van Azure bestand Vgx. Cache of vullen de cache met gegevens voor gebruik.

Importeren kan worden gebruikt om bestand Vgx. compatibele RDB bestanden die vanaf een willekeurige bestand Vgx. server die wordt uitgevoerd in een cloud of -omgeving, inclusief bestand Vgx. waarop Linux, Windows, of cloud providers zoals Amazon Web Services en anderen weer te geven. Het importeren van gegevens is een eenvoudige manier om te maken van een cache met vooraf ingestelde gegevens. Tijdens het importproces Azure bestand Vgx. Cache laadt de bestanden RDB van Azure opslagruimte in het geheugen en vervolgens de toetsen ingevoegd in de cache.

Exporteren kunt u de gegevens die zijn opgeslagen in Azure bestand Vgx. Cache naar het bestand Vgx. compatibele RDB-bestanden die worden geëxporteerd. U kunt deze functie gebruiken om gegevens te verplaatsen vanaf één exemplaar van de Azure bestand Vgx. Cache naar een andere of naar een ander bestand Vgx. server. Tijdens het exportproces, een tijdelijk bestand wordt gemaakt op de VM waarop de server-instantie van Azure bestand Vgx. Cache en het bestand wordt geüpload naar de aangewezen opslag-account. Wanneer de exportbewerking is voltooid met een van beide de status van slagen of mislukt, wordt het tijdelijke bestand verwijderd.

Lees [hoe u gegevens importeren en exporteren van gegevens van Azure bestand Vgx. Cache](cache-how-to-import-export-data.md)voor meer informatie.

## <a name="reboot"></a>Start opnieuw op

De premium-laag kunt u een of meer knooppunten van de cache op aanvraag starten. Hiermee kunt u uw toepassing voor tolerantie bij een storing testen. U kunt de volgende knooppunten opstarten.

-   Basispagina knooppunt van de cache
-   Slave knooppunt van de cache
-   Hoofd- en slave knooppunten van de cache
-   Wanneer u een premium-cache met cluster, kunt u het model, slave of beide knooppunten voor afzonderlijke shards in de cache opstarten

Zie [Start opnieuw op](cache-administration.md#reboot) en [Start opnieuw op veelgestelde vragen](cache-administration.md#reboot-faq)voor meer informatie.

## <a name="schedule-updates"></a>Updates plannen

De functie geplande updates kunt u een onderhoudsvenster voor de cache aanwijzen. Wanneer het onderhoudsvenster is opgegeven, wordt een bestand Vgx. server bijgewerkt tijdens dit venster. Als u wilt een onderhoudsvenster aanwijzen, selecteert u de gewenste dagen en het onderhoud starten venster uur per dag. Houd er rekening mee dat het onderhoud venster in UTC is. 

Zie [updates plannen](cache-administration.md#schedule-updates) en [updates plannen Veelgestelde vragen](cache-administration.md#schedule-updates-faq)voor meer informatie.

>[AZURE.NOTE] Alleen bestand Vgx. server updates zijn doorgevoerd tijdens het venster gepland onderhoud. Het onderhoudsvenster is niet van toepassing op updates voor Azure of het besturingssysteem VM.

## <a name="to-scale-to-the-premium-tier"></a>Voor de laag premium

Als u naar de premium-laag wilt verkleinen, kies een van de premium-niveaus in het blad **wijzigen prijzen laag** . U kunt ook de cache naar de premium-laag met PowerShell en CLI schalen. Zie [hoe u de schaal Azure bestand Vgx. Cache](cache-how-to-scale.md) en [hoe u een schaal bewerking automatiseren](cache-how-to-scale.md#how-to-automate-a-scaling-operation)voor stapsgewijze instructies.

## <a name="next-steps"></a>Volgende stappen

Maak een cache en ontdek de nieuwe functies van de premium-laag.

-   [Het configureren van permanente voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-persistence.md)
-   [Het configureren van Virtual Network ondersteuning voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-vnet.md)
-   [Het configureren van cluster voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-clustering.md)
-   [Hoe u gegevens importeren in en gegevens exporteren uit de Cache van Azure bestand Vgx.](cache-how-to-import-export-data.md)
-   [Het beheren van Azure-Cache bestand Vgx.](cache-administration.md)
  

