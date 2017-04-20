<properties
    pageTitle="Actie ontwikkeling met Linux gebaseerde HDInsight script | Microsoft Azure"
    description="Het aanpassen van HDInsight Linux gebaseerde clusters met scriptactie. Scriptacties zijn een manier om aan te passen Azure HDInsight clusters door te geven cluster configuratie-instellingen of aanvullende services, hulpprogramma's of andere software installeren op het cluster. "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>Script actie ontwikkeling met HDInsight

Scriptacties zijn een manier om aan te passen Azure HDInsight clusters door te geven cluster configuratie-instellingen of aanvullende services, hulpprogramma's of andere software installeren op het cluster. U kunt scriptacties gebruiken tijdens het maken van cluster of op een actieve cluster.

> [AZURE.NOTE] De informatie in dit document is specifiek voor HDInsight Linux gebaseerde clusters. Zie voor informatie over het gebruik van scriptacties met clusters op basis van Windows [Script actie ontwikkeling met HDInsight (Windows)](hdinsight-hadoop-script-actions.md).

## <a name="what-are-script-actions"></a>Wat zijn scriptacties?

Scriptacties zijn we vaker doen scripts die Azure wordt uitgevoerd knooppunten configuratiewijzigingen aanbrengen of software installeren. Een scriptactie wordt uitgevoerd als de hoofdmap en volledige toegangsrechten voor de knooppunten bevat.

Scriptacties kunnen worden toegepast met de volgende methoden:

| Gebruik deze opdracht om toe te passen een script... | Cluster maken tijdens... | Klik op een actieve cluster... |
| ----- |:-----:|:-----:|
| Azure-Portal | ✓ | ✓ |
| Azure PowerShell | ✓ | ✓ |
| Azure CLI | &nbsp; | ✓ |
| HDInsight .NET SDK | ✓ | ✓ |
| Azure resourcemanager-sjabloon | ✓ | &nbsp; |

