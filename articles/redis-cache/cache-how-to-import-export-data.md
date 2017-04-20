<properties 
    pageTitle="Importeren en exporteren van gegevens in de Cache van Azure bestand Vgx. | Microsoft Azure" 
    description="Meer informatie over het importeren en exporteren van gegevens naar en vanuit met uw exemplaren van de Azure bestand Vgx. Cache premium-blobopslag" 
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

# <a name="import-and-export-data-in-azure-redis-cache"></a>Gegevens importeren en exporteren in de Cache van Azure bestand Vgx.

Importeren/exporteren is een Azure bestand Vgx. Cache gegevens management bewerking die u kunt gegevens importeren in Azure bestand Vgx. Cache of gegevens exporteren vanuit Azure bestand Vgx. Cache door te importeren en exporteren van een momentopname van het bestand Vgx. Cache Database (RDB) uit een premium-cache naar een blob pagina in een Azure opslag-Account. Importeren/exporteren kunt u migreren tussen verschillende exemplaren van Azure bestand Vgx. Cache of vullen de cache met gegevens voor gebruik.

In dit artikel vindt u een handleiding voor het importeren en exporteren van gegevens met Azure bestand Vgx. Cache en vindt u antwoorden op veelgestelde vragen.

>[AZURE.IMPORTANT] Importeren/exporteren in de proefversie van is en is alleen beschikbaar voor [premium laag](cache-premium-tier-intro.md) cache.

## <a name="import"></a>Importeren

Importeren kan worden gebruikt om bestand Vgx. compatibele RDB bestanden die vanaf een willekeurige bestand Vgx. server die wordt uitgevoerd in een cloud of -omgeving, inclusief bestand Vgx. waarop Linux, Windows, of cloud providers zoals Amazon Web Services en anderen weer te geven. Het importeren van gegevens is een eenvoudige manier om te maken van een cache met vooraf ingestelde gegevens. Tijdens het importproces Azure bestand Vgx. Cache laadt de bestanden RDB van Azure opslagruimte in het geheugen en vervolgens de toetsen ingevoegd in de cache.

>[AZURE.NOTE] Voordat u de importbewerking begint, moet u ervoor zorgen dat uw bestand Vgx. Database (RDB)-bestand of de bestanden worden geüpload naar BLOB van pagina's in Azure opslag, in de regio en het abonnement als uw exemplaar van de Azure bestand Vgx. Cache. Zie [aan de slag met Azure-blobopslag](../storage/storage-dotnet-how-to-use-blobs.md)voor meer informatie. Als u uw RDB-bestand met de functie voor het [Exporteren van Azure bestand Vgx. Cache](#export) geëxporteerd, wordt uw bestand RDB al is opgeslagen in een pagina-blob en gereed is voor het importeren.

1. Een of meer geëxporteerde cache BLOB's, [bladert u naar de cache](cache-configure.md#configure-redis-cache-settings) in de portal van Azure importeren en klik op **gegevens importeren** uit het blad **Instellingen** van uw exemplaar van de cache.

    ![Gegevens importeren][cache-import-data]

2. **Kies Blob(s)** op en selecteer het account opslagruimte met de gegevens te importeren.

    ![Kies opslag-account][cache-import-choose-storage-account]

3. Klik op de container met de gegevens te importeren.

    ![Kies container][cache-import-choose-container]

