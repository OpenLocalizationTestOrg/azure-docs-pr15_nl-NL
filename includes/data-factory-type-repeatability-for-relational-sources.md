## <a name="repeatability-during-copy"></a>Herhaalbaarheid tijdens kopiëren

Als u gegevens van en tot relationele winkels, moet u herhaalbaarheid Houd er rekening mee onverwachte resultaten te vermijden. 

Een segment kan worden automatisch opnieuw uitvoeren in fabriek van Azure-gegevens aan de hand van het beleid opnieuw is opgegeven. Het is raadzaam dat u een beleid opnieuw beschermen tegen tijdelijke fouten instellen. Herhaalbaarheid is dus een belangrijk aspect naar verrichten tijdens de verplaatsing van gegevens. 

**Als een bron:**

> [AZURE.NOTE] In de volgende voorbeelden zijn van de SQL Azure, maar zijn van toepassing op alle gegevensopslag die ondersteuning biedt voor rechthoekige gegevenssets. Mogelijk moet u het **type** bron- en de eigenschap **query** aanpassen (bijvoorbeeld: query in plaats van sqlReaderQuery) voor de gegevens opslaan.   

Meestal tijdens het lezen van relationele winkels, zou u willen lezen alleen de gegevens die overeenkomt met het segment. Een manier om te doen zou met behulp van de variabelen WindowStart en WindowEnd beschikbaar in fabriek van Azure-gegevens. Lees meer over de variabelen en functies in Azure gegevens fabriek hier in het artikel [plannen en uitvoeren](../articles/data-factory/data-factory-scheduling-and-execution.md) . Voorbeeld: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Deze query leest gegevens van MijnTabel' dat in het segment duur bereik valt. Opnieuw uitvoeren van dit segment zou ook altijd zorgen dat dit probleem. 

In andere gevallen wilt u mogelijk de hele tabel lezen (voor één keer verplaatsen alleen Stel) en de sqlReaderQuery kan als volgt definiëren:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
