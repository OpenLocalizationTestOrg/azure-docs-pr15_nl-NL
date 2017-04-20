<properties 
    pageTitle="Bewaken en beheren van de Stream Analytics-taken met PowerShell | Microsoft Azure" 
    description="Leer hoe u via Azure PowerShell en de bijbehorende cmdlets bewaken en beheren van de Stream Analytics-taken." 
    keywords="Azure powershell, azure powershell-cmdlets, powershell-opdracht powershell-scripts" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Bewaken en beheren van de Stream Analytics-taken met Azure PowerShell-cmdlets

Leer hoe u bewaken en beheren van de Stream Analytics-resources met Azure PowerShell-cmdlets en powershell-scripts die basistaken Stream analyses uitvoeren.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Vereisten voor het uitvoeren van Azure PowerShell-cmdlets voor Stream Analytics

 - Maak een resourcegroep Azure in uw abonnement. Hierna volgt een voorbeeld Azure PowerShell-script. Zie voor informatie Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md);  

Azure PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Stream Analytics taken hebt gemaakt via programmacode hebben geen monitoring standaard ingeschakeld.  U kunt handmatig inschakelen in de Portal Azure cmdlets voor controle door te navigeren naar de pagina Monitor van de taak en te klikken op de knop inschakelen of kunt u dit via programmacode doen door de stappen die zich op [Azure Stream Analytics - Monitor Stream Analytics taken via programmacode](stream-analytics-monitor-jobs.md)bevindt.

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell-cmdlets voor Stream Analytics
De volgende Azure PowerShell-cmdlets kan worden gebruikt om te controleren en beheren van Azure Stream Analytics-taken. Houd er rekening mee dat Azure PowerShell verschillende versies heeft. 
**In de voorbeelden weergegeven de eerste opdracht is bedoeld voor Azure PowerShell 0.9.8, de tweede opdracht is bedoeld voor Azure PowerShell 1.0.** De opdrachten Azure PowerShell 1.0 hebben altijd 'AzureRM' in de opdracht.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Alle Stream Analytics taken die zijn gedefinieerd in het Azure abonnement of de opgegeven resourcegroep lijsten of taakinformatie ophalen over een bepaalde taak binnen een resourcegroep.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Deze PowerShell-opdracht geeft informatie over alle taken van de Stream analyses van de Azure-abonnement.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Deze PowerShell-opdracht retourneert informatie over alle de Stream Analytics-taken in de resourcegroep StreamAnalytics-standaard-centraal-nl.

**Voorbeeld 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Deze PowerShell-opdracht retourneert informatie over de taak Stream Analytics StreamingJob in de resourcegroep StreamAnalytics-standaard-centraal-nl.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Vindt u alle van de invoer die zijn gedefinieerd in een opgegeven Stream Analytics-taak of informatie ophalen over een bepaalde invoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Deze PowerShell-opdracht geeft informatie over alle invoer die zijn gedefinieerd in de taak StreamingJob.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Deze PowerShell-opdracht geeft informatie over de invoer met de naam EntryStream die zijn gedefinieerd in de taak StreamingJob.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
De uitvoer van die zijn gedefinieerd in een opgegeven Stream Analytics-taak alleoffice of informatie ophalen over een specifieke uitvoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Deze PowerShell-opdracht geeft informatie over de uitvoer van gedefinieerd in de taak StreamingJob.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Deze PowerShell-opdracht geeft informatie over de uitvoer uitvoer die zijn gedefinieerd in de taak StreamingJob met de naam.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Informatie over het quotum van streaming eenheden in een bepaald gebied krijgt.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Deze PowerShell-opdracht geeft informatie over de quota en het gebruik van streaming eenheden in de regio Central US.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Informatie over de gewenste transformatie gedefinieerd in een taak Stream Analytics krijgt.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Deze PowerShell-opdracht geeft informatie over de transformatie StreamingJob in de taak StreamingJob genoemd.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Nieuwe AzureStreamAnalyticsInput | Nieuwe AzureRMStreamAnalyticsInput
Hiermee maakt u een nieuwe invoer binnen een Stream Analytics-project of een bestaande opgegeven invoer wordt bijgewerkt.
  
De naam van de invoer kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, de naam op de opdrachtregel moet hetzelfde als de rij in het bestand.

Als u een invoer die al bestaat opgeven en geef geen het gedeelte-parameter dwingen de cmdlet wordt gevraagd of u al dan niet voor het vervangen van de bestaande invoer.

