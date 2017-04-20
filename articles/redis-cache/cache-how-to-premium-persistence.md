<properties 
    pageTitle="Het configureren van de permanente gegevens voor een Cache Premium Azure bestand Vgx." 
    description="Meer informatie over het configureren en beheren van gegevens permanente uw Premium laag Azure bestand Vgx. Cache-sessies" 
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
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Het configureren van de permanente gegevens voor een Cache Premium Azure bestand Vgx.

Azure bestand Vgx. Cache heeft verschillende cache aanbiedingen waarmee flexibiliteit bij de keuze van cachegrootte en functies, waaronder de nieuwe Premium laag.

De premium-laag Azure bestand Vgx. Cache bevat functies zoals cluster, permanente en virtuele netwerkondersteuning. In dit artikel wordt beschreven hoe permanente configureren in een exemplaar van de Azure bestand Vgx. Cache premium.

Zie [Inleiding tot de laag Azure bestand Vgx. Cache Premium](cache-premium-tier-intro.md)voor informatie over andere functies van de cache premium.

## <a name="what-is-data-persistence"></a>Wat is permanente gegevens?
Permanente bestand Vgx. kunt u gegevens die zijn opgeslagen in het bestand Vgx.. U kunt ook momentopnamen en back-up van de gegevens die u als een hardware is opgetreden laden kunt. Dit is een enorme voordeel ten opzichte van Basic of standaard laag waar alle gegevens worden opgeslagen in het geheugen en kunnen er potentieel gegevensverlies als een waar Cache knooppunten zijn niet beschikbaar is opgetreden. 

