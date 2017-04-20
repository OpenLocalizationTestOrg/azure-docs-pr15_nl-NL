## <a name="repeatability-during-copy"></a>Herhaalbaarheid tijdens kopiëren

Wanneer het kopiëren van gegevens met Azure SQL/SQL Server van andere gegevens worden opgeslagen, moet één herhaalbaarheid in gedachten moet houden om te voorkomen onverwachte resultaten. 

Als u gegevens met Azure SQL/SQL Server-Database, wordt de kopie activiteit al dan niet standaard toevoegen de gegevensverzameling aan de tabel sink al dan niet standaard. Als u gegevens uit een CSV (door komma's gescheiden waarden) bestand gegevensbron met twee records met Azure SQL/SQL Server-Database, is dit bijvoorbeeld hoe de tabel eruitziet:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Stel dat u fouten in het bronbestand gevonden en de hoeveelheid van omlaag buis van 2 tot en met 4 in het bronbestand bijgewerkt. Als u het segment gegevens opnieuw voor die periode uitvoeren, vindt u twee nieuwe records toegevoegd met Azure SQL/SQL Server-Database. De hieronder wordt ervan uitgegaan geen van de kolommen in de tabel de primaire sleutel hebben.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

U kunt dit voorkomen, moet u opgeven dat UPSERT semantiek gebruikmaken van een van de onder 2 regelingen die hieronder vermeld.

> [AZURE.NOTE] Een segment kan automatisch worden opnieuw uitgevoerd in Azure gegevens fabriek aan de hand van het beleid opnieuw is opgegeven.

### <a name="mechanism-1"></a>Om 1

U kunt gebruikmaken van de eigenschap **sqlWriterCleanupScript** zodat deze eerst opruimen actie uitvoeren tijdens een segment wordt uitgevoerd. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

Het script opruimen worden uitgevoerd eerste tijdens kopiëren voor een bepaald segment waarin de gegevens in de SQL-tabel die overeenkomt met het segment wilt verwijderen. De activiteit wordt vervolgens de gegevens in de SQL-tabel invoegen. 

Als het segment is nu opnieuw uitvoeren, moet u dat de hoeveelheid wordt bijgewerkt vindt als de gewenste.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Stel dat de record plat was wordt verwijderd uit het oorspronkelijke csv. Het segment opnieuw uit te voeren, resulteert het volgende resultaat: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Niets nieuwe moest worden uitgevoerd. De activiteit kopiëren hebt uitgevoerd het opruimen-script om de bijbehorende gegevens voor segment te verwijderen. En vervolgens deze de invoer van de csv (inhoudt vervolgens alleen 1 record) lezen en deze in de tabel ingevoegd. 

### <a name="mechanism-2"></a>Om 2
> [AZURE.IMPORTANT] sliceIdentifierColumnName wordt niet ondersteund voor Azure SQL Data Warehouse op dit moment. 

Er is een andere om om te bereiken herhaalbaarheid doordat een speciale kolom (**sliceIdentifierColumnName**) in de doellijst tabel. Deze kolom zou doen door Factory van Azure-gegevens worden gebruikt om ervoor te zorgen dat de bron- en doeltabellen gesynchroniseerde blijven. Deze methode werkt wanneer er sprake is van flexibiliteit bij wijzigen of het definiëren van het schema van de SQL-tabel bestemming. 

Deze kolom zou worden gebruikt door Azure gegevens Factory voor herhaalbaarheid doeleinden en in het proces Factory van Azure-gegevens worden niet schemawijzigingen aanbrengt in de tabel. Manier voor het gebruik van deze methode:

1.  Een kolom van het type binaire (32) in de bestemming van de SQL-tabel definiëren. Er moet worden geen beperkingen voor deze kolom. Laten we een naam in deze kolom als 'ColumnForADFuseOnly' in dit voorbeeld.
2.  Gebruikt u deze in de activiteit kopiëren als volgt:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure gegevens Factory worden deze kolom aan de hand van de nodig om ervoor te zorgen dat de bron- en doeltabellen blijven gesynchroniseerde ingevuld. De waarden van deze kolom moeten niet buiten deze context worden gebruikt door de gebruiker. 

Net zoals bij om 1, kopie activiteit wordt automatisch opschonen eerst de gegevens voor het opgegeven segment uit de bestemming van de SQL-tabel en voer vervolgens de activiteit kopiëren om de normale de gegevens uit de bron op bestemming voor het segment invoegen. 