Zie voor meer informatie over het gebruik van de volgende manieren om toe te passen scriptacties [aanpassen HDInsight clusters scriptacties gebruiken](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Aanbevolen procedures voor het ontwikkelen van scripts

Wanneer u een aangepast script voor een cluster HDInsight ontwikkelt, zijn er enkele aanbevolen procedures in gedachten moet houden:

- [Doel de Hadoop-versie](#bPS1)
- [TARGET versie van het besturingssysteem](#bps10)
- [Bieden stabiele koppelingen naar script resources](#bPS2)
- [Gebruik vooraf gecompileerde bronnen](#bPS4)
- [Ervoor zorgen dat het cluster aanpassing script idempotency is ingeschakeld is](#bPS3)
- [Betere beschikbaarheid van de architectuur cluster zorgen](#bPS5)
- [De aangepaste onderdelen als u wilt gebruiken van Azure-blobopslag configureren](#bPS6)
- [Informatie over naar STDOUT en STDERR schrijven](#bPS7)
- [ASCII-bestanden opslaan met LF regeleinden](#bps8)
- [Gebruik opnieuw logica herstellen uit tijdelijke fouten](#bps9)

> [AZURE.IMPORTANT] Scriptacties moeten worden voltooid binnen 60 minuten, of ze time-out wordt. Tijdens het knooppunt is geïnstalleerd, wordt het script uitgevoerd als andere processen installatie en configuratie. Concurrenten voor resources zoals CPU-tijd of netwerk bandbreedte mogelijk het script duurt langer dan in uw ontwikkelomgeving voltooien.

### <a name="bPS1"></a>Doel de Hadoop-versie

Verschillende versies van HDInsight hebben verschillende versies van Hadoop-services en de onderdelen die zijn geïnstalleerd. Als uw script een specifieke versie van een service of elk onderdeel verwacht, moet u alleen het script gebruiken met de versie van HDInsight waarin de vereiste onderdelen. U kunt informatie over het onderdeel versies geleverd bij HDInsight vinden met het document [HDInsight onderdeel versiebeheer](hdinsight-component-versioning.md) .

###<a name="bps10"></a>Versie van het besturingssysteem afstemmen

Linux gebaseerde HDInsight is gebaseerd op de Ubuntu Linux-verdeling. Verschillende versies van HDInsight, is afhankelijk van verschillende versies van Ubuntu, die kunnen worden beïnvloed door de werking van uw script. Bijvoorbeeld HDInsight 3,4 en lager zijn gebaseerd op Ubuntu-versies die Upstart gebruiken. Versie 3.5 is gebaseerd op Ubuntu 16.04, waarbij gebruik Systemd wordt. Systemd en Upstart zijn afhankelijk van andere opdrachten, zodat uw script moet worden geschreven voor gebruik met beide.

Een andere belangrijke verschil tussen HDInsight 3.4 en 3.5 is dat `JAVA_HOME` nu verwijst naar Java 8.

U kunt de versie van het besturingssysteem controleren met behulp van `lsb_release`. De volgende codefragmenten uit het script kleurtoon voor installatie wordt getoond hoe om te bepalen of het script is gestart op Ubuntu 14 of 16:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

U vindt het volledige script waarin deze fragmenten bij https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Voor de versie van Ubuntu die wordt gebruikt door HDInsight, raadpleegt u het document [HDInsight onderdeelversie](hdinsight-component-versioning.md) .

Als u wilt weten over de verschillen tussen Systemd en Upstart, raadpleegt u [Systemd voor Upstart gebruikers](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Bieden stabiele koppelingen naar script resources

U moet ervoor zorgen dat de scripts en resources die worden gebruikt door het script beschikbaar gedurende de levensduur van het cluster blijven en de versies van deze bestanden beter niet wijzigen voor de duur. Deze resources zijn vereist als nieuwe knooppunten worden toegevoegd aan het cluster tijdens schaalbaarheid bewerkingen.

De beste manier is om te downloaden en archiveren alles in een account Azure opslagruimte voor uw abonnement.

> [AZURE.IMPORTANT] Het opslag-account gebruikt, moet het opslag standaardaccount voor het cluster of een container openbare, alleen-lezen op een andere opslag-account.

Bijvoorbeeld, worden in de voorbeelden die is verstrekt door Microsoft opgeslagen in de [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) opslag-account, dat wil een openbare, alleen-lezen container beheerd door het team HDInsight zeggen.

### <a name="bPS4"></a>Gebruik vooraf gecompileerde bronnen

Als u wilt beperken hoe lang die het duurt het script wilt uitvoeren, geen bewerkingen die resources uit broncode compileren. In plaats daarvan vooraf compileren de resources en de binaire versie opslaan in Azure-blobopslag, zodat deze kan snel worden gedownload naar het cluster uit het script.

### <a name="bPS3"></a>Ervoor zorgen dat het cluster aanpassing script idempotency is ingeschakeld is

Scripts moeten zijn ontworpen voor idempotency wil zeggen dat als het script is opgetreden tijdens de op meerdere keren is ingeschakeld, moet ervoor zorgen dat het cluster is geretourneerd naar dezelfde staat telkens wanneer deze wordt uitgevoerd.

Bijvoorbeeld als u een aangepast script installeert u een toepassing op usr op de eerste uitvoeren en klik op elke volgende uitvoeren het script moet controleren of de toepassing bestaat al op de locatie usr voordat u verdergaat met de overige stappen voor het script.

### <a name="bPS5"></a>Betere beschikbaarheid van de architectuur cluster zorgen

Linux gebaseerde HDInsight clusters biedt twee hoofd knooppunten die binnen het cluster actief zijn en script acties worden uitgevoerd voor beide knooppunten. Als slechts één hoofd knooppunt verwacht dat u de onderdelen die u hebt geïnstalleerd, moet u een script dat het onderdeel kan alleen worden geïnstalleerd op een van de twee hoofd knooppunten in het cluster ontwerpen.

> [AZURE.IMPORTANT] Services die standaard geïnstalleerd als onderdeel van HDInsight zijn ontworpen om tussen de twee hoofd knooppunten indien nodig, deze functionaliteit is echter niet uitgebreid aan aangepaste onderdelen geïnstalleerd via script ctions mislukken. Als u de onderdelen die via een scriptactie ten zeerste beschikbaar moet zijn geïnstalleerd, moet u uw eigen failover om die gebruikmaakt van de twee beschikbare hoofd knooppunten implementeren.

### <a name="bPS6"></a>De aangepaste onderdelen als u wilt gebruiken van Azure-blobopslag configureren

Onderdelen die u op het cluster installeren mogelijk een standaard-configuratie die opslag in de Hadoop Distributed bestand System (HDFS wordt). Azure Blob Storage (WASB) HDInsight gebruikt als de standaard-opslag. Hier vindt u een compatibele bestandssysteem van HDFS dat zich blijft gegevens, voordoen zelfs als het cluster wordt verwijderd. U kunt u installeren als u wilt WASB gebruiken in plaats van HDFS onderdelen moet configureren.

Bijvoorbeeld kopieert de volgende de giraph-examples.jar bestand uit het lokale bestandssysteem naar WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>Informatie over naar STDOUT en STDERR schrijven

Gegevens naar STDOUT en STDERR geschreven tijdens het uitvoeren van scripts is aangemeld, en kan worden weergegeven via het web Ambari UI.

> [AZURE.NOTE] Ambari is alleen beschikbaar als het cluster is gemaakt. Als u een scriptactie tijdens het maken van cluster gebruiken en niet worden gemaakt, raadpleegt u de sectie probleemoplossing [dat aanpassen HDInsight clusters met de scriptactie](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) voor andere manieren om toegang tot geregistreerde gegevens.

De meeste hulpprogramma's en installatiepakketten schrijft al informatie STDOUT en STDERR, maar u kunt aanvullende logboekregistratie toevoegen. Tekst naar STDOUT gebruik te verzenden `echo`. Bijvoorbeeld:

        echo "Getting ready to install Foo"

Standaard `echo` stuurt de tekenreeks naar STDOUT. Toevoegen als u wilt dat deze naar STDERR, `>&2` voordat `echo`. Bijvoorbeeld:

        >&2 echo "An error occurred installing Foo"

Hiermee wordt informatie verzonden naar STDOUT (1, dat wil standaard dus hier niet wordt vermeld zeggen,) omgeleid naar STDERR (2). Zie voor meer informatie over IO omleiding, [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Zie voor meer informatie over het weergeven van gegevens door de scriptacties vastgelegd [aanpassen HDInsight clusters met de scriptactie](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

###<a name="bps8"></a>ASCII-bestanden opslaan met LF regeleinden

We vaker doen scripts moeten worden opgeslagen als ASCII-indeling met lijnen door LF beëindigd. Als u bestanden worden opgeslagen als UTF-8, waaronder eventueel een Byte Order Mark aan het begin van het bestand of met regeleinden van CRLF, die gebruikt voor Windows editors wordt, mislukt het script met fouten ongeveer als volgt uit:

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>Gebruik opnieuw logica herstellen uit tijdelijke fouten

Bij het downloaden van bestanden, installeert pakketten met enz ophalen of andere acties die gegevens via internet verzenden, wordt de actie mislukken vanwege tijdelijke netwerken fouten. De externe bron die u wilt communiceren mogelijk bijvoorbeeld wordt de pagina worden niet via een back-knooppunt.

Als u wilt uw script robuuste tijdelijke fouten, kunt u opnieuw logica implementeren. Hier volgt een voorbeeld van een functie die wordt elke opdracht doorgegeven ernaar uitvoeren en (als de opdracht mislukt,) maximaal drie keer opnieuw. Deze Wacht twee seconden tussen nieuwe pogingen.

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

Hier volgen enkele voorbeelden van het gebruik van deze functie.

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>Methoden voor aangepaste scripts

script actie methoden zijn hulpprogramma's die u gebruiken kunt bij het schrijven van aangepaste scripts. Deze zijn gedefinieerd in de [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)en kunnen worden opgenomen in uw scripts met het volgende:

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

Dit zorgt ervoor dat de volgende hulpprogramma's beschikbaar maken voor gebruik in uw script:

| Gebruik van de helper | Beschrijving |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | Een bestand uit de bron-URL naar het opgegeven pad gedownload. Standaard worden niet overschreven een bestaand bestand. |
| `untar_file TARFILE DESTDIR` | Een bestand tar haalt (met `-xf`,) naar de doelmap. |
| `test_is_headnode` | Als uitgevoerd op een cluster hoofd knooppunt, geeft als resultaat 1; anderszins, 0. |
| `test_is_datanode` | Als het huidige knooppunt gegevensknooppunt (werknemer) is, geeft als resultaat een 1; anderszins, 0. |
| `test_is_first_datanode` | Als het huidige knooppunt het eerste (werknemer) gegevensknooppunt (ook wel workernode0 genoemd) is, geeft als resultaat een 1; anderszins, 0. |
| `get_headnodes` | Geeft als resultaat de FQDN-naam van de headnodes in het cluster. Namen worden gescheiden door komma's. Een lege tekenreeks op een fout geretourneerd. |
| `get_primary_headnode` | De FQDN-naam van de primaire headnode krijgt. Een lege tekenreeks op een fout geretourneerd. |
| `get_secondary_headnode` | De FQDN-naam van de secundaire headnode krijgt. Een lege tekenreeks op een fout geretourneerd. |
| `get_primary_headnode_number` | Het numerieke achtervoegsel van de primaire headnode krijgt. Een lege tekenreeks op een fout geretourneerd. |
| `get_secondary_headnode_number` | Het numerieke achtervoegsel van de secundaire headnode krijgt. Een lege tekenreeks op een fout geretourneerd. |

## <a name="commonusage"></a>Algemene gebruikspatronen

In deze sectie bevat richtlijnen over het implementeren van enkele van de algemene gebruikspatronen die mogelijk optreden kunnen bij het schrijven van uw eigen aangepast script.

### <a name="passing-parameters-to-a-script"></a>Parameters doorgeven aan een script

In sommige gevallen kan het script parameters vragen. Bijvoorbeeld mogelijk moet u de admin-wachtwoord voor het cluster om te kunnen gegevens ophalen uit het Ambari REST API.

Parameters doorgegeven aan het script _positionele parameters_worden genoemd en zijn toegewezen aan `$1` voor de eerste parameter `$2` voor het tweede, en dus eenmalige aanmelding. `$0`bevat de naam van het script zelf.

Waarden worden doorgegeven aan het script als parameters moeten tussen enkele aanhalingstekens ('), zodat de doorgegeven waarde wordt behandeld als een letterlijke waarde en speciale behandeling niet is besteed aan opgenomen tekens bevat, zoals '!'.

### <a name="setting-environment-variables"></a>Instelling omgevingsvariabelen

Instellen van een omgevingsvariabele wordt uitgevoerd door het volgende:

    VARIABLENAME=value

Waarbij VARIABELENAAM is de naam van de variabele. Gebruiken voor toegang tot de variabele na dit, `$VARIABLENAME`. Bijvoorbeeld als u wilt een waarde die is verstrekt door een parameter positionele als omgevingsvariabele wachtwoord toewijst, gebruikt u het volgende:

    PASSWORD=$1

Latere toegang tot de gegevens kunt vervolgens `$PASSWORD`.

Het bereik van het script bevat alleen omgevingsvariabelen binnen het script zijn ingesteld. In sommige gevallen moet u mogelijk systeem breed omgevingsvariabelen die blijft nadat het script is voltooid toevoegen. Dit is meestal zodat gebruikers verbinding maken met het cluster via SSH de beschikking over de onderdelen die door het script is geïnstalleerd. U kunt dit doen door toe te voegen van de omgevingsvariabele `/etc/environment`. Bijvoorbeeld het volgende voorbeeld wordt __HADOOP\_CONF\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Toegang tot locaties waar de aangepaste scripts worden opgeslagen

Scripts die worden gebruikt voor het aanpassen van een cluster moeten naar een zich in het account van de standaard-opslag voor de cluster of, als op een andere opslag-account, in een openbare container voor alleen-lezen. Als uw script toegang heeft tot bronnen elders deze ook moeten worden in een openbaar (ten minste openbare alleen-lezen). Bijvoorbeeld u wilt mogelijk downloaden van een bestand met het cluster `download_file`.

Het bestand opgeslagen in een toegankelijke Azure opslag-account door het cluster (zoals het standaardaccount opslag), wordt de snelle toegang, bieden, zoals deze opslag binnen het netwerk van de Azure is.

### <a name="checking-the-operating-system-version"></a>De versie van het besturingssysteem controleren

Verschillende versies van HDInsight, is afhankelijk van bepaalde versies van Ubuntu. Het is mogelijk dat er verschillen tussen OS-versies die u in het script controleren moet. Mogelijk moet u bijvoorbeeld een binair getal dat is gebonden aan de versie van Ubuntu installeren.

Als u wilt controleren of de versie van het besturingssysteem, gebruikt u `lsb_release`. Bijvoorbeeld het volgende wordt getoond hoe een verwijzing naar een bestand verschillende tar afhankelijk van de versie van het besturingssysteem:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>Controlelijst voor het implementeren van de scriptactie voor een

Hier volgen de stappen die we hebt gemaakt, bij het implementeren van deze scripts voorbereiden:

- Zet de bestanden met de aangepaste scripts in een locatie die toegankelijk is voor de knooppunten tijdens de implementatie. Dit is een van de standaard- of extra opslagruimte accounts op het moment van cluster implementatie of een andere openbaar opslag container opgegeven.

- Controles in scripts om ervoor te zorgen dat ze impotently, uitvoeren, zodat het script kan meerdere keren worden uitgevoerd op hetzelfde knooppunt toevoegen.

- U kunt een tijdelijk bestand directory /tmp gebruiken om de gedownloade bestanden die worden gebruikt door de scripts en vervolgens opschonen van nadat scripts hebt uitgevoerd.

- In het geval dat OS niveau instellingen of Hadoop-service configuratiebestanden zijn gewijzigd, wilt u mogelijk opnieuw opstarten HDInsight services zodat ze een OS niveau-instellingen, zoals de omgevingsvariabelen instellen in de scripts kunnen verdergaan.

## <a name="runScriptAction"></a>De scriptactie voor een uitvoeren

U kunt scriptacties gebruiken om aan te passen HDInsight clusters met behulp van de Azure Azure PowerShell, Azure Resource Manager (ARM) sjablonen of de HDInsight .NET SDK-portal. Zie voor instructies voor [het gebruik van scriptactie](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Voorbeelden van aangepast script

Microsoft biedt voorbeeldscripts voor het installeren van onderdelen op een cluster HDInsight. De voorbeeldscripts en instructies over het gebruik van deze zijn beschikbaar op de onderstaande koppelingen:

- [Installeren en gebruiken van kleurtoon op HDInsight clusters](hdinsight-hadoop-hue-linux.md)
- [Installeren en gebruiken van R op HDInsight Hadoop clusters](hdinsight-hadoop-r-scripts-linux.md)
- [Installeren en gebruiken van Solr op HDInsight clusters](hdinsight-hadoop-solr-install-linux.md)
- [Installeren en gebruiken van Giraph op HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] De documenten die zijn gekoppeld hierboven zijn specifiek voor HDInsight Linux gebaseerde clusters. Zie [Script actie development met HDInsight (Windows)](hdinsight-hadoop-script-actions.md) of gebruik van de koppelingen beschikbaar aan het begin van elk artikel voor een scripts die werken met Windows gebaseerde HDInsight.

##<a name="troubleshooting"></a>Problemen oplossen

Hierna ziet u fouten die optreden kunnen bij het gebruik van scripts die u hebt ontwikkeld:

__Error__: `$'\r': command not found`. Soms wordt gevolgd door `syntax error: unexpected end of file`.

_Oorzaak_: deze fout treedt op als de regels in een script op CRLF eindigt. UNIX-systemen verwachten alleen LF als het einde van de regel.

Dit probleem treedt meestal wanneer het script is gemaakt op een Windows-omgeving, zoals CRLF is een algemene lijn voor veel teksteditors op Windows beëindigd.

_Oplossing_: als het een optie in een teksteditor, selecteer Unix-indeling of LF voor het einde van de regel. U kunt ook de volgende opdrachten op een Unix-systeem de CRLF wijzigen in een LF:

> [AZURE.NOTE] De volgende opdrachten zijn ongeveer equivalent in dat ze de regeleinden CRLF naar LF wijzigen moeten. Selecteer een op basis van de hulpprogramma's die beschikbaar zijn op uw systeem.

| Opdracht | Notities |
| ------- | ----- |
| `unix2dos -b INFILE` | Het oorspronkelijke bestand up wordt gemaakt met een. BAK-extensie |
| `tr -d '\r' < INFILE > OUTFILE` | UITVOERBESTAND bevat een versie met alleen LF uitgangen |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Hiermee wordt het bestand wijzigen rechtstreeks zonder een nieuw bestand maken |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | UITVOERBESTAND bevat een versie met alleen LF uitgangen.

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_Oorzaak_: deze fout treedt op wanneer het script is opgeslagen als UTF-8 met een Byte Order Mark (BOM).

_Oplossing_: sla het bestand als ASCII- of als UTF-8 zonder een stuklijst. U kunt ook de volgende opdracht uit op een Linux of Unix-systeem naar een nieuw bestand zonder de stuklijst maken:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Voor de bovenstaande opdracht, kunt u __BESTANDIN__ vervangen door het bestand met de stuklijst. __UITVOERBESTAND__ moet een nieuwe bestandsnaam, die het script zonder de stuklijst bevat.

## <a name="seeAlso"></a>Volgende stappen

* Leer hoe u [HDInsight aanpassen clusters met de scriptactie](hdinsight-hadoop-customize-cluster-linux.md)

* Gebruik van de [verwijzing HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx) voor meer informatie over het maken van .NET-toepassingen die HDInsight beheren

* Gebruik de [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) voor meer informatie over het gebruik van de REST management acties voor HDInsight clusters wilt uitvoeren.