Als u het gedeelte-parameter afdwingen en geef een bestaande invoer naam, de invoer wordt vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de JSON bestandsstructuur en inhoud, het [Maken van de invoer (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] gedeelte van de [Stream Analytics Management REST API verwijzing bibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Deze PowerShell-opdracht maakt u een nieuwe invoer van het bestand Input.json. Als u een bestaande invoer met de naam die is opgegeven in het invoer definitiebestand al is gedefinieerd, vraagt de cmdlet al dan niet te vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Deze PowerShell-opdracht Hiermee maakt u een nieuwe invoer in de taak EntryStream genoemd. Als u een bestaande invoer met deze naam al is gedefinieerd, vraagt de cmdlet al dan niet te vervangen.

**Voorbeeld 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Deze PowerShell-opdracht vervangt de definitie van de bestaande invoerbron EntryStream aangeroepen met de definitie van het bestand.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Nieuwe AzureStreamAnalyticsJob | Nieuwe AzureRMStreamAnalyticsJob
Hiermee maakt u een nieuwe taak van de Stream Analytics in Microsoft Azure of de definitie van een bestaande opgegeven taak wordt bijgewerkt.

De naam van de taak kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, de naam op de opdrachtregel moet hetzelfde als de rij in het bestand.

Als u de taaknaam van een die al bestaat opgeven en geef geen het gedeelte-parameter dwingen de cmdlet wordt gevraagd of u al dan niet voor het vervangen van de bestaande taak.

Als u het gedeelte-parameter afdwingen en geef de taaknaam van een bestaande, de taakdefinitie wordt vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de JSON bestandsstructuur en inhoud, de [Stream Analytics-taak maken] [ msdn-rest-api-create-stream-analytics-job] gedeelte van de [Stream Analytics Management REST API verwijzing bibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Deze PowerShell-opdracht maakt u een nieuwe taak van de definitie in JobDefinition.json. Als u een bestaande taak met de naam die is opgegeven in het definitiebestand voor de taak al is gedefinieerd, vraagt de cmdlet al dan niet te vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Deze PowerShell-opdracht vervangt de taakdefinitie voor StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Nieuwe AzureStreamAnalyticsOutput | Nieuwe AzureRMStreamAnalyticsOutput
Hiermee maakt u een nieuwe uitvoer binnen een Stream Analytics-project instellen, of een bestaande uitvoer bijwerken.  

De naam van de uitvoer kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, de naam op de opdrachtregel moet hetzelfde als de rij in het bestand.

Als u een uitvoer die al bestaat opgeven en geef geen het gedeelte-parameter dwingen de cmdlet wordt gevraagd of u al dan niet voor het vervangen van de bestaande uitvoer.

Als u het gedeelte-parameter afdwingen en geef de naam van een bestaande uitvoer, wordt de uitvoer vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de JSON bestandsstructuur en inhoud, de [Uitvoer maken (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] gedeelte van de [Stream Analytics Management REST API verwijzing bibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Deze PowerShell-opdracht Hiermee maakt u een nieuwe uitvoer 'uitvoer' genoemd in de taak StreamingJob. Als u een bestaande uitvoer met deze naam al is gedefinieerd, vraagt de cmdlet al dan niet te vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Deze PowerShell-opdracht vervangt de definitie voor 'uitvoer' in de taak StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Nieuwe AzureStreamAnalyticsTransformation | Nieuwe AzureRMStreamAnalyticsTransformation
Hiermee maakt u een nieuwe transformatie binnen een Stream Analytics-project instellen, of de bestaande transformatie bijwerken.
  
De naam van de transformatie kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, de naam op de opdrachtregel moet hetzelfde als de rij in het bestand.

Als u een transformatie die al bestaat opgeven en geef geen het gedeelte-parameter dwingen de cmdlet wordt gevraagd of u al dan niet voor het vervangen van de bestaande transformatie.

Als u het gedeelte-parameter afdwingen en geef de naam van een bestaande transformatie, wordt de transformatie vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de JSON bestandsstructuur en inhoud, de [Transformatie maken (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] gedeelte van de [Stream Analytics Management REST API verwijzing bibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Deze PowerShell-opdracht Hiermee maakt u een nieuwe transformatie StreamingJobTransform in de taak StreamingJob genoemd. Als u een bestaande transformatie al is gedefinieerd met deze naam, vraagt de cmdlet al dan niet te vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Deze PowerShell-opdracht vervangt de definitie van StreamingJobTransform in de taak StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Verwijderen AzureStreamAnalyticsInput | Verwijderen AzureRMStreamAnalyticsInput
Asynchroon Hiermee verwijdert u een specifieke invoer van een taak Stream Analytics in Microsoft Azure.  
Als u het gedeelte-parameter dwingen de invoer zonder bevestiging worden verwijderd.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Deze PowerShell-opdracht verwijdert de invoer EventStream in de taak StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Verwijderen AzureStreamAnalyticsJob | Verwijderen AzureRMStreamAnalyticsJob
Hiermee verwijdert u een bepaalde taak van de Stream Analytics in Microsoft Azure asynchroon.  
Als u het gedeelte-parameter dwingen de taak wordt verwijderd zonder bevestiging.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Deze PowerShell-opdracht Hiermee verwijdert u de taak StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Verwijderen AzureStreamAnalyticsOutput | Verwijderen AzureRMStreamAnalyticsOutput
Asynchroon Hiermee verwijdert u een specifieke uitvoer van een taak Stream Analytics in Microsoft Azure.  
Als u het gedeelte-parameter dwingen de uitvoer zonder bevestiging worden verwijderd.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Deze opdracht PowerShell verwijdert de uitvoer uitvoer in de taak StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob
Asynchroon wordt geïmplementeerd en begint een Stream Analytics-taak in Microsoft Azure.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Deze PowerShell-opdracht start de taak StreamingJob met een aangepaste uitvoer begintijd ingesteld op 12 December 2012 12:12:12 UTC.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob
Asynchroon stopt een taak Stream analyses uitvoeren in Microsoft Azure en de toegewezen resources die zijn die zijn die wordt gebruikt. De definitie en metagegevens blijven beschikbaar is binnen uw abonnement via Azure portal zowel management API's, zodat de taak kan worden bewerkt en opnieuw gestart. U moet niet betalen voor een taak in gestopt.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Deze PowerShell-opdracht drukt, stopt de taak StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput
Hiermee test u de mogelijkheid van Stream Analytics verbinding maken met een opgegeven invoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Deze PowerShell-opdracht Hiermee test u de verbindingsstatus van de invoer EntryStream in StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput
Hiermee test u de mogelijkheid van Stream Analytics verbinding maken met een opgegeven uitvoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Deze opdracht PowerShell tests de verbindingsstatus van de uitvoer uitvoer in StreamingJob.  

## <a name="get-support"></a>Ondersteuning
Probeer onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor meer hulp. 


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
