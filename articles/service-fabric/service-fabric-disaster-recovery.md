<properties
   pageTitle="Azure-Service stof herstel | Microsoft Azure"
   description="Azure-Service stof biedt de mogelijkheden die nodig zijn te handelen met alle typen systeemfouten. In dit artikel worden de soorten systeemfouten die zich kunnen voordoen en hoe u ze kunt afhandelen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Herstel in Azure-Service stof

Een belangrijk onderdeel van het geven van een toepassing voor hoge beschikbaarheid cloud is ervoor te zorgen dat deze alle verschillende soorten fouten, waaronder mensen die zich buiten het besturingselement kan worden bewaard. In dit artikel beschrijft de fysieke indeling van een Azure-Service stof cluster in de context van mogelijke systeemfouten en bevat richtlijnen voor het verwerken van dergelijke systeemfouten te beperken of de kans op pesterijen downtime of gegevensverlies voorkomen.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Fysiek lay-out van Service stof clusters in Azure wordt aangegeven

Als u wilt weten over het risico van verschillende soorten fouten, is het handig om te weten hoe clusters worden fysiek ingedeeld in Azure wordt aangegeven.

Wanneer u een Service stof cluster in Azure maakt, moet u kiest u een gebied waar deze worden gehost. De Azure infrastructuur bepalingen vervolgens de bronnen voor dat cluster in het gebied, met name het aantal virtuele machines (VMs) aangevraagd. We bekijken nauwer hoe en waar deze VMs is ingericht.

### <a name="fault-domains"></a>Domeinen voor foutenstructuuranalyse

Standaard zijn de VMs in het cluster gelijkmatig verdeeld over logische groepen bekend als foutenstructuuranalyse domeinen (FDs), waarin de computers op basis van mogelijke fouten in de host hardware segmenten. Specifieke taken als twee VMs in twee afzonderlijke FDs, kunt u Zorg ervoor dat ze de dezelfde power bron- of netwerk schakeloptie niet te delen. Hierdoor een lokale netwerk of één VM defect power heeft geen invloed op de andere, Service stof, de werklast van de computer niet reageert binnen het cluster opnieuw uit te staan.

U kunt de indeling van uw cluster visualiseren voor foutenstructuuranalyse domeinen met de cluster-kaart die beschikbaar zijn in [Service stof Explorer](service-fabric-visualizing-your-cluster.md):

![Knooppunten verdeeld over foutenstructuuranalyse domeinen in de Service stof Explorer][sfx-cluster-map]

>[AZURE.NOTE] De andere as in de map cluster ziet u de upgrade domeinen die logisch op basis van gepland onderhoudsactiviteiten knooppunten groeperen. Service stof clusters in Azure worden altijd ingedeeld in vijf upgrade domeinen.

### <a name="geographic-distribution"></a>Geografische verdeling

