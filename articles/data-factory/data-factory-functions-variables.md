<properties 
    pageTitle="Gegevens Factory-functies en variabelen | Microsoft Azure" 
    description="Overzicht van Azure gegevens Factory-functies en variabelen" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure gegevens Factory - functies en variabelen
In dit artikel vindt u informatie over de functies en variabelen die worden ondersteund door Factory van Azure-gegevens.
  
## <a name="data-factory-system-variables"></a>Gegevens Factory variabelen

De naam van de variabele | Beschrijving | Bereik van een object | JSON-bereik en gebruik dozen
------------- | ----------- | ------------ | ------------------------
WindowStart | Begin van het tijdsinterval voor huidige activiteit venster uitvoeren | activiteit | <ol><li>Selectie gegevensquery opgeeft. Zie connector artikelen waarnaar wordt verwezen in het artikel [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) .</li><li>Parameters doorgeven aan component script (voorbeeld hierboven).</li>
WindowEnd | Einde van tijdsinterval voor huidige activiteit venster uitvoeren | activiteit | Zelfde als hierboven
SliceStart | Begin van het tijdsinterval voor gegevens segment worden geproduceerd | activiteit<br/>gegevensset | <ol><li>Geef dynamische mappaden en bestandsnamen tijdens het werken met [Azure Blob](data-factory-azure-blob-connector.md) en [bestandssysteem gegevenssets](data-factory-onprem-file-system-connector.md).</li><li>Geef invoer afhankelijkheden met gegevens factory-functies in activiteit invoeritems verzameling.</li></ol>
SliceEnd | Einde van tijdsinterval voor de huidige gegevens segment worden geproduceerd | activiteit<br/>gegevensset | hetzelfde als hierboven. 

> [AZURE.NOTE] Gegevens factory momenteel is vereist dat de planning die is opgegeven in de activiteit precies overeenkomen met de opgegeven in de beschikbaarheid van de gegevensset uitvoer planning. Dit betekent WindowStart, WindowEnd en SliceStart en SliceEnd worden altijd toegewezen aan dezelfde periode en een segment één uitvoer.
 
## <a name="data-factory-functions"></a>Gegevens Factory-functies 

U kunt functies gebruiken in fabriek van de gegevens samen met hierboven genoemde variabelen voor de volgende doeleinden:

