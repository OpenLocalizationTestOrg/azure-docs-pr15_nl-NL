<properties
 pageTitle="Grootte voor cloudservices | Microsoft Azure"
 description="Bevat de grootte van de andere virtuele machine (en id's) voor rollen voor het web en werknemer van de Azure cloud."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Grootte voor Cloudservices

In dit onderwerp worden de beschikbare formaten en opties voor Cloudservice rol instanties (web rollen en werknemer rollen). Het biedt ook aandachtspunten voor de implementatie om rekening moet houden bij het plannen van deze bronnen. Elke grootte heeft een ID die u in uw [service definitiebestand](cloud-services-model-and-package.md#csdef)wordt opgeslagen.

Cloudservices is een van de verschillende soorten berekeningscluster bronnen op Azure. Klik [hier](cloud-services-choose-me.md) voor meer informatie over Cloudservices.

> [AZURE.NOTE]Gerelateerde Azure limieten, raadpleegt u [Azure-abonnement en limieten van de Service, quota, en voorwaarden](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Grootte voor het web en werknemer rol exemplaren

Er zijn verschillende standaard formaten selecteren uit op Azure. Overwegingen voor sommige van deze formaten opnemen:

* D-reeks VMs zijn ontworpen om uit te voeren toepassingen die hoger rekenkracht en prestaties van de tijdelijke schijf. D-reeks VMs bieden sneller processors, een hogere geheugen-naar-core-breedteverhouding en een solid-state drive (SSD) voor de tijdelijke schijf. Voor meer informatie raadpleegt u de aankondiging van de Azure blog, [Nieuwe D-reeks VM grootte](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Dv2-reeks, een opvolger van de oorspronkelijke D-reeks, functies een krachtiger CPU. De CPU Dv2-reeks is ongeveer 35% sneller dan de CPU D-reeks. Dit is gebaseerd op de nieuwste 2,4 GHz Intel Xeon® E5-2673 v3 processor (Haswell), en met de Intel Turbo Microfoonversterking Technology 2.0, tot 3.1 GHz kunt gaan. De Dv2-reeks heeft de dezelfde geheugen en schijfruimte configuraties als de D-reeks.

*   G-serie VMs bieden de meeste geheugen en worden uitgevoerd op hosts met Intel Xeon E5 V3 family processors.

*   De A-reeks VMs kunnen worden geïmplementeerd op een groot aantal hardwaretypen en processors. Het formaat is vertraagd, op basis van de hardware om consistente processorprestaties voor de sessie, ongeacht de hardware die wordt geïmplementeerd op te leveren. Query om te bepalen de fysieke hardware waarop deze grootte wordt geïmplementeerd, de virtuele hardware uit in de virtuele Machine.

*   De grootte A0 is te weinig geabonneerde op de fysieke hardware. Voor deze specifieke grootte alleen andere klant-implementaties invloed kunnen hebben op de prestaties van uw actieve werkbelasting. De relatieve prestaties worden hieronder beschreven als de verwachte basislijn, onderhevig aan een niet-geheel exacte variaties van 15 procent.


De grootte van de virtuele machine van invloed is op de prijzen. De grootte heeft ook gevolgen voor de verwerking, geheugen en opslag capaciteit van de virtuele machine. Opslagkosten worden berekend afzonderlijk op basis van gebruikte pagina's in de opslagruimte-account. Zie voor meer [Informatie over virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/) en [Prijzen van Azure-opslag](https://azure.microsoft.com/pricing/details/storage/). 


De volgende overwegingen mogelijk te kiezen een grootte:


* De A8 A11 en H-reeks grootte zijn ook bekend als *computerintensieve exemplaren*. De hardware die wordt uitgevoerd als deze formaten is ontworpen en geoptimaliseerd voor computerintensieve en netwerk-veel-toepassingen, waaronder high-performance computing (HPC) cluster toepassingen, modellering en simulaties. De reeks A8 A11 gebruikmaakt van Intel Xeon E5-2670 @ 2,6 GHZ en de H-reeks wordt gebruikgemaakt van Intel Xeon E5-2667 v3 @ 3,2 GHz. Zie voor en overwegingen over het gebruik van deze formaten voor gedetailleerde informatie [over de A-reeks H-reeks en computerintensieve VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2-reeks, D-reeks, G-reeks zijn ideaal voor toepassingen die snellere CPU's, betere lokale schijf prestaties of hogere geheugen eisen hebt.  Ze bieden een krachtige combinatie voor veel enterprise-grade-toepassingen.

*   Enkele van de fysieke hosts in Azure datacenters mogelijk geen ondersteuning voor grotere van virtuele machine, zoals A5: A11. Hierdoor ziet u het foutbericht **mislukt virtuele machine {computernaam} configureren** of **mislukt virtuele machine {computernaam} maken** wanneer het formaat van een bestaande VM naar een nieuwe grootte. een nieuwe virtuele machine maken in een virtueel netwerk gemaakt voordat de 16 April 2013; of een nieuwe virtuele machine toevoegen aan een bestaande cloudservice. Zie [Fout: "Is mislukt virtuele machine configureren"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) op het ondersteuningsforum voor tijdelijke oplossingen voor elk implementatiescenario.  

* Uw abonnement mogelijk ook het aantal cores die u in bepaalde gezinnen grootte implementeren kunt beperken. Als u wilt vergroten een quotum, neemt u contact Azure-ondersteuning.


## <a name="performance-considerations"></a>Prestatieoverwegingen

We hebt het concept van de Azure berekenen eenheid (ACU) op een manier van het afwegen van computerprestaties (CPU) via Azure SKU's van gemaakt. Hierdoor kunt u eenvoudig bepalen welke SKU hoogstwaarschijnlijk om te voldoen aan de prestatiebehoeften van uw.  ACU is momenteel gestandaardiseerd op een kleine (Standard_A1) VM vervolgens 100 en alle andere SKU's wordt vertegenwoordigen ongeveer hoe veel sneller dat een standaard vergelijkende kan worden uitgevoerd door SKU. 

>[AZURE.IMPORTANT] De ACU is alleen een richtlijn.  De resultaten voor uw werkzaamheden variëren. 

<br>

|SKU familie |ACU/Core |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5 tot en met 7](#a-series) |100 |
|[A8 A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210 - 250 *|
|[G1 TOT EN MET 5](#g-series)  |180 - 240 *|
|[H](#h-series) |290 - 300 *|

ACUs gemarkeerd met een * Intel® Turbo technologie wilt gebruiken om te vergroten CPU frequentie en geef een krachtige prestaties.  Het bedrag van de Microfoonversterking kunt hangen af van de grootte VM, werkdruk en andere werkbelasting waarop dezelfde host.

## <a name="size-tables"></a>De grootte van tabellen

De volgende tabellen bevatten de grootte en de capaciteit die ze bieden.

* Opslagcapaciteit wordt weergegeven in eenheden van toevoegen of 1024 ^ 3 bytes. Wanneer schijven vergelijken gemeten in GB (1000 ^ 3 bytes) naar schijven gemeten in verwijderen (1024 ^ 3) onthouden dat capaciteit getallen die zijn opgegeven in verwijderen kleinere mogelijk weergegeven. Bijvoorbeeld 1023 toevoegen = 1098.4 GB

* Schijfdoorvoer wordt gemeten in invoer/uitvoer-bewerkingen per seconde (IO's / s) en MBps waar MBps = 10 ^ 6 bytes/sec.

* Gegevensschijven kunnen gebruiken in in de cache of uncached modi. Voor de bewerking van de schijf in de cache opgeslagen gegevens, worden de host cache-modus is ingesteld op **alleen-lezen** of **ReadWrite**.  Voor de bewerking van de schijf uncached gegevens, is de host cache-modus ingesteld op **geen**.

* Maximale netwerkbandbreedte is de maximale geaggregeerde bandbreedte toegewezen en toegewezen per VM type. De maximale bandbreedte bevat richtlijnen voor het selecteren van het juiste type VM om ervoor te zorgen voldoende netwerkcapaciteit beschikbaar is. Wanneer u verplaatst tussen laag, Gemiddeld, hoog en zeer hoog, wordt de doorvoer dienovereenkomstig gewijzigd vergroten. Werkelijke netwerkprestaties hangt veel factoren, waaronder het netwerk en toepassing laadtijd en toepassingsinstellingen netwerk.


## <a name="a-series"></a>A-reeks

| Grootte        | CPU-kernen | Geheugen: verwijderen | Lokale harde schijf: verwijderen | Max gegevensschijven | De schijf gegevensdoorvoer max: IO's / s | Max NIC / netwerkbandbreedte |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / lage                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / gemiddelde              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4-x 500              | 1 / gemiddelde              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / hoog                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / hoog                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4-X 500              | 1 / gemiddelde              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / hoog                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / hoog                  |

## <a name="a-series---compute-intensive-instances"></a>A-reeks-computerintensieve exemplaren

Zie voor informatie en overwegingen over het gebruik van deze formaten, [over de A-reeks H-reeks en computerintensieve VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Grootte         | CPU-kernen | Geheugen: verwijderen | Lokale harde schijf: verwijderen | Max gegevensschijven | De schijf gegevensdoorvoer max: IO's / s | Max NIC / netwerkbandbreedte |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoog                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / zeer hoog             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoog                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / zeer hoog             |

* RDMA nodig

## <a name="d-series"></a>D-reeks


| Grootte         | CPU-kernen | Geheugen: verwijderen | Lokale SSD: verwijderen | Max gegevensschijven | De schijf gegevensdoorvoer max: IO's / s | Max NIC / netwerkbandbreedte |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / gemiddelde              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4-x 500              | 2 / hoog                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / hoog                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / hoog                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4-x 500              | 2 / hoog                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / hoog                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / hoog                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / zeer hoog             |

## <a name="dv2-series"></a>Dv2-reeks

| Grootte            | CPU-kernen | Geheugen: verwijderen | Lokale SSD: verwijderen | Max gegevensschijven | De schijf gegevensdoorvoer max: IO's / s | Max NIC / netwerkbandbreedte |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / gemiddelde              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4-x 500              | 2 / hoog                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / hoog                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / hoog                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8 / zeer hoog        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4-x 500              | 2 / hoog                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / hoog                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / hoog                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / zeer hoog        |
| Standard_D15_v2 | 20        | 140          | 1000                | 40             | 40 x 500             | 8 / zeer hoog        |

## <a name="g-series"></a>G-reeks

| Grootte        | CPU-kernen | Geheugen: verwijderen  | Lokale SSD: verwijderen  | Max gegevensschijven | Max schijfdoorvoer: IO's / s | Max NIC / netwerkbandbreedte |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4-x 500            | 1 / hoog                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / hoog                  |
| Standard_G3 | 8         | 112          | 1,536                | 16             | 16 x 500           | 4 / zeer hoog             |
| Standard_G4 | 16        | 224          | 3.072                | 32             | 32 x 500           | 8 / zeer hoog        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / zeer hoog        |


## <a name="h-series"></a>H-reeks

Azure H-reeks virtuele machines zijn de volgende generatie krachtige computing dat VMS bovengrens behoeften, zoals moleculaire modellering en rekenkundige flexibel dynamics gericht. Deze 8 en 16 core VMs zijn gebaseerd op de Intel Haswell E5-2667 V3 processortechnologie die bestaat uit DDR4 geheugen en lokale SSD op basis van opslag. 

Naast de aanzienlijke CPU kracht biedt de H-reeks diverse opties voor lage latentie RDMA netwerken FDR InfiniBand en verschillende geheugenconfiguraties gebruiken om te ondersteunen geheugen intensief rekenkundige vereisten.


| Grootte           | CPU-kernen | Geheugen: verwijderen | Lokale SSD: verwijderen | Max gegevensschijven | Max schijfdoorvoer: IO's / s | Max NIC / netwerkbandbreedte |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16 x 500                    | 8 / hoog                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / zeer hoog                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16 x 500                    | 8 / hoog                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / zeer hoog                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / zeer hoog                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / zeer hoog                  |


* RDMA nodig

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Opmerkingen: Standaard A0 - A4 CLI en PowerShell gebruiken 

In het implementatiemodel klassieke zijn sommige namen van de grootte VM iets anders uit in CLI en PowerShell:

* Standard_A0 is ExtraSmall 
* Standard_A1 is klein
* Standard_A2 is normaal
* Standard_A3 is groot
* Standard_A4 is ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Grootte voor Cloud Services configureren

Als onderdeel van het service-model beschreven door de [service-definitiebestand](cloud-services-model-and-package.md#csdef)kunt u de grootte VM van een exemplaar van de rol. De grootte van de rol bepaalt het aantal CPU cores, de geheugencapaciteit en de lokale systeem bestandsgrootte die is toegewezen aan een sessie. Kies de grootte van de rol op basis van de vereisten van uw toepassing.

Hier volgt een voorbeeld voor het instellen van de grootte van de rol moeten [Standard_D2](#general-purpose-d) voor een exemplaar van de rol van Web:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [azure-abonnement en limieten van de service, quota, en voorwaarden](../azure-subscription-service-limits.md).
- Lees meer [over de A-reeks H-reeks en computerintensieve VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) voor werkbelasting zoals High-performance Computing (HPC).

