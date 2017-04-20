<properties
   pageTitle="Betrouwbare Services meldingen | Microsoft Azure"
   description="Conceptuele documentatie voor betrouwbare configuratieservices Service meldingen"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Betrouwbare Services-meldingen

Meldingen toestaan clients voor het bijhouden van de wijzigingen die worden gesteld aan een object waarin ze geïnteresseerd bent. Twee typen objecten ondersteunen meldingen: *Betrouwbare staat Manager* en *Betrouwbare woordenlijst*.

Er zijn enkele veelvoorkomende redenen voor het gebruik van meldingen:

- Building gerealiseerd weergaven, zoals secundaire indexen of gefilterde weergaven van de status van de replica samengevoegd. Een voorbeeld is een gesorteerd index van alle toetsen in betrouwbare woordenlijst.
- Verzendende controlegegevens, zoals het aantal gebruikers die zijn toegevoegd in het laatste uur.

Meldingen worden gestart als onderdeel van het toepassen van bewerkingen. Vanwege die, worden meldingen zo snel mogelijk en synchrone gebeurtenissen dure bewerkingen niet mag opnemen behandeld.

## <a name="reliable-state-manager-notifications"></a>Betrouwbaar staat Manager meldingen

Betrouwbaar staat Manager biedt meldingen voor de volgende gebeurtenissen:

- Transactie
    - Doorvoeren
- Provinciale manager
    - Opnieuw opbouwen
    - Toevoeging van een betrouwbare staat
    - Verwijderen van een betrouwbare staat

Betrouwbaar staat Manager wordt de huidige inflight transacties bijgehouden. De enige wijziging transactie status, waardoor een melding naar u worden geactiveerd is een transactie vast te leggen.

Betrouwbaar staat Manager onderhoudt een verzameling betrouwbare Staten zoals betrouwbare woordenlijst en betrouwbare wachtrij. Meldingen van betrouwbaar staat Manager wordt uitgevoerd wanneer deze verzameling verandert: een betrouwbare staat wordt toegevoegd of verwijderd of de hele verzameling opnieuw is gemaakt.
De verzameling betrouwbare staat Manager opnieuw wordt opgebouwd in drie gevallen:

- Herstel: Wanneer een replica wordt gestart, wordt hersteld de vorige versie van de schijf worden verwijderd. Aan het einde van herstel, gebruikt de functie **NotifyStateManagerChangedEventArgs** een gebeurtenis die de set herstelde betrouwbare Staten bevat.
- Volledige kopie: voordat een replica kan deelnemen aan de configuratieset, er worden opgebouwd. Soms moet hiervoor een volledige kopie van betrouwbare staat van Manager staat uit de primaire replica moeten worden toegepast om de niet-actieve secundaire replica. De Manager betrouwbaar staat op de secundaire replica gebruikt **NotifyStateManagerChangedEventArgs** een gebeurtenis die de set betrouwbare Staten die deze opgehaald uit de primaire replica bevat.
- Terugzetten: In noodgevallen herstel scenario's van de replica status kan worden hersteld uit een back-up via **RestoreAsync**. In dit geval gebruikt betrouwbare staat Manager op de primaire replica **NotifyStateManagerChangedEventArgs** een gebeurtenis die de set betrouwbare Staten die deze uit de back-up teruggezet bevat.

Om u te registreren voor meldingen en/of staat manager meldingen, moet u met de **TransactionChanged** of **StateManagerChanged** gebeurtenissen op betrouwbare staat Manager registreren. Een gemeenschappelijke locatie registreren met deze gebeurtenissenafhandelingsroutines is de constructor van uw statuscontrole service. Wanneer u zich bij de constructor registreren, niet u een melding dat wordt veroorzaakt door een wijziging gedurende de levensduur van **IReliableStateManager**naar hun.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

De **TransactionChanged** gebeurtenis-handler gebruikt **NotifyTransactionChangedEventArgs** voor meer informatie over de gebeurtenis. De presentatie bevat de eigenschap action (bijvoorbeeld **NotifyTransactionChangedAction.Commit**) die het type wijziging aangeeft. De presentatie bevat ook de eigenschap transaction vindt u een verwijzing naar de transactie die gewijzigd.

>[AZURE.NOTE] Tegenwoordig kunnen **TransactionChanged** gebeurtenissen treden alleen als de transactie doorgevoerd is. De actie is vervolgens gelijk aan **NotifyTransactionChangedAction.Commit**. Maar in de toekomst gebeurtenissen voor andere soorten transactie staat wijzigingen kunnen worden verhoogd. Het is raadzaam om de actie controleren en verwerken van de gebeurtenis alleen als dit is een waarde die u zou verwachten.

Hier volgt een voorbeeld **TransactionChanged** gebeurtenis-handler.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

De **StateManagerChanged** gebeurtenis-handler gebruikt **NotifyStateManagerChangedEventArgs** voor meer informatie over de gebeurtenis.
**NotifyStateManagerChangedEventArgs** heeft twee subcategorieën: **NotifyStateManagerRebuildEventArgs** en **NotifyStateManagerSingleEntityChangedEventArgs**.
U kunt de eigenschap action in **NotifyStateManagerChangedEventArgs** **NotifyStateManagerChangedEventArgs** geconverteerd naar de juiste subcategorie:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** en **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Hier volgt een voorbeeld **StateManagerChanged** melding handler.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Betrouwbare woordenlijst-meldingen

