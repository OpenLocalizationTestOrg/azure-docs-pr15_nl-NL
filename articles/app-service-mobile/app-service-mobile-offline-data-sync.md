<properties
    pageTitle="Offline gegevens synchroniseren in Azure mobiele Apps | Microsoft Azure"
    description="Conceptuele verwijzing en overzicht van de functie voor het synchroniseren van offline gegevens voor Azure Mobile-Apps"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Offline gegevens synchroniseren in Azure mobiele Apps

## <a name="what-is-offline-data-sync"></a>Wat is offline gegevens synchroniseren?

Offline gegevens synchroniseren is een client en server SDK-functie van Azure Mobile-Apps waarmee u gemakkelijk voor ontwikkelaars om apps die functionele zonder een netwerkverbinding zijn te maken.

Als uw app offlinemodus, kunnen nog steeds gebruikers maken en wijzigen van gegevens, die wordt opgeslagen in een lokale archief. Wanneer de app weer online is, kunt deze lokale wijzigingen synchroniseren met uw backend Azure Mobile-App. De functie bevat ook de ondersteuning voor detectie van conflicten wanneer dezelfde record voor zowel de client als de backend wordt gewijzigd. Conflicten kunnen vervolgens worden verwerkt op de server of de client.

Offline synchronisatie heeft een aantal voordelen:

* App reactiesnelheid door caching van servergegevens lokaal op het apparaat
* Maken van krachtige apps die momenten wanneer er netwerkproblemen zijn
* Eindgebruikers maken en wijzigen van gegevens, zelfs wanneer er is geen netwerktoegang ondersteunende scenario's met weinig of geen connectivity toestaan
* Gegevens over meerdere apparaten synchroniseren en dezelfde record is gewijzigd door twee apparaten conflicten waarnemen
* Limiet netwerk gebruiken op hoge latentie of datalimiet netwerken

De volgende zelfstudies weergeven offline synchroniseren toevoegen aan uw mobiele clients gebruik Azure Mobile-Apps:

* [Android: Offline synchroniseren inschakelen]
* [iOS: offline synchroniseren inschakelen]
* [Xamarin iOS: offline synchroniseren inschakelen]
* [Xamarin Android: Offline synchroniseren inschakelen]
* [Xamarin.Forms: Inschakelen offline synchroniseren](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Universele Windows-Platform: Offline synchroniseren inschakelen]

## <a name="what-is-a-sync-table"></a>Wat is een tabel synchroniseren?

Voor toegang tot het eindpunt '/ tabellen', bieden de Azure mobiele client SDK's via interfaces zoals `IMobileServiceTable` (.NET-client SDK) of `MSTable` (iOS-client). Deze API's rechtstreeks verbinden met de backend Azure Mobile-App en loopt vast als het clientapparaat worden gebruikt geen netwerkverbinding.

Voor gebruik offline uw app moet in plaats daarvan gebruiken in de *synchronisatietabel* -API's, zoals `IMobileServiceSyncTable` (.NET-client SDK) of `MSSyncTable` (iOS-client). Dezelfde werken CRUD-bewerkingen (maken, lezen, Update, Delete) ten opzichte van de synchronisatietabel API's, behalve nu ze lezen of naar een *lokale archief schrijven*. Voordat alle bewerkingen van de tabel synchronisatie kunnen worden uitgevoerd, moet het lokale archief worden geïnitialiseerd.

## <a name="what-is-a-local-store"></a>Wat is een lokale archief?

Een lokale archief is de laag van de permanente gegevens op het clientapparaat worden gebruikt. De Mobile-Apps Azure-client SDK's bieden een standaard lokale store-implementatie. In Windows, Xamarin en android-apparaat, is gebaseerd op SQLite; Klik op iOS, is gebaseerd op Core gegevens.

Als u wilt de uitvoering SQLite gebaseerde op Windows Phone of Windows Store 8.1 gebruiken, moet u de extensie SQLite installeren. Zie voor meer informatie, [universele Windows-Platform: offline synchroniseren inschakelen]. Android en iOS geleverd met een versie van SQLite in het besturingssysteem van apparaat zelf, zodat u niet nodig is om te verwijzen naar uw eigen versie van SQLite.

Ontwikkelaars kunnen ook implementeren voor hun eigen lokale archief. Als u voor de opslag van gegevens in een versleutelde indeling op de mobiele client wilt, kunt u bijvoorbeeld een lokale archief die SQLCipher voor versleuteling wordt gebruikt definiëren.

## <a name="what-is-a-sync-context"></a>Wat is een context synchroniseren?

Een *synchronisatie context* is gekoppeld aan een mobiele client-object (zoals `IMobileServiceClient` of `MSClient`) en wordt bijgehouden wijzigingen die zijn aangebracht met synchronisatie tabellen. De synchronisatie-context onderhoudt een *bewerking wachtrij* blijft een geordende lijst van CUD bewerkingen (maken, Update, Delete) die later worden verzonden naar de server.

