<properties
   pageTitle="Problemen oplossen toepassingsupgrades | Microsoft Azure"
   description="In dit artikel worden enkele veelvoorkomende problemen rond het upgraden van een toepassing voor de Service stof en hoe u ze kunt oplossen."
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

# <a name="troubleshoot-application-upgrades"></a>Problemen met de toepassingsupgrades

In dit artikel worden enkele veelvoorkomende problemen rond het upgraden van een toepassing Azure-Service stof en hoe u ze kunt oplossen.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Problemen met een upgrade mislukte toepassing

Wanneer u een upgrade mislukt, wordt in de uitvoer van de opdracht **Get-ServiceFabricApplicationUpgrade** aanvullende informatie voor foutopsporing van de fout bevat.  De volgende lijst ziet hoe de aanvullende informatie kan worden gebruikt:

1. Wordt aangegeven welk type is mislukt.
2. Geef aan dat de reden is mislukt.
3. Isoleren een of meer mislukte onderdelen om verder te onderzoeken.

Deze gegevens zijn toegankelijk wanneer Service stof vastgesteld de fout waarbij het niet uitmaakt of de **FailureAction** voor terugdraaien of wachtstand voor de upgrade.

### <a name="identify-the-failure-type"></a>Wordt aangegeven welk type is mislukt

In de uitvoer van **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** geeft aan wat de tijdstempel (in UTC) waarop een upgrade fout gevonden Service stof en **FailureAction** is geactiveerd. **FailureReason** geeft aan wat een van de drie op hoog niveau mogelijke oorzaken van de fout:

1. UpgradeDomainTimeout - geeft aan dat een bepaald upgrade domein duurde te lang is om te voltooien en **UpgradeDomainTimeout** verlopen.
2. OverallUpgradeTimeout - geeft aan dat de algehele upgrade duurt te lang voltooien voordat **UpgradeTimeout** verlopen.
3. Health Check - geeft aan dat na een upgrade van een update-domein, de toepassing bleef beschadigd op basis van het beleid opgegeven gezondheid en **HealthCheckRetryTimeout** verlopen.

Deze vermeldingen alleen weergegeven in de uitvoer wanneer de upgrade is mislukt en weer verdergaat terugdraaien. Meer informatie is afhankelijk van het type van de fout weergegeven.

### <a name="investigate-upgrade-timeouts"></a>Upgrade-outs onderzoeken

Time-out fouten meestal door problemen met de service beschikbaarheid veroorzaakt worden een upgrade uitvoeren. De uitvoer hieronder wordt gebruikt voor de upgrades waar service replica's of exemplaren niet worden gestart in de nieuwe versie van de code. Het veld **UpgradeDomainProgressAtFailure** wordt vastgelegd een momentopname van een in behandeling aan de upgrade werk op het moment van is mislukt.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

In dit voorbeeld de upgrade is mislukt voor de upgrade domein *MYUD1* en twee partities (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* en *4b43f4d8-b26b-424e-9307-7a7a62e79750*) zijn vastgelopen. De partities zijn hangen omdat de runtime is geen op doelknooppunten *Knooppunt1* en *Knooppunt4*om primaire replica's (*WaitForPrimaryPlacement*).

De opdracht **Get-ServiceFabricNode** kan worden gebruikt om te bevestigen dat deze twee knooppunten in de upgrade domein *MYUD1 zijn*. De *UpgradePhase* zegt *PostUpgradeSafetyCheck*, wat betekent dat deze controles veiligheid optreden nadat alle knooppunten in het domein dat de upgrade systeem is bijgewerkt. Al deze informatie verwijst naar eventuele problemen met de nieuwe versie van de toepassingscode. De meest voorkomende problemen zijn service fouten in het openen of promoveren naar primaire codepaden.

Een *UpgradePhase* van *PreUpgradeSafetyCheck* betekent dat er zijn problemen met het voorbereiden van het domein dat de upgrade voordat deze is uitgevoerd. De meest voorkomende problemen zijn in dit geval service fouten in de sluiten of degraderen vanaf primaire codepaden.

De huidige **UpgradeState** is *RollingBackCompleted*, dus de oorspronkelijke upgrade moet zijn uitgevoerd met een terugdraaien **FailureAction**, die automatisch ongedaan de upgrade bij een fout. Als het oorspronkelijke upgrade is uitgevoerd met een handmatige **FailureAction**, klikt u vervolgens de upgrade zou in plaats daarvan worden in een onderbroken staat toe te staan dat live voor foutopsporing in van de toepassing.

### <a name="investigate-health-check-failures"></a>Systeemstatus controleren fouten onderzoeken

