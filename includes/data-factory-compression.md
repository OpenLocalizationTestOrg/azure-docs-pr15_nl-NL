### <a name="compression-support"></a>Compressieondersteuning voor  
Verwerking van grote gegevenssets kan leiden tot i/o- en netwerk knelpunten. Daarom kunt gecomprimeerde gegevens in winkels niet alleen overdracht van gegevens via het netwerk versnellen en Bespaar schijfruimte, maar ook weer aanzienlijke prestatieverbeteringen in groot gegevensverwerking. Op dit moment wordt compressie ondersteund voor gegevens op basis van een bestand winkels zoals Azure Blob of On-premises bestandssysteem.  

> [AZURE.NOTE] Compressie-instellingen worden niet ondersteund voor gegevens in de **AvroFormat**, **OrcFormat**of **ParquetFormat**. 

Als u wilt opgeven compressie voor een gegevensset, gebruikt u de **compressie** -eigenschap in de gegevensset JSON zoals in het volgende voorbeeld:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
De sectie **compressie** heeft twee eigenschappen:  
  
- **Type:** de compressiecodec **GZIP**, **Deflate** of **BZIP2**.  
- **Niveau:** de compressieratio, **optimale** of **snelst**. 
    - **Snelste:** De bewerking compressie overigens zo snel mogelijk, zelfs als het bestand is niet optimaal gecomprimeerd. 
    - **Optimale**: de compressie-bewerking moet worden optimaal gecomprimeerd, zelfs als de bewerking langer duurt om te voltooien. 
    
    Zie [Compressieniveau](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) onderwerp voor meer informatie. 

Stel dat het bovenstaande voorbeeld gegevensset wordt gebruikt als de uitvoer van een activiteit kopiëren, wordt de kopie activiteit wordt de uitvoergegevens gecomprimeerd met GZIP codec met optimale verhouding en noteert de gecomprimeerde gegevens in een bestand met de naam pagecounts.csv.gz in de Azure-blobopslag.   

Wanneer u de eigenschap compressie in een invoer gegevensset JSON opgeeft, kunt de pijplijn gecomprimeerde gegevens kunt lezen uit de bron en wanneer u de eigenschap in een uitvoer gegevensset JSON opgeeft, de activiteit kopie schrijven gecomprimeerde gegevens naar de bestemming. Hier volgen een paar steekproef-scenario's: 

- Alleen GZIP gecomprimeerd gegevens uit een Azure blob deze, uit en resultaatgegevens schrijven met een Azure SQL-database. U definiëren in dit geval de invoer Azure Blob gegevensset met de compressie JSON eigenschap. 
- Gegevens uit een tekstbestand vanaf on-premises implementatie bestandssysteem lezen, GZip opmaken met comprimeren en de gecomprimeerde gegevens naar een Azure blob schrijven. U definiëren een gegevensset uitvoer Azure Blob met de compressie JSON eigenschap in dit geval.  
- Een GZIP gecomprimeerde gegevens uit een Azure blob lezen, decomprimeren en comprimeren met BZIP2 resultaatgegevens naar een Azure blob schrijven. U definiëren de invoer Azure Blob gegevensset met compressietype is ingesteld op GZIP en de gegevensset uitvoer met compressietype is ingesteld op BZIP2 in dit geval.   
