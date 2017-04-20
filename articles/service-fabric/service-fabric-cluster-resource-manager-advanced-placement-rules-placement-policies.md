<properties
   pageTitle="Service stof Cluster resourcemanager - plaatsing beleidsregels | Microsoft Azure"
   description="Overzicht van beleidsregels voor extra plaatsing en regels voor Service configuratieservices"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="placement-policies-for-service-fabric-services"></a>Plaatsing beleidsregels voor service configuratieservices
Er zijn veel verschillende aanvullende regels die u mogelijk uiteindelijk caring over als uw cluster Service stof upset ik me meerdere datacenters of Azure regio's in een geografische afstanden, of als uw omgeving zich uitstrekt over meerdere gebieden van geopolitieke besturingselement (of enkele andere hoofdletters/kleine letters waarop u hebt recht of het gehanteerde beleid grenzen u bent geïnteresseerd bent of de afstanden werkelijke prestaties/latentie invloed hebben). De meeste kan worden geconfigureerd via de eigenschappen van het knooppunt en plaatsing beperkingen, maar sommige ingewikkelder zijn. Wij bieden deze aanvullende commmands dingen eenvoudiger. Net als met andere beperkingen plaatsing plaatsing beleid kunnen worden geconfigureerd op basis van service per benoemde exemplaar.

## <a name="specifying-invalid-domains"></a>Ongeldige domeinen opgeven
Het beleid van de plaatsing InvalidDomain kunt u opgeven dat een bepaald foutenstructuuranalyse-domein ongeldig voor de werklast is. Dit beleid zorgt ervoor dat een bepaalde service nooit wordt uitgevoerd in een bepaald gebied, bijvoorbeeld vanwege de geopolitieke of bedrijfs-beleid. Meerdere ongeldige domeinen kunnen worden opgegeven via afzonderlijke beleidsregels.

![Ongeldige domein-voorbeeld][Image1]

Code:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Vereiste domeinen opgeven
Het vereiste domein plaatsing beleid is vereist dat alle exemplaren van de stateless service voor de service of statuscontrole replica's in het opgegeven domein aanwezig zijn. Meerdere vereiste domeinen kunnen worden opgegeven via afzonderlijke beleidsregels.

![Voorbeeld van de vereiste domein][Image2]

Code:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Een voorkeur domein voor de primaire replica's opgeven
De voorkeur hoofddomein is een interessante besturingselement, aangezien deze selectie van het domein dat foutenstructuuranalyse waarin de primaire moet worden teruggezet als het is mogelijk dat te doen. Wanneer u alles in orde is uiteindelijk de primaire in dit domein. Als u moet het domein of de primaire replica fail of worden afgesloten om welke reden dat de primaire worden gemigreerd naar een andere locatie. Als deze locatie niet in de voorkeur domein, wordt klikt u vervolgens indien mogelijk de resourcemanager Cluster het afzonderen op het gewenste domein. Natuurlijk relevant deze instelling alleen is voor statuscontrole services. Dit beleid is vooral handig bij clusters die zijn omvatten Azure regio's of meerdere datacenters. In deze situaties u alle locaties voor redundantie gebruikt, maar liever de primaire replica's worden geplaatst in een bepaalde locatie kan alleen worden aangeboden lagere latentie voor bewerkingen die gaat u naar de primaire (schrijft en ook alle lees al dan niet standaard worden verwerkt in de primaire).

![Voorkeur primaire domeinen en overname bij storing][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Replica's moet worden verdeeld over alle domeinen en weigeren verpakking vereisen
Een ander beleid die kunt u opgeven is replica's moet altijd worden verdeeld over de domeinen van de beschikbare foutenstructuuranalyse vereisen. Dit gebeurt al dan niet standaard in de meeste gevallen waarin het cluster in orde is, er zijn echter degenerate zaken waar replica's voor een bepaald partition kan dit uiteindelijk tijdelijk naar een enkel foutenstructuuranalyse of de upgrade domein verpakt. Bijvoorbeeld: Stel dat die Hoewel het cluster heeft 9 knooppunten in 3 foutenstructuuranalyse domeinen (0, 1 en 2), en uw service 3 replica's heeft, de knooppunten die zijn die wordt gebruikt voor die replica in foutenstructuuranalyse domeinen 1 en 2 afgesloten is en door capaciteitsproblemen met de geen van de andere knooppunten in deze domeinen ongeldig zijn. Als Service stof vervanging voor die replica maken, de resourcemanager Cluster zou moeten plaatst u deze fout domein 0, maar die Hiermee maakt u een situatie waar de restrictie foutenstructuuranalyse domein wordt wordt geschonden. Deze ook de kans groter dat de hele replicaset verloren kan gaan (als FD 0 zijn moeten permananently verloren). (Voor meer informatie over beperkingen en beperking prioriteiten in het algemeen wordt uitchecken [in dit onderwerp](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Als u een waarschuwing systeemstatus zoals ooit hebt gezien ' de taakverdeling heeft een overtreding beperking dit Replica: stof gedetecteerd: /<some service name> secundaire Partition <some partition ID> de beperking is schenden: FaultDomain ' hebt u deze voorwaarde of iets wat lijkt raken. Meestal deze situaties worden tijdelijke (de knooppunten blijven niet omlaag lang of als ze doen en moeten we bouwen vervanging er zijn andere knooppunten in de domeinen van de juiste fouttolerantie die geldig zijn), maar er zijn enkele werklast die u wilt liever de beschikbaarheid van de kans op pesterijen alle hun replica's verliezen handel. We kunt dit doen door het opgeven van het beleid "RequireDomainDistribution", waarmee wordt gegarandeerd dat er geen twee replica's uit de dezelfde partition ooit zijn toegestaan in hetzelfde domein foutenstructuuranalyse of upgraden.

Sommige werkbelasting liever het doel aantal replica's (kopieën van de staat) te allen tijde (weddenschappen tegen totale domein fouten en weten dat ze meestal lokale staat kunnen herstellen), dat anderen de downtime eerder dan de bezwaren correct en dataloss risico liever wilt houden. Aangezien de meeste productie werkbelasting met meer dan 3 replica's worden uitgevoerd, wordt de standaardinstelling is niet vereisen domein verdeling en laten van taakverdeling en failover gevallen normaal afhandelen, zelfs als dit betekent dat tijdelijk een domein meerdere replica's verpakt erin heeft.

Code:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nu, zou het mogelijk deze configuraties voor services in een cluster die is niet geografisch omspannen gebruiken? Weet kan u! Maar er is een goede reden te – met name de vereiste ongeldige en voorkeur domeinconfiguraties moeten worden voorkomen, tenzij u daadwerkelijk een geografisch spanned cluster uitvoert-niet dat u eventuele om te proberen te sluiten van een bepaalde werkbelasting uitvoeren in een enkel rek, of enkele segment van uw lokale cluster liever op een andere, tenzij er verschillende typen hardware of werkbelasting toegelicht die aan de hand afdwingen , en die gevallen kunnen worden verwerkt via normale plaatsing beperkingen.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over de andere opties die beschikbaar zijn voor het configureren van services uitchecken het onderwerp op de andere Cluster resourcemanager-configuraties beschikbaar [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
