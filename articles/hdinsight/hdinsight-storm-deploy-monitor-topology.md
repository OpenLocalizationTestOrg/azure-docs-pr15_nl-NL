<properties
   pageTitle="Implementeren en te beheren Apache Storm topologieën op HDInsight | Microsoft Azure"
   description="Leer hoe u implementeren, controleren en beheren van Apache Storm topologieën met het Dashboard Storm op HDInsight. Gebruik Hadoop tools voor Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Implementeren en Apache Storm topologieën op Windows gebaseerde HDInsight beheren

Het Dashboard Storm kunt u eenvoudig distribueren en uitvoeren van Apache Storm topologieën aan uw cluster HDInsight via uw webbrowser. U kunt ook het dashboard gebruiken om te controleren en beheren van actieve topologieën. Als u Visual Studio gebruikt, wordt in de HDInsight-Tools voor Visual Studio vergelijkbare functies in Visual Studio bieden.

Het Dashboard Storm en de functies Storm in de HDInsight-hulpmiddelen, is afhankelijk van de Storm REST API, die kan worden gebruikt om te maken van uw eigen cmdlets voor controle en beheeroplossingen.

> [AZURE.IMPORTANT] De stappen in dit document moet een Storm op basis van Windows op HDInsight cluster. Zie voor informatie over het gebruik van een cluster Linux gebaseerde [distribueren en beheren van Apache Storm topologieën op Linux gebaseerde HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

##<a name="prerequisites"></a>Vereisten voor

* **Apache Storm op HDInsight** - Zie <a href="../hdinsight-storm-getting-started/" target="_blank">aan de slag met Apache Storm op HDInsight</a> voor stapsgewijze instructies voor het maken van een cluster

* Voor de **Storm Dashboard**: een modern webbrowser die ondersteuning biedt voor HTML5

* Voor **Visual Studio** - Azure SDK 2.5.1 of hoger en de HDInsight hulpprogramma's voor Visual Studio. Zie <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">aan de slag met HDInsight Tools for Visual Studio</a> installeren en configureren van de hulpmiddelen HDInsight voor Visual Studio.

    Een van de volgende versies van Visual Studio:

    * Visual Studio 2012 met <a href="http://www.microsoft.com/download/details.aspx?id=39305" target="_blank">4 bijwerken</a>

    * Visual Studio 2013 met <a href="http://www.microsoft.com/download/details.aspx?id=44921" target="_blank">4 bijwerken</a> of <a href="http://go.microsoft.com/fwlink/?LinkId=517284" target="_blank">Visual Studio-2013-Community</a>

    * <a href="http://visualstudio.com/downloads/visual-studio-2015-ctp-vs" target="_blank">Visual Studio 2015 CTP6</a>

    > [AZURE.NOTE] De HDInsight's voor Visual Studio ondersteunen Storm momenteel alleen op HDInsight cluster versie 3,2.

##<a name="storm-dashboard"></a>Storm Dashboard

Het Dashboard Storm is een webpagina die beschikbaar zijn op uw cluster Storm. De URL is **https://&lt;Clusternaam >.azurehdinsight.net/**, waarbij **clusternaam** de naam van uw Storm op HDInsight cluster.

Selecteren vanaf de bovenkant van het Dashboard Storm, **Topologie indienen**. Volg de instructies op de pagina om uit te voeren van de topologie van een steekproef of te uploaden en uitvoeren van een topologie die u hebt gemaakt.

![de pagina van de topologie verzenden][storm-dashboard-submit]

###<a name="storm-ui"></a>Storm UI

Selecteer de koppeling **Storm UI** vanuit het Dashboard Storm. Hiermee wordt informatie over het cluster, naast een actieve topologieën weergegeven.

![de gebruikersinterface storm][storm-dashboard-ui]

> [AZURE.NOTE] In sommige versies van Internet Explorer mogelijk de gebruikersinterface Storm wordt niet vernieuwd nadat u het eerst hebt bezocht. Bijvoorbeeld: kan niet worden weergegeven de nieuwe topologieën u ingediend of een topologie als actief kan worden weergegeven wanneer u deze eerder gedeactiveerd. Microsoft is op de hoogte van dit probleem en werkt aan een oplossing.

####<a name="main-page"></a>Hoofdpagina

De hoofdpagina van de gebruikersinterface Storm biedt de volgende informatie:

* **Cluster samenvatting**: basisinformatie over het cluster Storm.

* **Topologie overzicht**: een lijst met actieve topologieën. Gebruik de koppelingen in deze sectie meer informatie over specifieke topologieën kunnen bekijken.

* **Toezichthouder samenvatting**: informatie over de toezichthouder Storm.

* **Configuratie van nimbus**: Nimbus configuratie voor het cluster.

####<a name="topology-summary"></a>Topologie samenvatting

Selecteren van een koppeling in de sectie **topologie samenvatting** bevat de volgende informatie over de topologie:

* **Topologie samenvatting**: basisinformatie over de topologie.

* **Topologie acties**: Management acties die u voor de topologie uitvoeren kunt.

    * **Activeren**: cv's verwerking van een gedeactiveerd topologie.

    * **Deactiveren**: een actieve topologie onderbreekt.

    * **Vastleggen**: Hiermee past u de evenwijdigheid van de topologie. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u lopende topologieën vastleggen. Hierdoor wordt de topologie om aan te passen parallellisme ter voor het betere of beperktere aantal knooppunten in het cluster.

        Zie <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">informatie over de evenwijdigheid van de topologie van een Storm</a>voor meer informatie.

    * **Beëindigen**: beëindigt de topologie van een Storm na de opgegeven time-out.

* **Topologie stat**: statistieken over de topologie. Gebruik de koppelingen in de kolom van het **venster** voor het instellen van de tijdsperiode voor de resterende items op de pagina.

* **Spouts**: de spouts die worden gebruikt door de topologie. Gebruik de koppelingen in deze sectie meer informatie over specifieke spouts kunnen bekijken.

* **Bolts**: de bouten die worden gebruikt door de topologie. Gebruik de koppelingen in deze sectie meer informatie over specifieke bouten kunnen bekijken.

* **Configuratie van de topologie**: de configuratie van de geselecteerde topologie.

####<a name="spout-and-bolt-summary"></a>Spout en bout overzicht

Een spout selecteren in de secties **Spouts** of **Bolts** , wordt de volgende informatie over het geselecteerde item weergegeven:

* **Onderdeel samenvatting**: basisinformatie over de spout of bout.

* **Spout/bout stat**: statistieken over de spout of bout. Gebruik de koppelingen in de kolom van het **venster** voor het instellen van de tijdsperiode voor de resterende items op de pagina.

* **Invoer stat** (alleen bout): informatie over de invoer-streams dat door de bout.

* **Uitvoer stat**: informatie over de streams dat door dit spout of bout.

* **Executors**: informatie over de instanties van het spout of bout. Selecteer het item **poort** voor een specifieke executor een logboek diagnostische gegevens geproduceerd voor dit exemplaar te bekijken.

* **Fouten**: een foutgegevens hiervoor spout of bout.

##<a name="hdinsight-tools-for-visual-studio"></a>HDInsight Tools voor Visual Studio

De hulpmiddelen HDInsight kan worden gebruikt om in te dienen C# of hybride topologieën aan uw cluster Storm. De volgende stappen gebruikt een toepassing voor de steekproef. Zie voor informatie over het maken van uw eigen topologieën met behulp van de hulpmiddelen HDInsight [ontwikkelen C# topologieën met de HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Gebruik de volgende stappen implementeren van een steekproef naar uw Storm op HDInsight cluster, en vervolgens weergeven en beheren van de topologie.

1. Als u de nieuwste versie van de hulpmiddelen HDInsight hebt niet voor Visual Studio hebt geïnstalleerd, raadpleegt u <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">aan de slag met HDInsight Tools for Visual Studio</a>.

2. Visual Studio openen, selecteert u **bestand** > **Nieuw** > **Project**.

3. Vouw in het dialoogvenster **Nieuw Project** **geïnstalleerde** > **sjablonen**en selecteer vervolgens **HDInsight**. Selecteer in de lijst met sjablonen, **Storm steekproef**. Typ een naam voor de toepassing onderaan in het dialoogvenster.

    ![afbeelding](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

1. In **Solution Explorer**met de rechtermuisknop op het project en selecteer de optie **verzenden naar Storm op HDInsight**.

    > [AZURE.NOTE] Als u wordt gevraagd, voert u de aanmeldingsreferenties voor uw Azure-abonnement. Als u meer dan één abonnement hebt, moet u zich aanmelden bij de database met uw Storm op HDInsight cluster.

2. Selecteer uw Storm op HDInsight cluster in de vervolgkeuzelijst **Storm Cluster** en selecteer vervolgens **verzenden**. U kunt controleren of de indiening gelukt is met behulp van **het uitvoervenster** .

3. Wanneer de topologie is verzonden, wordt de **Storm topologieën** voor het cluster moet worden weergegeven. Selecteer de topologie in de lijst om informatie over de actieve topologie te bekijken.

    ![monitor met een Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

    > [AZURE.NOTE] U kunt ook **Storm topologieën** van **Server Explorer** weergeven door uit te vouwen **Azure** > **HDInsight**, en klik vervolgens met de rechtermuisknop op een Storm op HDInsight cluster en **Weergave Storm topologieën**selecteren.

    Selecteer de shape voor de spouts of bouten informatie over deze onderdelen kunnen bekijken. Een nieuw venster wordt geopend voor elk item is geselecteerd.
    
    > [AZURE.NOTE] De naam van de topologie is de naam van de klasse van de topologie (in dit geval `HelloWord`,) met een tijdstempel toegevoegd.

4. De weergave **Overzicht van de topologie** , selecteer **verwijderen** om te stoppen de topologie.

    > [AZURE.NOTE] Storm topologieën verder uitgevoerd totdat deze zijn stopgezet of het cluster wordt verwijderd.

##<a name="rest-api"></a>REST API

De gebruikersinterface Storm is gebouwd met de REST API, zodat u uitvoeren kunt, dezelfde management en functionaliteit bewaken met behulp van de REST API. U kunt de REST API gebruiken om te maken van aangepaste hulpmiddelen voor het beheren en controleren van Storm topologieën.

Zie [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md)voor meer informatie. De volgende informatie hoort bij het gebruik van de REST API met Apache Storm op HDInsight.

###<a name="base-uri"></a>Base URI

De basis-URI voor de REST API op HDInsight clusters is **https://&lt;Clusternaam >.azurehdinsight.net/stormui/api/v1/**, waarbij **clusternaam** de naam van uw Storm op HDInsight cluster.

###<a name="authentication"></a>Verificatie

Aanvragen voor de REST API moeten **Basisverificatie**, gebruiken zodat u de naam van de beheerder cluster HDInsight en het wachtwoord gebruiken.

> [AZURE.NOTE] Omdat basisverificatie is verzonden via uitschakelen voor tekst, moet u **altijd** gebruik HTTPS communicatie met het cluster beveiligen.

###<a name="return-values"></a>Retourwaarden

Informatie die wordt geretourneerd uit de REST API mogelijk alleen best uit in de cluster of virtuele machines op hetzelfde Azure virtuele netwerk als het cluster. Bijvoorbeeld de FQDN-naam (Fully Qualified) als resultaat gegeven voor Zookeeper-servers worden niet toegankelijk zijn vanuit Internet.

##<a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe om te implementeren en topologieën controleren met behulp van het Dashboard Storm wordt uitgelegd hoe u:

* [Ontwikkel C# topologieën met behulp van de hulpmiddelen HDInsight voor Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Java gebaseerde topologieën met Maven ontwikkelen](hdinsight-storm-develop-java-topology.md)

Zie voor een lijst met meer voorbeeld topologieën, [voorbeeld topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md).

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
