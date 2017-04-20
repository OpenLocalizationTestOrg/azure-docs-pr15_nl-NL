<properties
   pageTitle="Lokaal bewaken en diagnose stellen bij services geschreven met Azure-Service stof | Microsoft Azure"
   description="Informatie over het bewaken en diagnose stellen bij uw services geschreven met Microsoft Azure-Service stof op een computer plaatselijke ontwikkeling."
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
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Bewaken en diagnose stellen bij services in de instellingen voor een lokale computer ontwikkeling


> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Sta services om door te gaan met minimaal ongemak voor de gebruikerservaring Monitoring, detecteert, diagnose en probleemoplossing. Controle- en diagnostische hulpprogramma's zijn kritieke in een werkelijke geïmplementeerd productieomgeving. Vast te stellen een soortgelijke model tijdens de ontwikkeling van services, zorgt ervoor dat de diagnostische pijplijn werkt wanneer u naar een productieomgeving verplaatsen. Service stof kunt u gemakkelijk voor ontwikkelaars van de service willen implementeren diagnostisch hulpprogramma dat naadloos op één computer plaatselijke ontwikkeling instellingen zowel echte productie cluster instellingen werken kunt.


## <a name="debugging-service-fabric-java-applications"></a>Voor foutopsporing in Service stof Java-toepassingen

Voor Java-toepassingen zijn [meerdere logboekregistratie kaders](http://en.wikipedia.org/wiki/Java_logging_framework) beschikbaar. Aangezien `java.util.logging` is de standaardoptie met de JRE, wordt ook gebruikt voor de [voorbeelden van code in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  De volgende discussie wordt uitgelegd hoe u de `java.util.logging` framework. 
 
U kunt met java.util.logging uw toepassingslogboeken omleiden naar geheugen, uitvoer-streams, console-bestanden of sockets. Voor elk van deze opties, zijn er standaard mailhandlers al in het kader. Kunt u een `app.properties` bestand voor het configureren van de bestands-handler voor uw toepassing alle logboeken omleiden naar een lokaal bestand. 

Het volgende codefragment bevat een voorbeeldconfiguratie: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

De map waarnaar wordt verwezen door de `app.properties` bestand moet bestaan. Na het `app.properties` bestand is gemaakt, moet u ook wijzigen uw script vermelding punt `entrypoint.sh` in de `<applicationfolder>/<servicePkg>/Code/` map voor het instellen van de eigenschap `java.util.logging.config.file` naar `app.propertes` bestand. De vermelding ziet er als het codefragment van de volgende:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Deze configuratie resulteert in de logboeken worden verzameld in een draaiende manier bij `/tmp/servicefabric/logs/`. De **%u** en **%g** toestaan voor het maken van meer bestanden, met de bestandsnamen mysfapp0.log, mysfapp1.log, enzovoort. Als geen-handler expliciet is geconfigureerd, is al dan niet standaard de console-handler geregistreerd. Een kunt de logboeken syslog onder /var/log/syslog weergeven.
 
Zie de [voorbeelden van code in github](http://github.com/Azure-Samples/service-fabric-java-getting-started)voor meer informatie.  



## <a name="next-steps"></a>Volgende stappen
Dezelfde tracering code toegevoegd aan uw toepassing werkt ook met de diagnostische hulpprogramma's van uw toepassing op een Azure cluster. Bekijk deze artikelen die de verschillende opties voor de hulpmiddelen voor bespreken en wordt beschreven hoe u ze kunt instellen om de.
* [Logboeken met Azure diagnostische gegevens verzamelen](service-fabric-diagnostics-how-to-setup-lad.md)
