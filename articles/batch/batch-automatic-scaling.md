<properties
    pageTitle="Automatisch de knooppunten in een Azure Batch van toepassingen voor het berekenen van schaal | Microsoft Azure"
    description="Inschakelen van automatische schalen op een groep cloud dynamisch aanpassen het aantal knooppunten berekenen in de groep."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Automatisch de knooppunten in een Azure Batch van toepassingen voor het berekenen van schaal

De Batch Azure-service kan met automatisch schalen dynamisch toevoegen of verwijderen van berekeningscluster knooppunten in een groep op basis van parameters die u definieert. Kunt u veel tijd en geld potentieel besparen door automatisch aanpassen van het schuifblok wordt verplaatst van rekenkracht gebruikt door de toepassing--knooppunten toevoegen terwijl van uw taak taak vraag oplopen en deze te verwijderen wanneer ze verkleinen.

Inschakelen van automatische schalen op een groep berekeningscluster knooppunten door te koppelen aan deze een *automatisch schalen formule* die u, zoals definieert met de [PoolOperations.EnableAutoScale] [ net_enableautoscale] methode in de [Batch.NET](batch-dotnet-get-started.md) -bibliotheek. Deze formule de Batch-service vervolgens gebruikt om te bepalen van het aantal berekeningscluster knooppunten die nodig zijn voor het uitvoeren van uw werkzaamheden. Batch moet reageren op service aan de doelstellingen van de voorbeelden van de gegevens die regelmatig worden verzameld en past u het aantal knooppunten berekenen in de groep met een configureerbare interval op basis van de formule.

U kunt inschakelen automatische schaling wanneer een resourcegroep die is gemaakt, of op een bestaande toepassingen. U kunt ook een bestaande formule op een resourcegroep die is 'automatisch schalen"ingeschakeld wijzigen. Batch biedt de mogelijkheid om te evalueren van uw formules voordat toe te wijzen aan groepen, evenals de status van Automatische schaal wordt uitgevoerd.

## <a name="automatic-scaling-formulas"></a>Automatische schaal formules

Een automatische schaal formule is een tekenreekswaarde die u definieert die een of meer instructies bevat, en is toegewezen aan een groep van toepassingen [autoScaleFormula] [ rest_autoscaleformula] element (Batch REST) of [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] eigenschap (Batch .NET). Wanneer aan een groep is toegewezen, wordt in de Batch-service uw formule gebruikt om te bepalen van het doel aantal berekeningscluster knooppunten in de groep voor het volgende interval van verwerking (meer op intervallen later). De formule tekenreeks niet langer zijn dan 8 KB grootte, maximaal 100 instructies die zijn gescheiden door puntkomma's en regeleinden en opmerkingen kunnen opnemen kunt opnemen.

