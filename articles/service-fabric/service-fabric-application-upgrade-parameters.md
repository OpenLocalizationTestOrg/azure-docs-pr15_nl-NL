
<properties
   pageTitle="Upgrade van toepassing: upgraden parameters | Microsoft Azure"
   description="Beschrijving van parameters die zijn gerelateerd aan het bijwerken van een Service stof-toepassing, waaronder systeemstatus controles uit te voeren en beleid om automatisch de upgrade ongedaan te maken."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>



# <a name="application-upgrade-parameters"></a>Parameters voor het bijwerken van toepassing

In dit artikel worden de verschillende parameters die tijdens de upgrade van een toepassing Azure-Service stof toepassen. De parameters omvatten de naam en de versie van de toepassing. Ze zijn knoppen waarmee de time-outs en systeemstatus controles die worden toegepast tijdens de upgrade en ze verwijzen naar de beleidsregels die moeten worden toegepast wanneer u een upgrade mislukt.


<br>

| Parameter | Beschrijving |
| --- | --- |
| ApplicationName | De naam van de toepassing die wordt bijgewerkt. Voorbeelden: stof: / VisualObjects, stof: / ClusterMonitor  |
| TargetApplicationTypeVersion | De versie van de toepassing typt die de upgrade doelen. |
| FailureAction | De actie die u hebt gemaakt door de Service stof wanneer de upgrade is mislukt. De toepassing kan worden teruggezet naar de update van de oude versie (terugdraaien) of de upgrade kan worden gestopt in het huidige upgrade domein. De upgrade-modus wordt ook in dat geval wordt gewijzigd op handmatig. Toegestane waarden zijn terugdraaien en handmatig. |
| HealthCheckWaitDurationSec | De tijd om te wachten (in seconden) nadat de upgrade is voltooid in het domein aan de upgrade voordat Service stof evalueert de status van de toepassing. Deze duur kan ook worden beschouwd als de tijd die een toepassing moet worden uitgevoerd voordat deze kan worden beschouwd als orde. Als de statuscontrole worden doorgegeven, is het upgradeproces gaat door naar het volgende upgrade domein.  Als de statuscontrole mislukt, wordt de status van Service stof wacht op een interval (de UpgradeHealthCheckInterval) voordat opnieuw de statuscontrole opnieuw wordt geprobeerd totdat de HealthCheckRetryTimeout is bereikt. De standaard- en aanbevolen waarde is dan 0 seconden. |
| HealthCheckRetryTimeoutSec | De duur (in seconden) blijft die Service stof systeemstatus evaluatie vóór de upgrade declareren als mislukt uitvoeren. De standaardinstelling is 600 seconden. Deze duur wordt gestart nadat HealthCheckWaitDuration is bereikt. In dit HealthCheckRetryTimeout, mogelijk Service stof meerdere systeemstatus controles van de toepassingsstatus uitvoeren. De standaardwaarde is 10 minuten en correct voor uw toepassing moet worden aangepast. |
| HealthCheckStableDurationSec | De duur (in seconden) om te controleren of de toepassing stabiele voordat u verplaatsen naar het volgende upgrade domein of voltooiing van de upgrade is. De duur van deze wachten wordt gebruikt om te voorkomen dat niet-gevonden wijzigingen van de juiste status nadat de statuscontrole is uitgevoerd. De standaardwaarde is 120 seconden en correct voor uw toepassing moet worden aangepast. |
| UpgradeDomainTimeoutSec | Maximale tijd (in seconden) voor het upgraden van één upgrade domein. Als de time-outwaarde is bereikt, wordt de upgrade stopt en verloopt op basis van de instelling voor UpgradeFailureAction. De standaardwaarde is nooit (oneindig) en correct voor uw toepassing moet worden aangepast. |
| UpgradeTimeout | Een time-out (in seconden) die van toepassing voor de hele upgrade is. Als de time-outwaarde is bereikt, worden de upgrade stopt en UpgradeFailureAction wordt geactiveerd. De standaardwaarde is nooit (oneindig) en correct voor uw toepassing moet worden aangepast. |
| UpgradeHealthCheckInterval | De frequentie dat de status is ingeschakeld. Deze parameter is opgegeven in de sectie ClusterManager van de _cluster_ _bestandenlijst_en niet is opgegeven als onderdeel van de upgrade cmdlet. De standaardwaarde is 60 seconden.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Evaluatie van de service systeemstatus tijdens de upgrade van toepassing

<br>
De status van evaluatiecriteria zijn optioneel. Als u de criteria van de evaluatie status niet opgeeft wanneer een upgrade wordt gestart, gebruikt Service stof opgegeven in de ApplicationManifest.xml van het toepassingsexemplaar beleidsregels voor de status van de toepassingen.


<br>

