<properties
   pageTitle="De upgrade van een toepassing voor de Service stof configureren | Microsoft Azure"
   description="Leer hoe u de instellingen voor het upgraden van een toepassing voor de Service stof met behulp van Microsoft Visual Studio configureren."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>De upgrade van een toepassing voor de Service stof configureren in Visual Studio

Visual Studio tools voor Azure-Service stof bieden upgrade ondersteuning voor publicatie voor lokale of externe clusters. Er zijn twee voordelen voor de upgrade van uw toepassing naar een nieuwere versie in plaats van het vervangen van de toepassing tijdens testen en problemen oplossen:

- Toepassingsgegevens niet verloren tijdens de upgrade.
- Beschikbaarheid blijft hoog zodat er niet een serviceonderbreking tijdens de upgrade als er dat voldoende service-exemplaren verdeeld over de upgrade domeinen.

Tests kunnen worden uitgevoerd ten opzichte van een toepassing terwijl deze wordt bijgewerkt.

## <a name="parameters-needed-to-upgrade"></a>Parameters die nodig zijn om te upgraden

U kunt kiezen uit twee soorten implementatie: normaal of de upgrade. Een normale implementatie wist u alle vorige implementatie-informatie en de gegevens op het cluster, terwijl deze en een upgrade implementatie blijft behouden. Wanneer u een upgrade uitvoert van een toepassing Service stof in Visual Studio, moet u parameters voor het bijwerken van toepassing en servicestatus raadpleegt u beleid bieden. Parameters voor het bijwerken van toepassing helpen bepalen van de upgrade, terwijl het beleid voor het controleren van de servicestatus bepalen of de upgrade voltooid is. Zie [Service stof toepassing upgrade: parameters upgraden](service-fabric-application-upgrade-parameters.md) voor meer informatie.

Er zijn drie upgrade modi: *gecontroleerde*, *UnmonitoredAuto*en *UnmonitoredManual*.

  - Een upgrade gecontroleerde automatiseert de upgrade en statuscontrole van de toepassing.

  - Een upgrade UnmonitoredAuto automatiseert de upgrade, maar Hiermee worden overgeslagen de statuscontrole van de toepassing.

  - Wanneer u een upgrade UnmonitoredManual doet, moet u handmatig upgraden elk domein de upgrade.

Elke upgrade-modus is vereist voor verschillende sets met parameters. Zie de [toepassing parameters upgraden](service-fabric-application-upgrade-parameters.md) voor meer informatie over de beschikbare opties voor de upgrade.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Upgraden van een toepassing Service stof in Visual Studio

Als u de hulpmiddelen voor Visual Studio Service stof upgrade van een toepassing voor de Service stof gebruikt, kunt u een proces publiceren als u een upgrade in plaats van een gewone implementatie door het selectievakje **upgraden van de toepassing** in te schakelen.

### <a name="to-configure-the-upgrade-parameters"></a>Voor het configureren van de parameters voor het bijwerken

1. Klik op de knop **Instellingen** naast het selectievakje. Het dialoogvenster **Upgrade Parameters bewerken** wordt weergegeven. Het dialoogvenster **Upgrade Parameters bewerken** ondersteunt de upgrade modi gecontroleerde, UnmonitoredAuto en UnmonitoredManual.

2. Selecteer de upgrade modus die u wilt gebruiken en geef vervolgens de parameter raster.

    Elke parameter heeft standaardwaarden. De optionele parameter *DefaultServiceTypeHealthPolicy* haalt de invoer van een hash-tabel. Hier volgt een voorbeeld van de invoer tabelindeling hash voor *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* is een andere optionele parameter waarmee een hash Tabelinvoer in de volgende indeling:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Hier volgt een voorbeeld van de praktijk:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Als u de upgrade modus UnmonitoredManual selecteert, moet u handmatig een PowerShell-console als u wilt hervatten en klaar bent met het upgradeproces starten. Verwijzen naar [Service stof toepassing upgrade: geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md) voor meer informatie over hoe handmatige upgrade werkt.

## <a name="upgrade-an-application-by-using-powershell"></a>Upgraden van een toepassing via PowerShell

Upgrade van een toepassing voor de Service stof kunt u PowerShell-cmdlets. Zie [Service stof toepassing zelfstudie upgraden](service-fabric-application-upgrade-tutorial.md) en [Begin-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) voor gedetailleerde informatie.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Geeft u het selectievakje beleid in de toepassing manifest-bestand

Elke service in een toepassing voor de Service stof kunt een eigen systeemstatus beleidsparameters die de standaardwaarden overschrijven hebben. U kunt deze parameterwaarden in een bestand van de toepassing manifest opgeeft.

Het volgende voorbeeld ziet u hoe u een beleid voor het controleren van unieke servicestatus voor elke service in manifest van de toepassing toepast.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het implementeren van een toepassing [Deploy een bestaande toepassing in Azure Service stof](service-fabric-deploy-existing-app.md).
