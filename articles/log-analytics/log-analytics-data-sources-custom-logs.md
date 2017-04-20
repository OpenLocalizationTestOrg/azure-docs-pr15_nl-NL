<properties 
   pageTitle="Aangepaste logboeken in Log Analytics | Microsoft Azure"
   description="Log Analytics kunt gebeurtenissen verzamelen uit tekstbestanden op computers met Windows zowel Linux.  In dit artikel wordt beschreven hoe een nieuwe aangepaste logboek en details van de records die ze in de bibliotheek OMS maken definiëren."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-logs-in-log-analytics"></a>Aangepaste logboeken in Log Analytics

De logboeken aan de aangepaste gegevensbron in Log Analytics kunt u gebeurtenissen uit tekstbestanden op computers met Windows zowel Linux verzamelen. Veel toepassingen vastleggen informatie tekstbestanden in plaats van standaard logboekregistratie services zoals gebeurtenissenlogboek van Windows of Syslog.  Zodra die worden verzameld, kunt u elke record in het logboek parseren in afzonderlijke velden met de functie [Aangepaste velden](log-analytics-custom-fields.md) van Log Analytics.

![Aangepaste verzamelen](media/log-analytics-data-sources-custom-logs/overview.png)

De logboekbestanden moeten worden verzameld, moeten overeenkomen met de volgende criteria voldoet.

- Het logboek moet hebben een enkel item per regel of gebruiken van een tijdstempel die overeenkomen met een van de volgende indelingen aan het begin van elk item.

    JJJJ-MM-DD: MM: SS <br>
  M/D/JJJJ UU: MM: SS AM/PM <br>
  Ma DD, jjjj: mm: ss
    
- Het logboekbestand mag geen ronde updates toestaan waar het bestand wordt overschreven door nieuwe gegevens. 

## <a name="defining-a-custom-log"></a>Een aangepaste logboek definiëren

Gebruik de volgende procedure om te definiëren van een aangepaste logboekbestand.  Ga naar het einde van dit artikel voor een overzicht van een steekproef van het toevoegen van een aangepaste logboek.

### <a name="step-1-open-the-custom-log-wizard"></a>Stap 1. De Wizard aangepaste Log openen

De Wizard aangepaste Log wordt uitgevoerd in de portal OMS en kunt u een nieuwe aangepaste logboek voor het verzamelen van definiëren.

1.  Klik in de portal OMS Ga naar **Instellingen**.
2.  Klik op **gegevens** en klik vervolgens op **aangepaste logboeken**.
3.  Standaard worden alle configuratiewijzigingen worden automatisch verplaatst naar alle agenten.  Als u Linux wilt toevoegen, wordt een configuratiebestand verzonden naar de Fluentd gegevens verzamelen.  Als u wijzigen van dit bestand handmatig op elk Linux-agent wilt, schakelt u het vak *toepassen onder configuratie voor mijn computers Linux*.
4.  Klik op **Add +** om de aangepaste Log Wizard te openen.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Stap 2. Uploaden en parseren van een steekproef-logboek

Begint u met het uploaden van een steekproef van het aangepaste logboek.  De wizard parseert en weergeven van de items in dit bestand om te valideren.  Log Analytics wordt gebruikt in het scheidingsteken dat u opgeeft voor elke record.

**Nieuwe regel** is het standaardscheidingsteken en wordt gebruikt voor de logboekbestanden die één vermelding per regel hebt.  Als de regel met een datum en tijd op een van de beschikbare indelingen begint, kunt u een **tijdstempel** scheidingsteken dat ondersteuning biedt voor items die meer dan één regel beslaan opgeven. 

Als een scheidingsteken tijdstempel wordt gebruikt, wordt klikt u vervolgens de eigenschap TimeGenerated van elk van de records die zijn opgeslagen in OMS gevuld met de datum/tijd opgegeven voor dat item in het logboekbestand.  Als een nieuwe regel scheidingsteken wordt gebruikt, wordt klikt u vervolgens TimeGenerated gevuld met de datum en tijd dat Log Analytics het fragment verzameld. 

>[AZURE.NOTE]Log analyse wordt momenteel verwerkt de datum/tijd die worden verzameld uit een Meld u aan met een scheidingsteken tijdstempel als UTC.  Dit wordt binnenkort gebruik van de tijdzone op de agent worden gewijzigd. 
 
1.  Klik op **Bladeren** en bladert u naar een voorbeeldbestand.  Opmerking dat deze mogelijk knop wellicht **Bestand kiezen** in sommige browsers wordt aangeduid.
2.  Klik op **volgende**. 
3.  De Wizard aangepaste Log wordt het bestand uploaden en een lijst met de records die wordt aangegeven.
4.  Wijzig het scheidingsteken dat wordt gebruikt om te identificeren van een nieuwe record en selecteer het scheidingsteken die het beste de records in het logboekbestand worden geïdentificeerd.
5.  Klik op **volgende**.

