<properties
    pageTitle="Lijst van Azure opslag Resources met de bibliotheek met Microsoft Azure opslag-Client voor C++ | Microsoft Azure"
    description="Leer hoe u de vermelding API's gebruiken in Microsoft Azure opslag Client-bibliotheek voor C++ containers, BLOB's, wachtrijen, tabellen en entiteiten opsommen."
    documentationCenter=".net"
    services="storage"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="list-azure-storage-resources-in-c"></a>Lijst met Azure opslag Resources in C++

Vermelding bewerkingen zijn toets om te veel development-scenario's met Azure opslagmedia. In dit artikel wordt beschreven hoe zo efficiënt mogelijk opsommen objecten in de vermelding API's die zijn opgegeven in de bibliotheek met Microsoft Azure opslag-Client voor C++ met Azure-opslag.

>[AZURE.NOTE] Deze handleiding is bedoeld voor de bibliotheek Azure opslag-Client voor C++ versie 2.x, dat wil verkrijgbaar via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub zeggen](https://github.com/Azure/azure-storage-cpp).

De bibliotheek van de Client opslagruimte biedt allerlei manieren aan lijst- of query objecten in Azure Storage. In dit artikel worden de volgende scenario's:

-   Lijst met containers in een account
-   Lijst BLOB's in een container of virtuele blob directory
-   Lijst wachtrijen in een account
-   Lijst met tabellen in een account
-   Query entiteiten in een tabel

Elk van de volgende manieren wordt weergegeven met behulp van verschillende overbelastingen voor verschillende scenario's.

## <a name="asynchronous-versus-synchronous"></a>Asynchrone of synchrone

Omdat de bibliotheek van de Client opslag voor C++ is gebouwd in de [REST van de C++-bibliotheek](https://github.com/Microsoft/cpprestsdk), we impliciet worden ondersteund asynchrone bewerkingen met behulp van [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Bijvoorbeeld:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Synchrone bewerkingen laten teruglopen de bijbehorende asynchrone bewerkingen:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

Als u met meerdere threads toepassingen of services werkt, is het raadzaam dat u het asynchrone API's rechtstreeks in plaats van het maken van een thread gebruikt om te bellen van de synchronisatie-API's, waarin aanzienlijk heeft gevolgen voor uw optreden.

## <a name="segmented-listing"></a>Gesegmenteerde vermelding

De schaal van cloudopslag vereist gesegmenteerde aanbieding. U kunt bijvoorbeeld via een miljoen BLOB's in een container Azure blob of een miljard entiteiten in een tabel Azure hebben. Dit zijn niet theoretische getallen, maar reële klant gebruik gevallen.

Daarom niet praktisch voor een overzicht van alle objecten in één antwoord. U kunt in plaats daarvan een lijst met objecten met behulp van paginering. Elk van de vermelding API's heeft een *gesegmenteerd* overbelastingen.

Het antwoord voor een bewerking gesegmenteerde vermelding bevat:

-   <i>_segment</i>, waarin de set resultaten geretourneerd voor één aanroep van de vermelding-API.
-   *continuation_token*, dat wordt doorgegeven aan de volgende oproep om te krijgen van de volgende pagina met resultaten. Wanneer er geen meer weer te geven resultaten, is het token voortgezet is null.

Een normale oproep voor een overzicht van alle BLOB's in een container kan bijvoorbeeld de volgende codefragment eruit. De code is beschikbaar in onze [voorbeelden](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Houd er rekening mee dat het aantal resultaten geretourneerd door een pagina kan worden beheerd door de parameter *max_results* in de overbelasting van elke API, bijvoorbeeld:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Als u de parameter *max_results* niet opgeeft, wordt de standaardwaarde voor maximale van maximaal 5000 resultaten geretourneerd in één pagina.

Bedenk ook dat een query op Azure-tabelopslag geen records, of minder records zijn dan de waarde van de parameter weer *max_results* die u hebt opgegeven, retourneren kan zelfs als het token voortgezet niet leeg is. Een reden mogelijk dat de query kan niet worden voltooid in vijf seconden. De query moet blijven zo lang maken als het token voortgezet niet leeg is, en uw code moet niet wordt ervan uitgegaan dat de grootte van het segment resultaten.

De aanbevolen coding patroon voor de meeste scenario's is onderverdeeld vermelding, waar u expliciete voortgang van de vermelding of query's uitvoeren en hoe de service moet reageren op elk verzoek om een. Met name voor C++-toepassingen en services kunt lagere niveaus controle over de voortgang van de vermelding besturingselement geheugen en prestaties.

## <a name="greedy-listing"></a>Hebzuchtig aanbieding

Eerdere versies van de bibliotheek van de Client opslag voor C++ (versies 0.5.0 voorbeeld bekijken en eerder) opgenomen van niet-gesegmenteerd vermelding API's voor tabellen en wachtrijen, zoals in het volgende voorbeeld:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Deze methoden zijn geïmplementeerd als aanvullende inhoud gesegmenteerde API's. Voor elk antwoord van gesegmenteerde aanbieding, wordt de code de resultaten toegevoegd aan een vector en alle resultaten geretourneerd nadat de volledige containers zijn gescand.

Deze methode werkt mogelijk als het opslag-account of de tabel een klein aantal objecten bevat. Echter met een stijging van het aantal objecten, kan het geheugen vereist verhogen zonder limiet, omdat alle resultaten bleef in het geheugen. Één vermelding bewerking kunt erg lang duren, waarin de beller had geen informatie over de voortgang daarvan.

Deze hebzuchtig vermelding API's in de SDK niet aanwezig zijn in C#, Java of de JavaScript Node.js-omgeving. Als u wilt voorkomen dat de potentiële problemen van het gebruik van deze hebzuchtig API's, wordt verwijderd wanneer u deze versie 0.6.0 Preview.

Als uw code deze hebzuchtig API's bellen is:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Vervolgens moet u uw code voor het gebruik van de gesegmenteerde vermelding API's wijzigen:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

Als u de parameter *max_results* van het segment opgeeft, kunt u saldo vanaf tussen de nummers van aanvragen en geheugengebruik om te voldoen aan Prestatieoverwegingen voor uw toepassing.

Bovendien als u gesegmenteerde vermelding API's gebruikt, maar de gegevens in een lokale verzamelen in een 'hebzuchtig' stijl opgeslagen, maar ook wordt aangeraden dat u refactoring van de code voor het verwerken van de gegevens opslaat in een lokale verzameling zorgvuldig bij het op schaal.

## <a name="lazy-listing"></a>Fikse vermelding

Hoewel hebzuchtig vermelding potentiële problemen verheven, is het handig als er niet teveel objecten in de container.

Als u ook C# of Oracle Java SDK's gebruikt, moet u vertrouwd met het overzicht programming model, die een fikse stijl van een vermelding biedt, waar de gegevens van een bepaalde verschuiving alleen opgehaald indien nodig zijn. In C++ de iterator gebaseerde sjabloon biedt een soortgelijke manier.

Een typische fikse vermelding API, met **list_blobs** als voorbeeld, ziet er zo uit:

    list_blob_item_iterator list_blobs() const;

Een typische codefragment waarin het patroon fikse aanbieding uitzien als volgt:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Houd er rekening mee dat fikse aanbieding alleen beschikbaar in synchrone modus is.

Vergeleken met hebzuchtig vermelding, haalt fikse vermelding gegevens alleen indien nodig. Achter de ophaalt deze gegevens uit de opslag van Azure alleen wanneer de volgende herhaler naar volgende segment overgezet. Daarom geheugengebruik wordt beheerd met een gebonden grootte en de bewerking is snel.

Fikse vermelding API's worden opgenomen in de bibliotheek van de Client opslag voor C++ in versie 2.2.0.

## <a name="conclusion"></a>Sluiten

In dit artikel besproken we verschillende overbelastingen voor het aanbieden van API's voor diverse objecten in de bibliotheek van de Client opslag voor C++. Samenvatting:

-   Asynchrone APIs wordt ten zeerste aanbevolen onder meerdere threads scenario's.
-   Gesegmenteerde vermelding geschikt is voor de meeste scenario's.
-   Fikse vermelding wordt weergegeven in de bibliotheek als een handige Wikkel in synchroon scenario's.
-   Hebzuchtig aanbieding wordt niet aanbevolen en is verwijderd uit de bibliotheek.

##<a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over de opslag van Azure en Client-bibliotheek voor C++.

-   [Het gebruik van C++-blobopslag](storage-c-plus-plus-how-to-use-blobs.md)
-   [Het gebruik van Table Storage vanuit C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Het gebruik van de wachtrij opslag van C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Client-bibliotheek van de Azure opslag voor C++ API-documentatie.](http://azure.github.io/azure-storage-cpp/)
-   [Azure opslag teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
