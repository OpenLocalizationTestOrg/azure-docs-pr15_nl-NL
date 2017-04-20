<properties
 pageTitle="Begrijpen en oplossen van fouten WebHCat op HDInsight"
 description="Leer hoe naar informatie over veelvoorkomende fouten die het resultaat van WebHCat op HDInsight en hoe u ze kunt oplossen."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Begrijpen en oplossen van fouten die zijn ontvangen van WebHCat (Templeton), klik op HDInsight

Bij gebruik van WebHCat (voorheen bekend als Templeton,) om te werken met HDInsight, ontvangt u fouten. In dit document bevat richtlijnen op veelvoorkomende fouten – waarom ze voorkomen en wat u kunt doen om ze kunt oplossen.

##<a name="what-is-webhcat"></a>Wat is WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is een REST API voor [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), een tabel en opslag management laag voor Hadoop. WebHCat standaard HDInsight clusters is ingeschakeld en wordt gebruikt met verschillende hulpprogramma's om te verzenden van taken, taakstatus, enzovoort downloaden zonder het aanmelden bij het cluster.

##<a name="modifying-configuration"></a>Configuratie wijzigen

> [AZURE.IMPORTANT] Verscheidene van de fouten die in dit document optreden omdat geconfigureerde maximaal is overschreden. Wanneer de stap resolutie vermeldingen dat u een waarde kunt wijzigen, moet u een van de volgende opties om uit te voeren naar de wijzigen:

* Voor **Windows** -clusters: een scriptactie gebruiken voor het configureren van de waarde tijdens het maken van cluster. Zie [ontwikkelen scriptacties](hdinsight-hadoop-script-actions.md)voor meer informatie.

* Voor **Linux** clusters: gebruik Ambari (web of REST API) om de waarde te wijzigen. Zie [beheren HDInsight Ambari gebruiken](hdinsight-hadoop-manage-ambari.md) voor meer informatie

###<a name="default-configuration"></a>Standaardconfiguratie

Hierna ziet u standaardwaarden configuratie die kunnen WebHCat prestaties nadelig beïnvloeden of fouten veroorzaken als deze waarden worden overschreden:

| Instelling | Beschrijving | Standaardwaarde |
| ------- | ------------ | ------------- |
| [yarn.scheduler.Capacity.maximum-toepassingen][maximum-applications] | Het maximum aantal taken dat tegelijkertijd actief kan zijn (in behandeling of wordt uitgevoerd) | 10.000 |
| [templeton.Exec.Max-processor][max-procs] | Het maximum aantal aanvragen dat tegelijkertijd kan worden geholpen | 20 |
| [mapreduce.jobhistory.Max-leeftijd-ms][max-age-ms] | Het aantal dagen dat werkervaring blijven behouden | 7 dagen |

##<a name="too-many-requests"></a>Te veel aanvragen

**HTTP-statuscode**: 429

| Oorzaak | Resolutie |
| ----- | ---------- |
| Het maximum aantal gelijktijdige aanvragen served door WebHCat per minuut (standaard 20) is overschreden | Uw werkzaamheden om ervoor te zorgen dat u niet verzenden van meer dan het maximum aantal gelijktijdige aanvragen verhogen of verlagen de aanvraaglimiet van gelijktijdige met wijzigen `templeton.exec.max-procs`. Zie [configuratie van wijzigen](#modifying-configuration) voor meer informatie |

##<a name="server-unavailable"></a>Server is niet beschikbaar

**HTTP-statuscode**: 503

| Oorzaak | Resolutie |
| ---------------- | ------------------- |
| Dit gebeurt gewoonlijk tijdens een overname tussen de primaire en secundaire HeadNode voor het cluster | Wacht twee minuten en probeer het opnieuw |

##<a name="bad-request-content-could-not-find-job"></a>Ongeldige aanvraag inhoud: kan de taak niet vinden.

**HTTP-statuscode**: 400

| Oorzaak | Resolutie |
| ---------------- | ------------------- |
| Details van de taak hebt is opgeruimd door de werkervaring Reinigingsmedium | De bewaarperiode standaard voor werkervaring is zeven dagen. Dit kan worden gewijzigd doordat `mapreduce.jobhistory.max-age-ms`. Zie [configuratie van wijzigen](#modifying-configuration) voor meer informatie |
| Taak is afgebroken vanwege een failover | Probeer de taak-inzending voor maximaal twee minuten |
| Een ongeldige taak-id is gebruikt | Controleren of de taak-id juist is |

##<a name="bad-gateway"></a>Ongeldige gateway

**HTTP-statuscode**: 502

| Oorzaak | Resolutie |
| ---------------- | ------------------- |
| Interne opschonen optreedt in het proces van WebHCat | Wacht totdat opschonen om te voltooien of de WebHCat-service opnieuw starten |
| Time-out op een reactie van de ResourceManager-service. Dit kan gebeuren wanneer het aantal actieve toepassingen het maximum (standaard 10.000 gaat) | Wachten op een moment actief zijn taken om te voltooien of de taaklimiet van gelijktijdige vergroten doordat `yarn.scheduler.capacity.maximum-applications`. Zie [configuratie van wijzigen](#modifying-configuration) voor meer informatie  |
| Tijdens een poging om op te halen, alle taken tot en met de oproep [/jobs krijgen](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) terwijl `Fields` is ingesteld op`*` | Niet *alle* taakgegevens ophalen, in plaats daarvan gebruiken `jobid` om op te halen details voor taken die alleen groter is dan een bepaalde taak-id. Of niet te gebruiken`Fields` |
| De WebHCat-service is niet beschikbaar tijdens een HeadNode overname | Wachten op twee minuten en probeer het opnieuw |
| Er zijn meer dan 500 onvoltooide taken die zijn verzonden via WebHCat | Wacht totdat momenteel in behandeling taken zijn voltooid voordat u meer taken verzendt |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