4. Selecteer een of meer BLOB's door te klikken op het gebied aan de linkerkant van de naam van de blob importeren en klik vervolgens op **selecteren**.

    ![Kies BLOB 's][cache-import-choose-blobs]

5. Klik op **importeren** om te beginnen met het importproces.

    >[AZURE.IMPORTANT] De cache is niet toegankelijk voor cache clients tijdens het importeren en alle bestaande gegevens in de cache is verwijderd.

    ![Importeren][cache-import-blobs]

    U kunt de voortgang van de importbewerking controleren door te volgen de meldingen van de Azure-portal of door de gebeurtenissen in de [controle Meld u aan](cache-configure.md#support-amp-troubleshooting-settings)te bekijken.

    ![Voortgang importeren][cache-import-data-import-complete] 


## <a name="export"></a>Exporteren

Exporteren kunt u de gegevens die zijn opgeslagen in Azure bestand Vgx. Cache naar het bestand Vgx. compatibele RDB-bestanden die worden geëxporteerd. U kunt deze functie gebruiken om gegevens te verplaatsen vanaf één exemplaar van de Azure bestand Vgx. Cache naar een andere of naar een ander bestand Vgx. server. Tijdens het exportproces, een tijdelijk bestand wordt gemaakt op de VM waarop de server-instantie van Azure bestand Vgx. Cache en het bestand wordt geüpload naar de aangewezen opslag-account. Wanneer de exportbewerking is voltooid met een van beide de status van slagen of mislukt, wordt het tijdelijke bestand verwijderd.

1. Aan de huidige inhoud van de cache naar opslag, [Blader naar de cache](cache-configure.md#configure-redis-cache-settings) in de portal van Azure exporteren en klik op **gegevens exporteren** uit het blad **Instellingen** van uw exemplaar van de cache.

    ![Kies opslag container][cache-export-data-choose-storage-container]

2. Klik op **Choose opslag Container** en selecteer het gewenste opslag-account. De opslag-account moet zich in dezelfde abonnement en regio als uw cache.

    >[AZURE.IMPORTANT] Importeren/exporteren pagina BLOB's, die worden ondersteund door klassieke en ARM opslag accounts, maar niet worden ondersteund door [Blob storage accounts](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) op dit moment werkt.

    ![Opslag-account][cache-export-data-choose-account]

3. Kies de gewenste blob container en klik op **selecteren**. Met nieuwe een container, klikt u op **Container toevoegen** als u wilt deze eerst toevoegen en selecteert u deze in de lijst.

    ![Kies opslag container][cache-export-data-container]

4. Typ een **voorvoegsel Blob** en klik op **exporteren** om het proces exporteren te starten. Het voorvoegsel blob wordt gebruikt om de aanduiding voor de namen van bestanden die zijn gegenereerd door deze exportbewerking.

    ![Exporteren][cache-export-data]

    U kunt de voortgang van de exportbewerking controleren door te volgen de meldingen van de Azure-portal of door de gebeurtenissen in de [controle Meld u aan](cache-configure.md#support-amp-troubleshooting-settings)te bekijken.

    ![][cache-export-data-export-complete]

    Cache blijven beschikbaar maken voor gebruik tijdens het exportproces.


## <a name="importexport-faq"></a>Veelgestelde vragen over het importeren/exporteren

In deze sectie bevat veelgestelde vragen over de functie importeren/exporteren.

-   [Welke prijzen lagen kan importeren/exporteren gebruiken?](#what-pricing-tiers-can-use-importexport)
-   [Kan ik gegevens importeren vanuit een willekeurige server bestand Vgx.](#can-i-import-data-from-any-redis-server)
-   [Mijn cache is beschikbaar tijdens een bewerking importeren/exporteren?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Kan ik importeren/exporteren gebruiken met cluster bestand Vgx.](#can-i-use-importexport-with-redis-cluster)
-   [Hoe werkt importeren/exporteren met een aangepaste databases instellen?](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Importeren/exporteren op welke manier verschilt van bestand Vgx. permanente?](#how-is-importexport-different-from-redis-persistence)
-   [Kan ik automatiseren importeren/exporteren met PowerShell CLI of andere management-clients?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Ik heb ontvangen een time-out tijdens mijn importeren/exporteren. Wat betekent dit?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Ik krijg een foutmelding wanneer mijn gegevens exporteren naar Azure-blobopslag. Wat is er gebeurd?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Welke prijzen lagen kan importeren/exporteren gebruiken?

Importeren/exporteren is alleen beschikbaar in de prijzen van laag premium.

### <a name="can-i-import-data-from-any-redis-server"></a>Kan ik gegevens importeren vanuit een willekeurige server bestand Vgx.

Ja, behalve het importeren van gegevens die zijn geëxporteerd vanaf Azure bestand Vgx. Cache exemplaren, kunt u RDB bestanden importeren uit een bestand Vgx. server in cloud of -omgeving, zoals Linux, waarop Windows wordt uitgevoerd, of cloud providers zoals Amazon Web Services. Klik hiertoe RDB of het bestand in het gewenste bestand Vgx. server uploaden naar een pagina blob in een Azure opslag-Account en vervolgens te importeren in uw exemplaar van de Azure bestand Vgx. Cache premium. U wilt bijvoorbeeld de gegevens uit de cache productie exporteren en importeren in een cache gebruikt als onderdeel van een testomgeving voor testdoeleinden of de migratie. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Mijn cache is beschikbaar tijdens een bewerking importeren/exporteren?

-   **Exporteren** - cache blijven beschikbaar en u kunt doorgaan met het gebruik van de cache tijdens een exportbewerking.
-   **Importeren** - cache niet beschikbaar wanneer een importbewerking wordt gestart en worden beschikbaar maken voor gebruik wanneer de importbewerking is voltooid.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Kan ik importeren/exporteren gebruiken met cluster bestand Vgx.

Ja, en u kunt importeren/exporteren tussen een gegroepeerd cache en een niet-gegroepeerde cache. Sinds bestand Vgx. cluster [biedt alleen ondersteuning voor database 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), alle gegevens in databases dan 0 won't worden geïmporteerd. Wanneer een gegroepeerd cachegegevens zijn geïmporteerd, zijn de opnieuw van de toetsen verdeeld over de shards van het cluster. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Hoe werkt importeren/exporteren met een aangepaste databases instellen?

Sommige prijzen lagen hebben verschillende [databases limieten](cache-configure.md#databases), zodat er enkele overwegingen bij het importeren als u een aangepaste waarde voor geconfigureerd de `databases` instellen tijdens het maken van de cache.

-   Bij het importeren in een prijzen laag met een lagere `databases` limiet dan de laag waaruit u hebt geïmporteerd:
    -   Als u het standaardaantal `databases` welke 16 voor alle prijzen lagen is, zonder gegevens gaat verloren.
    -   Als u een aangepaste aantal `databases` die binnen de limieten valt voor de laag waaraan u importeert, zonder gegevens gaat verloren.
    -   Als uw geëxporteerde gegevens gegevens in een database die groter is dan de grenzen van de nieuwe laag bevatten, worden de gegevens uit die hoger databases zijn niet geïmporteerd.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Importeren/exporteren op welke manier verschilt van bestand Vgx. permanente?

Azure Cache bestand Vgx. permanente kunt u gegevens die zijn opgeslagen in het bestand Vgx. met Azure Storage. Wanneer permanente is geconfigureerd, persistent Azure bestand Vgx. Cache een momentopname van de cache bestand Vgx. in een bestand Vgx. binaire indeling naar schijf op basis van een configureerbare back-frequentie. Als er een fatale gebeurtenis plaatsvindt die zowel de primaire en de replica cache uitschakelt, wordt de cachegegevens hersteld automatisch met de meest recente momentopname. Lees [hoe u het configureren van de permanente gegevens voor een Premium Azure bestand Vgx. Cache](cache-how-to-premium-persistence.md)voor meer informatie.

Importeren / exporteren kunt u gegevens naar voren of exporteren uit Azure bestand Vgx. Cache. Deze niet configureren back-up en herstellen met behulp van bestand Vgx. permanente.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Kan ik automatiseren importeren/exporteren met PowerShell CLI of andere management-clients?

Ja, PowerShell instructies Zie voor [een bestand Vgx. cache importeren](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) en [exporteren van een bestand Vgx. cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Ik heb ontvangen een time-out tijdens mijn importeren/exporteren. Wat betekent dit?

Als u op het blad **gegevens importeren** of **exporteren van gegevens** voor meer dan 15 minuten voordat u begint met de bewerking blijven staan, ontvangt u een fout ongeveer als volgt uit.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Als u wilt de oplossing, het importeren starten of exportbewerking voordat u 15 minuten is verstreken.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Ik krijg een foutmelding wanneer mijn gegevens exporteren naar Azure-blobopslag. Wat is er gebeurd?

Importeren/exporteren werkt alleen met RDB-bestanden die zijn opgeslagen als BLOB van pagina's. Andere typen blob worden niet ondersteund op dit moment, inclusief blob storage accounts met warm- en leuke lagen.


## <a name="next-steps"></a>Volgende stappen
Leer hoe u meer cache premium-functies gebruiken.

-   [Inleiding tot de laag Premium van Azure Cache bestand Vgx.](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png








