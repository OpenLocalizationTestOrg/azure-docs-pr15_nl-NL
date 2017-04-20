## <a name="azure-storage-linked-service"></a>Azure opslag gekoppeld Service

De **opslag van Azure gekoppeld service** , kunt u een Azure opslag-account koppelen aan een factory Azure-gegevens met behulp van de **accountsleutel**. Dit biedt de fabriek gegevens met globale toegang tot de opslag van Azure. De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor Azure gekoppeld opslagservice.

| Eigenschap | Beschrijving | Vereist |
| :-------- | :----------- | :-------- |
| type | De eigenschap type moet zijn ingesteld op: **AzureStorage** | Ja |
| connectionString | Geef de gegevens die nodig zijn om te verbinden met Azure opslag voor de eigenschap connectionString. | Ja |

Zie het volgende artikel voor stapsgewijze instructies voor de accountsleutel kopie/weergeven voor een Azure-opslag: [weergave, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Voorbeeld:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Azure opslag SA's gekoppeld Service  
Een gedeelde access-handtekening (SA's) gedelegeerd toegang geeft tot bronnen in uw account opslag. Dit betekent dat u een client beperkte machtigingen aan objecten in uw account opslagruimte voor een bepaalde termijn van tijd en met een opgegeven reeks machtigingen, zonder dat u moet het delen van uw account toegangstoetsen kunt verlenen. De SA's is een URI die in de queryparameters omvat alle benodigde informatie voor toegang tot een opslagbron geverifieerd. Voor toegang tot opslag resources met de SA's, moet de client alleen om door te geven in de SA's naar de juiste constructor of methode. Zie voor gedetailleerde informatie over SA's [gedeeld Access handtekeningen: informatie over het Model SA's](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
De service Azure opslag SA's die zijn gekoppeld, kunt u een Azure opslag-Account koppelen aan een factory Azure-gegevens met behulp van een gedeeld Access handtekening (SA's). Hierdoor wordt de fabriek gegevens met beperkte/tijd-gebonden toegang tot alle/regiospecifieke resources (blob/container) in de opslagruimte. De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor de service Azure opslag SA's die zijn gekoppeld. 

| Eigenschap | Beschrijving | Vereist |
| :-------- | :----------- | :-------- |
| type | De eigenschap type moet zijn ingesteld op: **AzureStorageSas**  | Ja |
| sasUri | Gedeeld Access handtekening URI is opgeven naar de opslag van Azure-bronnen zoals blob, container of tabel. Zie de onderstaande opmerkingen voor meer informatie. | Ja | 


**Voorbeeld:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

Bij het maken van een **SA's URI**, de volgende:  

- Azure gegevens Factory ondersteunt alleen **Service SA's**, niet-Account SA's. Zie [Typen van gedeelde Access handtekeningen](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) voor meer informatie over deze twee typen.
- Juiste alleen-lezen/schrijven **machtigingen** moet worden ingesteld op op basis van hoe de gekoppelde service (lezen, schrijven, alleen-lezen/schrijven) worden gebruikt in uw gegevens fabriek objecten.
- **Verlooptijd** moet correct zijn ingesteld. Zorg ervoor dat de toegang tot Azure Storage objecten niet binnen de actieve periode van de pijplijn verloopt.
- URI moet worden gemaakt van de juiste container/blob of tabel op basis van de nodig. Een Uri SA's naar een Azure blob kan de gegevens Factory-service voor toegang tot die bepaalde blob. Een Uri SA's aan een container Azure blob kan de service Data Factory BLOB's in die container doorlopen. Als u nodig hebt voor toegang tot later meer/minder objecten bieden of de URI SAS bijwerken, moet u de gekoppelde service bijwerken met de nieuwe URI.   