Betrouwbare woordenlijst biedt meldingen voor de volgende gebeurtenissen:

- Opnieuw maken: Genoemd wanneer **ReliableDictionary** de status van een herstelde of gekopieerde lokale staat of een back-up is hersteld.
- Wissen: Genoemd wanneer de status van **ReliableDictionary** is uitgeschakeld door de methode **ClearAsync** .
- Toevoegen: Aangeroepen wanneer een item is toegevoegd aan **ReliableDictionary**.
- Update: Aangeroepen wanneer een item in **IReliableDictionary** is bijgewerkt.
- Verwijderen: Aangeroepen wanneer een item in **IReliableDictionary** is verwijderd.

Als u betrouwbare woordenlijst meldingen, moet u met de **DictionaryChanged** gebeurtenis-handler op **IReliableDictionary**registreren. Een gemeenschappelijke locatie met deze gebeurtenissenafhandelingsroutines hebt geregistreerd, is in de **ReliableStateManager.StateManagerChanged** melding toevoegen.
Registreren wanneer **IReliableDictionary** wordt toegevoegd aan **IReliableStateManager** zorgt ervoor dat u geen meldingen niet naar hun.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** is de voorbeeld-methode die het voorgaande voorbeeld van de **OnStateManagerChangedHandler** belt.

De bovenstaande code Hiermee stelt u de **IReliableNotificationAsyncCallback** -interface, samen met **DictionaryChanged**. Omdat **NotifyDictionaryRebuildEventArgs** bevat een interface **IAsyncEnumerable** --welke moet worden asynchroon--opgesomd meldingen opnieuw worden gestart door **RebuildNotificationAsyncCallback** in plaats van **OnDictionaryChangedHandler**.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] In de voorgaande code, als onderdeel van het verwerken van de melding opnieuw opbouwen is eerst de onderhouden geaggregeerde staat uitgeschakeld. Omdat de betrouwbare verzameling opnieuw met een nieuwe status gemaakt worden, worden alle vorige meldingen niet relevant zijn.

De **DictionaryChanged** gebeurtenis-handler gebruikt **NotifyDictionaryChangedEventArgs** voor meer informatie over de gebeurtenis.
**NotifyDictionaryChangedEventArgs** heeft vijf subcategorieën. Via de eigenschap action in **NotifyDictionaryChangedEventArgs** **NotifyDictionaryChangedEventArgs** geconverteerd naar de juiste subcategorie:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** en **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Aanbevelingen

- *Voer* de melding gebeurtenissen zo snel mogelijk voltooid.
- *Niet* alle dure bewerkingen uitvoeren (bijvoorbeeld i/o-bewerkingen) als onderdeel van synchrone gebeurtenissen.
- *Controleer de actietype voordat u de gebeurtenis verwerkt.* Nieuwe actietypen mogelijk in de toekomst worden toegevoegd.

Hier zijn enkele dingen in gedachten moet houden:

- Meldingen worden gestart als onderdeel van het uitvoeren van een bewerking. Een melding herstellen wordt bijvoorbeeld gestart als de laatste stap van een bewerking voor terugzetten. Een herstellen voltooid niet totdat de meldingsgebeurtenis wordt verwerkt.
- Omdat meldingen worden gestart als onderdeel van de toepassen bewerkingen, Zie clients alleen meldingen voor lokaal vastgelegde bewerkingen. En omdat bewerkingen zijn gegarandeerd alleen lokaal vastgelegde (dat wil zeggen aangemeld), ze kunnen niet ongedaan worden gemaakt in de toekomst of kunnen.
- Klik op het pad/opnieuw in, één melding gestart voor elke toegepaste bewerking. Dit betekent dat als transactie t 1 Create(X), Delete(X) en Create(X) bevat, u een melding voor het maken van X-, een voor het verwijderen en een voor het maken Klik nogmaals in die volgorde krijgt.
- Voor transacties die meerdere bewerkingen bevatten, zijn bewerkingen toegepast in de volgorde waarin ze op de primaire replica van de gebruiker zijn ontvangen.
- Als onderdeel van het verwerken van ONWAAR voortgang, mogelijk bepaalde bewerkingen ongedaan. Meldingen zijn voor deze bewerkingen ongedaan maken, de status van de replica terugdraaien aan een stabiele punt ingediend. Een belangrijk verschil van meldingen van ongedaan maken is dat de gebeurtenissen die dubbele sleutels hebben worden samengevoegd. Bijvoorbeeld als transactie t 1 is ongedaan worden gemaakt, ziet u slechts één kennisgeving naar Delete(X).

## <a name="next-steps"></a>Volgende stappen

- [Betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
- [Betrouwbare Services snel aan de slag](service-fabric-reliable-services-quick-start.md)
- [Betrouwbare Services back-up en herstellen (herstel)](service-fabric-reliable-services-backup-restore.md)
- [Naslaginformatie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