### <a name="step-3-add-log-collection-paths"></a>Stap 3. Log siteverzameling paden toevoegen

Klik op de agent waar deze de aangepaste log kunt vinden, moet u een of meer paden definiëren.  U kunt een specifiek pad en de naam voor het logboekbestand verstrekken of u kunt een pad met een jokerteken voor de naam opgeven.  Dit ondersteunt toepassingen die een nieuw bestand maken elke dag of wanneer een bepaalde grootte in één bestand heeft bereikt.  U kunt ook meerdere paden opgeven voor één logboekbestand.

Bijvoorbeeld kan een toepassing veroorzaken een logboekbestand elke dag met de datum in de naam zoals log20100316.txt in. Een patroon voor zoals een logboek mogelijk *log\*.txt* waarin wilt toepassen op een logboekbestand volgen van de toepassing bevindt zich naming kleurenschema.

De volgende tabel bevat voorbeelden van geldige patronen om op te geven van de verschillende logboekbestanden. 

| Beschrijving | Pad |
|:--|:--|
| Alle bestanden in *C:\Logs* met de extensie .txt op Windows-agent | C:\Logs\\\*.txt |
| Alle bestanden in *C:\Logs* met een naam die beginnen met logboekbestanden en extensie .txt op Windows-agent | C:\Logs\log\*.txt |
| Alle bestanden in */var/log/audit* met de extensie .txt op Linux-agent | /var/log/audit/*.txt |
| Alle bestanden in */var/log/audit* met een naam die beginnen met logboekbestanden en extensie .txt op Linux-agent | /var/log/audit/log\*.txt |
  

1.  Selecteer Windows of Linux om op te geven welke padindeling die u wilt toevoegen.
2.  Typ het pad en klik op de **+** knop.
3.  Herhaal het proces voor elke extra paden.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Stap 4. Geef een naam en beschrijving voor het logboek

De naam die u opgeeft wordt worden gebruikt voor het logboektype zoals hierboven is beschreven.  Deze eindigt altijd met _CL om ze te onderscheiden als een aangepaste logboek.

1.  Typ een naam voor het logboek.  De ** \_LC** achtervoegsel wordt automatisch weergegeven.
2.  Een optionele **Beschrijving**toevoegen.
3.  Klik op **volgende** als u wilt de definitie van aangepaste log opslaan.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Stap 5. Valideren dat de logboeken aan de aangepaste worden verzameld
Het kan duren naar uren voor de oorspronkelijke gegevens uit een nieuwe aangepaste logboek in Log Analytics moet worden weergegeven.  Start het verzamelen van gegevens uit de logboekbestanden zijn gevonden in het pad dat u hebt opgegeven vanaf het moment dat u de aangepaste log gedefinieerd.  Deze niet behouden blijft voor de items die u tijdens het maken van aangepaste log hebt geüpload, maar deze al bestaande vermeldingen in de logboekbestanden die wordt gezocht naar worden verzameld.

Zodra Log Analytics wordt gestart verzamelen van de aangepaste log, ziet u de records die met een zoekopdracht Log mogelijk.  Gebruik de naam die u het aangepaste logboek als het **Type** in uw query gegeven.

>[AZURE.NOTE] Als de eigenschap Nieuwrap: in het zoeken ontbreekt, moet u mogelijk sluiten en opnieuw openen van uw browser.

### <a name="step-6-parse-the-custom-log-entries"></a>Stap 6. Parseren van de aangepaste logboekvermeldingen

De volledige vermelding wordt opgeslagen in één eigenschap **Nieuwrap:**genoemd.  Wilt u waarschijnlijk scheidt u de verschillende onderdelen van de informatie in elk item in afzonderlijke eigenschappen die zijn opgeslagen in de record.  Doet u deze met de functie [Aangepaste velden](log-analytics-custom-fields.md) van Log Analytics.

Gedetailleerde stappen voor het parseren van de aangepaste vermelding daar niet vinden.  Raadpleeg de [Aangepaste velden](log-analytics-custom-fields.md) documentatie van deze gegevens.

## <a name="disabling-a-custom-log"></a>Een aangepaste logboek uitschakelen

U kunt de definitie van een aangepaste log niet verwijderen nadat deze is gemaakt, maar u deze uitschakelen kunt door het verwijderen van alle de paden van de siteverzameling.

1.  Klik in de portal OMS Ga naar **Instellingen**.
2.  Klik op **gegevens** en klik vervolgens op **aangepaste logboeken**.
3.  Klik op **Details** naast de definitie van het logboek met aangepaste wilt uitschakelen.
4.  Verwijder elk van de siteverzameling paden voor de aangepaste log definitie.


## <a name="data-collection"></a>Gegevens verzamelen

