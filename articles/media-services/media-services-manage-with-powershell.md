<properties 
    pageTitle="Azure Media serviceaccounts met PowerShell beheren" 
    description="Informatie over het beheren van Azure Media Services-accounts met PowerShell-cmdlets." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Azure Media serviceaccounts met PowerShell beheren

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Om te kunnen maken van een Azure Media Services-account, moet u een Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure gratis proefversie</a>voor meer informatie.

##<a name="overview"></a>Overzicht 

In dit artikel worden de Azure PowerShell-cmdlets voor Azure Media Services (AMS) in het kader resourcemanager Azure. De cmdlets bestaat niet in de naamruimte **Microsoft.Azure.Commands.Media** .

## <a name="versions"></a>Versies

**ApiVersion**: "2015-10-01"
               

## <a name="new-azurermmediaservice"></a>Nieuwe AzureRmMediaService

Hiermee maakt u een media-service.

### <a name="syntax"></a>Syntaxis

Parameter is ingesteld: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Parameter is ingesteld: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**-ResourceGroupName &lt;tekenreeks&gt;**

Geeft de naam van de resourcegroep die deze media-service hoort.

Aliassen | geen
---|---
Vereist?   |  waar
Positie?   |  0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren?  |ONWAAR

**-Accountnaam &lt;tekenreeks&gt;**

Hiermee geeft u de naam van het media-service.

Aliassen |Naam
---|---
Vereist? |waar
Positie? |1
Standaardwaarde |geen
Invoer pijplijn accepteren? |ONWAAR
Jokertekens accepteren? |ONWAAR

**-Locatie &lt;tekenreeks&gt;**

Hiermee geeft u de locatie van de resource van de media-service.

Aliassen |geen
---|---
Vereist? |waar
Positie? |2
Standaardwaarde  |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**-StorageAccountId &lt;tekenreeks&gt;**

Hiermee geeft u een primaire opslag-account dat die is gekoppeld aan de media-service.

- Nieuwe opslag-account (gemaakt met de API resourcemanager) alleen ondersteund.

- Het account opslagruimte moet bestaan en heeft dezelfde locatie met de media-service.

Aliassen |geen
---|---
Vereist? |waar
Positie? |3
Standaardwaarde  |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam instellen |StorageAccountIdParamSet
Jokertekens accepteren?|ONWAAR

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Hiermee geeft u opslag-accounts die zijn gekoppeld aan het media-service.

- Nieuwe opslag-account (gemaakt met de API resourcemanager) alleen ondersteund.

- Het account opslagruimte moet bestaan en heeft dezelfde locatie met de media-service.

- Slechts één opslag-account kan worden opgegeven als primaire.

Aliassen |geen
---|---
Vereist?  |waar
Positie?  |3
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam instellen |StorageAccountsParamSet
Jokertekens accepteren? |ONWAAR

**-Tags &lt;Hashtable&gt;**

Hiermee geeft u een hashtabel van de labels die zijn gekoppeld aan de media-service.

