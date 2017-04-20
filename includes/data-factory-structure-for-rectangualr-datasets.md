## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Structuurdefinitie voor rechthoekige gegevenssets opgeven
De sectie structuur in de gegevenssets JSON is een **optionele** sectie voor rechthoekige tabellen (met rijen en kolommen) en een reeks kolommen voor de tabel bevat. Gebruikt u de sectie structuur voor beide die type informatie voor typeconversies of kolomtoewijzingen doen. De volgende secties worden deze functies in detail beschreven. 

Elke kolom heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| naam | De naam van de kolom. | Ja |
| type | Het gegevenstype van de kolom. Zie type conversies sectie hieronder voor meer details over wanneer moet u type informatie opgeven | Nee |
| cultuur | .NET gebaseerd cultuur moet worden gebruikt wanneer type is opgegeven en .NET type Datetime of Datetimeoffset. Standaard is ' en-us '.  | Nee |
| indeling | Notatietekenreeks moet worden gebruikt wanneer type is opgegeven en .NET Typ Datetime of Datetimeoffset. | Nee |

In het onderstaande voorbeeld ziet u de sectie structuur JSON voor een tabel met drie kolommen gebruikers-id, naam en lastlogindate.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Gebruik de volgende richtlijnen voor wanneer "structuur" informatie wilt tonen en wat u wilt opnemen in de sectie **structuur** .

- **Voor gestructureerde gegevensbronnen** die winkelgegevens gegevens schema en een type samen met de gegevens zelf (gegevensbronnen zoals SQL Server, Oracle, Azure-tabel enz.), moet u de sectie 'structuur' alleen als u wilt doen? Klik kolommen toewijzen van specifieke bronkolommen aan specifieke kolommen in sink en hun namen zijn niet identiek (Zie details in de kolom toewijzing sectie hieronder). 

    Bovengenoemde, is het type informatie is optioneel in de sectie "structuur". Voor gestructureerde bronnen, type informatie is al beschikbaar als onderdeel van de definitie van de gegevensset in de winkel gegevens, dus moet u niet opnemen type informatie wanneer u de sectie "structuur" opneemt.
- **Voor schema op gelezen gegevensbronnen (specifiek Azure blob)** u kiezen kunt voor de opslag van gegevens zonder een schema of type informatie met de gegevens op te slaan. Voor deze soorten gegevensbronnen moet u "structuur" opnemen in de volgende 2 gevallen:
    - U wilt doen kolommen toewijzen.
    - Wanneer de gegevensset een bron in een activiteit in een kopiëren is, kunt u gegevens in "structuur" en gegevens factory gebruikt deze typegegevens voor conversie naar systeemeigen typen voor de sink. Zie [gegevens van en naar Azure Blob verplaatsen](../articles/data-factory/data-factory-azure-blob-connector.md) artikel voor meer informatie.

### <a name="supported-net-based-types"></a>Ondersteund. NET gebaseerde typen 
Gegevens factory ondersteunt de volgende CLS compatibele .NET gebaseerd typewaarden voor het type informatie in "structuur" voor schema op gelezen gegevensbronnen zoals Azure blob.

- Int16
- Int32 
- Int64
- Eén
- Dubbeltik
- Decimaal
- Byte]
- BOOL
- Tekenreeks 
- GUID
- Datum /
- DateTimeOffset
- Tijdspanne 

Voor datum-/ & Datetimeoffset kunt u desgewenst ook 'Cultuur' & 'indeling' tekenreeks om te vergemakkelijken parseren van uw aangepaste Datetime-tekenreeks opgeven. Zie voorbeeld voor typeconversie hieronder.

