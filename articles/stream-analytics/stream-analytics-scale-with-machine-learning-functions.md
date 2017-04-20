<properties
    pageTitle="De schaal van uw werk Stream analyses met Azure Machine Learning-functies aanpassen | Microsoft Azure"
    description="Leer hoe u goed schaal Stream Analytics taken (partitioneren, so hoeveelheid, en meer) bij gebruik van Azure Machine Learning-functies."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>De schaal van uw werk Stream analyses met Azure Machine Learning-functies aanpassen

Dit is het meestal vrij eenvoudig een Stream Analytics-taak instellen en uitvoeren van enkele voorbeeldgegevens door de werkmap. Wat doe we wanneer moeten we dezelfde taak uitvoert met hogere gegevensvolume? Er moeten ons weten hoe u de taak Stream Analytics zodanig configureren dat deze wordt schaal. In dit document gaan we in op de speciale aspecten van schaalbaarheid van de Stream Analytics-taken met Machine Learning-functies. Zie het artikel [schaal taken](stream-analytics-scale-jobs.md)voor meer informatie over hoe u de schaal Stream Analytics-taken in het algemeen.

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Wat is een Azure Machine Learning-functie in Stream Analytics?

Een Machine Learning-functie in Stream Analytics kan worden gebruikt als een gewone functieoproep in de querytaal Stream Analytics. De functie oproepen zijn echter achter de scène, daadwerkelijk Azure Machine Learning-webservice aanvragen. Machine Learning-webservices ondersteuning "batchen" meerdere rijen, die wel Mini batch, in dezelfde web service API aanroep van, de algehele doorvoer verbeteren. Raadpleeg de volgende artikelen voor meer informatie; [Azure Machine Learning-functies in Stream Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) en [Azure Machine Learning-webservices](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Een taak Stream Analytics configureren met Machine Learning-functies

Tijdens het configureren van een Machine Learning-functie voor Stream Analytics taak, zijn er twee parameters u rekening moet houden, de batchgrootte van de oproepen Machine Learning-functie en de eenheden Streaming (SUs) is deze is ingericht voor de taak Stream Analytics. Om te bepalen de juiste waarden voor deze, moet eerst een worden besloten tussen latentie en doorvoer, dat wil zeggen latentie van de Stream Analytics taak en doorvoer van elke SU. SUs kunnen altijd worden toegevoegd aan een taak om uit te breiden doorvoer van een goed gepartitioneerde Stream Analytics-query, hoewel extra SUs vergroot de kosten van het uitvoeren van de taak.

Het is dus belangrijk om te bepalen de *tolerantie* van latentie in een Stream Analytics-taak wordt uitgevoerd. Extra latentie Azure Machine Learning-serviceaanvragen uitvoeren, natuurlijk neemt met batchgrootte, die wordt samengesteld de latentie van de Stream Analytics-taak. Aan de andere kant, de taak Stream Analytics verwerkingstijd batch vergroten kunt *meer gebeurtenissen met de *hetzelfde getal * van Machine Learning web serviceaanvragen. De toename van Machine Learning web service latentie is vaak sub lineaire tot de toename van de batchgrootte dus is het belangrijk om rekening houden met de batchgrootte van de kosten-efficiëntste voor een Machine Learning-webservice in een bepaalde situatie. De standaardgrootte voor de batch voor de web-serviceaanvragen 1000 is en kan worden gewijzigd via de [Stream Analytics REST API]-(https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST API") of de [PowerShell-client voor Stream Analytics]-(stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShell-client voor Stream Analytics").

Wanneer de batchgrootte van een is bepaald, de hoeveelheid Streaming eenheden (SUs) worden bepaald, op basis van het aantal gebeurtenissen die de functie zal verwerken per seconde. Raadpleeg het artikel [Stream Analytics schaal taken](stream-analytics-scale-jobs.md#configuring-streaming-units)voor meer informatie over Streaming eenheden.

In het algemeen, zijn er 20 gelijktijdige verbindingen met de Machine Learning-webservice voor elke 6 SUs, behalve dat 1 SU taken en 3 SU taken 20 verbindingen ook krijgen.  Als het tarief weer dat invoergegevens 200.000 gebeurtenissen per seconde is en de batchgrootte er nog de standaardversie van 1000 is de resulterende web service latentie met 1000 gebeurtenissen Mini batch bijvoorbeeld 200 MS. Dit betekent dat elke verbinding 5 aanvragen kunt aanbrengen in de Machine Learning-webservice in seconden. Met 20 verbindingen, kan de taak Stream Analytics 20.000 gebeurtenissen in 200 MS en kunnen daarom 100.000 gebeurtenissen verwerken in een tweede. Zo te verwerken 200.000 gebeurtenissen per seconde, moet de Stream Analytics-taak 40 verbindingen, die u kunt uit 12 SUS kiezen. Het onderstaande diagram ziet u het aanvragen van de Stream Analytics-taak voor het Machine Learning web service-eindpunt: elke 6 SUs 20 gelijktijdige verbindingen met Machine Learning-webservice heeft op max.

![Schaal Stream analyses met Machine Learning functies 2 taak voorbeeld] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Schaal Stream analyses met Machine Learning functies 2 taak voorbeeld")

In het algemeen, ***B*** voor batchgrootte, ***L*** voor het web service latentie op batchgrootte B (in milliseconden), de doorvoer van een taak Stream analyses met ***N*** SUs luidt als volgt:

![De schaal van Stream analyses met Machine Learning-functies formule aanpassen] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "De schaal van Stream analyses met Machine Learning-functies formule aanpassen")

Een extra overweging mogelijk de oproepen' max gelijktijdige' aan de kant Machine Learning web service, het is raadzaam deze optie instellen aan de maximumwaarde (momenteel 200).

Voor meer informatie over deze instelling-Bekijk het [artikel schaal voor Machine Learning-webservices](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Voorbeeld – Sentiment analyse

Het volgende voorbeeld bevat een Stream Analytics-project met de analyse sentiment Machine Learning-functie, zoals wordt beschreven in de [Stream Analytics Machine Learning-integratie zelfstudie](stream-analytics-machine-learning-integration-tutorial.md).

De query is een eenvoudige volledig gepartitioneerde query, gevolgd door de functie **sentiment** , zoals hieronder wordt weergegeven:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Houd rekening met de volgende scenario; met een capaciteit van 10.000 tweets per seconde moet een taak Stream Analytics worden gemaakt als u wilt uitvoeren sentiment analyse van de tweets (gebeurtenissen). 1 SU gebruikt, kan deze taak Stream Analytics zichtbaar mogen zijn het verkeer verwerken? De grootte van de batch standaard van 1000 met moet de taak mogelijk bij de invoer te houden. Verder de toegevoegde Machine Learning-functie moet worden gegenereerd niet meer dan een tweede van latentie, dat wil de algemene zeggen standaard latentie van de analyse sentiment Machine Learning-webservice (met een batch standaardgrootte van 1000). De **algehele** of end-to-end latentie van de Stream Analytics-taak is meestal een paar seconden. Verdiep u meer gedetailleerde in deze stroom Analytics taak *met name* de Machine Learning-functie oproepen. Lukt het formaat van de batch als 1000, duurt een capaciteit van 10.000 gebeurtenissen ongeveer 10 aanvragen webservice. Zelfs met 1 SU, zijn er voldoende verbindingen zijn voor deze invoer-verkeer is toegestaan.

Maar wat gebeurt er als het tarief weer dat invoer gebeurtenis vergroot met 100 x en nu de Stream Analytics-taak moet worden verwerkt van 1.000.000 tweets per seconde? Er zijn twee opties:

1.  Vergroot het formaat batch of
2.  De invoer stream te verwerken van de gebeurtenissen parallel partitioneren

Met de eerste optie, wordt de taak **Latentie** vergroten.

Met de tweede optie moeten meer SUs worden deze is ingericht en kunnen daarom genereren meer gelijktijdige Machine Learning web-serviceaanvragen. Dit betekent dat de taak **kosten** , neemt.


Stel de latentie van de analyse sentiment Machine Learning-webservice 200 MS voor 1000-gebeurtenis batches of onder, 250ms voor 5.000-gebeurtenis batches, 300ms voor 10.000-gebeurtenis batches of 500ms voor 25.000-gebeurtenis batches.

1. De eerste optie, (**niet** meer SUs inrichting) gebruikt, kan de batchgrootte worden verhoogd tot **25.000**. Deze manier om de taak 1.000.000 om gebeurtenissen te verwerken met 20 gelijktijdige verbindingen met de Machine Learning-webservice (met een latentie van 500ms per gesprek) kunt. De extra latentie van de Stream Analytics-taak als gevolg van het aanvragen van de functie sentiment ten opzichte van de serviceaanvragen van Machine Learning web zou dus worden verhoogd van **200 MS** naar **500ms**. Echter notitie die batch grootte **kan niet** worden verbeterde oneindig terwijl de Machine Learning-webservices moet de grootte van de nettolading van een aanvraag kunt invullen 4 MB of kleiner webservice aanvraagt time-out na 100 seconden van bewerking.
2. De tweede optie gebruikt, de batchgrootte resteert op 1000, met 200 MS web service latentie, elke 20 gelijktijdige verbindingen met de webservice zou kunnen om 1000 *20* 5 gebeurtenissen te verwerken = 100.000 per seconde. Te verwerken 1.000.000 gebeurtenissen per seconde, moet de taak dus 60 SUs. Vergeleken met de eerste optie, maakt Stream Analytics taak meer web batch serviceaanvragen, een extra kosten beurtelings te genereren.

Hieronder vindt u een tabel worden de verwerkt van de Stream Analytics-taak voor verschillende SUs en grootte van de batch (in het aantal gebeurtenissen per seconde).

| Batchgrootte (ML latentie)  | 500 (200 MS) | 1.000 (200 MS) | 5.000 (250ms) | 10.000 (300ms) | 25.000 (500ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SU** | 2500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **3 SUs** | 2500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **6 SUs** | 2500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **12 SUs** | 5.000 | 10.000 | 40.000 | 60.000 | 100.000 |
| **18 SUs** | 7.500 | 15.000 | 60.000 | 90,000 | 150.000 |
| **24 SUs** | 10.000 | 20.000 | 80.000 | 120.000 | 200.000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25.000 | 50.000 | 200.000 | 300.000 | 500.000 |

Nu, moet u al een goed inzicht in de werking van Machine Learning-functies in Stream Analytics hebben. U waarschijnlijk begrijpen ook dat Stream Analytics taken "halen" gegevens uit gegevensbronnen en elke "halen" geeft als resultaat een reeks gebeurtenissen voor de taak Stream Analytics verwerken. Hoe deze halen model gevolgen de Machine Learning serviceaanvragen met webonderdelen?

De batchgrootte die we voor Machine Learning-functies instellen niet normaal gesproken vergt precies worden gedeeld door het aantal gebeurtenissen die het resultaat van elke taak Stream Analytics "halen". Dit gebeurt wanneer dat de Machine Learning-webservice wordt aangeroepen met "gedeeltelijke" batches. Dit gebeurt te realiseren van extra taak latentie in samenvoegen gebeurtenissen niet halen halen te maken.

## <a name="new-function-related-monitoring-metrics"></a>Nieuwe functie-gerelateerde controleren doelstellingen

In het gebied Monitor van een taak Stream Analytics zijn drie extra functie-gerelateerde statistieken toegevoegd. Dit zijn de functie aanvragen, gebeurtenissen voor de functie en MISLUKTE functie aanvragen, zoals wordt weergegeven in de onderstaande afbeelding.

![De schaal van Stream analyses met Machine Learning-functies aan de doelstellingen aanpassen] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "De schaal van Stream analyses met Machine Learning-functies aan de doelstellingen aanpassen")

De vraag are als volgt gedefinieerd:

**Functie aanvragen**: het aantal aanvragen van de functie.

**Functie gebeurtenissen**: de getal gebeurtenissen in de functie aanvragen.

**MISLUKTE functie aanvragen**: het aantal mislukte functie aanvragen.

## <a name="key-takeaways"></a>Belangrijke Takeaways  

Als u wilt samenvatten de belangrijkste punten, om te kunnen de schaal van een taak Stream analyses met Machine Learning-functies, moeten de volgende items worden beschouwd:

1.  Het tarief weer dat invoer gebeurtenis
2.  De mag latentie voor de actieve Stream Analytics-taak (en dus de batchgrootte van de serviceaanvragen van Machine Learning web)
3.  De ingerichte Stream Analytics SUs en het aantal Machine Learning web serviceaanvragen (de extra functie-gerelateerde kosten)

Een volledig gepartitioneerde Stream Analytics-query is als voorbeeld gebruikt. Als een meer complexe query nodig is het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) is een uitstekende resource voor het verkrijgen van extra hulp van de Stream Analytics-team.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Stream analyses, raadpleegt u:

- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