Log Analytics worden nieuwe gegevens worden verzameld uit elke aangepaste logboek ongeveer elke 5 minuten.  De agent wordt de positie opnemen in elk logboekbestand die worden verzameld uit.  Als de agent voor een periode offline gaat, klikt u vervolgens verzamelen Log Analytics vermeldingen van waar deze laatste gebleven was, zelfs als posten zijn gemaakt terwijl de-agent offline is.

De volledige inhoud van de vermelding worden geschreven naar één eigenschap **Nieuwrap:**genoemd.  U kunt dit parseren tot meerdere eigenschappen die kunnen worden geanalyseerd en gezocht afzonderlijk met [Aangepaste velden](log-analytics-custom-fields.md) definiëren nadat u de aangepaste log hebt gemaakt.


## <a name="custom-log-record-properties"></a>Aangepaste eigenschappen van record

Aangepaste logboekrecords hebben een type met de logboeknaam van de die u opgeeft en de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| TimeGenerated | Datum en tijd waarop de record zijn verzameld door Log Analytics.  Als het logboek gebruikmaakt van een scheidingsteken op basis van de is dit de tijd die worden verzameld uit het fragment. |
| SourceSystem  | Type agent de record zijn verzameld uit. <br> OpsManager – Windows-agent, een van beide direct verbinding maken of SCOM <br> Linux – alle Linux agenten  |
| Nieuwrap:             | Volledige tekst van de verzamelde invoer. |
| ManagementGroupName | De naam van de management group voor SCOM agenten.  Voor andere gemachtigden, is dit de AOI -\<werkruimte-ID\> |


## <a name="log-searches-with-custom-log-records"></a>Zoekopdrachten in het logboek met aangepaste logboekrecords

Records van aangepaste logboeken worden opgeslagen in de bibliotheek OMS net als records uit een andere gegevensbron.  Hebben ze een type overeenkomt met de naam die u opgeeft wanneer u het logboek definieert, zodat u de eigenschap Type in de zoekresultaten kunt die worden verzameld uit een specifieke logboek records op te halen.

De volgende tabel bevat verschillende voorbeelden van log zoekopdrachten die records van aangepaste logboeken ophalen.

| Query | Beschrijving |
|:--|:--|
| Typ = MyApp_CL | Alle gebeurtenissen uit een aangepaste Meld benoemde MyApp_CL. |
| Typ = MyApp_CL Severity_CF = fout | Alle gebeurtenissen uit een aangepaste logboek met de naam MyApp_CL met een waarde van *fout* in een aangepast veld met de naam *Severity_CF*. |




## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Voorbeeld-Stapsgewijze instructies voor het toevoegen van een aangepaste logboek

De volgende sectie begeleidt bij een voorbeeld van een aangepaste logboek te maken.  Het voorbeeldlogboek worden verzameld heeft één vermelding op elke regel beginnen met een datum en tijd en klik vervolgens met door komma's gescheiden velden voor code, status en het bericht.  Hieronder ziet u enkele voorbeelden van vermeldingen.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Uploaden en parseren van een steekproef-logboek

We bieden een van de logboekbestanden en de gebeurtenissen die wordt worden verzamelen kunt zien.  In dit geval is nieuwe regel een voldoende scheidingsteken.  Als u één vermelding in het logboek kan door meerdere regels reeks, klikt u vervolgens moet een scheidingsteken tijdstempel worden gebruikt.

![Uploaden en parseren van een steekproef-logboek](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Log siteverzameling paden toevoegen

De logboekbestanden wordt zich bevinden in *C:\MyApp\Logs*.  Een nieuw bestand wordt gemaakt elke dag met een naam die de datum in het patroon *appYYYYMMDD.log*bevat.  Een voldoende patroon voor dit logboek zou *C:\MyApp\Logs\\\*.log*.

![Log siteverzameling pad](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Geef een naam en beschrijving voor het logboek

We gebruiken een naam van *MyApp_CL* te typen in een **Beschrijving**.

![Naam van het logboek](media/log-analytics-data-sources-custom-logs/log-name.png)


### <a name="validate-that-the-custom-logs-are-being-collected"></a>Valideren dat de logboeken aan de aangepaste worden verzameld

We gebruiken een query met *Type = MyApp_CL* voor alle records van de verzamelde logboekgegevens.

![Log query zonder aangepaste velden](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Parseren van de aangepaste logboekvermeldingen

We aangepaste velden gebruiken om te definiëren de *EventTime*, *Code*, *Status*en *bericht* -velden en we het verschil in de records die zijn geretourneerd door de query kunt zien.

![Log query met aangepaste velden](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste velden](log-analytics-custom-fields.md) naar de vermeldingen in de aangepaste log in afzonderlijke velden parseren gebruiken.
- Meer informatie over [log zoekopdrachten](log-analytics-log-searches.md) om de gegevens die worden verzameld uit gegevensbronnen en oplossingen te analyseren. 