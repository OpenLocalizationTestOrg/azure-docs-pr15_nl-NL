<properties 
    pageTitle="Afhankelijkheden, uitzonderingen en tijden worden uitgevoerd in de web-apps Java controleren" 
    description="Uitgebreide controle van uw website Java met toepassing inzichten" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Afhankelijkheden, uitzonderingen en tijden worden uitgevoerd in de web-apps Java controleren

*Er is een toepassing inzichten in de proefversie.*

Als u [uw Java-web-app met de toepassing inzichten geïmplementeerd hebt][java], kunt u de Java-Agent om meer inzicht, zonder codewijzigingen:


* **Afhankelijkheden:** Gegevens over oproepen die zorgt ervoor dat uw toepassing op andere onderdelen, waaronder:
 * **REST-oproepen** via HttpClient, OkHttp en RestTemplate (Lenteachtergrond).
 * **Bestand Vgx.** oproepen via de Jedis-client. Als het gesprek langer duren dan 10s, haalt de agent ook de oproep-argumenten.
 * **[JDBC oproepen](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB of Apache Derby DB. "executeBatch" oproepen worden ondersteund. Als het gesprek langer duren dan 10s, rapporten de agent voor MySQL en PostgreSQL, het query-abonnement. 
* **Onderschept uitzonderingen:** Gegevens over uitzonderingen die zijn afgehandeld door uw code.
* **Verwerkingstijd methode:** Gegevens over de tijd die het duurt specifieke methoden uit te voeren.

Als u wilt gebruiken de Java-agent, installeren u dit op de server. Uw WebApps moeten worden geïmplementeerd met de [Toepassing inzichten Java SDK][java].

## <a name="install-the-application-insights-agent-for-java"></a>De toepassing inzichten-agent voor Java installeren

1. Op de computer waarop de Java-server, [de agent downloaden](https://aka.ms/aijavasdk).
2. De toepassing server opstartscript bewerken en de volgende JVM toevoegen:

    `javaagent:`*volledige pad naar de agent oppervlak-bestand*

    Bijvoorbeeld in Tomcat op een Linux-computer:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Start opnieuw op uw toepassingsserver.

## <a name="configure-the-agent"></a>De agent configureren

Maken van een bestand met de naam `AI-Agent.xml` en plaats het in dezelfde map als de agent oppervlak-bestand.

Stel de inhoud van het XML-bestand. Het volgende voorbeeld als u wilt opnemen of uitsluiten van de functies die u wilt bewerken. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

U moet rapporten uitzondering en tijdsinstelling van de methode voor afzonderlijke methoden inschakelen.

Standaard `reportExecutionTime` waar is en `reportCaughtExceptions` ONWAAR is.

## <a name="view-the-data"></a>Weergeven van de gegevens

In de bron-toepassing inzichten geaggregeerde externe afhankelijkheid en methode execution tijden wordt weergegeven [onder de tegel prestaties][metrics]. 

Als u wilt zoeken naar afzonderlijke exemplaren van rapporten afhankelijkheid, uitzondering en methode, open [Zoeken][diagnostic]. 

[Diagnose afhankelijkheidsproblemen - voor meer informatie](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Vragen? Problemen?

* Er zijn geen gegevens? [Uitzonderingen van de firewall instellen](app-insights-ip-addresses.md)
* [Java probleemoplossing](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
