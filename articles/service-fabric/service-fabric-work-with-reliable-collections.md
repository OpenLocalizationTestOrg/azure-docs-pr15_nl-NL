<properties
    pageTitle="Werken met betrouwbare collecties | Microsoft Azure"
    description="Informatie over de aanbevolen procedures voor het werken met betrouwbare siteverzamelingen."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Werken met betrouwbare collecties

Service stof biedt een statuscontrole programming model beschikbaar voor .NET-ontwikkelaars via betrouwbare siteverzamelingen. Name bevat-Service stof betrouwbare woordenlijst en betrouwbare wachtrij klassen. Wanneer u deze klassen gebruikt, wordt uw status partitioneren (voor schaalbaarheid), gerepliceerd (voor beschikbaarheid) en verwerkt binnen een partition (voor ZURE semantiek). Laten we kijkt u naar een veelvoorkomend gebruik van een betrouwbare woordenlijstobject en zien wat de werkelijk doen.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Alle bewerkingen op betrouwbare woordenlijst-objecten (met uitzondering van ClearAsync dat wil niet ongedaan maken zeggen), moet een object ITransaction. Dit object is gekoppeld aan dit een en alle wijzigingen die u wilt aanbrengen aan een betrouwbare woordenlijst en/of betrouwbare wachtrij objecten binnen een enkel partition. Een ITransaction opgehaalde object door de ondersteuning voor de partition bevindt zich de StateManager CreateTransaction methode.
 
In de bovenstaande code, wordt het object ITransaction doorgegeven aan van een betrouwbare woordenlijst AddAsync methode. Intern duren woordenlijst methoden waarin een sleutel een lezer/schrijver vergrendelen die zijn gekoppeld aan de toets. Als de methode de waarde van de sleutel wijzigt, wordt een slot schrijven op de toets en als de methode is alleen wordt gelezen uit de waarde van de sleutel, wordt een gelezen vergrendelen genomen op de toets. Aangezien AddAsync de waarde van de sleutel aan de nieuwe, doorgegeven waarde wijzigt, van de sleutel schrijven lock is die u hebt gemaakt. Zo is, 2 (of meer) threads probeert om waarden te tellen met dezelfde sleutel op hetzelfde moment, één thread wordt vergrendelen voor het schrijven als de andere threads blokkeert. Standaard methoden blokkeren voor maximaal vier seconden de vergrendeling; te verkrijgen na 4 seconden genereren de methoden een TimeoutException. Methode overbelastingen bestaan zodat u kunt een expliciete time-outwaarde doorgeven als u liever.
 
Meestal schrijft u uw code als u wilt reageren op een TimeoutException door deze te vangen en het hele opnieuw (zoals weergegeven in de bovenstaande code). Gebeld NET Task.Delay doorgeven van 100 milliseconden telkens wanneer in mijn eenvoudige code. U kunt echter, in feite mogelijk beter met een soort exponentiële back-uit vertraging klikken.
 
Zodra de vergrendeling wordt opgehaald, wordt de sleutel en waarde objectverwijzingen naar een interne tijdelijke woordenlijst dat is gekoppeld aan het object ITransaction door AddAsync toegevoegd. Dit gebeurt waarmee u alleen-uw-eigenaar bent van-schrijft semantiek. Dat wil zeggen, wanneer u AddAsync hebt gebeld, retourneert een hoger oproep naar TryGetValueAsync (met behulp van het object dat dezelfde ITransaction) de waarde zelfs als u de transactie nog niet zijn doorgevoerd. Vervolgens AddAsync serialiseert uw sleutel en waarde byte matrices objecten en voegt u deze matrices byte toe aan een logboekbestand op het lokale knooppunt. AddAsync verzendt ten slotte de matrices byte naar alle secundaire replica zodat ze dezelfde sleutelwaarde/gegevens bevatten. Hoewel de sleutelwaarde/zijn geschreven in een logboekbestand, de informatie wordt niet beschouwd als onderdeel van de woordenlijst totdat de transactie die ze zijn gekoppeld doorgevoerd is. 

De oproep door naar CommitAsync doorgevoerd in de bovenstaande code alle bewerkingen van de transactie. Specifiek, voegt doorvoeren informatie toe aan het logboekbestand op het lokale knooppunt en wordt de doorvoerrecord ook verzonden naar de secundaire replica's. Wanneer een quorum (meeste) replica heeft gereageerd, alle wijzigingen aan de gegevens worden beschouwd als permanente en eventuele vergrendelingen die is gekoppeld aan de toetsen die zijn bewerkt via het object ITransaction worden uitgebracht, zodat andere threads/transacties de dezelfde sleutels en hun waarden kunt bewerken.

