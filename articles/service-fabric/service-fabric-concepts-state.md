<properties
   pageTitle="Definiëren en beheren van staat | Microsoft Azure"
   description="Hoe definieert en servicestatus in de Service stof beheren"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="service-state"></a>Servicestatus
**Servicestatus** verwijst naar de gegevens die de service is vereist. Het bevat de gegevensstructuren en variabelen die de service gelezen en geschreven met werken.

Houd rekening met een eenvoudige rekenmachine-service, bijvoorbeeld. Deze service duurt van twee getallen en geeft als resultaat de som. Dit is een zuiver stateless service zonder gegevens die zijn gekoppeld.

Bekijk nu de dezelfde Rekenmachine, maar naast computing som, heeft dit ook een methode om de laatste som die deze heeft berekend te retourneren. Deze service is nu stateful--enkele provincie waarvan deze gegevens worden geschreven bevat naar (bij het berekenen van een nieuwe som) en leest uit (wanneer deze de laatste berekende som retourneert).

In Azure-Service stof, is de eerste service een stateless service genoemd. De tweede-service is een statuscontrole service genoemd.

## <a name="storing-service-state"></a>Servicestatus opslaan
Status kunt u externalized of kan zich bevinden door de code die is de status bewerken. Externalization van staat wordt meestal gedaan met behulp van een externe database of store. In ons voorbeeld Rekenmachine mogelijk een SQL-database waarin het huidige resultaat is opgeslagen in een tabel. Elk verzoek om te berekenen van de som kunt u een update uitvoeren op deze rij.

De staat kan ook bevinden samen met de code die deze code bewerkt. Statuscontrole services in Service stof worden gemaakt met het gebruik van dit model. Service stof levert de infrastructuur om ervoor te zorgen dat deze status ten zeerste beschikbaar is en fouttolerantie bij een storing.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service stof concepten:

- [Beschikbaarheid van Service configuratieservices](service-fabric-availability-services.md)

- [Schaalbaarheid van Service configuratieservices](service-fabric-concepts-scalability.md)

- [Service configuratieservices partitioneren](service-fabric-concepts-partitioning.md)