Een lokale archief is gekoppeld aan de context van de synchronisatie met een methode initialisatie, zoals `IMobileServicesSyncContext.InitializeAsync(localstore)` in de [.NET-client SDK].

## <a name="how-sync-works"></a>Hoe offline synchronisatie werkt

Wanneer u met de synchronisatie van tabellen, besturingselementen voor uw klant-code wanneer lokale wijzigingen worden gesynchroniseerd met een backend Azure Mobile-App. Niets wordt verzonden naar de backend totdat er een oproep naar *push* lokale wijzigingen. Daarnaast wordt het lokale archief gevuld met nieuwe gegevens alleen wanneer er een oproep wilt *halen* gegevens.

* **Push**: Push is een bewerking op de context van de synchronisatie en alle CUD wijzigingen sinds de laatste push verzendt. Houd er rekening mee dat dit niet mogelijk is om te verzenden alleen een afzonderlijke tabel wijzigingen, is omdat anders bewerkingen kunnen worden verzonden volgorde. Push Hiermee voert u een reeks REST oproepen naar uw backend Azure Mobile-App, die wordt wijzigen om uw server-database.

* **Halen**: halen wordt uitgevoerd op basis van de per tabel en kan worden aangepast dat met een query om op te halen slechts een subset van de server-gegevens. De resulterende gegevens van de Azure mobiele client SDK's worden vervolgens in het lokale archief invoegen.

* **Impliciet worden**: als u een halen is uitgevoerd voor een tabel met lokale updates, de halen eerst een push wordt uitgevoerd op de context synchroniseren. Hiermee kunt minimaliseren conflicten tussen de wijzigingen die al in de wachtrij en nieuwe gegevens van de server.

* **Incrementele synchronisatie**: de eerste parameter voor de bewerking halen is een *querynaam* die alleen op de client wordt gebruikt. Als u de naam van een niet-null query gebruikt, wordt de SDK van Azure Mobile een *incrementele synchronisatie*uitvoeren.
  Telkens wanneer een bewerking halen geeft als resultaat een reeks resultaten, de nieuwste `updatedAt` tijdstempel uit die resultatenset is opgeslagen in de SDK lokaal systeemtabellen. Volgende halen bewerkingen ophaalt alleen records nadat de tijdstempel.

  Als u wilt gebruiken incrementele synchroniseren, moet uw server zinvolle retourneren `updatedAt` -waarden en sorteren op dit veld moet ook ondersteunen. Aangezien de SDK eigen sorteren op het veld updatedAt toevoegt, geen u echter een halen query met een eigen gebruik `$orderBy$` component.

  De naam van de query is een willekeurige tekenreeks die u kiest, maar deze moet uniek zijn voor elke logische query in uw app.
  Anders verschillende halen bewerkingen, kunnen u de dezelfde incrementele synchronisatie tijdstempel overschrijven en uw query's kunnen onjuiste resultaten worden geretourneerd.

  Als de query een parameter heeft, is een manier om u te maken van de naam van een unieke query kunt u de parameterwaarde.
  Bijvoorbeeld als u op gebruikers-id filtert, de querynaam van de mogelijk als volgt (in C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Als u zich afmelden bij incrementele synchronisatie wilt, doorgeven `null` als de query-ID. In dit geval alle records opgehaald bij elke oproep naar `PullAsync`, welke is mogelijk niet efficiënt.

* **Purging**: U kunt de inhoud van het gebruik van de lokale archief wissen `IMobileServiceSyncTable.PurgeAsync`.
  Dit kan het nodig zijn als er verouderde gegevens in de client-database of als u wilt verwijderen die alle wachtende wijzigingen.

  Een leegmaken, wordt een tabel uit het lokale archief gewist. Als er bewerkingen in behandeling synchronisatie met de server-database, wordt het wissen een uitzondering genereren, tenzij de parameter *dwingen wissen* is ingesteld.

  Een voorbeeld van verouderde gegevens op de client ziet in het voorbeeld 'takenlijst' Device1 ophaalt alleen items die niet zijn voltooid. Kies vervolgens een todoitem "Kopen melk" is gemarkeerd als voltooid op de server door een ander apparaat. Echter steeds Device1 nog de todoitem 'Kopen melk' in de lokale archief omdat deze alleen trekt aan items die zijn niet gemarkeerd als voltooid. Een leegmaken, wordt deze verouderde item gewist.

## <a name="next-steps"></a>Volgende stappen

* [iOS: offline synchroniseren inschakelen]
* [Xamarin iOS: offline synchroniseren inschakelen]
* [Xamarin Android: Offline synchroniseren inschakelen]
* [Universele Windows-Platform: Offline synchroniseren inschakelen]

<!-- Links -->
[.NET-client SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Offline synchroniseren inschakelen]: app-service-mobile-android-get-started-offline-data.md
[iOS: offline synchroniseren inschakelen]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: offline synchroniseren inschakelen]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Offline synchroniseren inschakelen]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Universele Windows-Platform: Offline synchroniseren inschakelen]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