Als CommitAsync niet wordt aangeroepen (meestal vanwege een uitzondering wordt), wordt het object ITransaction verwijderd. Bij de buitengebruikstelling een niet-doorgevoerde ITransaction-object, Service stof afbreken informatie toegevoegd aan het logboekbestand van het lokale knooppunt en niets moet worden verzonden naar elke secundaire replica. En vervolgens een vergrendelingen die is gekoppeld aan de toetsen die zijn bewerkt via de transactie zijn uitgebracht.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Algemene nadelen en hoe u ze voorkomen
Nu dat u hoe de betrouwbare verzamelingen intern werken weet, laten we Bekijk enkele veelvoorkomende misbruik van deze. Zie de onderstaande code:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Tijdens het werken met een normale .NET-woordenlijst, kunt u een sleutelwaarde toevoegen aan de woordenlijst en wijzig de waarde van een eigenschap (zoals LastLogin). Echter werkt deze code niet goed met een betrouwbare woordenlijst. Onthoud dat uit de eerdere discussie, de oproep door naar AddAsync serialiseert het sleutelwaarde/objecten op byte matrices en klikt u vervolgens de matrices in een lokaal bestand opslaat en wordt deze ook verzonden naar de secundaire replica's. Als u een eigenschap later verandert, wordt hiermee de waarde van de eigenschap in het geheugen alleen; Deze heeft geen gevolgen voor het lokale bestand of de gegevens die zijn verzonden naar de replica's. Als het proces vastloopt, is wat staat er in het geheugen weggegooid. Wanneer een nieuw proces wordt gestart of als een andere replica primaire verandert, klikt u vervolgens op de oude eigenschapwaarde wat is beschikbaar is. 

Ik kan niet genoeg benadrukken genoeg hoe makkelijk dat het is om te maken van het soort fout hierboven. En u wordt alleen meer informatie over de fout als of wanneer dat het proces uitvalt. De juiste manier voor het schrijven van de code is gewoon naar de twee regels omkeren:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Hier volgt een ander voorbeeld met een algemene fout:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Klik nogmaals met normale .NET woordenlijsten, de bovenstaande code werkt prima en is een algemene patroon: een sleutel in de ontwikkelaar wordt gebruikt om een waarde te zoeken. Als de waarde bestaat, verandert de ontwikkelaar de waarde van een eigenschap. Met betrouwbare verzamelingen, echter vertoont deze code hetzelfde probleem zo al besproken: __moet u een object niet wijzigen nadat u deze aan een betrouwbare siteverzameling hebt doorgegeven.__
 
De juiste manier voor het bijwerken van een waarde in een betrouwbare verzameling.een Help-verzameling is voor het ophalen van een verwijzing naar de huidige waarde en houd rekening met het object waarnaar wordt verwezen door deze onveranderlijke verwijzing. Maak een nieuw object dat wil een exacte kopie van het oorspronkelijke object zeggen. U kunt nu de status van deze nieuwe object wijzigen en het nieuwe object in de verzameling zodat deze wordt serieel naar byte matrices, toegevoegd aan het lokale bestand en verzonden naar de replica's schrijven. Nadat u de wijzigingen zijn doorgevoerd, hebben de objecten in het geheugen, het lokale bestand en alle replica's dezelfde exacte staat. Alle is een goed idee!

De volgende code bevat de juiste manier voor het bijwerken van een waarde in een betrouwbare siteverzameling:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Onveranderlijke gegevenstypen om te voorkomen dat programma fout definiëren

In het ideale geval graag we de compileerprogramma fouten rapporteren wanneer u per ongeluk code die de status van een object dat u gewoonlijk aandachtspunten onveranderlijke mutates produceren. Maar de C#-compileerprogramma beschikt niet over de functionaliteit u dit wilt doen. Zo is, om te voorkomen mogelijke programma bugs bij te werken, wordt ten zeerste aanbevolen dat u de typen definieert u gebruikt met betrouwbare verzamelingen moeten onveranderlijke typen. Dit houdt in dat u core waardetypen (zoals getallen [Int32, UInt64, enz.], DateTime, Guid, tijdspanne en dergelijke) blijven. En natuurlijk ook kunt u tekenreeks. Is het raadzaam om te voorkomen dat de eigenschappen van de siteverzameling als serialiseren en deserialiseren ze kunt regelmatig kunt schade prestaties. Echter als u gebruiken eigenschappen van siteverzameling wilt, wordt ten zeerste aangeraden het gebruik van. De NET onveranderlijke verzamelingen bibliotheek (System.Collections.Immutable). Deze bibliotheek is beschikbaar voor downloaden van http://nuget.org. Ook aangeraden sluiting van uw lessen en de velden alleen-lezen zodat zo veel mogelijk.

