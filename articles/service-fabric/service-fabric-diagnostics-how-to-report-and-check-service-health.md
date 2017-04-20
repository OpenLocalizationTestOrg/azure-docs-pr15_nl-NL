<properties
   pageTitle="Report en controleren met Azure-Service stof | Microsoft Azure"
   description="Leer hoe u statusrapporten verzenden vanuit uw servicecode en hoe u de status van uw service controleren met behulp van de servicestatus controleren hulpmiddelen stof van Azure-Service."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Report en servicestatus controleren
Wanneer uw services problemen ondervindt, is de mogelijkheid om te beantwoorden en het oplossen van incidenten en bijvoorbeeld afhankelijk van de mogelijkheid om te bepalen de problemen snel. Als u problemen en fouten bij de manager van de servicestatus Azure-Service stof vanuit uw servicecode, kunt u standaard servicestatus bewaken hulpmiddelen die Service stof biedt om de status van de servicestatus te controleren.

Er zijn twee manieren dat u de status van de service kunt melden:

- [Partition](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) of [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) -objecten gebruiken.  
U kunt de `Partition` en `CodePackageActivationContext` objecten op het rapporteren van de status van elementen die deel uitmaken van de huidige context. Code die wordt uitgevoerd als onderdeel van een replica kunt bijvoorbeeld systeemstatus melden alleen op deze, de partition waartoe het behoort en de die dit onderdeel van is toepassing.

- Gebruik `FabricClient`.   
U kunt `FabricClient` naar rapport status van de servicecode als het cluster niet is [beveiligd](service-fabric-cluster-security.md) of als de service wordt uitgevoerd met beheerdersbevoegdheden. Dit is niet waar in de meeste echte-scenario's. Met `FabricClient`, u kunt een entiteit die een deel uitmaakt van het cluster status rapporteren. In het ideale geval echter moet servicecode alleen verzenden rapporten die zijn gerelateerd aan een eigen status.

In dit artikel begeleidt u bij een voorbeeld van de status van de servicecode-rapporten. Het voorbeeld ziet ook hoe de Service stof hulpmiddelen kunnen worden gebruikt om te controleren van de status. In dit artikel is bedoeld om te worden van een snelle introductie over de controlefuncties van Service stof. U kunt de reeks uitgebreide artikelen over gezondheid die met de koppeling aan het einde van dit artikel beginnen lezen voor meer informatie.

## <a name="prerequisites"></a>Vereisten voor
U moet de volgende programma's hebben:

   * Visual Studio-2015
   * Service stof SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Een lokale secure ontwikkelaar cluster maken
- Open PowerShell met beheerdersbevoegdheden en voer de volgende opdrachten.

