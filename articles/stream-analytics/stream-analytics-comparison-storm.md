<properties
    pageTitle="Analytics platforms: Apache Storm vergelijking met Stream Analytics | Microsoft Azure"
    description="Lees hoe u een cloud analytics-platform met behulp van een vergelijking Apache Storm naar Stream Analytics te kiezen. Meer informatie over functies en verschillen."
    keywords="Analytics-platform, analytics platforms, cloud analytics platform, storm-vergelijking"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Help voor het kiezen van een streaming analytics-platform: Apache Storm vergelijking met Azure Stream Analytics

Lees hoe u een cloud analytics-platform met behulp van deze vergelijking Apache Storm naar Azure Stream Analytics te kiezen. De toegevoegde waarden van de Stream Analytics versus Apache Storm als een beheerde service op Azure HDInsight, zodat u de juiste oplossing voor uw bedrijf kiezen kunt gebruiken zaken te begrijpen.

Beide analytics platforms bieden voordelen van een oplossing PaaS, er zijn enkele belangrijke opvallende mogelijkheden die ze onderscheiden. Mogelijkheden alsmede de beperkingen van de volgende services worden vermeld onder om te helpen u terechtkomt op de oplossing die u nodig hebt om uw doelstellingen te bereiken.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Vergelijking met Stream Analytics storm: algemene functies ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Bron openen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nee, Azure Stream Analytics is een Microsoft-farmaceutische aanbod.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja, Apache Storm is een technologie Apache licentie.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsoft ondersteund</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hardwarevereisten</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Er zijn geen hardwarevereisten. Azure Stream Analytics is een Azure-Service.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Er zijn geen hardwarevereisten. Apache Storm is een Azure-Service.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Eenheid van het bovenste niveau</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Met Azure Stream Analytics klanten implementeren en streaming taken controleren.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Met Apache Storm op HDInsight klanten implementeren en controleren van een hele cluster, wat meerdere Storm taken, evenals andere werkbelasting (inclusief batch hosten kunt).
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Prijs</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream analyses met een prijs per hoeveelheid gegevens die worden verwerkt en het aantal streaming eenheden (per uur die de taak wordt uitgevoerd) vereist.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Verder prijsinformatie vindt u hier.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Voor Apache Storm op HDInsight, de indeling van de eenheid van aanschaf is cluster gebaseerde en in rekening wordt gebracht op basis van de tijd waarop die het cluster wordt uitgevoerd, onafhankelijk van taken die zijn geïmplementeerd.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Verder prijsinformatie vindt u hier.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Authoring op elke analytics-platform##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Mogelijkheden: SQL-DSL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja, een gebruiksvriendelijke ondersteuning van de SQL-taal beschikbaar is.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nee, moeten gebruikers programmacode schrijven in Java C# of Trident API's gebruiken.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Mogelijkheden: Tijdelijke operatoren</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Een venster aggregaties en tijdelijke joins worden ondersteund afmelden bij het vak.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Tijdelijke operatoren moeten worden uitgevoerd door de gebruiker.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Development-ervaring</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Interactieve authoring en foutopsporing ervaring via Azure-Portal met voorbeeldgegevens.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ontwikkeling, foutopsporing en ervaring voor controle wordt geleverd door de Visual Studio ervaring voor .NET-gebruikers, terwijl voor Java en andere talen ontwikkelaars de IDE van hun keuze mogelijk gebruikt.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ondersteuning voor foutopsporing</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics biedt eenvoudige taakstatus en bewerkingen Logboeken als een manier voor foutopsporing, maar momenteel biedt geen flexibiliteit in waar/hoe veel is opgenomen in de logboeken aan ie: uitgebreide modus.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Gedetailleerde logboeken zijn beschikbaar voor foutopsporing. Er zijn twee manieren om te surface logboeken aan gebruiker, via een visual Studio of gebruiker kunt u RDP in het cluster voor toegang tot logboeken.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ondersteuning voor UDF (door gebruiker gedefinieerde functies)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Er is momenteel geen ondersteuning voor UDF's.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
UDF's kunnen worden geschreven in C#, Java of de taal van uw keuze.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Extensible - aangepaste code</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Er is geen ondersteuning voor extensible-code in Stream Analytics.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja, is de beschikbaarheid van de aangepaste code in C#, Java of andere ondersteunde talen van Storm schrijven.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Gegevensbronnen en uitvoer##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Invoergegevens bronnen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>De ondersteunde invoerbronnen zijn Azure gebeurtenis Hubs en Azure BLOB's.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Verbindingslijnen zijn beschikbaar voor de gebeurtenis Hubs, Service Bus, Kafka, enzovoort. Niet-ondersteunde verbindingslijnen kunnen worden uitgevoerd via een aangepaste code.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Invoergegevens indelingen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ondersteunde invoer indelingen zijn Avro, JSON, CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Een indeling kan worden uitgevoerd via een aangepaste code.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Uitvoer</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Een streaming taak moet mogelijk meerdere uitvoer. Ondersteunde uitvoer: Azure gebeurtenis Hubs, Azure-blobopslag Azure tabellen, Azure SQL-DB en PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ondersteuning voor veel uitvoer in een topologie elke uitvoer aangepaste logica voor de verwerking van de volgende mogelijk hebben. Afmelden bij het vak bevat Storm verbindingslijnen voor PowerBI, Azure gebeurtenis Hubs, Azure Blob Store, Azure DocumentDB, SQL en HBase. Niet-ondersteunde verbindingslijnen kunnen worden uitgevoerd via een aangepaste code.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Gegevensindelingen-codering</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics vereist UTF-8-gegevensindeling te gebruiken.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Een codering gegevensindeling kan worden uitgevoerd via een aangepaste code.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Management en bedrijfsactiviteiten##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Taak-implementatie-model</strong>
                </p>
                <p>
                    - <strong>Azure-Portal</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Implementatie is geïmplementeerd via Azure-Portal, PowerShell en REST API's.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment wordt geïmplementeerd via Azure-Portal, PowerShell, Visual Studio en REST API's.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Cmdlets voor controle (Stat, items, enz.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Cmdlets voor controle wordt geïmplementeerd via Azure-Portal en REST API's.
                </p>
                <p>
De gebruiker kan ook Azure waarschuwingen configureren.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Cmdlets voor controle wordt geïmplementeerd via Storm UI en REST API's.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Schaalbaarheid</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Het aantal eenheden Streaming voor elke taak. Elke eenheid Streaming verwerkt maximaal 1 MB/s. Max van 50 eenheden al dan niet standaard. De oproep om uit te breiden limiet.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Het aantal knooppunten in het cluster HDI Storm. Geen limiet op aantal knooppunten (bovenste limiet gedefinieerd door de quota voor uw Azure). De oproep om uit te breiden limiet.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Gegevensverwerking limieten</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Gebruikers kunnen schaal omhoog of omlaag aantal eenheden Streaming om te vergroten gegevensverwerking of kosten optimaliseren.
                </p>
                <p>
De schaal van maximaal 1 GB/s aanpassen </p>
            </td>
            <td width="246" valign="top">
                <p>
Gebruiker kan schalen omhoog of omlaag clustergrootte behoeften.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Stoppen/cv</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stoppen en ga verder van de laatste plaats gestopt.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Stoppen en cv van laatste plaatsen gestopt op basis van het watermerk.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Update-service en framework</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automatische patches met geen downtime.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automatische patches met geen downtime.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Bedrijfscontinuïteit via een zeer Service aan gegarandeerd SERVICEOVEREENKOMST</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
SLA van 99,9% beschikbaarheid </p>
                <p>
Met automatisch herstel van fouten </p>
                <p>
Herstel statuscontrole tijdelijke operators is ingebouwd.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
SLA van 99,9% beschikbaarheid van het cluster Storm. Apache Storm is een foutenstructuuranalyse fouttolerante streaming-platform, maar het is de klanten verantwoordelijkheid om ervoor te zorgen dat hun streaming taken ononderbroken uitgevoerd.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Geavanceerde functies##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Vertraagde landing en het afhandelen van volgorde gebeurtenissen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ingebouwde configureerbare beleid voor het opnieuw ordenen, neerzetten of pas de gebeurtenistijd.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Gebruiker moet logica voor de afhandeling van dit scenario implementeren.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Referentiegegevens</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
De verwijzingsgegevens is verkrijgbaar via Azure BLOB's voor de maximale grootte van 100 MB in het geheugen opzoeken cache. Vernieuwen van verwijzingsgegevens wordt beheerd door de service.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Geen beperkingen voor de gegevensgrootte. Verbindingslijnen beschikbaar voor HBase, DocumentDB, SQL Server en Azure. Niet-ondersteunde verbindingslijnen kunnen worden uitgevoerd via een aangepaste code.
                </p>
                <p>
Vernieuwen van referentiegegevens moet worden verwerkt door aangepaste code.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Integratie met Machine Learning</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Door het configureren van Azure Machine Learning-modellen als-functies zijn gepubliceerd in ASA taak maken <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(privé preview)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Beschikbaar via de Storm bouten.
                </p>
            </td>
        </tr>
    </tbody>
</table>