Het type UserInfo onderstaande laat zien hoe een onveranderlijke type profiteren van de bovengenoemde aanbevelingen definiëren.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

Het type ItemId is ook een onveranderlijke type, zoals hier wordt getoond:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Schema versiebeheer (upgrades)

Intern serialiseren betrouwbare verzamelingen uw objecten gebruiken. NET van DataContractSerializer. De serienummer objecten op de lokale schijf van de primaire replica worden vastgelegd en worden ook doorgegeven aan de secundaire replica's. Als uw service ouder, is het waarschijnlijk dat u wilt wijzigen van het type gegevens (schema) uw service vereist. U moet versiebeheer van uw gegevens met zorgvuldig benadert. Allereerst omdat, kunt u moet altijd terugconverteren oude gegevens. Dit houdt in uw code deserialisatie moet oneindig compatibel: 333 versie van de servicecode moet kunnen werken met gegevens in een betrouwbare collectie geplaatst door versie 1 van de servicecode 5 jaar geleden.

Bovendien is servicecode bijgewerkte één upgrade domein tegelijk. Ja, tijdens een upgrade hebt u twee verschillende versies van de servicecode die gelijktijdig wordt uitgevoerd. U moet voorkomen dat van de nieuwe versie van de servicecode gebruiken van het nieuwe schema, zoals oude versies van de servicecode van uw mogelijk niet u omgaat met het nieuwe schema. Indien mogelijk, moet u elke versie van uw service doorsturen compatibel door 1 versie zijn ontwerpen. Specifiek, betekent dit dat V1 van uw servicecode moet kunnen gewoon negeren een schema-elementen die dit niet expliciet verwerkt. Dit moet wel kunt opslaan van alle gegevens die zijn deze expliciet niets bekend over en gewoon schrijven dat alleen terug af bij het bijwerken van een woordenlijst-sleutel of waarde. 

>[AZURE.WARNING] Terwijl u het schema van een sleutel wijzigen kunt, moet u ervoor zorgen dat van uw sleutel hashcode en gelijk is aan algoritmen stabiele zijn. Als u hoe een van deze algoritmen werken wijzigen, is het niet mogelijk de sleutel binnen de betrouwbare woordenlijst ooit opnieuw opzoeken.
  
U kunt ook uitvoeren wat meestal wordt aangeduid als een upgrade 2-fase. Met een 2-fase-upgrade is u uw service vanuit upgraden V1 naar V2: V2 bevat de code die het omgaan met de nieuwe schemawijziging weet, maar deze code niet uitvoeren. Wanneer de code V2 V1 gegevens leest, werkt op is geïnstalleerd en schrijft V1 gegevens. Klik wanneer de upgrade voltooid voor alle upgrade domeinen is, kunt u op signaal in de actieve V2 exemplaren dat de upgrade voltooid is. (Kunt signaal dit is om te implementeren een upgrade configuratie; Dit is wat is dit een upgrade 2-fase.) Nu kunnen de exemplaren V2 V1 gegevens lezen, converteren naar V2 gegevens worden uitgevoerd en wegschrijven als V2 gegevens. Wanneer andere exemplaren V2 gegevens lezen, wordt deze niet hoeft te converteren, ze alleen worden uitgevoerd en schrijven V2 gegevens weggegooid.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het maken van doorsturen compatibel gegevenstype contracten, [Doorsturen-compatibele gegevenscontracten](https://msdn.microsoft.com/library/ms731083.aspx).

Zie meer informatie over aanbevolen procedures voor versiebeheer gegevenscontracten, [Gegevens Contract versiebeheer](https://msdn.microsoft.com/library/ms731138.aspx). 

Zie meer informatie over het implementeren van versie fouttolerante gegevenscontracten, [Versie fouttolerantie serialisatie terugbellen](https://msdn.microsoft.com/library/ms733734.aspx). 

Als u wilt weten hoe u een gegevensstructuur die voor de verschillende versies kunt samenwerken bieden, raadpleegt u [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