| Parameter | Beschrijving |
| --- | --- |
| ConsiderWarningAsError | Standaardwaarde is ONWAAR. De status waarschuwingen voor de toepassing als fouten behandelen om te bepalen of de status van de toepassing tijdens de upgrade. Standaard geeft Service stof geen waarschuwing systeemstatus gebeurtenissen worden fouten (fouten), zodat de upgrade kunt zelfs als er waarschuwingen resultaat.   |
| MaxPercentUnhealthyDeployedApplications | Standaard en aanbevolen waarde is aan 0. Geef het maximum aantal gebruikte toepassingen (Zie de [sectie status](service-fabric-health-introduction.md)) dat beschadigd worden kan voordat de toepassing wordt beschouwd als beschadigd en de upgrade mislukt. Deze parameter definieert de toepassingsstatus op het knooppunt en helpt problemen opsporen tijdens de upgrade. Meestal krijgen de kopieën van de toepassing taakverdeling naar het andere knooppunt, waarmee de toepassing wilt weergeven in orde, zodat de upgrade om verder te gaan. Door het opgeven van de status van een strikte MaxPercentUnhealthyDeployedApplications, kan Service stof snel een probleem met de toepassingspakket detecteren en te maken van een fail snel aan de upgrade. |
| MaxPercentUnhealthyServices | Standaard en aanbevolen waarde is aan 0. Geef het maximum aantal services in een exemplaar van de toepassing die beschadigd worden kan voordat de toepassing wordt beschouwd als beschadigd en de upgrade mislukt. |
| MaxPercentUnhealthyPartitionsPerService | Standaard en aanbevolen waarde is aan 0. Geef het maximum aantal partities in een service die beschadigd worden kan voordat de service wordt beschouwd als beschadigd. |
| MaxPercentUnhealthyReplicasPerPartition | Standaard en aanbevolen waarde is aan 0. Geef het maximum aantal replica's in partition dat beschadigd worden kan voordat de partition als beschadigd wordt beschouwd. |
| UpgradeReplicaSetCheckTimeout | **Stateless service**--binnen één upgrade domein, Service stof wordt geprobeerd om ervoor te zorgen dat extra exemplaren van de service beschikbaar zijn. Als het doel exemplaar count meer dan één, wacht Service stof meer dan één exemplaar kan worden gecontroleerd, tot een maximum time-outwaarde. Deze time-out is opgegeven met behulp van de eigenschap UpgradeReplicaSetCheckTimeout. Als de time-out is verlopen, gaat Service stof door met de upgrade, ongeacht het aantal exemplaren van de service. Als het aantal doel-exemplaar een is, Service stof niet wacht en direct gaat door met de upgrade. **Statuscontrole-service**--binnen één upgrade domein, Service stof wordt geprobeerd om ervoor te zorgen dat de replicaset een quorum heeft. Service stof wacht op een quorum kan worden gecontroleerd, tot een maximum-outwaarde voor (opgegeven door de eigenschap UpgradeReplicaSetCheckTimeout). Als de time-out is verlopen, gaat Service stof door met de upgrade, ongeacht quorum. Deze instelling is ingesteld als nooit (oneindig) wanneer doorsturen schuivend, en 900 seconden wanneer terugdraaien. |
| ForceRestart | Als u een configuratie of gegevenspakket zonder het bijwerken van de servicecode bijwerkt, de service is gestart alleen als de eigenschap ForceRestart is ingesteld op waar. Wanneer het bijwerken voltooid is, krijgt Service stof de service dat een nieuwe configuratie of gegevenspakket beschikbaar is. De service is verantwoordelijk voor het toepassen van de wijzigingen. Zo nodig kunt zelf door de service opnieuw. |



<br>
<br>
De criteria MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService en MaxPercentUnhealthyReplicasPerPartition kunnen per servicetype voor een toepassingsexemplaar worden opgegeven. Instelling van deze parameters per-service kan voor een toepassing aan het typen van de verschillende services met verschillende evaluatie beleidsregels bevatten. Een type stateless gateway-service kan bijvoorbeeld een MaxPercentUnhealthyPartitionsPerService die verschilt van een servicetype statuscontrole-engine voor een bepaalde toepassingsexemplaar hebben.

## <a name="next-steps"></a>Volgende stappen

[Een upgrade van uw toepassing gebruik Visual Studio](service-fabric-application-upgrade-tutorial.md) , wordt u begeleidt bij de upgrade van een toepassing gebruik van Visual Studio.

[Een upgrade van uw Powershell voor het gebruiken van toepassing](service-fabric-application-upgrade-tutorial-powershell.md) , wordt u begeleidt bij de upgrade van een toepassing via PowerShell.

Uw toepassingsupgrades compatibel maken door te leren hoe u [Gegevens serialisatie](service-fabric-application-upgrade-data-serialization.md)gebruiken.

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [Geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in de [Toepassingsupgrades probleemoplossing](service-fabric-application-upgrade-troubleshooting.md).
 