Azure bestand Vgx. Cache biedt bestand Vgx. permanente met het [model RDB](http://redis.io/topics/persistence), waar de gegevens worden opgeslagen in een Azure opslag-account. Wanneer permanente is geconfigureerd, persistent Azure bestand Vgx. Cache een momentopname van de cache bestand Vgx. in een bestand Vgx. binaire indeling naar schijf op basis van een configureerbare back-frequentie. Als er een fatale gebeurtenis plaatsvindt die zowel de primaire en de replica cache uitschakelt, wordt de cache opnieuw met de meest recente momentopname opgebouwd.

Permanente kan worden geconfigureerd vanaf het **Nieuwe bestand Vgx. Cache** blad tijdens het maken van de cache en klik op het blad **Instellingen** voor bestaande premium cache.

## <a name="create-a-premium-cache"></a>Een premium-cache maken

Als u wilt een cache maken en configureren van permanente, aanmelden bij de [portal van Azure](https://portal.azure.com) en klikt u op **Nieuw**->**gegevens + opslagruimte**>**Bestand Vgx. Cache**.

![Een bestand Vgx. Cache maken][redis-cache-new-cache-menu]

Als u wilt configureren permanente, selecteert u een van de **Premium** -cache eerst in het blad **kiest u uw prijzen laag** .

![Kies uw prijzen laag][redis-cache-premium-pricing-tier]

Wanneer u een premium prijzen van laag is geselecteerd, klikt u op **bestand Vgx. permanente**.

![Bestand permanente Vgx.][redis-cache-persistence]

De stappen in de volgende sectie wordt uitgelegd hoe u het bestand Vgx. permanente configureren op uw nieuwe premium-cache. Nadat het bestand Vgx. permanente is geconfigureerd, klikt u op **maken** om uw nieuwe premium-cache bij een bestand Vgx. permanente verbinding maken.

## <a name="configure-redis-persistence"></a>Configureren permanente bestand Vgx.

Bestand Vgx. permanente is geconfigureerd op het blad **bestand Vgx. permanente gegevens** . Deze blade wordt geopend tijdens het maken van cache voor nieuwe cache, zoals wordt beschreven in de vorige sectie. Voor bestaande cache, is het blad **bestand Vgx. gegevens permanente** toegankelijk vanuit het blad **Instellingen** voor de cache.

![Bestand Vgx instellingen.][redis-cache-settings]

Om te schakelen bestand Vgx. permanente, klikt u op **ingeschakeld** om in te schakelen RDB (database bestand Vgx.) back-up. Als u wilt uitschakelen permanente bestand Vgx. Klik op een eerder ingeschakeld premium-cache, klikt u op **uitgeschakeld**.

Om het back-interval configureren, selecteert u een **Back-frequentie** in de vervolgkeuzelijst. Kunt kiezen uit **15 minuten**, **30 minuten** **60 minuten**, **6 uur**, **12 uur**en **24 uur**. Dit interval begint te tellen omlaag nadat de vorige back-up is voltooid en wanneer deze verstrijkt een nieuwe back-up wordt gestart.

Op **Opslag-Account** als u wilt selecteren de rekening opslagruimte wilt gebruiken en kiest u de **primaire sleutel** of de **secundaire sleutel** uit de vervolgkeuzelijst **Opslag-toets** gebruiken. Moet u een account opslagruimte in hetzelfde gebied, als de cache en een **Premium-opslag** -account wordt aanbevolen, omdat de premium-opslag heeft sneller worden verwerkt. 

>[AZURE.IMPORTANT] Als de sleutel opslagruimte voor uw account permanente opnieuw wordt gegenereerd, moet u de gewenste toets rechoose in de vervolgkeuzelijst **Storage Key** .

![Bestand permanente Vgx.][redis-cache-persistence-selected]

Klik op **OK** om de configuratie permanente opslaan.

De volgende back-up (of de eerste back-up maken voor nieuwe cache) wordt gestart zodra de back-frequentie is verstreken.



## <a name="persistence-faq"></a>Veelgestelde vragen over permanente

De volgende lijst bevat antwoorden op veelgestelde vragen over permanente Azure bestand Vgx. Cache.

-   [Kan ik gebruiken op een eerder gemaakte cache inschakelen?](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Kan ik de frequentie van de back-ups wijzigen nadat ik de cache maken?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Waarom als er een back-frequentie van 60 minuten ziet u meer dan 60 minuten tussen back-ups?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Wat gebeurt er met de oude back-ups wanneer een nieuwe back-up wordt gemaakt?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Wat gebeurt er als ik aan een andere grootte hebt aangepast en een back-up is hersteld dat is aangebracht voordat u de schaal bewerking?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Kan ik gebruiken op een eerder gemaakte cache inschakelen?

Ja, bestand Vgx. permanente kan worden geconfigureerd in cache maken voor zowel bestaande premium in de cache van.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Kan ik de frequentie van de back-ups wijzigen nadat ik de cache maken?

Ja, kunt u de back-frequentie op het blad **bestand Vgx. permanente gegevens** wijzigen. Zie [permanente configureren bestand Vgx.](#configure-redis-persistence)voor instructies.

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Waarom als er een back-frequentie van 60 minuten ziet u meer dan 60 minuten tussen back-ups?

De back-frequentie begint niet totdat de vorige back-up is voltooid. Als de frequentie van de back-up 60 minuten is en het duurt een back-proces 15 minuten te voltooien, start de volgende back-up niet pas 75 minuten na de begintijd van de vorige back-up.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Wat gebeurt er met de oude back-ups wanneer een nieuwe back-up wordt gemaakt?

Alle back-ups, behalve voor de meest recente bewerking worden automatisch verwijderd. Deze verwijdering mogelijk niet onmiddellijk plaatsvindt, maar oudere back-ups niet voor onbepaalde tijd worden doorgevoerd.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Wat gebeurt er als ik aan een andere grootte hebt aangepast en een back-up is hersteld dat is aangebracht voordat u de schaal bewerking?

-   Als u de schaal naar groter hebt aangepast, is er geen gevolgen.
-   Als u hebt aangepast kleiner en er een aangepaste [databases](cache-configure.md#databases) die is groter dan de [databases beperken](cache-configure.md#databases) voor de nieuwe grootte van de gegevens in die databases is niet worden hersteld. Zie voor meer informatie [Is mijn aangepaste databases desbetreffende instellen tijdens het schaalbaarheid?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Als u hebt aangepast kleiner en er niet voldoende ruimte in de kleinere grootte hebben de gegevens uit de laatste back-up voor, wordt toetsen tijdens het herstelproces, meestal met het beleid van de verwijdering [allkeys-recentelijk](http://redis.io/topics/lru-cache) worden verwijderd.

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van meer premiumfuncties van cache.

-   [Inleiding tot de laag Premium van Azure Cache bestand Vgx.](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
