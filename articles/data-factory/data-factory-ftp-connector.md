<properties 
    pageTitle="Verplaats gegevens van FTP-server | Microsoft Azure" 
    description="Meer informatie over het verplaatsen van gegevens uit een FTP-server Azure gegevens Factory gebruiken." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Verplaats gegevens van een FTP-server Azure gegevens Factory gebruiken
In dit artikel vindt u een overzicht van het gebruik van de activiteit kopiëren in Azure gegevens fabriek gegevens verplaatsen van een FTP-server naar een ondersteunde sink gegevensopslag. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) waarin een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en de lijst met gegevens winkels ondersteund als bronnen/sinks. 

Gegevens factory ondersteunt momenteel alleen zwevend gegevens uit een FTP-server naar andere winkels gegevens, maar niet voor het verplaatsen van gegevens uit andere winkels gegevens naar een FTP-server. Beide on-premises implementatie wordt ondersteund en cloud FTP-servers. 

Als u gegevens uit een **on-premises implementatie** FTP-server naar een cloud-gegevensopslag verplaatst (voorbeeld: Azure-blobopslag), installeren en gebruiken van Data Management Gateway. De Data Management Gateway is een client-agent die is geïnstalleerd op uw computer on-premises implementatie, waarmee cloudservices verbinding maken met lokale bron. Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor meer informatie over de gateway. Zie [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) -artikel voor stapsgewijze instructies over het instellen van de gateway en het gebruik van deze. De gateway kunt u verbinding maken met een FTP-server, zelfs als de server onderdeel op een IaaS Azure virtuele machine (VM is). 

U kunt de gateway installeren op dezelfde lokale computer of de Azure IaaS VM als de FTP-server. We raden echter aan dat u de gateway installeren op een aparte machine of een afzonderlijke Azure IaaS VM om te voorkomen bronnen wordt geminimaliseerd en voor betere prestaties. Wanneer u de gateway op een aparte computer installeert, is de computer moet kunnen toegang tot de FTP-server. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die gegevens opgehaald uit een FTP-server is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

De volgende voorbeelden bieden definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Voorbeeld: Gegevens uit FTP-server naar Azure blob kopiëren

In dit voorbeeld ziet hoe u gegevens uit een FTP-server naar een Azure-blobopslag kopiëren. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

- Een gekoppelde service van het type [FtpServer](#ftp-linked-service-properties).
- Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Een invoer [dataset](data-factory-create-datasets.md) van het type [bestandsshare](#fileshare-dataset-type-properties).
- Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [FileSystemSource](#ftp-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

De steekproef kopieert gegevens uit een FTP-server naar een Azure blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. 

**FTP gekoppeld service** Dit voorbeeld wordt de basisverificatie met de gebruikersnaam en wachtwoord in tekst zonder opmaak. U kunt ook een van de volgende manieren gebruiken: 

- Anonieme verificatie 
- Basisverificatie met versleutelde referenties
- FTP via SSL/TLS (TRANSFEREERT)

Zie sectie [FTP gekoppelde service](#ftp-linked-service-properties) voor verschillende soorten verificatie die u kunt gebruiken. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
    }

**Azure gekoppeld opslagservice**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**FTP invoer gegevensset** Deze dataset verwijst naar de map FTP `mysharedfolder` en de bestandsnaam `test.csv`. De pijplijn kopieert het bestand naar de bestemming. 

Instellen "externe": "true" melding aan de Data Factory-service dat de gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Azure Blob uitvoer gegevensset**

Gegevens worden geschreven naar een nieuwe blob per uur (frequentie: uur, interval: 1). Het pad naar de blob geëvalueerd dynamisch op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Pijplijn met activiteit kopiëren**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **FileSystemSource** en **sink** type is ingesteld op **BlobSink**. 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>Eigenschappen van de gekoppelde Service FTP

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor FTP-gekoppelde service.

| Eigenschap | Beschrijving | Vereist | Standaard |
| -------- | ----------- | -------- | ------- | 
| type | De eigenschap type moet zijn ingesteld op FtpServer | Ja | &nbsp;
| host | Naam of het IP-adres van de FTP-Server | Ja | &nbsp;
| authenticationType | Verificatietype opgeven | Ja | Eenvoudige, anonieme |
| gebruikersnaam | Gebruiker aan wie toegang tot de FTP-server heeft | Nee | &nbsp;
| wachtwoord | Wachtwoord voor de gebruiker (gebruikersnaam) | Nee | &nbsp;
| encryptedCredential | Versleutelde referentie voor toegang tot de FTP-server | Nee | &nbsp;
| gatewayName | Naam van de gateway Data Management Gateway verbinding maken met een on-premises FTP-server | Nee    | &nbsp;
| poort | Poort waarop u de FTP-server luistert | Nee | 21 |
| enableSsl | Geef op of gebruik FTP-via SSL/TLS-kanaal | Nee | waar | 
| enableServerCertificateValidation | Opgeven of voor server SSL-certificaat validatie bij FTP via SSL/TLS-kanaal | Nee | waar | 

### <a name="using-anonymous-authentication"></a>Anonieme verificatie gebruikt

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Gebruik van gebruikersnaam en wachtwoord in tekst zonder opmaak voor basisverificatie
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Gebruik poort, enableSsl, enableServerCertificateValidation

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>EncryptedCredential gebruiken voor verificatie en gateway

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Zie [instelling referenties en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van de referenties voor een on-premises implementatie FTP-gegevensbron.

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Eigenschappen van FTP-kopiëren activiteit type

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype. Voor een activiteit kopiëren varieert de eigenschappen van het type afhankelijk van de soorten bronnen en sinks.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 

- [Kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een activiteit kopiëren. 