Er zijn momenteel [26 Azure regio's in de hele wereld][azure-regions], met diverse meer aangekondigd. Een afzonderlijke gebied kan een of meer fysieke datacenters afhankelijk van de aanvraag en de beschikbaarheid van geschikt locaties, onder andere factoren bevatten. Bedenk wel dat zelfs in de regio's die meerdere fysieke datacenters bevatten, er geen garantie is dat van uw cluster VMs gelijkmatig worden verdeeld over de fysieke locaties. Daadwerkelijk, op dit moment alle VMs voor een bepaald cluster is ingericht binnen één fysieke site.

## <a name="dealing-with-failures"></a>Omgaan met fouten

Er zijn verschillende soorten fouten die van invloed kunnen uw cluster, elk voorzien van een eigen risicobeperking. Gaan we ze in de volgorde van kans voordoen.

### <a name="individual-machine-failures"></a>Afzonderlijke machine fouten

Zoals vermeld, geen risico op hun eigen kunnen afzonderlijke machine fouten, binnen de VM of in de hardware of software waarop deze binnen een domein foutenstructuuranalyse inhouden. Service stof wordt meestal de fout in enkele seconden detecteren en reageren dienovereenkomstig gewijzigd op basis van de status van de cluster. Als het knooppunt de primaire replica's voor een partition host is, is een nieuwe primaire uit de secundaire partitiereplica gekozen. Als Azure ervoor zorgt dat het mislukte machine back-up, wordt er automatisch opnieuw deelnemen aan het cluster en maakt u nogmaals op de delen van de werklast.

### <a name="multiple-concurrent-machine-failures"></a>Meerdere gelijktijdige machine fouten

Foutenstructuuranalyse domeinen het risico van gelijktijdige machine fouten aanzienlijk wordt verkleind, maar er is altijd de mogelijkheden voor meerdere willekeurig mislukken om over te brengen naar beneden meerdere computers in een cluster tegelijk.

In het algemeen, zolang een grootste deel van de knooppunten beschikbaar, blijft het cluster werken, zij het op lagere capaciteit zoals statuscontrole replica's in een kleinere set machines verpakt krijgen en minder stateless exemplaren beschikbaar zijn voor verspreiden laden.

#### <a name="quorum-loss"></a>Quorum verlies

Als een meeste replica voor een statuscontrole service partition omlaag gaan, status die partition een bekend als "quorum verlies". Op dit moment stopt Service stof schrijven naar die partition om ervoor te zorgen dat de status consistente en betrouwbare blijft staan. We kiest in feite een periode van niet beschikbaar om ervoor te zorgen dat klanten niet worden vermeld dat hun gegevens is opgeslagen, het is niet accepteren. Houd er rekening mee dat als u zich hebt aangemeld voor het lezen zodat van secundaire replica's voor die statuscontrole service, u blijven kunt om uit te voeren die lezen bewerkingen terwijl u in deze status. Een partition blijft in quorum verlies totdat er voldoende replica's keert u terug of totdat de beheerder van het cluster forceert u het systeem verplaatsen op de [Herstellen-ServiceFabricPartition-API]gebruiken[repair-partition-ps].

>[AZURE.WARNING] Verlies van gegevens treden uitvoeren van een actie herstellen terwijl de primaire replica omlaag is.

Systeemservices kunnen ook quorum verlies, met de impact die specifiek zijn voor de desbetreffende service wordt verslechteren. Bijvoorbeeld invloed quorum verlies in de naming service naamresolutie, dat in de service van de manager failover quorum verlies nieuwe maken van de service en failovers blokkeert. Houd er rekening mee dat in tegenstelling tot voor uw eigen services, poging om het systeem reparatie is *niet* aanbevolen. In plaats daarvan verdient te gewoon wachten tot de omlaag replica's weer.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>De kans op pesterijen quorum verlies minimaliseren

U kunt de risico's van quorum verlies minimaliseren door te vergroten doel replica instellen voor uw service. Is het handig om na te denken van het aantal replica's u nodig hebt gezien het aantal niet-beschikbare knooppunten die u mogelijk op zonder zodra tijdens het resterende beschikbaar voor schrijven, onthouden in erg vindt om van die toepassing of cluster upgrades kunnen aanbrengen knooppunten tijdelijk niet beschikbaar is, naast problemen met de hardware.

Houd rekening met de volgende voorbeelden ervan uitgaande dat u uw services als u wilt dat een MinReplicaSetSize van drie, het kleinste getal aanbevolen voor boorputten hebt geconfigureerd. Met een TargetReplicaSetSize van drie (een primaire en secundaire servers voor twee), een hardware is mislukt tijdens een upgrade (twee replica's omlaag) leidt quorum verlies en uw service worden alleen-lezen. U kunt ook als er zijn vijf replica's, zou u kunnen twee fouten tijdens de upgrade (drie replica's omlaag) opnemen, zoals de resterende twee replica kunnen nog steeds een quorum in de minimale replicaset vormen.

### <a name="data-center-outages-or-destruction"></a>Data center bijvoorbeeld of vernietiging

In sommige gevallen is fysiek datacenters tijdelijk niet beschikbaar vanwege verlies van power of netwerkverbindingen. Uw Service stof clusters en toepassingen ook niet beschikbaar in deze gevallen, maar uw gegevens blijven behouden. Voor kolomgroepen uitgevoerd in Azure kunt u updates weergeven op bijvoorbeeld op de [statuspagina van Azure][azure-status-dashboard].

In zeer zelden dat een hele fysieke datacenter is verwijderd, is elke Service stof clusters gehost er verloren, samen met hun status.

Als u wilt beveiligen tegen deze mogelijkheid, deze ernstige belangrijke geregeld [back-up maken van uw status](service-fabric-reliable-services-backup-restore.md) naar een geografische-redundante store en zorg ervoor dat u de mogelijkheid herstellen hebt gevalideerd. Hoe vaak u een back-up uitvoeren is afhankelijk van uw herstel punt doelstelling (vrijgegeven Productieorder). Zelfs als u back-up en herstellen nog volledig niet hebt geïmplementeerd, moet u een handler voor implementeren de `OnDataLoss` gebeurtenis zodat u kunt zich aanmelden als het zich daadwerkelijk voordoet als volgt:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Softwarefouten en andere bronnen van gegevensverlies

Als een oorzaak van gegevensverlies komen de code afwijkingen in services, menselijke operationele fouten en beveiliging inbreuk op vaker dan de algemene gegevens center fouten. In alle gevallen de herstelstrategie is echter hetzelfde: Volg regelmatige back-ups van alle statuscontrole services om te oefenen de mogelijkheid om te herstellen die staat.

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u verschillende fouten met het [testbaarheid framework](service-fabric-testability-overview.md) simuleren
- Overige informatiebronnen herstel en hoge beschikbaarheid lezen. Microsoft heeft een grote hoeveelheid informatie vinden over deze onderwerpen die zijn gepubliceerd. Terwijl u enkele van deze documenten verwijzen naar specifieke technieken voor gebruik in andere producten, bevatten ze veel algemene aanbevolen procedures die u in als u ook de context van die Service stof toepassen kunt:
 - [Beschikbaarheid van controlelijst](../best-practices-availability-checklist.md)
 - [Uitvoering van een noodgevallen herstel meer details](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Herstel en beschikbaarheid van Azure-toepassingen][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