![Opdrachten die het maken van een beveiligde ontwikkelaar cluster weergeven](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Een toepassing implementeren en de status controleren

1. Open Visual Studio als beheerder.

2. Een project maken met behulp van de sjabloon **Stateful-Service** .

    ![Een Service stof-toepassing bouwen met Stateful-Service](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Druk op **F5** om uit te voeren van de toepassing in de foutopsporingsmodus voor. De toepassing wordt geïmplementeerd in de lokale cluster.

4. Nadat de toepassing wordt uitgevoerd, met de rechtermuisknop op het pictogram Lokaal Cluster Manager in het systeemvak en selecteer **Lokale Cluster beheren** in het snelmenu Service stof Explorer openen.

    ![Service stof Explorer openen vanuit systeemvak](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. De toepassingsstatus moet worden weergegeven zoals in deze afbeelding. Op dit moment kan zijn de toepassing zonder fouten in orde.

    ![Orde toepassing in de Service stof Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. U kunt ook de status controleren via PowerShell. U kunt ```Get-ServiceFabricApplicationHealth``` om te controleren van een toepassing gezondheid, en u de beschikking over ```Get-ServiceFabricServiceHealth``` van een service status controleren. Het rapport servicestatus voor dezelfde toepassing in PowerShell is in deze afbeelding.

    ![Orde-toepassing in PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Aangepaste status gebeurtenissen toevoegen aan uw servicecode
De Service stof projectsjablonen in Visual Studio bevatten steekproef code. In de volgende stappen wordt uitgelegd hoe kunt u aangepaste status gebeurtenissen melden vanuit uw servicecode. Deze rapporten wordt automatisch weergegeven in de standaardprogramma's voor statuscontrole biedt Service stof, zoals Service stof Explorer, Azure portal systeemstatus weergave en PowerShell.

1. Open de toepassing die u eerder hebt gemaakt in Visual Studio opnieuw of een nieuwe toepassing maken met behulp van de **Service Stateful** Visual Studio-sjabloon.

2. Open het bestand Stateful1.cs en zoek de `myDictionary.TryGetValueAsync` belt u in de `RunAsync` methode. U kunt zien dat deze methode geeft als resultaat een `result` die bevat de huidige waarde van de teller aangezien de belangrijkste logica in deze toepassing is een telling uitgevoerd. Als dit een echte toepassing zijn, en het gebrek aan resultaat dat wordt voorgesteld een fout, wilt u markeren van die gebeurtenis.

3. Voeg de volgende stappen uit als u wilt rapporteren van een gebeurtenis systeemstatus wanneer het gebrek aan resultaat duidt op een fout.

    een. Toevoegen de `System.Fabric.Health` naamruimte naar het bestand Stateful1.cs.

    ```csharp
    using System.Fabric.Health;
    ```

    b. Voeg de volgende code na de `myDictionary.TryGetValueAsync` bellen

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    We rapporteren replica status omdat deze met een statuscontrole-service wordt gerapporteerd. De `HealthInformation` parameter bevat informatie over het probleem met de status die wordt gerapporteerd.

    Als u een stateless service hebt gemaakt, gebruikt u de volgende code

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Als uw service wordt uitgevoerd met beheerdersbevoegdheden of als het cluster niet [beveiligd is](service-fabric-cluster-security.md), kunt u ook gebruiken `FabricClient` voor rapport gezondheid zoals wordt weergegeven in de volgende stappen uit.  

    een. Maak de `FabricClient` exemplaar na de `var myDictionary` declaratie.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    b. Voeg de volgende code na de `myDictionary.TryGetValueAsync` bellen.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Laten we deze fout simuleren en deze worden weergegeven in de servicestatus controleren hulpmiddelen te zien. Opmerking deze de vorm van de fout, uit de eerste regel in de status rapportage-code die u eerder hebt toegevoegd. Nadat u opmerkingen uit de eerste regel toevoegen, eruitziet de code in het volgende voorbeeld.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Deze code wordt nu uitgevoerd voor deze statusrapport telkens wanneer `RunAsync` wordt uitgevoerd. Nadat u de wijziging aanbrengt, drukt u op **F5** om de toepassing te starten.

6. Nadat de toepassing wordt uitgevoerd, opent u de Service stof Explorer als u wilt controleren van de status van de toepassing. Schakel ditmaal wordt Service stof Explorer weergegeven dat de toepassing beschadigd is. Dit is vanwege de fout die is gerapporteerd van de code die we eerder hebt toegevoegd.

    ![Beschadigde groep in de Service stof Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Als u de primaire replica in de boomstructuur van Service stof Explorer selecteert, ziet u dat **Status** duidt op een fout, ook. Service stof Explorer wordt ook weergegeven details van het rapport status, die zijn toegevoegd aan de `HealthInformation` parameter in de code. Hier ziet u de dezelfde statusrapporten in PowerShell en de Azure-portal.

    ![Replica servicestatus in Service stof Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Dit rapport blijft in de systeemstatus manager totdat deze wordt vervangen door een ander rapport of totdat deze replica wordt verwijderd. Omdat we niet hebt ingesteld `TimeToLive` voor dit statusrapport in de `HealthInformation` object aan, het rapport nooit verloopt.

Het is raadzaam dat status op het meest gedetailleerde niveau, namelijk in dit geval de replica moet worden gerapporteerd. U kunt ook rapporteren systeemstatus `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Rapport gezondheid op `Application`, `DeployedApplication`, en `DeployedServicePackage`, gebruikt u `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Volgende stappen
[Uitgebreide Kennismaking over status van Service stof](service-fabric-health-introduction.md)