U kunt zien van Automatische schaal formules als met een Batch automatisch schalen "taal." Formule instructies zijn vrij te geven gevormd expressies die zowel de service gedefinieerde variabelen (variabelen die zijn gedefinieerd door de service Batch) en de gebruiker gedefinieerde variabelen (variabelen die u definieert) kunnen opnemen. Ze kunnen verschillende bewerkingen voor deze waarden uitvoeren met behulp van ingebouwde typen, operatoren en functies. Een instructie mogelijk bijvoorbeeld er als volgt uit:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formules bevatten gewoonlijk meerdere instructies die bewerkingen uitvoeren op waarden die zijn verkregen in vorige instructies. Bijvoorbeeld eerst we een waarde voor krijgt `variable1`, klikt u vervolgens doorgegeven aan een functie om te vullen `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Bij deze instructies in de formule, uw doel is aankomt op een aantal berekeningscluster knooppunten die de groep moet worden aangepast aan--het **doel** aantal **speciale knooppunten**. Het is mogelijk dat dit nummer hoger, kleine.letters of hetzelfde als het huidige aantal knooppunten in de groep. Batch evalueert van een resourcegroep die automatisch schalen formule na een bepaald tijdsinterval ([Automatische schaal intervallen](#automatic-scaling-interval) worden besproken hieronder). Vervolgens wordt deze het doel aantal knooppunten in de groep naar het nummer dat de formule automatisch schalen Hiermee geeft u op het moment van evaluatie aanpassen.

Als een snel voorbeeld geeft deze formule twee regels automatisch schalen dat het aantal knooppunten moet worden aangepast op basis van het aantal actieve taken, tot maximaal 10 berekeningscluster knooppunten aan:

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

De volgende secties van dit artikel bespreken de verschillende entiteiten waaruit automatisch schalen formules, inclusief variabelen, operatoren, bewerkingen en functies. U vindt meer over het verkrijgen van diverse berekeningscluster resource en taak cijfers in Batch. U kunt deze gegevens gebruiken om aan te passen slim van uw groep knooppunt tellen op basis van de taak en de status van resource. Vervolgens leert u hoe u een formule maken en inschakelen van automatische schalen op een groep met behulp van de REST van de Batch en de .NET-API's. We wordt voltooien met een paar voorbeeldformules.

> [AZURE.IMPORTANT] Elke Batch Azure-account is beperkt tot een maximum aantal cores (en dus berekeningscluster knooppunten) die kan worden gebruikt voor de verwerking. De Batch-service maakt knooppunten alleen tot deze limiet core. Daarom mogelijk het doel aantal berekeningscluster knooppunten die is opgegeven met een formule niet worden bereikt. Zie [quota en beperkingen voor de Batch Azure-service](batch-quota-limit.md) voor informatie over het weergeven en de quota voor uw account met groter wordende.

## <a name="variables"></a>Variabelen

U kunt zowel **service gedefinieerde** als **de gebruiker gedefinieerde** variabelen gebruiken in formules automatisch schalen. De service gedefinieerde variabelen zijn ingebouwd in de Batch-service enkele zijn alleen-lezen, en andere alleen-lezen. De gebruiker gedefinieerde variabelen zijn variabelen die *u* definieert. In de bovenstaande, twee regels voorbeeldformule `$TargetDedicated` is een door de service gedefinieerde variabele, terwijl `$averageActiveTaskCount` is van een gebruiker gedefinieerde variabele.

De onderstaande tabellen weergeven alleen-lezen en alleen-lezen variabelen die zijn gedefinieerd door de service Batch.

Kunt u **ophalen** en **instellen van** de waarden van deze service gedefinieerde variabelen voor het beheren van het aantal berekeningscluster knooppunten in een groep:

| Alleen-lezen-schrijven service gedefinieerde variabelen | Beschrijving |
| --- | --- |
| $TargetDedicated | Het **doel** aantal **knooppunten berekeningscluster specifiek** voor de toepassingen. Dit is het aantal berekeningscluster knooppunten die de groep moet worden aangepast aan. Dit is een getal "target" Aangezien is het mogelijk dat voor een groep niet op het doel aantal knooppunten hebt bereikt. Dit kan gebeuren als het doel aantal knooppunten voordat de groep met de oorspronkelijke doelstelling is bereikt opnieuw door een latere automatisch schalen evaluatie is gewijzigd. Dit kan ook gebeuren als een Batch account knooppunt of core quotum is bereikt voordat het doel aantal knooppunten is bereikt. |
| $NodeDeallocationOption | De actie die optreedt wanneer berekeningscluster knooppunten worden verwijderd uit een groep. Mogelijke waarden zijn:<ul><li>**requeue**--taken onmiddellijk beëindigd en weer in de wachtrij plaatst, zodat ze opnieuw zijn gepland.<li>**beëindigen**--taken onmiddellijk beëindigd en worden deze verwijderd uit de wachtrij.<li>**taskcompletion**--wachten op voor de actieve taken om te voltooien en het knooppunt verwijdert uit de groep.<li>**retaineddata**--wacht voor alle lokale taak behouden gegevens op het knooppunt dat moet worden opgeschoond voordat het knooppunt wordt verwijderd uit de groep.</ul> |

U kunt **krijgen** de waarde van deze service gedefinieerde variabelen om aanpassingen die zijn gebaseerd op de doelstellingen van de Batch-service te maken:

| Alleen-lezen-service gedefinieerde variabelen | Beschrijving |
| --- | --- |
| $CPUPercent | Het gemiddelde percentage CPU-gebruik. |
| $WallClockSeconds | Het aantal seconden verbruikt. |
| $MemoryBytes | Het gemiddelde aantal megabytes op dat wordt gebruikt. |
| $DiskBytes | Het gemiddelde aantal gigabytes gebruikt op de lokale schijf. |
| $DiskReadBytes | Het aantal bytes dat is gelezen. |
| $DiskWriteBytes | Het aantal bytes dat is geschreven. |
| $DiskReadOps | Het aantal gelezen schijfbewerkingen. |
| $DiskWriteOps | Het aantal schrijven schijf bewerkingen die zijn uitgevoerd. |
| $NetworkInBytes | Het aantal binnenkomende bytes. |
| $NetworkOutBytes | Het aantal uitgaande bytes. |
| $SampleNodeCount | Het aantal knooppunten berekeningscluster. |
| $ActiveTasks | Het aantal taken in een actieve status. |
| $RunningTasks | Het aantal taken actief. |
| $PendingTasks | De som van $ActiveTasks en $RunningTasks. |
| $SucceededTasks | Het aantal taken dat is voltooid. |
| $FailedTasks | Het aantal taken dat is mislukt. |
| $CurrentDedicated | Het huidige aantal speciale berekenen knooppunten. |

> [AZURE.TIP] De alleen-lezen, service gedefinieerde variabelen die zijn hierboven zijn *objecten* die bieden verschillende methoden voor toegang tot gegevens die zijn gekoppeld aan elk. Zie [Voorbeeldgegevens verkrijgen](#getsampledata) hieronder voor meer informatie.

## <a name="types"></a>Typen

Deze **typen** worden ondersteund in een formule.

- dubbele
- doubleVec
- doubleVecList
- tekenreeks
- tijdstempel--tijdstempel is een samengestelde structuur die de volgende leden bevat:

    - jaar
    - maand (1-12)
    - dag (1-31)
    - Weekday (in de notatie van getal, bijvoorbeeld 1 voor maandag)
    - Hour (in 24-uursnotatie getalnotatie, bijvoorbeeld 13 betekent 1 uur)
    - minuten (00-59)
    - tweede (00-59)
- TimeInterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Bewerkingen

Deze **bewerkingen** zijn toegestaan voor de typen die hierboven zijn vermeld.

| Bewerking                             | Ondersteunde operatoren   | Resultaattype   |
| ------------------------------------- | --------------------- | ------------- |
| dubbele *operator* dubbele              | +, -, *, /            | dubbele            |
| dubbele *operator* timeinterval        | *                     | TimeInterval      |
| doubleVec *operator* dubbele           | +, -, *, /            | doubleVec         |
| doubleVec *operator* doubleVec        | +, -, *, /            | doubleVec         |
| TimeInterval *operator* dubbele        | *, /                  | TimeInterval      |
| TimeInterval *operator* timeinterval  | +, -                  | TimeInterval      |
| TimeInterval *operator* tijdstempel     | +                     | tijdstempel         |
| tijdstempel *operator* timeinterval     | +                     | tijdstempel         |
| tijdstempel *operator* tijdstempel        | -                     | TimeInterval      |
| *operator*dubbele                      | -, !                  | dubbele            |
| *operator*timeinterval                | -                     | TimeInterval      |
| dubbele *operator* dubbele              | <, < =, ==, > =, >,! =  | dubbele            |
| tekenreeks *operator* tekenreeks              | <, < =, ==, > =, >,! =  | dubbele            |
| tijdstempel *operator* tijdstempel        | <, < =, ==, > =, >,! =  | dubbele            |
| TimeInterval *operator* timeinterval  | <, < =, ==, > =, >,! =  | dubbele            |
| dubbele *operator* dubbele              | & &, & #124; & #124;      | dubbele            |

Bij het testen van een dubbel met een ternaire operator (`double ? statement1 : statement2`), andere **waar**is en nul **Onwaar**is.

## <a name="functions"></a>Functies

Deze vooraf gedefinieerde **functies** zijn beschikbaar voor u in het definiëren van een automatische schaal formule gebruiken.

| Functie                          | Retourtype   | Beschrijving
| --------------------------------- | ------------- | --------- |
| Avg(doubleVecList)                | dubbele        | Geeft als resultaat de gemiddelde waarde voor alle waarden in de doubleVecList.
| Len(doubleVecList)                | dubbele        | Retourneert de lengte van de vector die is gemaakt op basis van de doubleVecList.
| LG(Double)                        | dubbele        | Geeft als resultaat het logboek grondtal 2 van de dubbele.
| LG(doubleVecList)                 | doubleVec     | Geeft als resultaat de componentwise log grondtal 2 van de doubleVecList. Een vec(double) moet expliciet voor de parameter worden doorgegeven. Alle andere gevallen wordt de dubbele lg(double) versie uitgegaan.
| ln(Double)                        | dubbele        | Geeft als resultaat de natuurlijke logboek met de dubbele.
| ln(doubleVecList)                 | doubleVec     | Geeft als resultaat de componentwise log grondtal 2 van de doubleVecList. Een vec(double) moet expliciet voor de parameter worden doorgegeven. Alle andere gevallen wordt de dubbele lg(double) versie uitgegaan.
| log(Double)                       | dubbele        | Geeft als resultaat het logboek grondtal 10 van de dubbele.
| log(doubleVecList)                | doubleVec     | Geeft als resultaat de componentwise log grondtal 10 van de doubleVecList. Een vec(double) moet expliciet voor de enkele dubbele parameter worden doorgegeven. Alle andere gevallen wordt de dubbele log(double) versie uitgegaan.
| Max(doubleVecList)                | dubbele        | Geeft als resultaat de maximumwaarde in de doubleVecList.
| min(doubleVecList)                | dubbele        | Geeft als resultaat de minimumwaarde in de doubleVecList.
| norm(doubleVecList)               | dubbele        | Geeft als resultaat de twee-norm van de vector die is gemaakt van de doubleVecList.
| percentiel (doubleVec v, dubbele p) | dubbele        | Geeft als resultaat het element percentiel van de vector v.
| ASELECT()                            | dubbele        | Geeft als resultaat een willekeurige waarde tussen 0,0 en 1,0.
| Range(doubleVecList)              | dubbele        | Geeft als resultaat het verschil tussen de min en max waarden in de doubleVecList.
| Std(doubleVecList)                | dubbele        | Retourneert de standaarddeviatie van de steekproef van de waarden in de doubleVecList.
| Stop()                            |               | Evaluatie van de expressie autoscaling stopt.
| SUM(doubleVecList)                | dubbele        | Geeft als resultaat de som van alle onderdelen van de doubleVecList.
| tijd (dateTime tekenreeks = "")          | tijdstempel     | Geeft als resultaat het tijdstempel van de huidige tijd als er geen parameters worden doorgegeven, of het tijdstempel van de tekenreeks dateTime als deze wordt doorgegeven. Ondersteunde dateTime-indelingen zijn W3C-DTF- en RFC 1123.
| Val (doubleVec v, dubbele i)        | dubbele        | Geeft als resultaat de waarde van het element dat zich op locatie i in vector v, met een begin-index van nul.

Sommige functies die worden beschreven in de tabel hierboven kunt een lijst accepteren als een argument. De lijst door komma's gescheiden is een combinatie van *dubbele* en *doubleVec*. Bijvoorbeeld:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

De waarde *doubleVecList* wordt geconverteerd naar een enkel *doubleVec* vóór evaluatie. Bijvoorbeeld als `v = [1,2,3]`, klikt u vervolgens bellen `avg(v)` is gelijk aan om te bellen kwijt `avg(1,2,3)`. Bellen `avg(v, 7)` is gelijk aan om te bellen kwijt `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Voorbeeldgegevens verkrijgen