Systeemstatus controleren fouten kunnen worden veroorzaakt door verschillende problemen die optreden kunnen nadat alle knooppunten in een upgrade-domein hebt opwaardeert, en alle veiligheid controles doorgeven. De uitvoer hieronder is van een upgrade mislukt vanwege mislukte systeemstatus controles typische. Het veld **UnhealthyEvaluations** wordt vastgelegd een momentopname van de servicestatus controles die is op het moment van de upgrade op basis van het opgegeven [beleid](service-fabric-health-introduction.md)is mislukt.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Wordt onderzocht systeemstatus controleren fouten vereist inzicht in het model van de servicestatus Service stof eerst. Maar zelfs zonder een uitgebreidere lidmaatschap, kunnen we zien dat twee services beschadigd zijn: *stof: / DemoApp/Svc3* en *stof: / DemoApp/Svc2*, samen met de fout statusrapporten ('InjectedFault' in dit geval). In dit voorbeeld zijn twee vier services beschadigd en wat is lager dan het doel van de standaard van 0% beschadigd (*MaxPercentUnhealthyServices*).

De upgrade is geschorst na mislukte door op te geven van een **FailureAction** van handmatig bij het starten van de upgrade. In deze modus kunt wij de live-mailsysteem in de status mislukt onderzoeken voordat u een andere actie.

### <a name="recover-from-a-suspended-upgrade"></a>Terughalen uit een geschorst upgrade

Met een terugdraaien **FailureAction**, moet u er geen herstel sinds de upgrade automatisch ongedaan gemaakt na het mislukte nodig is. Met een handmatige **FailureAction**, zijn er verschillende herstelopties:

1. Handmatig een terugdraaien activeren
2. Volg de stappen in de rest van de upgrade handmatig
3. De gecontroleerde upgrade hervatten

De opdracht **Start-ServiceFabricApplicationRollback** kan worden gebruikt op elk gewenst moment start de toepassing terugdraaien. Nadat de opdracht het resultaat is, wordt het verzoek ongedaan maken in het systeem is geregistreerd en wordt kort daarna wordt gestart.

De opdracht **Cv-ServiceFabricApplicationUpgrade** kan worden gebruikt om handmatig, gaat u verder met de rest van de upgrade één upgrade domein tegelijk. In deze modus, worden alleen veiligheid controles uitgevoerd door het systeem. Niet meer worden uitgevoerd. Deze opdracht kan alleen worden gebruikt wanneer de *UpgradeState* *RollingForwardPending*, wat betekent toont dat het huidige domein voor de upgrade is voltooid, een upgrade uitvoert, maar de volgende wijziging niet heeft gestart (in behandeling).

De opdracht **Update-ServiceFabricApplicationUpgrade** kan worden gebruikt om de gecontroleerde upgrade met beide safety hervatten en controleren wordt uitgevoerd.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

De upgrade blijft uit de upgrade domein waar de onderbroken is en het gebruik dezelfde parameters en beleid van de servicestatus als voordat upgraden. Indien nodig kunnen een van de parameters voor het bijwerken en de servicestatus beleidsregels weergegeven in de voorgaande uitvoer worden gewijzigd in dezelfde opdracht als de upgrade wordt gehaald. In dit voorbeeld is de upgrade hervat in de modus voor gecontroleerde, met de parameters en de systeemstatus-beleid ongewijzigd.

## <a name="further-troubleshooting"></a>Problemen op te lossen

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Service stof is geen Volgers van het beleid opgegeven systeemstatus

Mogelijke oorzaak 1:

Service stof equivalent van alle percentages in het werkelijke aantal entiteiten (bijvoorbeeld replica's, partities en services) voor de evaluatie van de servicestatus en altijd wordt afgerond naar hele entiteiten. Bijvoorbeeld als de maximale _MaxPercentUnhealthyReplicasPerPartition_ 21% is en er vijf kopieën zijn, waarna Service stof kunt maximaal twee beschadigd replica's (die is,'Math.Ceiling (5\*0.21)). Servicestatus beleid moeten dus dienovereenkomstig worden ingesteld.

Mogelijke oorzaak 2:

Servicestatus beleidsregels zijn opgegeven in percentages van totale services en niet specifieke service-exemplaren. Bijvoorbeeld, voordat u een upgrade, als een toepassing vier heeft service-exemplaren A, B, C en D, waarin service D beschadigd is, maar met weinig gevolgen voor de toepassing. Willen we de bekende beschadigd service D negeren tijdens de upgrade en de parameter *MaxPercentUnhealthyServices* 25%, ervan uitgaande dat er alleen A, B en C moet zijn in orde instellen.

Tijdens de upgrade D mogelijk worden echter orde terwijl C beschadigd raakt. De upgrade zou nog steeds mislukt omdat slechts 25% van de services beschadigd zijn. Echter, kan dit resulteren in onverwachte fouten vanwege C wordt onverwacht afgesloten beschadigd in plaats van d te drukken. In dit geval D moet worden gezien als een andere servicetype in A, B en C. Aangezien systeemstatus beleidsregels zijn opgegeven per servicetype, kunnen andere beschadigd percentage drempelwaarden worden toegepast op verschillende services. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Ik het beleid voor de upgrade van de toepassing niet hebt opgegeven, maar de upgrade nog steeds niet voor sommige time-outs die ik heb opgegeven

Wanneer de statusbeleidsregels niet worden doorgegeven aan de upgrade aanvraag, worden ze overgenomen van de *ApplicationManifest.xml* van de versie van de huidige toepassing. Bijvoorbeeld als u een toepassing X van versie 1.0 naar versie 2.0, beleidsregels voor toepassingen servicestatus upgrade uitvoert voor opgegeven in versie 1.0 worden gebruikt. Als u een ander beleid moet worden gebruikt voor de upgrade, moet het beleid worden opgegeven als onderdeel van de upgrade API-aanroep van toepassing. Het beleid dat is opgegeven als onderdeel van het gesprek API zijn alleen van toepassing tijdens de upgrade. Wanneer de upgrade voltooid is, wordt het beleid dat is opgegeven in de *ApplicationManifest.xml* worden gebruikt.

### <a name="incorrect-time-outs-are-specified"></a>Onjuiste time-outs zijn opgegeven

U mogelijk hebt afgevraagd wat gebeurt er wanneer time-outs inconsistent zijn ingesteld. U wellicht bijvoorbeeld een *UpgradeTimeout* die kleiner is dan de *UpgradeDomainTimeout*. Het antwoord is wordt een fout geretourneerd. Fouten worden geretourneerd als de *UpgradeDomainTimeout* kleiner is dan de som van *HealthCheckWaitDuration* en *HealthCheckRetryTimeout*of als *UpgradeDomainTimeout* kleiner is dan de som van *HealthCheckWaitDuration* en *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>Mijn upgrades duurt lang

Het tijdstip voor een upgrade om uit te voeren, is afhankelijk van de servicestatus controles en time-outs die zijn opgegeven. Controles voor status en time-outs is afhankelijk van de hoe lang het duurt om te kopiëren en de toepassing stabiliseren implementeren. Wordt ook agressieve met time-outs mogelijk meer mislukte upgrades, gemiddelde, zodat u het beste eerst voorzichtig met langer time-outs.

Hier volgt een snelle geheugensteun op hoe de time-outs ermee aan de upgrade tijden:

Upgrades voor een upgrade domein sneller dan *HealthCheckWaitDuration*kan niet voltooid + *HealthCheckStableDuration*.

Kan geen sneller dan *HealthCheckWaitDuration*worden uitgevoerd door de upgrade is mislukt + *HealthCheckRetryTimeout*.

De upgrade tijd voor een domein dat de upgrade wordt beperkt door *UpgradeDomainTimeout*.  Als *HealthCheckRetryTimeout* en *HealthCheckStableDuration* beide dan nul zijn en de status van de toepassing blijft heen en weer schakelen, klikt u vervolgens de upgrade uiteindelijk treedt er een time-out op *UpgradeDomainTimeout*. *UpgradeDomainTimeout* begint te tellen omlaag zodra de upgrade voor het huidige upgrade domein begint.

## <a name="next-steps"></a>Volgende stappen

[Een upgrade van uw toepassing gebruik Visual Studio](service-fabric-application-upgrade-tutorial.md) , wordt u begeleidt bij de upgrade van een toepassing gebruik van Visual Studio.

[Een upgrade van uw Powershell voor het gebruiken van toepassing](service-fabric-application-upgrade-tutorial-powershell.md) , wordt u begeleidt bij de upgrade van een toepassing via PowerShell.

Bepalen hoe uw toepassing upgrades met behulp van [Parameters upgraden](service-fabric-application-upgrade-parameters.md).

Uw toepassingsupgrades compatibel maken door te leren hoe u [Gegevens serialisatie](service-fabric-application-upgrade-data-serialization.md)gebruiken.

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [Geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in de [Toepassingsupgrades probleemoplossing](service-fabric-application-upgrade-troubleshooting.md).
 
