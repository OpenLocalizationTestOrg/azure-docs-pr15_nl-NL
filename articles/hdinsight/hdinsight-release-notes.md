<properties
    pageTitle="Releaseopmerkingen voor Hadoop-onderdelen op Azure HDInsight | Microsoft Azure"
    description="Meest recente releaseopmerkingen en versies van Hadoop-onderdelen voor Azure HDInsight. Ontwikkeling tips en informatie opvragen voor Hadoop, Apache Storm en HBase."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Releaseopmerkingen voor Hadoop-onderdelen op Azure HDInsight

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Notities voor 10/26/2016 release van R-Server op HDInsight

- R-Server op HDInsight cluster inrichting is gestroomlijnd.
- R-Server op HDInsight is nu beschikbaar als normale HDInsight "R-Server" cluster type en niet meer hebt geïnstalleerd als een afzonderlijke HDInsight-toepassing. De randknooppunt en R Server binaire bestanden zijn nu deze is ingericht als onderdeel van de implementatie van de cluster R-Server. Dit verbetert de snelheid en betrouwbaarheid van de inrichting van. Prijzen model voor R-Server is bijgewerkt.
- R-Server cluster type prijs is nu gebaseerd op standaard laag prijs plus R Server aanvulling prijs. Premium laag wordt nu worden gereserveerd voor Premium-functies die beschikbaar zijn in verschillende typen ander cluster en worden niet gebruikt voor R cluster servertype. Deze wijziging heeft geen invloed op de effectieve prijzen van R-Server, alleen verandert manier waarop de kosten die worden weergegeven in de factuur. Alle bestaande R-serverclusters nog steeds werkt en ARM sjablonen blijven gebruiken tot afschrijving kennisgeving. **Het wordt aanbevolen Hoewel bij uw script implementaties als nieuwe ARM sjabloon wilt gebruiken.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Notities voor 08/30/2016 release van R-Server op HDInsight

De nummers van de volledige versie van HDInsight Linux gebaseerde clusters geïmplementeerd in deze versie:

|HDI |HDI cluster versie   |HDP |HDP opbouwen   |Ambari opbouwen |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2,4 |2.4.2.4-5   |2.2.1.12-4   |

De nummers van de volledige versie van Windows gebaseerde HDInsight clusters geïmplementeerd in deze versie:

|HDI |HDI cluster versie   |HDP |HDP opbouwen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Notities voor 17-08/2016 release van R-Server op HDInsight

