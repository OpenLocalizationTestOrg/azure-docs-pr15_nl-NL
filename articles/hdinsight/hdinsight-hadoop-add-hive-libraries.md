<properties
pageTitle="Component bibliotheken toevoegen tijdens het maken van HDInsight cluster | Azure"
description="Leer hoe u de component bibliotheken (oppervlak-bestanden,) toevoegen aan een cluster HDInsight gemaakt."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Component bibliotheken toevoegen tijdens het HDInsight cluster maken

Als u bibliotheken die u vaak met onderdeel op HDInsight gebruikt hebt, is dit document bevat informatie over het gebruik van de actie voor een Script vooraf geladen de bibliotheken bij het maken van het cluster. Hiermee maakt u de bibliotheken globaal beschikbaar in component (niet nodig kunt [Toevoegen JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) laden.)

##<a name="how-it-works"></a>Werkwijze

Wanneer u een cluster maakt, kunt u desgewenst een scriptactie die wordt uitgevoerd als een script knooppunten terwijl ze worden gemaakt. Het script in dit document accepteert één parameter, dat wil een WASB-locatie die de bibliotheken zeggen (opgeslagen als oppervlak-bestanden) dat wordt vooraf geladen bevat.

Tijdens het maken van cluster het script opgesomd van de bestanden, kopieert deze naar de `/usr/lib/customhivelibs/` directory op kop- en werknemer knooppunten, vervolgens toevoegt aan de `hive.aux.jars.path` eigenschap in de `core-site.xml` bestand. Klik op Linux gebaseerde kolomgroepen dit wordt ook bijgewerkt via de `hive-env.sh` bestand met de locatie van de bestanden.

> [AZURE.NOTE] Met de scriptacties in dit artikel zorgt ervoor dat de bibliotheken beschikbaar in de volgende scenario's:
>
> * __Linux gebaseerde HDInsight__ - bij gebruik van de __component opdrachtregel__, __WebHCat__ (Templeton) en __HiveServer2__.
> * __Op basis van Windows HDInsight__ - bij gebruik van de __component opdrachtregel__ en __WebHCat__ (Templeton).

##<a name="the-script"></a>Het script

__Locatie van script__

Voor __Linux gebaseerde clusters__: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Voor __Windows gebaseerde clusters__: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Vereisten__

* De scripts moeten worden toegepast op de __knooppunten van hoofd__ - en de __werknemer knooppunten__.

* De potten die u wilt installeren moeten worden opgeslagen in Azure-blobopslag in een __enkel container__. 

* De opslag-account met de bibliotheek van het oppervlak bestanden __moeten__ worden gekoppeld aan het cluster HDInsight tijdens het maken van. Dit kan op twee manieren doen:

    * Doordat in een container op het standaardaccount voor de opslag voor het cluster.
    
    * Doordat in een container op een container gekoppelde opslag. U kunt bijvoorbeeld __Optionele configuratie__, __gekoppeld opslag accounts__ toevoegen van extra opslagruimte gebruiken in de portal.

* Het pad WASB aan de container moet worden opgegeven als een parameter voor de Script-actie. Bijvoorbeeld als de potten zijn opgeslagen in een container met de naam __libs__ op een benoemde __mystorage__opslag-account, de parameter zou __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] In dit document wordt ervan uitgegaan die u hebt al een opslag-account maken, blob container en de bestanden geüpload naar deze. 
    >
    > Als u geen een opslag-account hebt gemaakt, kunt u dit doen via de [portal van Azure](https://portal.azure.com). Vervolgens kunt u een hulpprogramma zoals [Azure opslag Explorer](http://storageexplorer.com/) maken van een nieuwe container in het account en bestanden naar hebt geüpload.

##<a name="create-a-cluster-using-the-script"></a>Een cluster met het script maken

> [AZURE.NOTE] De volgende stappen maken een nieuw Linux gebaseerde HDInsight cluster. Als u wilt maken een nieuw cluster op basis van Windows, __Windows__ selecteert als het cluster OS bij het maken van het cluster en het script Windows (PowerShell) gebruiken in plaats van het script we vaker doen.
> 
> U kunt ook een cluster met dit script maken met Azure PowerShell of de HDInsight .NET SDK gebruiken. Zie voor meer informatie over het gebruik van de volgende manieren [aanpassen HDInsight clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

1. Start de inrichting van een cluster met behulp van de stappen in de [voorziening HDInsight clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), maar inrichting niet voltooien.

2. Selecteer **Scriptacties**op het blad **Optionele configuratie** en de informatie opgeven, zoals hieronder wordt weergegeven:

    * __Naam__: Voer een beschrijvende naam voor de scriptactie.
    * __SCRIPT URI__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __Hoofd__: Schakel deze optie
    * __Werknemer__: Schakel dit selectievakje in.
    * __ZOOKEEPER__: laat dit vak leeg.
    * __PARAMETERS__: Voer het adres WASB naar de container en opslagruimte rekening met de potten. Bijvoorbeeld __wasbs://libs@mystorage.blob.core.windows.net/__.

3. Gebruik de knop **selecteren** de configuratie opslaan onderaan in de **Script-acties**.

4. Klik op het blad **Optionele configuratie** __Gekoppelde opslag Accounts__ selecteren en selecteer de koppeling __toevoegen een sleutel opslag__ . Selecteer het account opslagruimte met de potten en gebruik de knoppen __selecteren__ naar instellingen opslaan en het blad __Optionele configuratie__ terug te keren.

5. Gebruik de knop **selecteert u** onder aan het blad **Optionele configuratie** optionele configuratie informatie wilt opslaan.

6. Ga verder met het cluster inrichting zoals is beschreven in de [voorziening HDInsight clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

Zodra het maken van het cluster is voltooid, is mogelijk de potten toegevoegd via dit script uit component zonder gebruik te hoeven gebruiken de `ADD JAR` instructie.

##<a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe component bibliotheken die zijn opgenomen in oppervlak-bestanden naar een cluster HDInsight tijdens het maken van cluster toevoegen. Zie voor meer informatie over het werken met component, [Gebruik component met HDInsight](hdinsight-use-hive.md)
