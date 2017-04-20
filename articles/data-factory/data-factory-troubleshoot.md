<properties 
    pageTitle="Problemen met Azure gegevens Factory oplossen" 
    description="Informatie over het oplossen van problemen met het gebruik van Azure gegevens Factory." 
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
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Problemen met gegevens Factory oplossen
Dit artikel bevat tips voor probleemoplossing voor problemen bij het gebruik van Azure gegevens Factory. In dit artikel niet wordt vermeld in de mogelijke problemen bij het gebruik van de service, maar deze bepaalde problemen en algemene probleemoplossing tips bedekt.   

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Fout: Het abonnement dat niet is geregistreerd als u wilt gebruiken naamruimte 'Microsoft.DataFactory'
Als u deze fout ontvangt, is niet de provider van de resource Azure gegevens Factory geregistreerd op uw computer. Ga als volgt: 

1. Azure PowerShell starten. 
2. Meld u aan bij uw Azure-account met de volgende opdracht.
        Login-AzureRmAccount 
3. Voer de volgende opdracht voor het registreren van de Azure gegevens Factory-provider.
        Register-AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Probleem: Onbevoegde fout bij het uitvoeren van een Data Factory-cmdlet
U waarschijnlijk niet de rechts Azure-account of een abonnement gebruikt met de PowerShell Azure. Gebruik de volgende cmdlets om de rechts Azure-account en een abonnement voor gebruik met de PowerShell Azure te selecteren. 

1. Login-AzureRmAccount - gebruik de juiste gebruikers-ID en wachtwoord
2. Get-AzureRmSubscription - weergave van alle abonnementen voor het account. 
3. Selecteer AzureRmSubscription &lt;de naam van abonnement&gt; -Selecteer het juiste abonnement. Gebruik hetzelfde die u kunt maken van een fabriek gegevens op de Azure-portal.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Probleem: Mislukt aan Data Management Gateway Express instellen starten via Azure-portal
De Express-instellingen voor de Data Management Gateway is Internet Explorer of een Microsoft-ClickOnce compatibele webbrowser vereist. Als de instelling Express kan niet worden gestart, voer een van de volgende: 

- Internet Explorer of een Microsoft-ClickOnce compatibele webbrowser gebruiken.

    Als u Chrome gebruikt, gaat u naar de [webwinkel Chrome](https://chrome.google.com/webstore/), zoeken met "ClickOnce" trefwoord, kies een van de ClickOnce-extensies en installeer deze. 
    
    Doe hetzelfde voor Firefox (installatie-invoegtoepassing). Klik op de knop Menu openen op de werkbalk (drie horizontale lijnen in de rechterbovenhoek), klik op invoegtoepassingen zoekopdracht met trefwoord "ClickOnce", kies een van de ClickOnce-extensies en installeren. 

- Gebruik de **Handmatige Setup** -koppeling weergegeven op het hetzelfde blad in de portal. U kunt deze methode installatiebestand downloaden en deze handmatig uitvoeren. Nadat de installatie voltooid is, kunt u zien in het dialoogvenster Data Management gatewayconfiguratie. De **sleutel** in de portal scherm kopiëren en deze in de configuratiemanager gebruiken om u te registreren handmatig de gateway bij de service.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Probleem: Geen verbinding met een lokale SQL Server 
**Data Management Gateway Configuration Manager** starten op de gatewaycomputer en het gebruik van het tabblad **Probleemoplossing** voor het testen van de verbinding met SQL Server van de gatewaycomputer. Zie [problemen oplossen met de gateway problemen](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway gerelateerde problemen.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Probleem: Invoer segmenten zijn in de wacht staat voor de perfecte

De segmenten kunnen niet **wachten** om verschillende redenen. Een van de enkele veelvoorkomende redenen is dat de eigenschap **external** niet is ingesteld op **waar**. Een gegevensset die buiten het bereik van Azure gegevens Factory is geproduceerd moet worden gemarkeerd met **externe** eigenschap. Deze eigenschap wordt aangeduid dat de gegevens en externe niet back-ups door pijplijnen binnen de fabriek gegevens. Gegevens segmenten zijn gemarkeerd als **Gereed** wanneer de gegevens beschikbaar in de desbetreffende store zijn. 

Zie het volgende voorbeeld voor het gebruik van de **externe** eigenschap. Desgewenst kunt u opgeven **externalData*** wanneer u externe op waar instelt.

Zie [gegevenssets](data-factory-create-datasets.md) artikel voor meer informatie over deze eigenschap.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

De fout oplossen, de eigenschap **external** en de sectie optioneel **externalData** toevoegen aan de JSON-definitie van de invoertabel en opnieuw maken in de tabel. 

### <a name="problem-hybrid-copy-operation-fails"></a>Probleem: Hybride kopiëren mislukt
Zie [problemen met gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor stapsgewijze instructies voor het oplossen van problemen met kopiëren vanuit een on-premises implementatie gegevensopslag met de Data Management Gateway. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Probleem: Op aanvraag HDInsight inrichting mislukt?
Wanneer u met een gekoppelde service van het type HDInsightOnDemand, moet u een linkedServiceName die naar een Azure-blobopslag verwijst opgeven. Factory-gegevensservice gebruikt deze opslag voor de opslag van Logboeken en ondersteunende bestanden voor uw cluster op aanvraag-HDInsight.  Soms inrichten van een op aanvraag HDInsight cluster mislukt met de volgende fout:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Deze fout wordt meestal geeft aan dat de locatie van het opslag-account dat is opgegeven in de linkedServiceName zich niet in dezelfde gegevens center locatie waar de inrichting van HDInsight komt. Voorbeeld: als uw gegevens factory in West VS is en de Azure opslag in Oost-VS is, het op aanvraag inrichten mislukt in West VS.

Daarnaast wordt een tweede JSON eigenschap additionalLinkedServiceNames waar mogelijk extra opslagruimte accounts worden opgegeven in op aanvraag HDInsight. Deze extra opslagruimte van de gekoppelde accounts moeten op dezelfde locatie als het cluster HDInsight of met het probleem is mislukt.

### <a name="problem-custom-net-activity-fails"></a>Probleem: Aangepaste .NET activiteit mislukt
Zie [fouten opsporen in een pijplijn met aangepaste activiteit](data-factory-use-custom-activities.md#debug-the-pipeline) voor gedetailleerde stappen. 

## <a name="use-azure-portal-to-troubleshoot"></a>Azure-portal gebruiken om op te lossen 

### <a name="using-portal-blades"></a>Gebruik van de portal bladen
Zie [Monitor pijplijn](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) voor stapsgewijze instructies. 

### <a name="using-monitor-and-manage-app"></a>Met beeldscherm en App beheren
Zie [beeldscherm en beheren van gegevens factory pijpleidingen beeldscherm en App beheren](data-factory-monitor-manage-app.md) voor meer informatie. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Azure PowerShell gebruiken om op te lossen

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Azure PowerShell gebruiken om een fout  
Zie [Monitor gegevens Factory pijpleidingen Azure PowerShell gebruiken](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) voor meer informatie. 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 