- Voorbeeld:@{"tag1"="value1";"tag2"=:value2"}

Aliassen |geen
---|---
Vereist?  |ONWAAR
Positie?  |met de naam
Standaardwaarde |geen
Invoer pijplijn accepteren? |ONWAAR
Jokertekens accepteren? |ONWAAR

**&lt;Opdrachtparameters&gt;**

De algemene parameters ondersteuning biedt voor deze cmdlet:-opsporen, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable ondersteund,-OutBuffer, - PipelineVariable, - uitgebreide, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u voor de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitvoer

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService

Een media-service bijwerken.

### <a name="syntax"></a>Syntaxis

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**-ResourceGroupName &lt;tekenreeks&gt;**

Geeft de naam van de resourcegroep die deze media-service hoort.

Aliassen |geen
---|---
Vereist?  |waar
Positie?  |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**-Accountnaam &lt;tekenreeks&gt;**

Hiermee geeft u de naam van het media-service.

Aliassen |Naam
---|---
Vereist? |Waar
Positie? |1
Standaardwaarde |Geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Hiermee geeft u opslag-accounts die zijn gekoppeld aan het media-service.

- Nieuwe opslag-account (gemaakt met de API resourcemanager) alleen ondersteund.

- Het account opslagruimte moet bestaan en heeft dezelfde locatie met de media-service.

- Slechts één opslag-account kan worden opgegeven als primaire.

Aliassen |geen
---|---
Vereist? |ONWAAR
Positie? |Met de naam
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam instellen |StorageAccountsParamSet
Jokertekens accepteren? |ONWAAR

**-Tags &lt;Hashtable&gt;**

Hiermee geeft u een hashtabel van de labels die zijn gekoppeld aan deze media-service.

- De labels die zijn gekoppeld aan de service media worden vervangen door de waarde die is opgegeven door de klant.

Aliassen |geen
---|---
Vereist? |ONWAAR
Positie?  |Met de naam
Standaardwaarde |Geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**&lt;Opdrachtparameters&gt;**

De algemene parameters ondersteuning biedt voor deze cmdlet:-opsporen, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable ondersteund,-OutBuffer, - PipelineVariable, - uitgebreide, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u voor de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitvoer

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="remove-azurermmediaservice"></a>Verwijderen AzureRmMediaService

Hiermee verwijdert u een media-service.

### <a name="syntax"></a>Syntaxis

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**-ResourceGroupName &lt;tekenreeks&gt;**

Geeft de naam van de resourcegroep die deze media-service hoort.

Aliassen |geen
---|---
Vereist? |waar
Positie? |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**-Accountnaam &lt;tekenreeks&gt;**

Hiermee geeft u de naam van het media-service.

Aliassen |geen
---|---
Vereist? |waar
Positie? |2
Standaardwaarde |Geen
Invoer pijplijn accepteren?  |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**&lt;Opdrachtparameters&gt;**

De algemene parameters ondersteuning biedt voor deze cmdlet:-opsporen, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable ondersteund,-OutBuffer, - PipelineVariable, - uitgebreide, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u voor de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitvoer

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService

Hiermee haalt u alle mediaservices in een resourcegroep of een media-service met een opgegeven naam.

### <a name="syntax"></a>Syntaxis

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**-ResourceGroupName &lt;tekenreeks&gt;**

Geeft de naam van de resourcegroep die deze media-service hoort.

Aliassen |geen
---|---
Vereist? |waar
Positie?  |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam instellen |ResourceGroupParameterSet, AccountNameParameterSet
Jokertekens accepteren?   ONWAAR

**-Accountnaam &lt;tekenreeks&gt;**

Hiermee geeft u de naam van het media-service.

Aliassen |geen
---|---
Vereist? |waar
Positie?  |1
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam instellen  |AccountNameParameterSet
Jokertekens accepteren? |ONWAAR

**&lt;Opdrachtparameters&gt;**

De algemene parameters ondersteuning biedt voor deze cmdlet:-opsporen, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable ondersteund,-OutBuffer, - PipelineVariable, - uitgebreide, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u voor de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitvoer

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys

Toetsen van een media-service krijgt.

### <a name="syntax"></a>Syntaxis

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**-ResourceGroupName &lt;tekenreeks&gt;**

Geeft de naam van de resourcegroep die deze media-service hoort.

Aliassen |geen
---|---
Vereist? |waar
Positie?  |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**-Accountnaam &lt;tekenreeks&gt;**

Hiermee geeft u de naam van het media-service.

Aliassen |geen
---|---
Vereist? |waar
Positie? |1
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**&lt;Opdrachtparameters&gt;**

De algemene parameters ondersteuning biedt voor deze cmdlet:-opsporen, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable ondersteund,-OutBuffer, - PipelineVariable, - uitgebreide, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u voor de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitvoer

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey

Genereert u deze opnieuw een primaire of secundaire sleutel van een media-service.

### <a name="syntax"></a>Syntaxis

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**-ResourceGroupName &lt;tekenreeks&gt;**

Geeft de naam van de resourcegroep die deze media-service hoort.

Aliassen |geen
---|---
Vereist?  |waar
Positie?  |0
Standaardwaarde |geen
Invoer pijplijn accepteren?  |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**-Accountnaam &lt;tekenreeks&gt;**

Hiermee geeft u de naam van het media-service.

Aliassen |geen
---|---
Vereist? |waar
Positie?  |1
Standaardwaarde |geen
Invoer pijplijn accepteren?   |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**-KeyType &lt;KeyType&gt;**

Hiermee geeft u het type sleutel van de media-service.

- Primaire of secundaire

Aliassen |geen
---|---
Vereist?  |waar
Positie?  |2
Standaardwaarde |geen
Invoer pijplijn accepteren? |ONWAAR
Jokertekens accepteren? |ONWAAR

**&lt;Opdrachtparameters&gt;**

De algemene parameters ondersteuning biedt voor deze cmdlet:-opsporen, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable ondersteund,-OutBuffer, - PipelineVariable, - uitgebreide, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u voor de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitvoer

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="sync-azurermmediaservicestoragekeys"></a>Synchronisatie-AzureRmMediaServiceStorageKeys

Synchroniseert opslag account sleutels voor een opslag-account dat is gekoppeld aan de media-service.

### <a name="syntax"></a>Syntaxis

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**-ResourceGroupName &lt;tekenreeks&gt;**

Geeft de naam van de resourcegroep die deze media-service hoort.

Aliassen |geen
---|---
Vereist? |waar
Positie? |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**-Accountnaam &lt;tekenreeks&gt;**

Hiermee geeft u de naam van het media-service.

Aliassen |geen
---|---
Vereist? |waar
Positie? |1
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**-StorageAccountId &lt;tekenreeks&gt;**

Hiermee geeft u het opslag-account dat is gekoppeld aan de media-service.

Aliassen |ID
---|---
Vereist? |waar
Positie?  |2
Standaardwaarde |geen
Invoer pijplijn accepteren? |      True(ByPropertyName)
Jokertekens accepteren? |ONWAAR

**&lt;Opdrachtparameters&gt;**

De algemene parameters ondersteuning biedt voor deze cmdlet:-opsporen, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable ondersteund,-OutBuffer, - PipelineVariable, - uitgebreide, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u voor de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitvoer

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="next-step"></a>Volgende stap 

Bekijk de Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
