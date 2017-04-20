<properties 
  pageTitle="Uw eigen register privé Docker op Azure implementeren | Microsoft Azure"
  description="Wordt beschreven hoe u Docker register kunt gebruiken voor het hosten van uw afbeeldingen container op de Azure Blob Storage-service."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>Uw eigen register privé Docker op Azure implementeren

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



In dit document wordt beschreven welke Docker privé register is en ziet u hoe u een afbeelding van de container Docker register 2.0 kunt implementeren naar een Docker privé register op Microsoft Azure met Azure-blobopslag.

In dit document wordt ervan uitgegaan:

1. U weet hoe u Docker gebruiken en hebben Docker afbeeldingen om op te slaan. (U niet? [Meer informatie over Docker](https://www.docker.com))
2. U hebt een server waarop Docker-engine is geïnstalleerd. (U niet? [Dit snel doen op Azure.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Wat is een privé Docker register?

Om te kunnen verzenden uw beperkte toepassingen in de cloud, die u kunt maken van een afbeelding van de container Docker en deze ergens opslaan zodat deze kan worden gebruikt door zelf en door anderen. 

Afbeelding van een container maken en verzenden naar de cloud eenvoudig is, is maar het een uitdaging de gegenereerde afbeelding om op te slaan. Daarom Docker biedt een gecentraliseerde service [Docker Hub] genoemd[ docker-hub] om op te slaan container afbeeldingen in de cloud en kunt u containers op elk gewenst moment met deze afbeeldingen maken.

Hoewel de [Docker Hub] [ docker-hub] is een betaalde service voor het opslaan van uw privé-toepassingscontainer afbeeldingen, Docker respecteert ontwikkelaars behoeften en biedt een open source set hulpmiddelen voor het opslaan van uw afbeeldingen in uw eigen persoonlijke Docker register achter een firewall of on-premises zonder op openbare Internet.
Omdat Azure-blobopslag gemakkelijk is te beveiligen, kunt u deze snel gebruiken om te maken en gebruiken van een privé Docker register in Azure wordt aangegeven die u zelf bepalen.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Waarom moet u een register Docker op Azure Host?

Door het hosten van uw exemplaar Docker register op Microsoft Azure en opslaan van uw afbeeldingen op Azure-blobopslag, hebt u verschillende voordelen:

**Beveiliging:** Uw afbeeldingen Docker laat niet Azure datacenters, zodat ze geen openbare Internet overschrijden als wanneer u Docker Hub zouden.
  
**Prestaties:** Uw afbeeldingen Docker worden opgeslagen in de datacenter of de regio als uw toepassingen. Dit betekent dat de afbeeldingen worden opgevraagd sneller en efficiënter vergeleken met Docker Hub.

**Betrouwbaarheid:** Met behulp van Microsoft Azure-blobopslag, kunt u gebruik van de eigenschappen van het veel opslag zoals beschikbaarheid, redundantie premium opslag (SSD), enzovoort.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Docker register gebruik van Azure-blobopslag configureren

(Het is raadzaam om te lezen van de [Docker register 2.0 documentatie][register-documenten] voordat u verdergaat.)

U kunt [configureren] [ registry-config] het register Docker op twee verschillende manieren.
U kunt:

1. Gebruik een `config.yml` bestand. In dit geval moet u een afzonderlijke Docker afbeelding boven maken `registry` afbeelding.
2. Het standaardconfiguratiebestand via omgevingsvariabelen overschrijven: dingen zonder te maken en onderhouden van een afzonderlijke Docker afbeelding krijgt.

In dit onderwerp volgt omdat dit eenvoudiger, optie 2, met behulp van de omgevingsvariabelen.

Een register Docker exemplaar om te kunnen uitvoeren die:

* uw Azure opslag-Account gebruikt voor het opslaan van de afbeeldingen
* op de VM poort 5000 luistert
* heeft geen verificatie geconfigureerd (niet aanbevolen, Zie de onderstaande notitie)

u moet uitvoeren van de volgende opdracht uit Docker in uw we vaker doen terminal (vervangen `<storage-account>` en `<storage-key>` met uw referenties):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Nadat de opdracht afgesloten, kunt u de container hostingprovider van uw persoonlijke exemplaar van de Docker register door te voeren zien de `docker ps` opdracht op de host:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Beveiliging configureren voor het register Docker valt buiten in dit document en het register zijn toegankelijk voor iedereen zonder verificatie al dan niet standaard als u de poort op de Register-poort van het eindpunt VM openen of de belasting voor documentconversies als u de opdracht implementatie bovenstaande gebruikt.
>
> Lees het [Configureren van Docker register] [ registry-config] documentatie voor meer informatie over het beveiligen van het exemplaar van het register en uw afbeeldingen.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw register instellen hebt, is het tijd om te gaan bepaalde meer gebruiken. Beginnen met de [register-documenten]die docker. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[register-documenten]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
