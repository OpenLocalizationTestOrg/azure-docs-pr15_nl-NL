<properties 
    pageTitle="Het beheren van Azure bestand Vgx. Cache | Microsoft Azure"
    description="Leer hoe u beheerderstaken zoals opnieuw opstarten en planning updates voor de Cache van Azure bestand Vgx."
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="how-to-administer-azure-redis-cache"></a>Het beheren van Azure-Cache bestand Vgx.

In dit onderwerp wordt beschreven hoe beheerderstaken zoals opnieuw opstarten en plannen van updates naar uw exemplaren Azure bestand Vgx. Cache.

>[AZURE.IMPORTANT] De juiste instellingen en functies die worden beschreven in dit artikel zijn alleen beschikbaar voor Premium laag cache.


## <a name="administration-settings"></a>Beheerinstellingen

Het **beheer** van de Azure bestand Vgx. Cache-instellingen kunnen u de volgende beheertaken uitvoeren voor de premium-cache. Voor toegang tot beheerinstellingen, klikt u op **Instellingen** of **alle instellingen** uit de Cache bestand Vgx. blade en blader naar de sectie **beheer** in het blad **Instellingen** .

![Beheer](./media/cache-administration/redis-cache-administration.png)

-   [Start opnieuw op](#reboot)
-   [Updates plannen](#schedule-updates)

## <a name="reboot"></a>Start opnieuw op

Het blad **opnieuw opstarten** kunt u een of meer knooppunten van de cache opnieuw. Hiermee kunt u uw toepassing voor tolerantie bij een storing testen.

![Start opnieuw op](./media/cache-administration/redis-cache-reboot.png)

Als u een premium-cache met cluster ingeschakeld hebt, kunt u welke shards van de cache op te starten.

![Start opnieuw op](./media/cache-administration/redis-cache-reboot-cluster.png)

Als u wilt een of meer knooppunten van de cache opnieuw, selecteert u de gewenste knooppunten en klik op **Start opnieuw op**. Als u een premium-cache met cluster ingeschakeld hebt, selecteert u de shard(s) te starten en klik vervolgens op **Start opnieuw op**. Na een paar minuten, het geselecteerde knooppunten opnieuw opstarten en er wordt een back-online later een paar minuten.

De invloed op clienttoepassingen varieert afhankelijk van de knooppunten die u opnieuw opstart.

-   **Basispagina** - wanneer de basispagina knooppunt opnieuw is opgestart, Azure bestand Vgx. Cache wordt overgenomen door het knooppunt replica en verhoogt het niveau van dit model. Tijdens een deze overname, is er mogelijk een kort interval waarin verbindingen aan de cache kunnen mislukken.
-   **Slave** - wanneer het knooppunt slave opnieuw is opgestart, is meestal geen gevolgen voor de cache-clients.
-   **Beide basispagina en slave** - wanneer beide knooppunten cache opnieuw worden gestart, worden alle gegevens gaat verloren in de cache en verbindingen met de cache-fail totdat het primaire knooppunt weer online komt. Als u [gegevens permanente](cache-how-to-premium-persistence.md)hebt geconfigureerd, worden de meest recente back-up is hersteld wanneer de cache weer online komt. Houd er rekening mee dat elke cache schrijft die zijn aangebracht na de meest recente back-up gaan verloren.
-   **Knooppunten van een premium-cache met cluster ingeschakeld** - wanneer u de knooppunten van een premium-cache opnieuw opstart met cluster ingeschakeld, met het gedrag is hetzelfde als wanneer u knooppunten van een niet-gegroepeerde cache opnieuw opstart.


>[AZURE.IMPORTANT] Opnieuw opstarten is alleen beschikbaar voor Premium laag cache.

## <a name="reboot-faq"></a>Start opnieuw op veelgestelde vragen

-   [Welk knooppunt moet ik start opnieuw op om te testen van mijn toepassing?](#which-node-should-i-reboot-to-test-my-application)
-   [Kan ik de cache als u wilt wissen clientverbindingen opnieuw opstarten?](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Ik gaan verloren gegevens uit mijn cache als ik opnieuw doen?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [Kan ik mijn met PowerShell CLI of andere hulpmiddelen voor projectbeheer cache opnieuw opstarten?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [Welke prijzen lagen, kan het opnieuw opstarten-functionaliteit gebruiken?](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Welk knooppunt moet ik start opnieuw op om te testen van mijn toepassing?

Als u wilt testen de tolerantie van uw toepassing tegen fouten van het primaire knooppunt van de cache, start opnieuw op het knooppunt **basispagina** . Als u wilt testen de tolerantie van uw toepassing tegen fouten van het secundaire knooppunt, start opnieuw op het knooppunt **Slave** . Als u wilt testen de tolerantie van uw toepassing ten opzichte van de totale uitval van de cache, start opnieuw op **beide** knooppunten.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Kan ik de cache als u wilt wissen clientverbindingen opnieuw opstarten?

Ja, als u de cache voor die alle clientverbindingen zijn uitgeschakeld opnieuw opstart. Dit kan handig zijn in de zaak waar alle clientverbindingen worden gebruikt, bijvoorbeeld vanwege een logicafout of een fout in de clienttoepassing. Elke prijzen laag heeft verschillende [client verbindingslimieten](cache-configure.md#default-redis-server-configuration) voor de verschillende grootte en zodra deze limieten worden bereikt, niet meer clientverbindingen worden geaccepteerd. De cache opgestart biedt een manier om te wissen van alle clientverbindingen.

>[AZURE.IMPORTANT] Als uw clientverbindingen zijn gebruikt vanwege een logicafout of fout in uw clientcode, houd er rekening mee dat StackExchange.Redis automatisch opnieuw verbonden zodra het knooppunt bestand Vgx. weer online is. Als het onderliggende probleem niet is opgelost, blijft de clientverbindingen worden gebruikt.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Ik gaan verloren gegevens uit mijn cache als ik opnieuw doen?

Als u het **diamodel** en de **Slave** knooppunten opnieuw opstart zijn alle gegevens in de cache (of in die shard als u een premium-cache gebruikt met cluster ingeschakeld) gaat verloren. Als u [gegevens permanente](cache-how-to-premium-persistence.md)hebt geconfigureerd, wordt de meest recente back-up worden hersteld wanneer de cache weer online gaat. Houd er rekening mee dat elke cache schrijft die zijn opgetreden nadat de back-up is gemaakt, gaan verloren.

Als u slechts één van de knooppunten opnieuw opstart, gegevens is niet meestal verloren, maar nog steeds kan zijn. Bijvoorbeeld als de basispagina knooppunt opnieuw is opgestart en een schrijven cache wordt uitgevoerd, de gegevens uit de cache schrijven is verbroken. Een ander scenario voor gegevensverlies zou zijn als u één knooppunt opnieuw opstarten en het andere knooppunt gebeurt er omlaag gaan vanwege een fout op hetzelfde moment. Zie voor meer informatie over mogelijke oorzaken van gegevensverlies [Wat is er gebeurd met mijn gegevens in het bestand Vgx.?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Kan ik mijn met PowerShell CLI of andere hulpmiddelen voor projectbeheer cache opnieuw opstarten?

Ja, voor PowerShell instructies Zie [Start opnieuw op een bestand Vgx. cache](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Welke prijzen lagen, kan het opnieuw opstarten-functionaliteit gebruiken?

Opnieuw is alleen beschikbaar in de prijzen van laag premium.

## <a name="schedule-updates"></a>Updates plannen

Het blad **planning updates** kunt u een onderhoudsvenster voor de cache aanwijzen. Wanneer het onderhoudsvenster is opgegeven, wordt een bestand Vgx. server bijgewerkt tijdens dit venster. Houd er rekening mee dat het onderhoudsvenster is alleen van toepassing op bestand Vgx. serverupdates en niet op een Azure bijwerkt of bijgewerkt naar het besturingssysteem van de VMs die als de cache host.

![Updates plannen](./media/cache-administration/redis-schedule-updates.png)

Als u een onderhoudsvenster, controleren van de gewenste dagen opgeven van het onderhoud venster start uur per dag en klik op **OK**. Houd er rekening mee dat het onderhoud venster in UTC is. 

>[AZURE.NOTE] Het venster standaard onderhoud voor updates is vijf uur. Deze waarde kan niet worden geconfigureerd vanaf de portal van Azure, maar u kunt deze configureren aan PowerShell met de `MaintenanceWindow` -parameter van de cmdlet [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) . Zie voor meer informatie [kan ik de geplande updates met PowerShell CLI of andere hulpmiddelen voor projectbeheer beheerd?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Veelgestelde vragen over updates plannen

-   [Wanneer updates worden uitgevoerd als ik de functie planning updates niet gebruikt?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Welk soort updates zijn doorgevoerd tijdens het venster gepland onderhoud?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Kan ik beheerde geplande updates met PowerShell CLI of andere beheerprogramma's?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [Welke prijzen lagen, kan de functie van de updates van planning gebruiken?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Wanneer updates worden uitgevoerd als ik de functie planning updates niet gebruikt?

Als u een onderhoudsvenster niet opgeeft, kunnen updates op elk gewenst moment worden aangebracht.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Welk soort updates zijn doorgevoerd tijdens het venster gepland onderhoud?

Alleen bestand Vgx. server updates zijn doorgevoerd tijdens het venster gepland onderhoud. Het onderhoudsvenster is niet van toepassing op updates voor Azure of het besturingssysteem VM.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Kan ik beheerde geplande updates met PowerShell CLI of andere beheerprogramma's?

Ja, kunt u de geplande updates met het volgende PowerShell-cmdlets beheren.

-   [Get-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Nieuwe AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Nieuwe AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [Verwijderen AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Welke prijzen lagen, kan de functie van de updates van planning gebruiken?

Updates plannen is alleen beschikbaar in de prijzen van laag premium.

## <a name="next-steps"></a>Volgende stappen

-   Meer [Azure bestand Vgx. Cache premium laag](cache-premium-tier-intro.md) functies verkennen.





