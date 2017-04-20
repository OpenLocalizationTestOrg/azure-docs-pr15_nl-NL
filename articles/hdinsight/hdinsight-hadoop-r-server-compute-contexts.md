<properties
   pageTitle="Context-opties voor R-Server op HDInsight (preview) berekenen | Microsoft Azure"
   description="Meer informatie over de verschillende berekeningscluster context opties beschikbaar voor gebruikers met R-Server op HDInsight (preview)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Context-opties voor R-Server op HDInsight (preview) berekenen

Microsoft R Server op Azure HDInsight (preview) biedt de nieuwste mogelijkheden voor R gebaseerde analytics. Gegevens die zijn opgeslagen in HDFS in een container in uw account van [Azure Blob](../storage/storage-introduction.md "Azure-blobopslag") opslag of het lokale Linux-bestandssysteem wordt gebruikt. Aangezien R-Server is gebaseerd op bron openen R, kunnen de R-toepassingen die u samenstelt gebruikmaken van een van de bron openen R-pakketten 8000 +. Ze kunnen ook gebruikmaken van de routines in [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Revolutie Analytics ScaleR"), Microsofts groot gegevens analytics-pakket dat is inbegrepen in R-Server.  

Het randknooppunt van een Premium-cluster is een handige locatie om de verbinding maken met het cluster en uitvoeren van scripts R. Met een randknooppunt hebt u de optie van het uitvoeren van ScaleR parallelized verdeelde functies over de cores van de randserver knooppunt. U hebt ook de mogelijkheid om uit te voeren ze op de knooppunten van het cluster met behulp van ScaleR Hadoop kaart verkleinen of een contexten berekenen.

## <a name="compute-contexts-for-an-edge-node"></a>Contexten voor een randknooppunt berekenen

In het algemeen een R-script dat R Server wordt uitgevoerd op het randknooppunt uitgevoerd binnen de interpreter R op dat knooppunt. De uitzondering is deze stappen die een functie ScaleR bellen. De ScaleR oproepen uitvoeren in een berekeningscluster-omgeving waarin wordt bepaald door de manier waarop u de context van de berekeningscluster ScaleR instelt.  Wanneer u uw R-script vanuit een randknooppunt uitvoeren, de mogelijke waarden van de context berekeningscluster lokale opeenvolgende ('local'), worden lokale parallel (localpar), verkleinen van de kaart en een.

De opties 'local' en 'localpar' verschillen alleen met hoe rxExec oproepen worden uitgevoerd. Beide uitvoeren andere gesprekken rx-functie in een parallelle wijze over alle beschikbare cores, tenzij anderszins opgegeven door middel van de ScaleR numCoresToUse optie, bijvoorbeeld rxOptions(numCoresToUse=6). De volgende bevat een overzicht van de verschillende opties voor de context van berekenen

| Context berekenen  | Het instellen                      | Uitvoeringscontext                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Lokaal opeenvolgende | rxSetComputeContext('local')    | Parallelized uitvoering over de cores van de randserver knooppunt, behalve voor rxExec oproepen die serie worden uitgevoerd |
| Lokale parallel   | rxSetComputeContext('localpar') | Parallelized uitvoering over de boormonsters van het knooppunt randserver                                 |
| Elektrische            | RxSpark()                       | Verdeelde uit te voeren via een op de knooppunten van het cluster HDI parallelized      |
| Map verkleinen       | RxHadoopMR()                    | Verdeelde uit te voeren via kaart verkleinen op de knooppunten van het cluster HDI parallelized |


Ervan uitgaande dat u parallelized worden uitgevoerd voor de toepassing van prestaties wilt, klik zijn er drie opties. Welke optie u kiest, is afhankelijk van de aard van uw werk analyses, en de grootte en de locatie van uw gegevens.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Richtlijnen voor het kiezen van een berekeningscluster context

Er is momenteel geen formule die wordt aangegeven welke context berekenen te gebruiken. Er zijn echter enkele principes die u de juiste keuze te maken kunnen, of ten minste helpen u uw keuzen verfijnen voordat u een referentie uitvoert. Deze beginselen leidt omvatten:

1.  Het lokale Linux-bestandssysteem is sneller dan HDFS.
2.  Herhaalde analyses zijn sneller als de gegevens lokale en XDF is.
3.  Is het raadzaam om te streamen kleine hoeveelheden gegevens uit een tekst-gegevensbron; Als de hoeveelheid gegevens groter is, converteren naar XDF vóór analyse.
4.  De realiseren van kopiëren of streaming van de gegevens naar het randknooppunt voor analyse wordt voor zeer grote hoeveelheden gegevens.
5.  Elektrische is sneller dan kaart beperken voor analyse in Hadoop.

Deze beginselen gegeven, zijn enkele algemene regels van miniatuur voor het selecteren van de context van een berekeningscluster:

### <a name="local"></a>Lokale

- Wanneer de hoeveelheid gegevens te analyseren klein is en niet voor herhaalde analyse vereist is, deze rechtstreeks in de analyse-routine streamen en gebruikt u 'local' of 'localpar'.
- Als de hoeveelheid gegevens te analyseren kleine of middelgrote is en herhaalde analyse vereist, klikt u vervolgens kopiëren naar het lokale bestandssysteem, op XDF importeren en analyseren via 'local' of 'localpar'.

### <a name="hadoop-spark"></a>Hadoop elektrische

- Als de hoeveelheid gegevens te analyseren groot is, op importeren XDF in HDFS (tenzij opslag een probleem is), en deze via de 'Een' te analyseren.

### <a name="hadoop-map-reduce"></a>Hadoop-kaart verkleinen

- Alleen gebruiken als u een onoverkomelijke probleem met gebruik van de context van een berekeningscluster optreden omdat in het algemeen deze lager zijn.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Inline-help op rxSetComputeContext

Voor meer informatie en voorbeelden van ScaleR berekeningscluster contexten, raadpleegt u de inline in R help bij de methode rxSetComputeContext, bijvoorbeeld:

    > ?rxSetComputeContext

U kunt ook verwijzen naar de "ScaleR Distributed Computing Guide' die is beschikbaar in de [R Server MSDN]-(https://msdn.microsoft.com/library/mt674634.aspx "R-Server op MSDN") -bibliotheek.


## <a name="next-steps"></a>Volgende stappen

In dit artikel u hebt geleerd hoe u maakt een nieuwe HDInsight cluster met R-Server. U weet hoe u ook de basisbeginselen van het gebruik van de R-console uit een SSH-sessie. U kunt nu de volgende artikelen als u wilt ontdekken andere manieren van het werken met R-Server op HDInsight lezen:

- [Overzicht van de Server R voor Hadoop](hdinsight-hadoop-r-server-overview.md)
- [Aan de slag met R-server voor Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [RStudio Server toevoegen aan HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure opslagopties voor R-Server op HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