Automatisch schalen formules worden uitgevoerd wanneer de doelstellingen gegevens (voorbeelden) die is verstrekt door de Batch-service. Een formule vergroot of verkleind groepsgrootte op basis van de waarden die deze wordt opgehaald van de service. De service gedefinieerde variabelen die hierboven zijn beschreven zijn objecten die bieden verschillende methoden voor toegang tot gegevens die is gekoppeld aan dat object. De volgende expressie bevat bijvoorbeeld een aanvraag om de laatste vijf minuten van CPU-gebruik:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Methode | Beschrijving |
| --- | --- |
| GetSample() | De `GetSample()` methode geeft als resultaat een vector voorbeelden.<br/><br/>Een voorbeeld is 30 seconden zegt met gegevens van de doelstellingen. Voorbeelden worden met andere woorden, elke 30 seconden verkregen. Maar anders vermeld, er is een vertraging op tussen wanneer een steekproef worden verzameld en wanneer deze beschikbaar is in een formule is. Niet alle voorbeelden voor een bepaalde periode mogelijk als zodanig zijn beschikbaar voor evaluatie van een formule.<ul><li>`doubleVec GetSample(double count)`<br/>Hiermee geeft u het aantal voorbeelden van de meest recente voorbeelden die zijn verzameld verkrijgen.<br/><br/>`GetSample(1)`retourneert de laatste beschikbaar steekproef. Aan de doelstellingen zoals `$CPUPercent`, echter niet gebruik deze omdat er geen om te weten *wanneer* de steekproef is verzameld. Het kan zijn recente of, vanwege systeemproblemen, kan het veel ouder zijn. Is het beter in dat geval gebruik van een tijdsinterval zoals hieronder wordt weergegeven.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Hiermee geeft u een bepaald tijdsbestek voor het verzamelen van voorbeeldgegevens. (Optioneel) specificeert het ook het percentage van de voorbeelden die beschikbaar zijn in de gevraagde tijdsbestek moet zijn.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`het resultaat 20 voorbeelden als alle voorbeelden voor het laatste tien minuten aanwezig in de geschiedenis CPUPercent zijn. Als de laatste minuut van geschiedenis niet beschikbaar is, maar zou alleen 18 voorbeelden geretourneerd. In dit geval:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`zou mislukt omdat slechts 90 procent van de voorbeelden beschikbaar zijn.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`dat alleen.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Hiermee geeft u een bepaald tijdsbestek voor het verzamelen van gegevens, met zowel een begintijd en eindtijd.<br/><br/>Zoals eerder vermeld, moet u er een vertraging op tussen wanneer een steekproef worden verzameld en wanneer deze is beschikbaar voor een formule is. Dit moet worden beschouwd als u gebruikt de `GetSample` methode. Zie `GetSamplePercent` hieronder.|
| GetSamplePeriod() | Geeft als resultaat de periode van voorbeelden die zijn die u hebt gemaakt in een verzameling historische voorbeeldgegevens. |
| Count() | Geeft als resultaat het totale aantal voorbeelden in de geschiedenis van metrische. |
| HistoryBeginTime() | Geeft als resultaat het tijdstempel van de oudste steekproef van de beschikbare gegevens voor de meetwaarde. |
| GetSamplePercent() |Geeft als resultaat het percentage van de voorbeelden die beschikbaar voor een bepaald tijdsinterval zijn. Bijvoorbeeld:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Omdat de `GetSample` methode mislukt als het percentage van de voorbeelden geretourneerd is kleiner is dan het `samplePercent` opgegeven, kunt u de `GetSamplePercent` methode eerst controleren. Vervolgens kunt u een alternatieve actie uitvoeren onvoldoende voorbeelden aanwezig zijn, zonder de automatische schaal evaluatie stoppen.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Voorbeelden, percentage van de steekproef en de methode *GetSample()*

De bewerking core van een formule automatisch schalen is retourneren taak- en resourcekalenders metrische gegevens en vervolgens aanpassen van de groepsgrootte op basis van die gegevens. Als zodanig is het belangrijk om wissen begrijpen hoe automatisch schalen formules werken met gegevens aan de doelstellingen of "voorbeelden."

**Voorbeelden**

De service Batch regelmatig duurt *voorbeelden* van de taak- en resourcekalenders aan de doelstellingen en beschikbaar maken voor uw formules automatisch schalen. Deze voorbeelden worden elke 30 seconden door de service Batch opgenomen. Er is echter meestal sommige latentie waardoor een vertraging tussen wanneer deze voorbeelden zijn opgenomen en wanneer ze beschikbaar zijn gesteld voor (en kunnen worden gelezen door) automatisch schalen formules. Daarnaast kunnen vanwege verschillende factoren zoals netwerk of andere problemen infrastructuur voorbeelden mogelijk niet is geregistreerd voor een bepaald tijdsinterval. Dit resulteert in "ontbrekende" voorbeelden.

**Percentage van de steekproef**

Wanneer `samplePercent` wordt doorgegeven aan de `GetSample()` methode of de `GetSamplePercent()` methode wordt aangeroepen, "percentage" verwijst naar een vergelijking tussen de totale *mogelijke* aantal voorbeelden die door de service Batch worden geregistreerd en het aantal voorbeelden die daadwerkelijk *beschikbaar* voor uw formule automatisch schalen zijn.

We bekijken een tijdspanne 10 minuten als voorbeeld. Omdat voorbeelden zijn opgenomen elke 30 seconden, binnen een tijdspanne 10 minuten, zou het maximum aantal voorbeelden die door de Batch worden geregistreerd 20 voorbeelden (2 per minuut). Echter vanwege de inherent latentie van de rapportage om of enkele andere uitgifte binnen de infrastructuur van Azure, er zijn mogelijk alleen 15 voorbeelden die beschikbaar voor uw formule automatisch schalen om te lezen zijn. Dit betekent dat voor die periode 10 minuten slechts **75 procent** van het totale aantal voorbeelden opgenomen werkelijk beschikbaar is voor uw formule.

**Bereiken voor GetSample() en voorbeeld**

Formules automatisch schalen wilt worden vergroten en verkleinen van uw groepen--knooppunten toevoegen of verwijderen van knooppunten. Omdat knooppunten u geld kost, die u wilt zorgen dat uw formules maken gebruik van een intelligente methode analyse die is gebaseerd op voldoende gegevens. Daarom, is het raadzaam een analyse populair van tekst in formules te gebruiken. Dit type wordt groter en kleiner maken op basis van een *bereik* van verzamelde voorbeelden van uw toepassingen.

Gebruik hiervoor `GetSample(interval look-back start, interval look-back end)` om terug te keren een **vector** van voorbeelden:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Wanneer de bovenstaande regel wordt geëvalueerd door Batch, wordt een aantal voorbeelden geretourneerd als een vector van waarden. Bijvoorbeeld:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Nadat u de vectorvariant van voorbeelden hebt verzameld, u kunt functies, zoals `min()`, `max()`, en `avg()` naar duidelijke waarden zijn afgeleid van de verzamelde bereik.

Voor extra beveiliging, kunt u een formule-evaluatie *mislukt* afdwingen als kleiner dan een bepaald percentage van de steekproef beschikbaar voor een bepaalde periode is. Wanneer u een formule-evaluatie mislukt afdwingen, u de opdracht Batch te beëindigen verder evaluatie van de formule als het opgegeven percentage van de voorbeelden niet beschikbaar is is, geeft en geen wijziging groepsgrootte komen. Een vereiste percentage van de voorbeelden voor de evaluatie te kunnen uitvoeren, opgeven door deze als de derde parameter voor `GetSample()`. Hier wordt een vereiste van 75 procent van de voorbeelden bepaald:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Het is ook belangrijk, vanwege de hierboven genoemde vertraging in de steekproef beschikbaarheid, kunt u altijd een tijdsbereik opgeven met een uiterlijk-back-begintijd die ouder is dan één minuut. Dit is omdat het duurt ongeveer een minuut voor voorbeelden doorgeven via het systeem, dus voorbeelden in het bereik `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` vaak niet meer beschikbaar. Klik nogmaals kunt u de parameter percentage van `GetSample()` afdwingen van een bepaalde steekproef percentage vereiste.

> [AZURE.IMPORTANT] We **adviseren ten zeerste** dat u *alleen de optie *voorkomen te vertrouwen ** op `GetSample(1)` in uw formules automatisch schalen **. Dit komt doordat `GetSample(1)` in principe wordt gemeld dat de Batch-service, "ik krijgen de laatste steekproef die u hebt, ongeacht hoe lang geleden u deze hebt ontvangen." Aangezien er slechts een steekproef zijn één en is het mogelijk een oudere steekproef, het niet mogelijk medewerker van de afbeelding groter van recente taak of resource staat. Als u gebruik maakt van `GetSample(1)`, ervoor zorgen dat dit onderdeel van een grotere instructie en niet de komma alleen gegevens die afhankelijk zijn van de formule is.

## <a name="metrics"></a>Aan de doelstellingen

U kunt zowel de **resource** en de **taak** kunt gebruiken wanneer u een formule definieert. U aanpassen het doel aantal speciale knooppunten in de groep op basis van de doelstellingen-gegevens die u verkrijgen en evalueren. Zie de sectie [variabelen](#variables) hierboven voor meer informatie over elke metrisch.

<table>
  <tr>
    <th>Metrisch</th>
    <th>Beschrijving</th>
  </tr>
  <tr>
    <td><b>Resource</b></td>
    <td><p><b>De doelstellingen van de resource</b> op basis van de processor, bandbreedte en geheugen gebruik van berekeningscluster knooppunten, alsmede het aantal knooppunten.</p>
        <p> Deze service gedefinieerde variabelen zijn handig om aanpassingen op basis van knooppunt tellen:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Deze service gedefinieerde variabelen zijn handig om aanpassingen op basis van knooppunt Resourcegebruik:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Taak</b></td>
    <td><p><b>Aan de doelstellingen van taak</b> zijn op basis van de status van taken, zoals actief, in behandeling, en voltooid. De volgende service gedefinieerde variabelen zijn handig om de grootte van de groep aanpassen op basis van de doelstellingen van de taak:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Een formule automatisch schalen schrijven

U zodat er overzichten die gebruikmaken van de bovenstaande onderdelen van een formule automatisch schalen maken en vervolgens deze instructies in een volledige formule combineren. In dit gedeelte maken we een voorbeeldformule automatisch schalen dat sommige echte schaal beslissingen kunt uitvoeren.

Eerst de vereisten voor onze nieuwe automatisch schalen formule we definiëren. De formule moet uitvoeren:

1. **Vergroot** het doel aantal berekeningscluster knooppunten in een groep als CPU-gebruik hoog is.
2. **Verkleinen** het doel aantal berekeningscluster knooppunten in een groep CPU-gebruik is laag.
3. Altijd beperken het **maximum** aantal knooppunten tot 400.

*Vergroot* het aantal knooppunten tijdens hoog CPU-gebruik, wordt de instructie waarmee wordt gevuld van een gebruiker gedefinieerde variabele definiëren (`$totalNodes`) met een waarde die is 110 procent van het huidige doel aantal knooppunten, maar alleen als de minimale gemiddelde CPU-gebruik tijdens de laatste 10 minuten boven 70 procent is. Anders gebruiken we de huidige speciale waarde.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

Te *verkleinen* het aantal knooppunten tijdens lage CPU-gebruik de volgende instructie in onze formule stelt hetzelfde `$totalNodes` variabele 90 procent van het huidige doel aantal knooppunten als het gemiddelde CPU-gebruik in de afgelopen 60 minuten onder 20 procent is. Anders gebruiken de huidige waarde van `$totalNodes` die we in de bovenstaande instructie ingevuld.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Nu het doel aantal speciale berekeningscluster knooppunten op een **maximum** van 400 beperken:

```
$TargetDedicated = min(400, $totalNodes)
```

Hier volgt de volledige formule:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Een automatisch schalen ingeschakelde van toepassingen maken

Als u wilt een nieuwe groep maken met autoscaling ingeschakeld, kunt u een van de volgende technieken:

**Batch .NET**

1. De groep met [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx)maken.
1. Stel de eigenschap [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) op `true`.
1. Stel de eigenschap [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) met de formule automatisch schalen.
1. (Optioneel) Stel de eigenschap [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) (standaard is 15 minuten).
1. De groep met [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) of [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx)doorvoeren.

**Batch REST API**

* [Een resourcegroep die bij een account toevoegen](https://msdn.microsoft.com/library/azure/dn820174.aspx): Geef de `enableAutoScale` en `autoScaleFormula` elementen in uw aanvraag REST API voor het configureren van automatische schaling voor een groep wanneer u deze hebt gemaakt.

Het volgende codefragment Hiermee maakt u een automatisch schalen ingeschakelde van toepassingen met behulp van de [Batch.NET] [ net_api] bibliotheek. Van de groep automatisch schalen formule wordt ingesteld voor het doel aantal knooppunten tot en met 5 maandag en 1 op elke andere dag van de week. Het [interval voor automatisch schaal](#automatic-scaling-interval) is ingesteld op 30 minuten. In dit document en de andere C# fragmenten in dit artikel, "myBatchClient" is een goed geïnitialiseerd exemplaar van [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Naast de Batch REST API en .NET SDK, kunt u een van de andere [Batch SDK's](batch-technical-overview.md#batch-development-apis), [Batch PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)en de [Batch CLI](batch-cli-get-started.md) gebruiken om te werken met autoscaling.

> [AZURE.IMPORTANT] Wanneer u een toepassingen automatisch schalen ingeschakelde maakt, moet u **niet** opgeven de `targetDedicated` parameter. Ook als u handmatig wilt een toepassingen automatisch schalen ingeschakelde formaat (bijvoorbeeld met [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]), moet u eerste **uitschakelen** Automatische schaal in de groep, klikt u vervolgens het formaat wijzigen.

### <a name="automatic-scaling-interval"></a>Interval voor automatisch schaal

Standaard de Batch-service Hiermee past u de grootte van een groep op basis van de formule automatisch schalen elke **15 minuten**. Dit interval is echter configureerbare, met behulp van de volgende eigenschappen van de groep.

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API)

De minimale interval is vijf minuten en het maximale aantal is 168 uur. Als een interval buiten dit bereik is opgegeven, retourneert de Batch-service een fout Ongeldige aanvragen (400).

> [AZURE.NOTE] AutoScaling momenteel niet is bedoeld om te reageren op wijzigingen in een handomdraai, maar liever is bedoeld voor het aanpassen van de grootte van uw groep geleidelijk als u een werkbelasting uitvoeren.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Autoscaling op een bestaande toepassingen inschakelen

Als u al een groep hebt gemaakt met een bepaald aantal knooppunten berekenen met behulp van de parameter *targetDedicated* , kunt u nog steeds autoscaling op de toepassingen inschakelen. Elke Batch SDK vindt u een bewerking 'inschakelen automatisch schalen', bijvoorbeeld:

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (Batch .NET)
*  [Inschakelen van automatische schalen op een groep] [ rest_enableautoscale] (REST API)

Wanneer u autoscaling op een bestaande toepassingen inschakelt, wordt het volgende geldt:

* Als automatische schaling is momenteel **uitgeschakeld** in de groep wanneer u het verzoek "inschakelen automatisch schalen" verzendt, opgeven u *moet* een geldige automatisch schalen formule wanneer u de aanvraag verzendt. *Desgewenst* kunt u een automatisch schalen evaluatie interval opgeven. Als u een interval niet opgeeft, is de standaardwaarde van 15 minuten wordt gebruikt.

* Als automatisch schalen momenteel **ingeschakeld** op de groep is, kunt u een formule automatisch schalen, een interval evaluatie of beide. U kunt geen beide eigenschappen weglaat.

  * Als u een nieuw automatisch schalen evaluatie interval opgeeft, klikt u vervolgens de bestaande evaluatie planning wordt gestopt en een nieuwe planning wordt gestart. Begintijd van de nieuwe planning is de tijd waarop het verzoek 'automatisch schalen inschakelen' is uitgegeven.

  * Als u de formule automatisch schalen of evaluatie interval, de Batch-service weglaat blijft de huidige waarde van deze instelling gebruiken.

> [AZURE.NOTE] Als een waarde is opgegeven voor de parameter *targetDedicated* wanneer de groep is gemaakt, wordt deze genegeerd wanneer de automatische schaal formule is geëvalueerd.

Deze C#-codefragment wordt de [Batch.NET] [ net_api] bibliotheek autoscaling op een bestaande toepassingen inschakelen:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Een formule automatisch schalen bijwerken

U één "inschakelen automatisch schalen" verzoek *bijwerken* de formule op een bestaande automatisch schalen ingeschakelde van toepassingen gebruiken (bijvoorbeeld met [EnableAutoScale] [ net_enableautoscale] in Batch .NET). Er is geen speciale "bijwerken automatisch schalen" bewerking. Bijvoorbeeld als autoscaling al op 'myexistingpool' is ingeschakeld wanneer de volgende code wordt uitgevoerd, de formule automatisch schalen vervangen door de inhoud van `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Het interval voor het automatisch schalen