- R Server 8.0.5 – voornamelijk de versie van een fout repareren. Zie de [Releaseopmerkingen R-Server](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) voor meer informatie. 
- AzureML pakket op het randknooppunt – [Dit R-pakket](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) kunt R-modellen moeten worden gepubliceerd en verbruikt als een webservice Azure ML.  Zie het gedeelte [' mogelijk maken van een Model '](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) van onze [' overzicht van R Server op HDInsight '](hdinsight-hadoop-r-server-overview.md) -artikel voor meer informatie.
- Linux afhankelijkheden van de [bovenste 100 populairste R-pakketten](https://github.com/metacran/cranlogs) – deze Linux pakketafhankelijkheden zijn nu vooraf geïnstalleerd.  
- Optie voor het gebruik van de cessies‑retrocessies CRAN wanneer R toevoegen aan de gegevensknooppunten pakketten. Zie het gedeelte ['Installeren R-pakketten'](hdinsight-hadoop-r-server-get-started.md#install-r-packages) van onze ['Aan de slag met R-Server op HDInsight'](hdinsight-hadoop-r-server-get-started.md) -artikel voor meer informatie.
- Verbeteringen in opties om de betrouwbaarheid van de inrichting van R Server wanneer clusters worden gemaakt.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Notities op 08/01/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight Linux gebaseerde clusters geïmplementeerd in deze versie:

|HDI |HDI cluster versie   |HDP |HDP opbouwen   |Ambari opbouwen |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2,4 |2.4.2.4-5   |2.2.1.12-4   |

De nummers van de volledige versie van Windows gebaseerde HDInsight clusters geïmplementeerd in deze versie:

|HDI |HDI cluster versie   |HDP |HDP opbouwen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Clustertype (bijvoorbeeld een, Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Wijzigingen in HDInsight 3.4 clusters | De standaardwaarde voor de volgende component configuraties worden gewijzigd voor betere prestaties <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Service    | Alle| N/B|
| Volgende correcties zijn opgenomen in deze release | COMPONENT-13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Service    | Alle| N/B

## <a name="notes-for-07142016-release-of-hdinsight"></a>Notities op 14-07/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight Linux gebaseerde clusters geïmplementeerd in deze versie:

|HDI |HDI cluster versie   |HDP |HDP opbouwen   |Ambari opbouwen |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2,4 |2.4.2.0     |2.2.1.12-2   |

De nummers van de volledige versie van Windows gebaseerde HDInsight clusters geïmplementeerd in deze versie:

|HDI |HDI cluster versie   |HDP |HDP opbouwen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Notities op 07/07/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight Linux gebaseerde clusters geïmplementeerd in deze versie:

|HDI |HDI cluster versie   |HDP |HDP opbouwen   |
|----|----------------------|----|------------|
|3,2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2,4 |2.4.2.0     |

De nummers van de volledige versie van Windows gebaseerde HDInsight clusters geïmplementeerd in deze versie:

|HDI |HDI cluster versie   |HDP |HDP opbouwen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Clustertype (bijvoorbeeld een, Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [HDInsight hulpprogramma's voor IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) | IntelliJ IDEE-invoegtoepassing voor HDInsight Spark clusters is nu geïntegreerd met Azure Toolkit voor IntelliJ. Deze ondersteunt Azure SDK v2.9.1, meest recente Java SDK's, en bevat alle functies van de zelfstandige HDInsight Plugin voor IntelliJ.| Hulpmiddelen voor    | Elektrische| N/B|
| [HDInsight hulpprogramma's voor Eclips](hdinsight-apache-spark-eclipse-tool-plugin.md) | Azure Toolkit voor Eclips ondersteunt nu HDInsight Spark clusters. De volgende functies inschakelen <ul><li>Maak en een toepassing een eenvoudig in Scala en Java schrijven met eerste class authoring ondersteuning voor IntelliSense, automatisch opmaken, foutcontrole, enzovoort.</li><li>Test de toepassing van een lokaal.</li><li>Taken aan HDInsight Spark cluster indienen en de resultaten op te halen.</li><li>Meld u aan bij Azure en toegang tot alle elektrische clusters die is gekoppeld aan uw Azure-abonnementen.</li><li>Alle bijbehorende opslagbronnen van uw cluster HDInsight Spark navigeren.</li></ul>| Hulpmiddelen voor    | Elektrische| N/B

Beginnen met deze release, hebben we het Gast OS patch beleid voor HDInsight Linux gebaseerde clusters gewijzigd. Het doel van het nieuwe beleid is aanzienlijk Beperk het aantal opnieuw opstarten vanwege herstellen. Het nieuwe beleid blijft patches virtuele machines (VMs) op Linux clusters elke maandag of donderdag gerekend vanaf 12 AM UTC op Versprongen wijze over de knooppunten in een bepaald cluster. Een bepaald VM zal echter alleen opnieuw opstarten maximaal eenmaal per 30 dagen vanwege Gast OS herstellen. Daarnaast wordt de eerste keer opnieuw opstarten voor een nieuw cluster niet eerder dan 30 dagen na de aanmaakdatum van cluster plaatsvindt.

>[AZURE.NOTE] Deze wijzigingen geldt alleen voor nieuwe clusters gelijk of groter is dan deze versie.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Notities op 06/06/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

|HDP    |HDI versie    |Een versie  |Ambari Build-nummer    |HDP Build-nummer|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2,4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Clustertype (bijvoorbeeld een, Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Elektrische op HDInsight is algemeen beschikbaar | Deze release Hiermee verbeteringen in beschikbaarheid, schaalbaarheid en productiviteit bron Apache elektrische op HDInsight openen. <ul><li>Toonaangevende beschikbaarheid SLA van 99,9% waardoor u deze geschikt voor enterprise werkbelasting vragen.</li><li>Scalable opslaglaag met Azure Lake gegevensopslag.</li><li>Productiviteit hulpprogramma's voor elke fase van gegevens verkennen en ontwikkeling. Jupyter notitieblokken met aangepaste elektrische kernel inschakelen interactieve gegevens verkennen, integratie met BI-dashboards zoals Power BI, Tableau en Qlik is een goed idee voor snel gegevens delen en continue rapportage, IntelliJ-invoegtoepassing is betrouwbare optie voor de ontwikkeling van de lange termijn code onderdeel en foutopsporing.</li></ul>| Service    | Elektrische| N/B|
| HDInsight hulpprogramma's voor IntelliJ | Dit is een invoegtoepassing IntelliJ IDEE voor HDInsight Spark clusters. De volgende functies inschakelen<ul><li>Maak en een toepassing een eenvoudig in Scala en Java schrijven met eerste class authoring ondersteuning voor IntelliSense, automatisch opmaken, foutcontrole, enzovoort.</li><li>Test de toepassing van een lokaal.</li><li>Taken aan HDInsight Spark cluster indienen en de resultaten op te halen.</li><li>Meld u aan bij Azure en toegang tot alle elektrische clusters die is gekoppeld aan uw Azure-abonnementen.</li><li>Alle bijbehorende opslagbronnen van uw cluster HDInsight Spark navigeren.</li><li>Alle taken geschiedenis en taakgegevens voor uw cluster HDInsight Spark navigeren.</li><li>Fouten opsporen in een taken op afstand van uw desktopcomputer.</li></ul>| Hulpmiddelen voor    | Elektrische| N/B

## <a name="notes-for-05132016-release-of-hdinsight"></a>Notities op 05/13/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight (Windows) 2.1.10.875.2159884 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.875.2159884 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.922.2266903 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Clustertype (bijvoorbeeld een, Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Dus bijwerken van versie en andere correcties | Deze versie de versie elektrische in HDInsight cluster naar 1.6.1 bijgewerkt en worden opgelost die andere bugs bij te werken| Service    | Elektrische| N/B

## <a name="notes-for-04112016-release-of-hdinsight"></a>Notities op 04/11/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight (Windows) 2.1.10.889.2191206 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.889.2191206 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.889.2191206 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-ongewijzigd)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aangepaste metastore upgrade problemen voor HDI 3.4 | Maken van het cluster zou mislukt als u een aangepaste metastore, die eerder is gebruikt op een lagere versie van een ander HDInsight cluster hebt gebruikt. Dit is vanwege een script upgradefout die is nu opgelost| Cluster maken    | Alle | N/B
| Hier vastlopen herstel | Taak status tolerantie biedt voor elke taak die is verzonden via hier | Betrouwbaarheid | Een op Linux| N/B
| Jupyter inhoud HA | Biedt de mogelijkheid om te slaan en Jupyter notitieblok inhoud naar en vanuit het opslag-account dat is gekoppeld aan het cluster laden. Zie [Kernels beschikbaar voor Jupyter notitieblokken](hdinsight-apache-spark-jupyter-notebook-kernels.md)voor meer informatie.| Notitieblokken | Een op Linux| N/B
| Verwijdering van hiveContext in Jupter notitieblokken | Gebruik `%%sql` magische in plaats van `%%hive` bijzonder. SqlContext is gelijk aan hiveContext. Zie voor meer informatie [Kernels beschikbaar voor Jupyter notitieblokken](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Notitieblokken    | Elektrische clusters op Linux| N/B
| Afschrijving van oudere versies van een | Oudere versie elektrische 1.3.1 verwijderd uit de service op 31-5 | Service | Elektrische clusters in Windows | N/B

## <a name="notes-for-03292016-release-of-hdinsight"></a>Notities op 03/29/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight (Windows) 2.1.10.875.2159884 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.875.2159884 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.875.2159884 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.875.2159884 (2.2.9.1-7 HDP - ongewijzigd)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (2.2.9.1-8 HDP - ongewijzigd)
* HDInsight (Linux) 3.3.1000.0.7193255 (2.3.3.1-7 HDP - ongewijzigd)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Toegevoegde HDInsight 3.4 versie en bijgewerkte versies van HDP voor alle HDInsight clusters | In deze release we HDInsight v3.4 (gebaseerd op HDP 2,4) hebt toegevoegd en ook andere versies HDP hebt bijgewerkt. Releaseopmerkingen HDP 2,4 zijn beschikbaar [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) en meer informatie over HDInsight versies kunnen worden gevonden [hier](hdinsight-component-versioning.md).| Service    | Alle Linux clusters| N/B
| HDInsight Premium | HDInsight is nu beschikbaar in twee categorieën - Standard en Premium. HDInsight Premium is momenteel in de Preview-versie en alleen beschikbaar voor Hadoop en een clusters op Linux. Zie voor meer informatie [hier](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Service    | Hadoop en een op Linux| N/B
| Microsoft-R-Server | HDInsight Premium biedt Microsoft R Server die toegevoegd aan Hadoop en een clusters op Linux worden kan. Zie [Overzicht van R Server op HDInsight](hdinsight-hadoop-r-server-overview.md)voor meer informatie.| Service    | Hadoop en een op Linux| N/B
| Elektrische 1.6.0 | HDInsight 3.4 clusters bevatten nu een 1.6.0| Service    | Elektrische clusters op Linux| N/B
| Verbeteringen in het notitieblok van Jupyter | Jupyter notitieblokken beschikbaar voor communicatie met een clusters bieden extra elektrische nu kernels. Ook de opties omvatten verbeteringen zoals benut %% bijzonder, automatisch-visualisatie en integratie met Python visualisatie bibliotheken (zoals matplotlib). Zie [Kernels beschikbaar voor Jupyter notitieblokken](hdinsight-apache-spark-jupyter-notebook-kernels.md)voor meer informatie. | Service | Elektrische clusters op Linux | N/B

## <a name="notes-for-03222016-release-of-hdinsight"></a>Notities op 03/22/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight (Windows) 2.1.10.875.2159884 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.875.2159884 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.875.2159884 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.875.2159884 (2.2.9.1-7 HDP - ongewijzigd)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (2.2.9.1-8 HDP - ongewijzigd)
* HDInsight (Linux) 3.3.1000.0.7193255 (2.3.3.1-7 HDP - ongewijzigd)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte HDInsight versies voor alle HDInsight clusters | In deze release HDInsight versies voor alle HDInsight clusters is bijgewerkt| Service    | Alle| N/B


## <a name="notes-for-03102016-release-of-hdinsight"></a>Notities op 03/10/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight (Windows) 2.1.10.859.2123216 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.859.2123216 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.859.2123216 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (2.3.3.1-5 HDP - ongewijzigd)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte HDInsight versies voor alle HDInsight clusters | In deze release HDInsight versies voor alle HDInsight clusters is bijgewerkt| Service    | Alle| N/B

## <a name="notes-for-01272016-release-of-hdinsight"></a>Notities op 01/27/2016-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight (Windows) 2.1.10.817.2028315 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.817.2028315 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.817.2028315 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (2.3.3.1-5 HDP - ongewijzigd)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte HDInsight versies voor alle HDInsight clusters | In deze release HDInsight versies voor alle HDInsight clusters is bijgewerkt| Service    | Alle| N/B

## <a name="notes-for-12022015-release-of-hdinsight"></a>Notities op 02-12-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight (Windows) 2.1.10.763.1931434 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.763.1931434 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.763.1931434 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.763.1931434 (2.2.7.1-34 HDP - ongewijzigd)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (2.2.7.1-34 HDP - ongewijzigd)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Toegevoegde HDInsight 3.3 versie en bijgewerkte versies van HDP voor alle HDInsight clusters | In deze release we HDInsight v3.3 (gebaseerd op HDP 2.3) hebt toegevoegd en ook andere versies HDP hebt bijgewerkt. Releaseopmerkingen HDP 2.3 zijn beschikbaar [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) en meer informatie over HDInsight versies kunnen worden gevonden [hier](hdinsight-component-versioning.md).| Service    | Alle| N/B

## <a name="notes-for-11302015-release-of-hdinsight"></a>Notities op 30-11-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight (Windows) 2.1.10.757.1923908 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.757.1923908 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.757.1923908 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte HDInsight versies voor alle HDInsight clusters en HDP versies voor HDInsight 3,2 clusters (Windows en Linux) | In deze release zijn HDInsight en HDP versies bijgewerkt | Service    | Alle| N/B


## <a name="notes-for-10272015-release-of-hdinsight"></a>Notities op 27-10-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight (Windows) 2.1.10.726.1866228 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.726.1866228 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.726.1866228 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte HDInsight versies voor alle HDInsight clusters (Windows en Linux) | In deze release zijn HDInsight en HDP versies bijgewerkt | Service    | Alle| N/B
| Vaste Jupyter voor Windows een clusters met hoofdletters clusters | Clusters die DNS-namen die zijn opgegeven in hoofdletters had had problemen met Jupyter notitieblokken vanwege een controle van de aanvraag origin. De correctie hebt gewijzigd van de DNS-naam voor de configuratie van Jupyter in kleine letters. | Service    | HDInsight elektrische (Windows)| N/B


## <a name="notes-for-10202015-release-of-hdinsight"></a>Notities op 20-10-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - ongewijzigd)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - ongewijzigd)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDP standaardversie gewijzigd in HDP 2.2 | De standaardversie voor HDInsight Windows clusters wordt gewijzigd in HDP 2.2. HDInsight versie 3,2 (HDP 2.2) is sinds algemeen beschikbaar zijn in februari 2015. Deze wijziging wordt alleen de standaardversie cluster, gespiegeld wanneer een expliciete selectie niet bij de inrichting van het gebruik van de Azure-portal, PowerShell-cmdlets of de SDK cluster is gemaakt. | Service    | Alle| N/B                  |
|Wijzigingen in de notatie voor de naam voor de implementatie van meerdere HDInsight op Linux clusters in één virtuele netwerk VM | Ondersteuning voor meerdere HDInsight Linux clusters in één virtuele netwerk implementeren wordt in deze release toegevoegd. Onderdeel van dit is de notatie van VM namen in het cluster is gewijzigd van headnode\*, workernode\* en zookeepernode\* naar hn\*, wn\*, en zk\* respectievelijk. Deze wordt niet aanbevolen moet worden ondernomen een directe afhankelijkheid de opmaak van de namen van de virtuele machine, aangezien dit kan gewijzigd worden. Gebruik de "hostname -f" op de lokale computer of Ambari APIs om te bepalen de lijst met hosts en de toewijzing van onderdelen hosts. U vindt meer informatie aan [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) en [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). | Service | HDInsight clusters op Linux | N/B |
| Wijzigingen in de configuratie | De volgende configuraties zijn nu beschikbaar voor HDInsight 3.1 kolomgroepen: <ul><li>tez.yarn.ATS.Enabled en yarn.log.server.url. Hiermee worden de tijdlijn toepassingsserver en de server Log moeten kunnen fungeren out Logboeken.</li></ul>Voor HDInsight 3,2 kolomgroepen zijn de volgende configuraties gewijzigd: <ul><li>mapreduce.fileoutputcommitter.Algorithm.Version is ingesteld op 2. Hiermee worden gebruik van de V2-versie van de FileOutputCommitter.</li></ul> | Service | Alle | N/B |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Notities op 09-09-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.675.1768697 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.675.1768697 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.675.1768697 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.6.675.1768697 (2.2.6.1-0012 HDP - ongewijzigd)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte HDInsight versies voor alle HDInsight clusters | In deze release zijn HDInsight versies bijgewerkt | Service    | Alle| N/B                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Notities op 31-07-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.640.1695824 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.640.1695824 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.640.1695824 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.6.640.1695824 (2.2.6.1-0012 HDP - ongewijzigd)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Een cluster knooppunt opnieuw afbeeldingen werkstroom oplossen | Een fout die is veroorzaakt door knooppunten niet herstellen na opnieuw afbeelding aan een vaste | Service    | Elektrische| N/B                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Notities op 31-07-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.635.1684502 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.635.1684502 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.635.1684502 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.6.635.1684502 (2.2.6.1-0012 HDP - ongewijzigd)
* SDK 1.5.8

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte HDInsight versies voor alle HDInsight clusters | In deze release zijn HDInsight versies bijgewerkt | Service    | Alle| N/B                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Notities op 07-07-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.610.1630216 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.610.1630216 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.610.1630216 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Type (bijvoorbeeld Hadoop, HBase of Storm) cluster | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte HDP versies voor HDInsight 3,2 clusters | In deze release implementeren HDInsight 3,2 HDP 2.2.6.1-0012 | Service    | Alle                                                 | N/B                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Notities op 26-06-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.601.1610731 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.601.1610731 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.601.1610731 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Bijgewerkte HDP versies voor HDInsight 3,2 clusters</td>
<td>In deze release implementeren HDInsight 3,2 HDP 2.2.6.1</td>
<td>Service</td>
<td>Alle</td>
<td>N/B</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Notities voor 18-06-2015 release van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.596.1601657 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.596.1601657 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Aanvullende HTTPS-poorten zijn geopend</td>
<td>De cloudservice wordt nu geopend 5 poorten 8001 naar 8005 blijven ingedeeld op het cluster bijv bij https://<clustername>.azurehdinsight.net:8001/. Aanvragen voor deze URL's worden geverifieerd via van de methode Basisverificatie wachtwoord als poort 443. Deze poorten binden aan dezelfde poort op de actieve headnode. Scriptacties kunnen worden gebruikt om te luisteren op deze poort op de headnode en de route naar buiten het cluster klantenservice.</td>
<td>Cloudservice</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>Door onregelmatige MapReduce willekeurige volgorde probleem voor HDInsight 3,2</td>
<td>Oplossing voor een voorwaarde niet vaak voorkomen, door onregelmatige raceauto in kaart verkleinen willekeurige volgorde op grote clusters waardoor er fouten. Zie <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> voor meer informatie.</td>
<td>Hadoop Core</td>
<td>Alle</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a></td>
</tr>

<tr>
<td>Verplaatsen naar de meest recente Azure Java SDK 2.2 voor HDInsight 3,2</td>
<td>Verplaatst naar de meest recente versie van de Azure-SDK voor Java gebruikt door het stuurprogramma WASB. De meest recente SDK heeft een paar reparaties en de releaseopmerkingen voor hetzelfde zijn beschikbaar op https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Hadoop Core</td>
<td>Alle</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP-11959</a></td>
</tr>

<tr>
<td>Verplaatsen naar HDP 2.1.15 voor HDInsight 3.1 clusters</td>
<td>Hortonworks releaseopmerkingen voor de versie vindt u <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">hier</a>.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/B</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Notities op 04-06-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.583.1575584 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.583.1575584 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.583.1575584 (2.1.12.1-0003 HDP - ongewijzigd)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Voor 502 Ongeldige gateway fout voor Storm clusters oplossen</td>
<td>Deze release worden opgelost die een fout dat dit gevolgen heeft de taak indiening API waardoor de website omlaag na het opnieuw opstarten.</td>
<td>Service</td>
<td>Storm</td>
<td>N/B</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Notities voor 01-06-2015 release van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.577.1563827 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.577.1563827 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.577.1563827 (2.1.12.1-0003 HDP - ongewijzigd))
* HDInsight 3.2.4.577.1563827 (2.2.6.0-2800 HDP - ongewijzigd)
* SDK 1.5.8


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Verschillende correcties</td>
<td>Deze release worden opgelost die fouten die zijn gerelateerd aan de inrichting van cluster.</td>
<td>Service</td>
<td>Alle clustertypen</td>
<td>N/B</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Notities op 27-05-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Andere cluster versies en SDK zijn niet als onderdeel van deze release geïmplementeerd.


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>HDP 2.2 bijwerken</td>
<td>Deze versie van HDInsight 3,2 HDP 2.2.6 bevat en levert met verschillende belangrijke correcties met HDInsight. De volledige releaseopmerkingen beschikbaar is binnen <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 Release-informatie voor</a>.</td>
<td>HDP</td>
<td>Alle clustertypen</td>
<td>N/B</td>
</tr>

<tr>
<td>Wijzigen in standaard garens Container geheugenconfiguratie</td>
<td>In deze update wordt is de standaard beschikbare geheugen naar garens containers (yarn.nodemanager.resource.memory mb en yarn.scheduler.maximum-toewijzing-mb), dat is gestart door knooppunt Manager groter, zodat 5632MB. Eerder dit is beperkt tot 4608MB, maar u kunt op basis van verschillende taak wordt uitgevoerd, de nieuwe waarde moet bieden betere betrouwbaarheid en prestaties tot de meeste taken, dus is een betere standaard. Als de gebruikelijke manier als u een kritieke afhankelijkheid voor deze geheugenconfiguratie, stel deze expliciet tijdens het maken van het cluster.</td>
<td>HDP</td>
<td>Alle clustertypen</td>
<td>N/B</td>
</tr>

<tr>
<td>Standaard Config overeenkomst voor HBase en Storm clusters</td>
<td>Deze update herstelt u Hbase en Storm clusters als u wilt dezelfde waarden van garens configuraties gebruiken als Hadoop clusters. Dit gebeurt voor overeenkomst in alle clustertypen.</td>
<td>HDP</td>
<td>HBase, Storm</td>
<td>N/B</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Notities op 20-05-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.564.1542093 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.564.1542093 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>SCP.NET EventHub ondersteuning</td>
<td>De bijgewerkte cluster-pakketten voor HDInsight Storm weer SCP.NET nieuwe functies. U hebt nu toegang tot nieuwe API's in de opbouwfunctie voor topologie die het gemakkelijker EventHubSpout of Java Spouts te gebruiken. De klant SCP.NET SDK werken met nieuwe clusters, zoals de contracten zijn bijgewerkt, moet u bijwerken. Raadpleeg het Leesmij-bestand opgenomen in het SCP.NET nuget-pakket voor meer informatie over de nieuwe API's, gebruik en release notities (inclusief correcties).</td>
<td>TEGENOVER gereedschap</td>
<td>Storm HDInsight 3,2 clusters</td>
<td>N/B</td>
</tr>

<tr>
<td>JDBC stuurprogramma bijwerken</td>
<td>Het stuurprogramma bijgewerkt naar de versie van SQL Server worden ondersteund in sqljdbc_4.1.5605.100.</td>
<td>Metastore</td>
<td>Alle</td>
<td>N/B</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Notities op 27-04-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.537.1486660 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.537.1486660 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.537.1486660 (2.1.12.0-2329 HDP - ongewijzigd)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>DLL-afhankelijkheid oplossen</td>
<td>Hiermee verwijdert u HDInsight afhankelijkheid van eenheid Test Framework.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>

<tr>
<td>Fout-oplossing voor raceauto voorwaarde</td>
<td>Een cluster verzoek nu wacht op opslag aanvraag om deel te worden geaccepteerd voordat peiling op de status maken</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Notities op 14-04-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.521.1453250 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.521.1453250 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.521.1453250 (2.1.12.0-2329 HDP - ongewijzigd)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6 WORDEN UITGEZET

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Tez correcties</td>
<td>Correcties voor Apache TEZ. 2214 en TEZ 1923 zijn opgenomen in deze release van HDI 3,2. Deze nodig zijn specifiek voor bepaalde component query's op Tez waarvoor een aanzienlijke hoeveelheid gegevens willekeurige.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ. 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Notities op 04-06/2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.521.1453250 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.521.1453250 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.521.1453250 (2.1.12.0-2329 HDP - ongewijzigd)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6 WORDEN UITGEZET

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>HDInsight .NET SDK 1.5.6 worden uitgezet</td>
<td>Updates voor het verwijderen van sommige interne klassen voor HDInsight op Linux.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>

<tr>
<td>Avro bibliotheek 1.5.6 worden uitgezet</td>
<td>Toegevoegde <b>KnownTypeAttribute</b> voor methode <b>GetAllKnownTypes</b>. Vaste NullReferenceException wanneer een type null voor GetAllKnownTypes methode is.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>

<tr>
<td>Correcties</td>
<td>Verschillende correcties voor de service</td>
<td>Service</td>
<td>Alle</td>
<td>N/B</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Notities voor 01-04-2015 release van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Mogelijkheid om te extern bureaublad referenties op Windows-clusters via .NET SDK in-/ uitschakelen</td>
<td>Programma ondersteuning voor in- of uitschakelen in Windows clusters RDP-referenties.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>Mogelijkheid om in te schakelen extern bureaublad referenties op clusters terwijl ze zijn wordt ingericht</td>
<td>Programma ondersteuning voor het externe bureaublad referenties inschakelen wanneer de cluster wordt gemaakt. Hiermee verwijdert u het proces voor het eerst inrichting van het cluster en klikt u vervolgens het inschakelen van extern bureaublad.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>Bijgewerkte Python naar punt 2.7.8</td>
<td>Bijgewerkte Python op HDInsight Clusters naar Python punt 2.7.8, die enkele belangrijke beveiliging bevat oplossingen voor HDInsight versie 2.1, 3.0, 3.1 en 3,2</td>
<td>Service</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>GARENS configuratie wijzigen</td>
<td>Gewijzigde garens configuratie yarn.resourcemanager.max voltooid-toepassingen op 1000 voor alle clustertypen voor HDInsight versie 3.1 en 3,2. Deze waarde bepaalt alleen de lijst met voltooide toepassingen in de gebruikersinterface van garens. Voor informatie over toepassingen die zijn ingediend voordat u de lijst met toepassingen die worden weergegeven op de gebruikersinterface, kunt u rechtstreeks naar de Server geschiedenis gaan.</td>
<td>GARENS</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>Het formaat van knooppunten in een cluster HBase</td>
<td>HBase clusters kunnen nu het formaat van knooppunten (omhoog en omlaag) voor HDInsight versie 3.1 en 3,2</td>
<td>Service</td>
<td>HBase</td>
<td>N/B</td>
</tr>

<tr>
<td>JDBC upgrade</td>
<td>SQL-JDBC-stuurprogramma wordt bijgewerkt naar versie sqljdbc_4.0.2206.100 voor HDInsight versie 3,2. In deze versie bevat belangrijke verbeteringen.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>JVM configuratie-update</td>
<td>Bijgewerkte JVM configuratie networkaddress.cache.ttl tot 300 seconden uit de standaardwaarde van -1 voor HDInsight versie 3.1 en 3,2. Deze configuratiewaarde bepaalt het in de cache beleid voor zoekacties succesvolle naam van de naamservice. Hiermee worden opgelost die een fout die betrekking hebben op vergroten en verkleinen HBase clusters.</td>
<td>Service</td>
<td>HBase</td>
<td>N/B</td>
</tr>

<tr>
<td>Een upgrade uitvoeren naar Azure opslag SDK for Java 2.0</td>
<td>HDInsight versie 3,2 wordt bijgewerkt als u wilt de nieuwste versie van de Azure opslag SDK voor Java gebruiken. Deze verschillende belangrijke correcties bevatten over de huidige 0.6.0 versie.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>Bijgewerkt naar Apache WASB-broncode</td>
<td>HDInsight versie 3,2 nu wordt gebruikt voor de meest recente opmaakcode voor het stuurprogramma WASB bestandssysteem van Apache Hadoop. Met deze wijziging wordt nu het stuurprogramma WASB geleverd als een afzonderlijke oppervlak. Dit is zuiver een wijziging verpakking en bevat geen wijzigingen in gedrag van het stuurprogramma WASB. De naam van deze oppervlak-bestand is hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>Bestandsnaam-updates in HDInsight 3,2 JAR</td>
<td>Deze update met HDInsight versie 3,2 bevat verschillende correcties en een paar interne potten verpakt als onderdeel van HDP zijn bijgewerkt. Houd er rekening mee dat deze oppervlak-bestanden interne aan het pakket HDP en niet voor directe gebruikt door de klanttoepassingen zijn. Toepassingen moeten hun eigen versie van de potten inpakken, zodat een upgrade naar het interne potten HDP doen klanttoepassingen niet verbreken.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/B</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Notities op 03-03-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.488.1375841 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.488.1375841 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.3.488.1375841 (2.1.10.0-2290 HDP - ongewijzigd)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-. 2340 - ongewijzigd)
* SDK 1.5.0 (ongewijzigd)

Deze versie bevat de volgende update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA</th>
</tr>


<tr>
<td>Verbeteringen van de betrouwbaarheid</td>
<td>We reparaties waarmee de service aan de nieuwe schaal beter met de verbeterde laden met betrekking tot het maken van het cluster hebt aangebracht.</td>
<td>Service</td>
<td>Alle</td>
<td>N/B</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Notities voor 18-02-2015 release van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.471.1342507 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.471.1342507 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.3.471.1342507 (2.1.10.0-2290 HDP - ongewijzigd)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-. 2340)
* SDK 1.5.0

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>HDInsight 3,2 clusters</td>
<td>Hadoop 2.6/HDP2.2 is beschikbaar met HDInsight 3,2 clusters. De presentatie bevat belangrijke updates voor alle open source onderdelen. Zie Wat is er nieuw in HDInsight en <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 Release-informatie</a>voor meer informatie.</td>
<td>Open source-software</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>HDinsight op Linux (Preview)</td>
<td>Clusters kunnen worden geïmplementeerd op Ubuntu Linux uitgevoerd. Zie aan de slag met HDInsight op Linux voor meer informatie.</td>
<td>Service</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>

<tr>
<td>Beschikbaarheid van de storm algemeen</td>
<td>Apache Storm clusters zijn algemeen beschikbaar. Voor meer informatie Zie aan de slag met Storm in HDInsight.</td>
<td>Service</td>
<td>Storm</td>
<td>N/B</td>
</tr>

<tr>
<td>VM grootte</td>
<td>Azure HDInsight is beschikbaar op meer VM bestandstypen en -grootte. HDInsight kan gebruikmaken van A2 A7 grootte die is ingebouwd voor algemene doeleinden; D-reeks knooppunten die solid-state stations (SSD) en een 60 procent snellere processor aanbevelen; en A8 en A9 puntgrootten die InfiniBand hebt ondersteuning voor snelle toegang.</td>
<td>Service</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>Cluster schaalbaarheid</td>
<td>U kunt het aantal gegevensknooppunten voor een actieve HDInsight cluster wijzigen zonder dat u moet verwijderen of opnieuw maken. Op dit moment hebben alleen voor Hadoop-Query en Apache Storm clustertypen deze mogelijkheid, maar ondersteuning voor Apache HBase clustertype snel is kunt volgen. Zie beheren HDInsight clusters voor meer informatie.</td>
<td>Service</td>
<td>Hadoop, Storm</td>
<td>N/B</td>
</tr>

<tr>
<td>Visual Studio tooling</td>
<td>Naast het volledige tooling voor Apache Storm, is de tooling voor Apache component in Visual Studio bijgewerkt Instructievoltooiing, lokale gegevensvalidatie en verbeterde ondersteuning voor foutopsporing op te nemen. Zie aan de slag met HDInsight Hadoop-hulpprogramma's voor Visual Studio voor meer informatie.</td>
<td>Gereedschap</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>

<tr>
<td>Hadoop-Connector voor DocumentDB</td>
<td>Met Hadoop-Connector voor DocumentDB, kunt u complexe aggregaties, analyse en bewerkingen uitvoeren op uw schema minder JSON-documenten die zijn opgeslagen op DocumentDB verzamelingen of op database-accounts. Zie voor meer informatie en een zelfstudie uitvoeren Hadoop-taken met DocumentDB en HDInsight.</td>
<td>Service</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>

<tr>
<td>Correcties</td>
<td>We hebben verschillende secundaire correcties voor HDInsight services aangebracht. Geen wijziging van de klant is gericht gedrag wordt verwacht.</td>
<td>Service</td>
<td>Alle</td>
<td>N/B</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Notities op 06-02-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.463.1325367 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.463.1325367 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK N/B

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Correcties</td>
<td>We hebben verschillende secundaire correcties voor HDInsight services aangebracht. Geen wijziging van de klant is gericht gedrag wordt verwacht.</td>
<td>Service</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>HDP 2.1 onderhoud bijwerken</td>
<td>HDInsight 3.1 wordt bijgewerkt als u wilt implementeren HDP 2.1.10.0. Zie <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">releaseopmerkingen HDP-2.1.10</a>voor meer informatie. </td>
<td>Open source-software</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>Binaire HDP-updates</td>
<td>Er zijn een paar oppervlak-bestanden in HBase voor welke namen zijn bijgewerkt. Deze oppervlak-bestanden worden intern gebruikt door HBase, zodat deze niet verwacht wordt dat klanten een afhankelijkheid op de namen van deze oppervlak-bestanden hebben. Hierbij:
<ul>
<li>./lib/jetty-6.1.26.hwx.jar</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.jar</li>
<li>./lib/jetty-Util-6.1.26.hwx.jar</li>
</ul>
</td>
<td>Open source-software</td>
<td>HBase</td>
<td>N/B</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Notities op 29-1-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.455.1309616 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.455.1309616 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.2.455.1309616 (2.1.9.0-2196 HDP - ongewijzigd)
* SDK N/B

Deze versie bevat de volgende update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Type (bijvoorbeeld Hadoop, HBase of Storm) cluster</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>

<td>Correcties</td>
<td>We hebben enkele belangrijke correcties ter verbetering van de betrouwbaarheid van de Clusters HDInsight tijdens Azure upgrades aangebracht.</td>
<td>Service</td>
<td>Alle</td>
<td>N/B</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Notities op 5-1-2015-versie van HDInsight

De nummers van de volledige versie van HDInsight clusters geïmplementeerd in deze versie:

* HDInsight 2.1.10.420.1246118 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.420.1246118 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.2.420.1246118 (2.1.9.0-2196 HDP - ongewijzigd)


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Voorbeelden voor Twitter trendanalyse en Mahout gebaseerde film aanbevelingen</td>
<td><p>In deze release heeft de HDInsight Query-console twee aanvullende voorbeelden:</p>

<p><b>Twitter trendanalyse</b><br>
Openbare API's dat is verstrekt door sites zoals Twitter zijn handig gegevensbron voor het analyseren en informatie over populaire trends. In deze zelfstudie leert u hoe u de component gebruiken om een lijst met Twitter-gebruikers die de meeste tweets met een bepaald woord verzonden. </p>

<p><b>Mahout film aanbeveling</b><br>
Apache Mahout is een Apache Hadoop-machine learning-bibliotheek. Mahout bevat algoritmen voor gegevensverwerking (zoals filteren, indeling en cluster). In deze zelfstudie gebruikt u een aanbeveling-engine om te genereren film aanbevelingen op basis van films die uw vrienden hebt gezien.</p></td>
<td>Query-console</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>

<tr>
<td>Wijzigen in de standaardwaarde voor component configuratie: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Deze configuratie grootte is van toepassing op automatisch geconverteerde kaart joins. De waarde geeft de som van de grootte van de tabellen die kunnen worden omgezet in hash kaarten die in het geheugen passen. In een eerdere versie verhoogd deze waarde van de standaardwaarde van 10 MB naar 128 MB. De nieuwe waarde 128 MB is echter als gevolg van taken mislukt einddatum onvoldoende geheugen. Deze release hersteld de standaardwaarde terug naar 10 MB. Klanten kunnen nog steeds kiezen om te negeren deze waarde gemaakt, op basis van hun query's en de grootte van de tabel. Zie voor meer informatie over deze instelling en hoe u deze overschrijven <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Conversie automatisch deelnemen aan optimaliseren</a> in Hortonworks documentatie. </p></td>
<td>Component</td>
<td>Hadoop, Hbase</td>
<td>N/B</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Notities op 23-12-2014 versie van HDInsight

De volledige versienummers voor HDInsight clusters geïmplementeerd in deze versie zijn:

* HDInsight 2.1.10.420.1246783 (HDP versie ongewijzigd)
* HDInsight 3.0.6.420.1246783 (HDP versie ongewijzigd)
* HDInsight 3.1.1.420.1246783 (HDP versie ongewijzigd)

Deze versie bevat de volgende update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Fouten bij het maken van door onregelmatige cluster vanwege overtollige laden</td>
<td><p>Verbeterde algoritme voor het downloaden van HDP-pakketten tijdens het maken van cluster kunt krachtiger afhandeling van fouten vanwege overtollige laden.</p></td>
<td>Service</td>
<td>Hadoop, Hbase, Storm</td>
<td>N/B</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Notities voor 18-12-2014 release van HDInsight

Deze versie bevat de volgende component update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Cluster aanpassing algemeen Avalability</a></td>
<td><p>Aanpassing biedt de mogelijkheid om u om aan te passen Azure HDInsight clusters met projecten die beschikbaar in het Apache Hadoop-systeem zijn. Met deze nieuwe functie, kunt u experimenteren en implementeren van Hadoop projecten met Azure HDInsight. Dit is ingeschakeld via de functie **Scriptactie** , die Hadoop clusters in willekeurige manieren wijzigen kunt met behulp van aangepaste scripts. Deze aanpassing is beschikbaar op alle typen HDInsight clusters inclusief Hadoop, HBase en Storm. Voorbeelden van de kracht van deze mogelijkheid hebben we het proces voor het installeren van de populaire <a href = "hdinsight-hadoop-spark-install.md" target="_blank">elektrische</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>en <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> modules beschreven. Deze release wordt ook de mogelijkheid voor klanten om op te geven hun actie aangepast script via de portal Azure bevat richtlijnen en aanbevolen procedures over het maken van aangepast scriptacties met methoden en bevat richtlijnen over het testen van de scriptactie. </p></td>
<td>Beschikbaarheid van de functie algemeen</td>
<td>Alle</td>
<td>N/B</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Notities op 05-12-2014 versie van HDInsight

De volledige versienummers voor HDInsight clusters geïmplementeerd in deze versie zijn:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK n/b

Deze versie bevat de volgende component-updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td>Fout fix: door onregelmatige fout bij het toevoegen van grote aantallen partities aan een tabel in een DDL component </td>
<td><p>Als er een fout met de verbinding met de component metastore database wanneer u een groot aantal partities toevoegt aan een tabel component, wordt de DDL component kan mislukken. De volgende-instructie is zichtbaar in het foutenlogboek van de component als deze fout treedt op: </p><p>"Fout [main]: ql. Stuurprogramma (SessionState.java:printError(547)) - is mislukt: fout bij uitvoering, geretourneerde code 1 van org.apache.hadoop.hive.ql.exec.DDLTask. MetaException (message:java.lang.RuntimeException: commitTransaction heet maar openTransactionCalls = 0. Hiermee waarschijnlijk wordt aangeduid dat er ongebalanceerde oproepen naar openTransaction/commitTransaction)"</p></td>
<td>Component</td>
<td>Hadoop, Hbase</td>
<td>COMPONENT-482 (dit is een interne JIRA, zodat deze extern kan niet worden vermeld. Vermeld hier voor verwijzing.)</td>
</tr>

<tr>
<td>Fout fix: incidentele loopt vast in de Query-Console HDInsight</td>
<td>Als dit gebeurt, de volgende instructie ziet u in het logboek WebHCat voor de taak van WebHCat startprogramma voor apps: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): kan niet worden geladen geschiedenisbestand {wasb url naar het geschiedenisbestand} "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>COMPONENT-482 (dit is een interne JIRA, zodat deze extern kan niet worden vermeld. Vermeld hier voor verwijzing.)</td>
</tr>

<tr>
<td>Fout fix: incidentele Prikker in latentie van Hbase query's</td>
<td>Als dit gebeurt, zult gebruikers een incidentele Prikker van 3 seconden in de latentie van Hbase query's. </td>
<td>HDInsight Cluster Gateway</td>
<td>HBase</td>
<td>N/B</td>
</tr>

<tr>
<td>Bestand naamwijzigingen HDP JAR</td>
<td>Cluster versie 3.0, er een paar wijzigingen in de interne oppervlak-bestanden geïnstalleerd door HDP voor HDI. jetty-6.1.26.jar is vervangen door jetty-6.1.26.hwx.jar. jetty-util-6.1.26.jar is vervangen door jetty-util-6.1.26.hwx.jar. Deze wijzigingen toepassen op Hadoop, Mahout, WebHCat en Oozie projecten.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>N/B</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Notities op 11-21-2014 versie van HDInsight

De volledige versienummers voor HDInsight clusters geïmplementeerd in deze versie zijn:

* HDInsight 2.1.9.382.1169709 (niet veranderd ten opzichte van 14-11-2014)
* HDInsight 3.0.5.382.1169709 (niet veranderd ten opzichte van 14-11-2014)
* HDInsight 3.1.1.382.1169709 (niet veranderd ten opzichte van 14-11-2014)
* HDINsight SDK 1.4.0

Deze versie bevat de volgende component-updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td>Toegang tot toepassingslogboeken aan de</td>
<td>De mogelijkheid via programmacode opsommen toepassingen die zijn uitgevoerd op uw clusters en voor het downloaden van relevante toepassingsspecifieke of container / regiospecifieke Logboeken om u te helpen foutopsporing problematisch-toepassingen.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>

<tr>
<td>Mogelijkheid om op te geven van de regionaam van de in IHdInsightClient.DeleteCluster </td>
<td>De SDK van Azure HDInsight biedt de mogelijkheid om op te geven van de naam van een gebied wanneer u een **DeleteCluster**gebruikt. Hiermee kunt klanten die twee resources met dezelfde naam uit verschillende regio's en had kan niet worden verwijderd van een van de blokkering opheffen.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>Het object **ClusterDetails** geeft als resultaat een **DeploymentID** veld dat een unieke id voor het cluster vertegenwoordigt. Dit is gegarandeerd unieke in cluster maken pogingen met dezelfde naam.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/B</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Notities op 14-11-2014 versie van HDInsight

De volledige versienummers voor HDInsight clusters geïmplementeerd in deze versie zijn:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Deze versie bevat de volgende nieuwe functies, component updates en correcties.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td>Scriptactie (Preview)</td>
<td>Voorbeeld van de functie voor het aanpassen van cluster waarmee de wijziging van Hadoop-clusters in willekeurige manieren met behulp van aangepaste scripts. Gebruikers kunnen met deze functie experimenteren met en implementeren van projecten die beschikbaar bij het Apache Hadoop-systeem met Azure HDInsight clusters zijn. Deze functie aanpassing is beschikbaar op alle typen HDInsight kolomgroepen inclusief Hadoop, HBase en Storm.</td>
<td>Nieuwe functie</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>Vooraf gedefinieerde taken voor Azure websites en opslag log-analyse</td>
<td>De Query-Console HDInsight heeft een galerie met aan de slag die ondersteuning biedt voor oplossingen die u kunt gebruiken op uw gegevens of op voorbeeldgegevens.
<p>**Oplossingen die bezig bent met uw gegevens**:<br>
We hebt taken voor een deel van de meest voorkomende gegevensanalyse-scenario's op te geven van een beginpunt voor het maken van uw eigen oplossingen gemaakt. U kunt uw gegevens aan de taak gebruiken om te zien hoe dit werkt. Wanneer u klaar bent, gebruikt u wat u hebt geleerd maken van een oplossing die is gebaseerd op de vooraf gedefinieerde taak.</p>
<p>**Oplossingen die bezig bent met voorbeeldgegevens**:<br>
Informatie over het werken met HDInsight door enkele eenvoudige scenario's (zoals het analyseren van weblogs en sensorgegevens) doorlopen. U leert hoe u deze gegevens analyseren met HDInsight en hoe u andere toepassingen en services op deze gegevens kunt aansluiten. Gegevens visualiseren door te verbinden met Microsoft Excel wordt een voorbeeld van de kracht van deze methode.</p></td>
<td>Query-console</td>
<td>Hadoop</td>
<td>N/B</td>
</tr>

<tr>
<td>Geheugenlek fix in Templeton</td>
<td>Een geheugen zijn verdwenen in Templeton die beïnvloed klanten die hebt u er langdurige clusters of 100s van project zijn indienen vraagt om dat een tweede is opgelost. Het probleem manifested als Templeton 5xx fouten en de tijdelijke oplossing is om de service opnieuw te starten. De tijdelijke oplossing is niet meer nodig.</td>
<td>Templeton</td>
<td>Alle</td>
<td>https://Issues.apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Opmerking**: U kunt de nieuwe functies die beschikbaar zijn aangebracht door cluster aanpassing zien, de procedures met de scriptactie elektrische en R modules installeren op een cluster hebt is beschreven. Zie voor meer informatie:

* [Installeren en gebruiken van een 1.0 op HDInsight clusters](hdinsight-hadoop-spark-install.md)
* [Installeren en gebruiken van R op HDInsight Hadoop clusters](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Notities op 11-07/2014 versie van HDInsight

De volledige versienummers voor HDInsight clusters die geïmplementeerd in deze versie zijn:

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Deze versie bevat de volgende component-updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>Deze versie is gebaseerd op Hortonworks gegevens Platform (HDP) 2.1.7. Zie <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">Releaseopmerkingen voor HDP 2.1.7</a>voor meer informatie.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/B</td>
</tr>

<tr>
<td>GARENS tijdlijn Server</td>
<td>De garens tijdlijn Server (ook wel bekend als de algemene geschiedenis toepassingsserver) is standaard ingeschakeld. De tijdlijn Server bevat algemene informatie over voltooide toepassingen (zoals toepassings-ID, toepassingsnaam toepassingsstatus, toepassing indiening tijd en toepassing voltooiingstijd).

Deze toepassingsinformatie kan worden opgehaald uit het hoofd knooppunt via de URI http://headnodehost:8188 of door de garens-opdracht uit te voeren: garens toepassing – – appStates een lijst met alle.

Deze informatie kan ook worden opgehaald extern Hoewel een REST API bij https://{ClusterDnsName}. azurehdinsight.NET/ws/V1/applicationhistory/.

Zie <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Garens tijdlijn Server</a>voor meer informatie.</td>
<td>Service, garens</td>
<td>Hadoop, HBase</td>
<td>N/B</td>
</tr>

<tr>
<td>Cluster implementatie-ID</td>
<td>Beginnen met SDK versie 1.3.3.1.5426.29232, gebruikers hebben toegang tot een unieke ID voor elk cluster, dat is uitgegeven HDInsight. Hierdoor kunnen klanten om te bepalen wanneer een DNS-naam wordt opnieuw wordt gebruikt unieke exemplaren van clusters horizontaal maken of neerzetten scenario's.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/B</td>
</tr>
</table>
<br>

**Opmerking**: de fout waardoor het nummer van de volledige versie niet wordt weergegeven in de portal of worden geretourneerd door de SDK of door Windows PowerShell is verholpen in deze release.

## <a name="notes-for-10152014-release"></a>Notities voor de versie van 10/15/2014

Deze release hotfix opgelost een geheugen zijn verdwenen in Templeton die de dikke gebruikers van Templeton beïnvloed. In sommige gevallen ziet de gebruikers die Templeton intensief fouten manifested als 500 foutcodes, omdat de aanvragen geen onvoldoende geheugen om uit te voeren. De tijdelijke oplossing beschikbaar voor dit probleem is de Templeton-service opnieuw starten. Dit probleem is opgelost.


## <a name="notes-for-1072014-release"></a>Notities voor 7-10-2014 release

* Bij gebruik van Ambari eindpunt, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", het veld *hostnaam* geeft als resultaat de FQDN-naam (Fully Qualified Domain Name) van het knooppunt in plaats van alleen de hostnaam. In plaats van "**headnode0**" wordt geretourneerd, krijgt u bijvoorbeeld de FQDN-naam '**headnode0. { ClusterDNS} .azurehdinsight .net**". Deze wijziging is vereist om scenario's waarin meerdere clustertypen (zoals HBase en Hadoop) kunnen worden geïmplementeerd in één virtueel netwerk te vereenvoudigen. Dit gebeurt, bijvoorbeeld wanneer u HBase als een back-enddatabase platform voor Hadoop.

* We hebben opgegeven nieuwe geheugeninstellingen voor de standaard-implementatie van het cluster HDInsight. Vorige standaardinstellingen geheugen niet voldoende account voor de richtlijnen voor het aantal CPU cores wordt geïmplementeerd. Deze nieuwe geheugeninstellingen moeten geven betere standaardwaarden (volgens Hortonworks aanbevelingen). Zie de SDK verwijzing-documentatie over het wijzigen van de configuratie van het cluster als u wilt deze wijzigen. De nieuwe geheugeninstellingen die worden gebruikt door de standaard 4 CPU core (8 container) HDInsight cluster zijn gespecificeerde in de volgende tabel. (De waarden die wordt gebruikt voordat u deze release zijn ook beschikbaar getallen.)

<table border="1">
<tr><th>Onderdeel</th><th>Geheugentoewijzing</th></tr>
<tr><td> yarn.scheduler.minimum-toewijzing</td><td>768 MB (eerder 512 MB)</td></tr>
<tr><td> yarn.scheduler.maximum-toewijzing</td><td>6144 MB (ongewijzigd)</td></tr>
<tr><td>yarn.nodemanager.resource.Memory</td><td>6144 MB (ongewijzigd)</td></tr>
<tr><td>mapreduce.map.Memory</td><td>768 MB (eerder 512 MB)</td></tr>
<tr><td>mapreduce.map.Java.opts</td><td>(eerder - Xmx410m) =-Xmx512m kiest</td></tr>
<tr><td>mapreduce.reduce.Memory</td><td>1.536 MB (eerder 1024 MB)</td></tr>
<tr><td>mapreduce.reduce.Java.opts</td><td>(eerder - Xmx819m) =-Xmx1024m kiest</td></tr>
<tr><td>yarn.app.mapreduce.AM.resource</td><td>768 MB (eerder 1024 MB)</td></tr>
<tr><td>yarn.app.mapreduce.AM.Command</td><td>(eerder - Xmx819m) =-Xmx512m kiest</td></tr>
<tr><td>mapreduce.Task.IO.Sort</td><td>256 MB (eerder 200 MB)</td></tr>
<tr><td>tez.AM.resource.Memory</td><td>1.536 MB (ongewijzigd)</td></tr>

</table><br>

Zie voor meer informatie over de configuratie-instellingen van het geheugen die wordt gebruikt door garens en MapReduce op het Hortonworks gegevens Platform die wordt gebruikt door HDInsight, [HDP geheugen configuratie-instellingen bepalen](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks is ook beschikbaar voor een hulpmiddel om te berekenen van de juiste geheugeninstellingen.

Met betrekking tot de PowerShell Azure en het foutbericht HDInsight SDK: '*Cluster niet is geconfigureerd voor HTTP-services toegang*':

* Deze fout is een bekend [compatibiliteitsprobleem](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) dat een verschil in de versie van de HDInsight SDK of Azure PowerShell en de versie van het cluster kan te wijten. Clusters gemaakt op 8/15 of hoger hebben ondersteuning voor nieuwe inrichten mogelijkheid in virtuele netwerken. Maar deze functie is niet correct geïnterpreteerd door oudere versies van de HDInsight SDK of Azure PowerShell. Het resultaat is een fout in een bepaalde taak indiening bewerkingen. Als u HDInsight SDK-API's of Azure PowerShell-cmdlets (**Gebruik AzureRmHDInsightCluster** of **Roep-AzureRmHDInsightHiveJob**) om in te dienen taken, deze bewerkingen kunnen mislukken met het foutbericht '*Cluster <clustername> niet is geconfigureerd voor HTTP services toegang*. " Of (afhankelijk van de bewerking), andere foutberichten worden weergegeven, zoals "*kan geen verbinding met cluster*" mogelijk ontvangt.

* Deze compatibiliteitsproblemen worden omgezet in de meest recente versie van de HDInsight SDK en Azure PowerShell. Het is raadzaam de HDInsight SDK bijwerken naar versie 1.3.1.6 of hoger en Azure PowerShell-hulpmiddelen voor naar versie 0.8.8 of hoger. U kunt toegang krijgen voor de meest recente HDInsight SDK van [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) en de hulpprogramma's voor PowerShell Azure bij [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Notities voor 9-12-2014 release HDInsight 3.1

* Deze versie is gebaseerd op Hortonworks gegevens Platform (HDP) 2.1.5. Zie voor een lijst met de fouten in deze release, de pagina [vast in deze Release](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) op de site Hortonworks.
* Klik in de map varken bibliotheken is het bestand "avro-mapred-1.7.4.jar" gewijzigd in "avro-mapred-1.7.4-hadoop2.jar." De inhoud van dit bestand bevat een secundaire bug correctie vaste is. Het wordt aanbevolen dat klanten geen een directe afhankelijkheid op de naam van het bronbestand oppervlak maken-einden voorkomen wanneer de namen van bestanden worden gewijzigd.


## <a name="notes-for-8212014-release"></a>Notities voor 8-21-2014 release

* We wilt de volgende WebHCat configuratie (Component-7155), waarin de standaard geheugenlimiet voor een taak van de controller Templeton wordt ingesteld als 1 GB toevoegen. (De vorige standaardwaarde is 512 MB).

     templeton.Mapper.Memory.MB (= 1024)

    * Deze wijziging-adressen voor de volgende fout die bepaalde component query's had uitgevoerd omdat de onderste geheugenlimieten: "De Container wordt uitgevoerd voorbij fysiek geheugenlimieten."
    * Als u wilt terugkeren naar de oude standaardwaarden, kunt u deze configuratiewaarde aan 512 via Azure PowerShell tijdens het maken van een cluster instellen met behulp van de volgende opdracht uit:

        Toevoegen AzureRmHDInsightConfigValues-Core@{"templeton.mapper.memory.mb"="512";}


* De hostnaam van de rol zookeeper is gewijzigd in *zookeeper*. Dit geldt voor naamresolutie binnen het cluster, maar deze niet van invloed op externe REST API's. Als u onderdelen met de naam van de host *zookeepernode* hebt, moet u bijgewerkt om ze als nieuwe naam wilt gebruiken. De nieuwe namen voor de drie zookeeper knooppunten zijn:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase versie ondersteuningsmatrix wordt bijgewerkt. Alleen HDInsight versie 3.1 (HBase versie 0,98) wordt ondersteund voor voor HBase werkbelasting. Versie 3.0 (dat is beschikbaar voor preview) wordt niet ondersteund zwevend doorsturen.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Opmerkingen over clusters die zijn gemaakt voordat u 15-8-2014

Een foutbericht Azure PowerShell of HDInsight SDK "Cluster <clustername> niet is geconfigureerd voor HTTP services toegang" (of afhankelijk van de bewerking andere foutberichten, zoals: "Kan geen verbinding met cluster") vanwege een versie verschil tussen Azure PowerShell of de HDInsight SDK en een cluster kan zich voordoen. Clusters gemaakt op 8/15 of hoger hebben ondersteuning voor nieuwe inrichten mogelijkheid in virtuele netwerken. Deze functie is niet correct in oudere versies van de Azure PowerShell of de SDK HDInsight, waardoor fouten taak indiening bewerkingen voor beschouwd. Als u HDInsight SDK-API's of Azure PowerShell-cmdlets (zoals gebruik AzureRmHDInsightCluster of roep-AzureRmHDInsightHiveJob) gebruikt om in te dienen taken, wordt deze bewerkingen kunnen mislukken met een van de foutberichten beschreven.

Deze compatibiliteitsproblemen worden omgezet in de meest recente versie van de HDInsight SDK en Azure PowerShell. Het is raadzaam de HDInsight SDK bijwerken naar versie 1.3.1.6 of hoger en Azure PowerShell-hulpmiddelen voor naar versie 0.8.8 of hoger. U kunt de toegang tot de meest recente HDInsight SDK krijgen via [NuGet][nuget-link]. U kunt toegang tot de Azure PowerShell-hulpmiddelen met [Microsoft Web Platform Installer][webpi-link].


## <a name="notes-for-7282014-release"></a>Notities voor 28-7-2014 release

* **Beschikbaar in de nieuwe gebieden HDInsight**: We HDInsight geografische aanwezigheid uitgevouwen voor drie gedeelten. HDInsight klanten kunnen clusters maken in deze regio's:
    * Oost-Azië
    * Centraal Noord-Amerikaanse
    * Zuid-Central US
* HDInsight versie 1,6 (HDP 1.1 en Hadoop 1.0.3) en HDInsight versie 2.1 (HDP1.3 en Hadoop 1.2) zijn die wordt verwijderd uit de Azure-portal. U kunt doorgaan met Hadoop clusters voor deze versies maken met behulp van de Azure PowerShell-cmdlet, [Nieuw-AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) of met behulp van de [HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx). Raadpleeg de pagina [HDInsight component versiebeheer](hdinsight-component-versioning.md) voor meer informatie.
* Hortonworks gegevens Platform (HDP) wordt gewijzigd in deze release:

<table border="1">
<tr><th>HDP</th><th>Wijzigingen</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Geen wijzigingen</td></tr>
<tr><td>HDP 2.0 / HDI 3.0</td><td>Geen wijzigingen</td></tr>
<tr><td>HDP 2.1 / HDI 3.1</td><td>zookeeper: [3.4.5.2.1.3.0-1948]-[3.4.5.2.1.3.2-0002] ></td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Notities voor de 24-6-2014 release

Deze versie bevat verbeteringen in de service HDInsight:

* **Beschikbaarheid van de HDP 2.1**: HDInsight 3.1 (die bevat HDP 2.1) in het algemeen beschikbaar is en de standaardversie voor nieuwe clusters.
* **HBase – Azure verbeteringen in de portal**: We stapt HBase clusters beschikbaar in de Preview-versie. U kunt HBase clusters maken van de portal met een paar muisklikken. 

Met HBase, kunt u allerlei realtime werkbelasting bouwen op HDInsight, van interactieve websites die werken met grote gegevenssets naar services sensor en telemetrielogboek gegevens uit miljoenen eindpunten opslaat. De volgende stap zou om de gegevens in deze werkbelasting met Hadoop-taken te analyseren en dit is mogelijk in HDInsight via Azure PowerShell en de component cluster dashboard.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout vooraf geïnstalleerd op HDInsight 3.1

 [Mahout](http://hortonworks.com/hadoop/mahout/) is vooraf geïnstalleerd op HDInsight 3.1 Hadoop kolomgroepen zodat u kunt taken Mahout zonder dat u extra clusterconfiguratie uitvoeren. Bijvoorbeeld: u kunt externe in een Hadoop-cluster met behulp van Remote Desktop Protocol (RDP) en zonder extra stappen, kunt u de volgende Hallo wereld Mahout-opdracht uitvoeren:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Zie de documentatie van het [Breiman voorbeeld](https://mahout.apache.org/users/classification/breiman-example.html) op de website van Apache Mahout voor een gedetailleerde uitleg van deze procedure.


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Query's component kunnen Tez in HDInsight 3.1 gebruiken

Component 0,13 is beschikbaar in HDInsight 3.1, en kan worden uitgevoerd van query's met Tez, die kan worden gebruikt voor aanzienlijke prestatieverbeteringen ten.
Tez is niet inschakelen al dan niet standaard voor component query's. Als u wilt gebruiken, moet u ervoor kiezen. U kunt Tez inschakelen door het volgende codefragment uit te voeren:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks heeft een gedetailleerd overzicht van component query prestatieverbeteringen met Tez gepubliceerd bezorgd in standard benchmarks. Zie [Apache benchmarks component 13 voor Enterprise Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/)voor meer informatie.

Zie voor meer informatie over het gebruik van de component met Tez, [component op Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Algemene beschikbaarheid
Met de versie van HDInsight op Hadoop 2.2, heeft Microsoft gesteld HDInsight beschikbaar in alle primaire regio's waar Azure beschikbaar is. Specifiek, hebben de west Europa en Zuidoost-Azië datacenters online is gebracht. Hiermee worden klanten om clusters in een datacenter dat zich en mogelijk in een zone van soortgelijke nalevingsvereisten te vinden.


###<a name="dos--donts-between-cluster-versions"></a>Richtlijnen voor effectieve en de tussen cluster versies

**Oozie metastores gebruikt met een cluster HDInsight 3.1 zijn niet compatibel met HDInsight 2.1 kolomgroepen en ze kunnen niet worden gebruikt met deze eerdere versie**.

Een aangepaste Oozie metastore database geïmplementeerd met een cluster HDInsight 3.1 worden niet gebruikt met een cluster HDInsight 2.1. Dit is de hoofdletters/kleine letters, zelfs als de metastore afkomstig met een cluster HDInsight 2.1 is. Dit scenario wordt niet ondersteund omdat het schema metastore wordt bijgewerkt wanneer u gebruikt met een cluster HDInsight 3.1, zodat u niet langer compatibel is met de metastore die is vereist door de clusters HDInsight 2.1. Elke poging een Oozie metastore dat is gebruikt met een cluster HDInsight 3.1 opnieuw te worden het cluster HDInsight 2.1 onbruikbaar weergegeven.

**Oozie metastores kan niet worden gedeeld met clusters.**

Oozie metastores zijn gekoppeld aan specifieke clusters en ze in clusters kunnen niet worden gedeeld.

###<a name="breaking-changes"></a>Wijzigingen verbreken

**Syntaxis van de aanduiding**: alleen de "wasbs: / / ' syntaxis wordt ondersteund in HDInsight 3.1 en 3.0 clusters. De oudere "luchtverzadigingswaarde: / / ' syntaxis wordt ondersteund in HDInsight 2.1 en 1,6 clusters, maar deze niet wordt ondersteund in HDInsight 3.1 of 3.0 clusters. Dit betekent dat alle taken verzonden naar een 3.1 HDInsight of 3.0 cluster die expliciet gebruiken de "luchtverzadigingswaarde: / / ' syntaxis, mislukt. De "wasbs: / / ' syntaxis in plaats daarvan moet worden gebruikt. Bovendien de taken die zijn verzonden naar een 3.1 HDInsight of 3.0 clusters die zijn gemaakt met een bestaande metastore expliciete verwijzingen naar bronnen gebruikt met de ' luchtverzadigingswaarde: / / ' syntaxis, mislukt. Deze metastores moet opnieuw worden gemaakt met de ' wasbs: / / ' syntaxis van de adres-resources.


**Poorten**: de poorten die worden gebruikt door de service HDInsight zijn gewijzigd. Er zijn de poortnummers die zijn die wordt gebruikt in het poortbereik kortstondige van het Windows-besturingssysteem. Poorten worden automatisch een vooraf gedefinieerde kortstondige datumbereik voor tijdelijk Internet protocol gebaseerde communicatie toegewezen. De nieuwe set toegestane Hortonworks gegevens Platform (HDP) service-poortnummers zijn buiten dit bereik om te voorkomen dat zich voordoen tijdens de conflicten die kunnen optreden met de poorten die worden gebruikt door services worden uitgevoerd op het hoofd knooppunt. De nieuwe poortnummers zou geen recente wijzigingen veroorzaken. Dit zijn de getallen die worden gebruikt:

 **HDInsight 1,6 (HDP 1.1)**
<table border="1">
<tr><th>Naam</th><th>Waarde</th></tr>
<tr><td>DFS.http.Address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.Address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.Tracker.http.Address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.server.http.Address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

 **HDInsight 3.1 en 3.0 (HDP 2.1 en 2.0)**
<table border="1">
<tr><th>Naam</th><th>Waarde</th></tr>
<tr><td>DFS.namenode.HTTP-adres</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.https-adres</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.HTTP-adres</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.WebApp.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Afhankelijkheden

De volgende afhankelijkheden zijn toegevoegd in HDInsight 3.x (HDP2.x):

* guice-servlet
* optiq core
* javax.inject
* activering
* jsr305
* geronimo-jaspic_1.0_spec
* jul-naar-slf4j
* Java-xmlbuilder
* ant
* Commons-compileerprogramma
* jdo-api
* Commons-math3
* paranamer
* jaxb-impl
* stringtemplate
* eigenbase-xom
* Jersey-servlet
* Commons-Raad
* jaxb-api
* jetty-alle-server
* janino
* xercesImpl
* optiq-avatica
* jta
* eigenbase-eigenschappen
* groovy-alle
* hamcrest core
* e-mail
* linq4j
* jpam
* Jersey-client
* aopalliance
* geronimo-annotation_1.0_spec
* ant-startprogramma voor apps
* Jersey-guice
* XML-API 's
* stax-api
* Asm-commons
* Asm-structuur
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni-alle
* de migratiesnelheid
* jettison
* treffende java
* jetty-alle
* Commons-dbcp

De volgende afhankelijkheden niet langer bestaat niet in HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* klimop
* aspectjrt
* JSON
* Core
* jdo2-api
* avro-mapred
* datanucleus-enhancer
* JSP
* Commons-logboekregistratie-api
* Commons-wiskundige
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* treffende

###<a name="version-changes"></a>Wijzigingen in versie

De volgende versiewijzigingen zijn aangebracht tussen HDInsight 2.x (HDP1.x) en HDInsight 3.x (HDP2.x):

* aan de doelstellingen core: [2.1.2]-[3.0.0] >
* derbynet: [10.4.2.0]-[10.10.1.1] >
* datanucleus: ['rdbms-3.0.8'] -> ['rdbms-3.2.9']
* Jasper-compileerprogramma: [5.5.12]-[5.5.23] >
* log4j: ['en met 1.2.15', '1.2.16'] -> ['1.2.16', '1.2.17']
* derbyclient: [10.4.2.0]-[10.10.1.1] >
* httpcore: [4.2.4]-[4.2.5] >
* hsqldb: [1.8.0.10]-[2.0.0] >
* jets3t: [0.6.1]-[0.9.0] >
* protobuf-java: [2.4.1]-[2.5.0] >
* derby: [10.4.2.0]-[10.10.1.1] >
* Jasper: ['runtime-5.5.12'] -> ['runtime-5.5.23']
* Commons-daemon: [1.0.1]-[1.0.13] >
* datanucleus core: [3.0.9]-[3.2.10] >
* datanucleus-api-jdo: [3.0.7]-[3.2.6] >
* zookeeper: [3.4.5.1.3.9.0-01320]-[3.4.5.2.1.3.0-1948] >
* bonecp: [0.7.1.RELEASE] -> ['
* 0.8.0.RELEASE']


### <a name="drivers"></a>Stuurprogramma 's
Het stuurprogramma Java Database Connnectivity (JDBC) voor SQL Server wordt intern door HDInsight gebruikt en wordt niet gebruikt voor externe bewerkingen. Als u wilt verbinden met HDInsight via Open Database Connectivity (ODBC), voert u de component ODBC-stuurprogramma. Zie [Excel verbinden met HDInsight via het Microsoft Component ODBC-stuurprogramma](hdinsight-connect-excel-hive-odbc-driver.md)voor meer informatie.


### <a name="bug-fixes"></a>Correcties

We hebben de volgende HDInsight-versies met verschillende correcties vernieuwd in deze release:

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)


## <a name="hortonworks-release-notes"></a>Releaseopmerkingen voor Hortonworks

Releaseopmerkingen voor de Hortonworks gegevens Platforms (HDPs) die worden gebruikt door HDInsight versie clusters zijn beschikbaar op de volgende locaties:

* HDInsight versie 3.1 gebruikt een Hadoop-verdeling op basis van het [Platform voor Hortonworks 2.1.7][hdp-2-1-7]. Dit is het standaard Hadoop cluster gemaakt wanneer u met behulp van de Azure portal na 11-7-2014. HDInsight 3.1 clusters die zijn gemaakt voordat 11-7-2014 zijn op basis van de [Hortonworks gegevens Platform 2.1.1][hdp-2-1-1]

* HDInsight versie 3.0 gebruikt een Hadoop-verdeling op basis van de [Hortonworks gegevens Platform 2.0][hdp-2-0-8].

* HDInsight versie 2.1 gebruikt een Hadoop-verdeling op basis van de [Hortonworks gegevens Platform 1.3][hdp-1-3-0].

* HDInsight versie 1,6 gebruikt een Hadoop-verdeling op basis van de [Hortonworks gegevens Platform 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