1.  Selectie gegevensquery opgeeft (Zie connector artikelen waarnaar wordt verwezen door het artikel [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) .

    De syntaxis voor een gegevens factory-functie roepen is: ** $$ ** voor selectie gegevensquery en andere eigenschappen in de activiteit en gegevenssets.  
2. Siteverzameling invoer afhankelijkheden geven met de gegevens factory-functies in activiteit ingangen (Zie voorbeeld hierboven).

    $$ is niet nodig voor het opgeven van de invoer afhankelijkheid expressies.   

In het onderstaande voorbeeld wordt de eigenschap **sqlReaderQuery** in een JSON-bestand toegewezen aan een waarde die het resultaat van de functie **Text.Format** . In dit voorbeeld wordt ook gebruikt voor systeemvariabele **WindowStart**, waarmee de begintijd van de activiteit venster uitvoeren.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Functies

De volgende tabellen bevatten de functies in Azure gegevens fabriek:

Categorie | Functie | Parameters | Beschrijving
-------- | -------- | ---------- | ----------- 
Tijd | AddHours(X,Y) | X: DateTime <br/><br/>Y: int | Y uren toevoegt aan de opgegeven tijd X. <br/><br/>Voorbeeld: 9/5/2013 12:00:00 PM + 2 uur = 9/5/2013 2:00:00 PM
Tijd | AddMinutes(X,Y) | X: DateTime <br/><br/>Y: int | X van Y minuten toevoegen<br/><br/>Voorbeeld: 9/15/2013 12:00:00 PM + 15 minuten = 9/15/2013 12:15:00 PM
Tijd | StartOfHour(X) | X: Datetime | De begintijd voor het uur dat wordt aangeduid met het onderdeel uur van X krijgt. <br/><br/>Voorbeeld: StartOfHour van 9/15/2013 05:10:23 PM is 9/15/2013 05:00:00 PM
Datum | AddDays(X,Y) | X: DateTime<br/><br/>Y: int | Telt Y dagen op bij X.<br/><br/>Voorbeeld: 9/15/2013 12:00:00 PM + 2 dagen = 9/17/2013 12:00:00 PM
Datum | AddMonths(X,Y) | X: DateTime<br/><br/>Y: int | X van Y maanden toevoegen<br/><br/>Voorbeeld: 9/15/2013 12:00:00 PM + 1 maand = 10/15/2013 12:00:00 PM 
Datum | AddQuarters(X,Y) | X: DateTime <br/><br/>Y: int | Telt Y * 3 maanden x.<br/><br/>Voorbeeld: 9/15/2013 12:00:00 PM + 1 kwartaal = 12/15/2013 12:00:00 PM
Datum | AddWeeks(X,Y) | X: DateTime<br/><br/>Y: int | Telt Y * 7 dagen x<br/><br/>Voorbeeld: 9/15/2013 12:00:00 PM + 1 week = 9/22/2013 12:00:00 PM
Datum | AddYears(X,Y) | X: DateTime<br/><br/>Y: int | Y-jaar optellen bij X.<br/><br/>Voorbeeld: 9/15/2013 12:00:00 PM + 1 jaar = 9/15/2014 12:00:00 PM
Datum | Day(X) | X: DateTime | Hiermee wordt de dagcomponent van X.<br/><br/>Voorbeeld: Dag van 9/15/2013 12:00:00 PM is 9. 
Datum | DayOfWeek(X) | X: DateTime | Hiermee wordt de dag van week onderdeel van X.<br/><br/>Voorbeeld: Dag van de week van 9/15/2013 12:00:00 PM zondag is.
Datum | DayOfYear(X) | X: DateTime | Hiermee wordt de dag in het jaar, uitgedrukt in het onderdeel van het jaar van X.<br/><br/>Voorbeelden:<br/>1-12-2015: dag 335 van 2015<br/>31-12-2015: dag 365 van 2015<br/>12/31/2016: dag 366 van 2016 (sprong jaar)
Datum | DaysInMonth(X) | X: DateTime | Krijgt de dagen van de maand dat wordt aangeduid met het onderdeel van de maand van de parameter X.<br/><br/>Voorbeeld: DagenInMaand van 9/15/2013 zijn 30, aangezien er 30 dagen in de maand September.
Datum | EndOfDay(X) | X: DateTime | De datum-tijd die het einde van de dag (dagcomponent) van X aangeeft krijgt.<br/><br/>Voorbeeld: EndOfDay van 9/15/2013 05:10:23 PM is 9/15/2013 23:59:59 uur.
Datum | EndOfMonth(X) | X: DateTime | Krijgt het einde van de maand, per maand onderdeel van de parameter X weergegeven. <br/><br/>Voorbeeld: EndOfMonth van 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM (datum-tijd die het einde van de maand September aangeeft)
Datum | StartOfDay(X) | X: DateTime | Het begin van de dag dat wordt aangeduid met het onderdeel van de dag van de parameter X krijgt.<br/><br/>Voorbeeld: StartOfDay van 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM.
Datum / | FROM(X) | X: tekenreeks | Parseren tekenreeks X naar een datum-tijd.
Datum / | Ticks(X) | X: DateTime | Krijgt de maatstreepjes eigenschap van de parameter X. Eén tik gelijk is aan 100 nanoseconden. De waarde van deze eigenschap geeft het aantal tikken dat is verstreken sinds middernacht 12:00:00, 1 januari 0001. 
Tekst | Format(X) | X: tekenreeksvariabele | De tekst wordt opgemaakt.

#### <a name="textformat-example"></a>Voorbeeld van Text.Format

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

Zie [aangepaste datum en tijd opmaken tekenreeksen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) onderwerp dat beschrijft de verschillende opmaakopties die u kunt gebruiken (bijvoorbeeld: jj versus jjjj). 

> [AZURE.NOTE] Wanneer u met een functie in een andere functie, hoeft u niet te gebruiken **$$** voorvoegsel voor de binnenste functie. Bijvoorbeeld: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' en RowKey ge \\' {0:yyyy-MM-dd: mm: SS}\\'', Time.AddHours (SliceStart, -6)). In dit voorbeeld ziet u dat **$$** voorvoegsel wordt niet gebruikt voor de functie **Time.AddHours** . 
  