Terwijl met het bijwerken van een formule automatisch schalen, u de dezelfde [EnableAutoScale gebruikt] [ net_enableautoscale] methode om het interval automatisch schalen evaluatie van een bestaande automatisch schalen ingeschakelde-toepassingen te wijzigen. Als u bijvoorbeeld het interval van de evaluatie automatisch schalen en 60 minuten voor een resourcegroep die al is automatisch schalen ingeschakelde instellen:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Een automatisch schalen Formule evalueren

Voordat u het naar een groep toepast, kunt u een formule evalueren. Op deze manier kunt u een "test uitvoeren" van de formule om te zien hoe de instructies evalueren voordat u de formule genomen uitvoeren.

Als u wilt een formule automatisch schalen evalueren, moet u de eerste **autoscaling inschakelen** op de groep met een **geldige formule**. Als u testen van een formule in een resourcegroep die autoscaling ingeschakeld nog niet hebt wilt, kunt u de formule van één regel `$TargetDedicated = 0` wanneer u autoscaling eerst inschakelen. Gebruik vervolgens een van de volgende handelingen uit om te evalueren van de formule die u wilt testen:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) of [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Deze methoden Batch .NET vragen om de ID van een bestaande toepassingen en een tekenreeks met de formule automatisch schalen wordt berekend. De evaluatieresultaten zijn opgenomen in de resulterende [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) exemplaar.

* [Een automatische schaal Formule evalueren](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    Geef de ID voor de toepassingen in de URI en de formule automatisch schalen in het element *autoScaleFormula* van het hoofdgedeelte van de aanvraag in dit verzoek REST API. Het antwoord van de bewerking bevat een foutgegevens die mogelijk zijn gerelateerd aan de formule.

In deze [Batch.NET] [ net_api] codefragment, we een formule voordat u toepast op de [CloudPool]evalueren[net_cloudpool]. Als de verzameling geen autoscaling ingeschakeld, wordt deze inschakelen eerst.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Succesvolle evaluatie van de formule in deze fragment resulteert in de uitvoer ongeveer als volgt uit:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Informatie over automatisch schalen wordt uitgevoerd

Om ervoor te zorgen dat de formule wordt uitgevoerd, zoals verwacht, is het raadzaam dat u de resultaten van de autoscaling "wordt uitgevoerd' Batch uitvoeren met uw groep regelmatig controleren. Als een verwijzing naar de groep, u zo is, (of vernieuwen) en de kenmerken van de laatste automatisch schalen uitvoeren.

In Batch .NET bevat de eigenschap [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) verschillende eigenschappen verstrekken van informatie over de meest recente Automatische schaal uitvoeren uitgevoerd op de groep door de Batch-service.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

Het verzoek [informatie over een groep](https://msdn.microsoft.com/library/dn820165.aspx) in de REST API, geeft als resultaat informatie over de toepassingen, waaronder de meest recente Automatische schaal uitvoeren van informatie in [autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun).

Het volgende C#-codefragment gebruikt de Batch .NET-bibliotheek informatie over de laatste autoscaling worden uitgevoerd op de groep "myPool" afdrukken:

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Voorbeeld van uitvoer van de voorgaande fragment:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Automatisch schalen voorbeeldformules

Laten we eens kijken enkele formules die een andere manier om aan te passen van de hoeveelheid berekeningscluster resources in een groep.

### <a name="example-1-time-based-adjustment"></a>Voorbeeld 1: Op tijd gebaseerde correctie

Misschien wilt u de groepsgrootte op basis van de dag van de week en tijd van de dag, te vergroten of verkleinen van het aantal knooppunten in de groep dienovereenkomstig aanpassen.

Deze formule wordt eerst opgehaald van de huidige tijd. Als dit een weekdag (1-5 is) en binnen werktijden (8 uur tot 6 uur), wordt de grootte van de doel-groep is ingesteld op 20 knooppunten. Anders wordt ingesteld op 10 knooppunten.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Voorbeeld 2: Taken gebaseerde aanpassing

In dit voorbeeld wordt de grootte van de groep op basis van het aantal taken in de wachtrij aangepast. Houd er rekening mee dat zowel opmerkingen als regeleinden acceptabel in formule tekenreeksen zijn.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Voorbeeld 3: Accounting voor parallelle taken

Dit is een ander voorbeeld die past u de groepsgrootte op basis van het aantal taken. Deze formule ook rekening met de [MaxTasksPerComputeNode] [ net_maxtasks] waarde die is ingesteld voor de groep. Dit is vooral handig in situaties waarin [uitvoering van parallelle taken](batch-parallel-node-tasks.md) is ingeschakeld voor uw groep.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Voorbeeld 4: Een eerste groepsgrootte instellen

In dit voorbeeld ziet u een C#-codefragment met een formule automatisch schalen die Hiermee stelt u de grootte van de groep op een bepaald aantal knooppunten voor een eerste tijdsperiode. En vervolgens deze Hiermee past u de grootte van de groep op basis van het getal te weinig en actieve taken na de eerste periode is verstreken.

De formule in het volgende codefragment:

- Hiermee stelt de eerste groepsgrootte op vier knooppunten.
- Grootte van de groep niet aangepast binnen de eerste tien minuten van de levenscyclus van de groep.
- Na tien minuten, verkrijgt u de maximale waarde van het aantal actieve en actieve taken tijdens de afgelopen 60 minuten.
  - Als beide waarden zijn aan 0 (dat aangeeft dat er geen taken uitgevoerd of in de laatste 60 minuten actief zijn), wordt de grootte van toepassingen is ingesteld op 0.
  - Als een waarde groter is dan nul is, wordt geen wijziging wordt aangebracht.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Volgende stappen

* [Azure Batch maximaliseren berekenen Resourcegebruik met gelijktijdige knooppunt taken](batch-parallel-node-tasks.md) bevat informatie over hoe u meerdere taken tegelijkertijd op de knooppunten berekenen in uw groep uitvoeren kunt. Deze functie kunt naast autoscaling, naar de duur van de taak voor sommige werkbelastingen, zodat u geld rechtsonder.

* Zorg ervoor dat uw toepassing Batch query's in de Batch-service in de meest optimale manier voor een andere efficiency donorlading. In [de Batch Azure-service efficiënt Query](batch-efficient-list-queries.md)leert u hoe u beperkingen instellen voor de hoeveelheid gegevens die over de kabel wanneer u de status van mogelijk duizenden berekeningscluster knooppunten of taken opvragen.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
