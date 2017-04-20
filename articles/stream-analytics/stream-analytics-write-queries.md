<properties 
    pageTitle="Query's schrijven in Stream Analytics | Microsoft Azure" 
    description="Query's in de Stream analyses en querygegevens schrijven | leren padsegment."
    keywords="hoe u query's, querygegevens schrijft, schrijven van een query, query's schrijven"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="how-to-write-queries-in-stream-analytics"></a>Query's schrijven in Stream Analytics

Query's voor de verwerking van logica in Azure Stream Analytics stream schrijven wordt geïmplementeerd als een 'permanente query' die is gedefinieerd voordat een taak wordt gestart en wordt uitgevoerd op gegevens, zoals de taak worden bereikt. De gegevenstransformatie wordt uitgedrukt in een SQL-achtige querytaal, dat wil grotendeels een subset van de T-SQL met bepaalde taaluitbreidingen toegevoegde zeggen zoals [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) tijdelijke semantiek Express gebruikt.

## <a name="writing-queries"></a>Query's schrijft: ##

1. Klik in uw project Stream Analytics in de beheerportal van Azure op **Query**.

    ![Selecteer Query](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Klik in de Portal Azure op **Query**.

    ![Selecteer Queryvoorbeeld](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Nieuwe taken hebben een querysjabloon kunt aan de slag te gaan. De querysjabloon voert een "Pass Through" query waarbij de projecten die alle velden uit invoer gebeurtenissen in de uitvoer.  

    - Als u ten minste één invoer en uitvoer voor uw werk hebt gedefinieerd, kunt u de tijdelijke aanduiding "[YourOutputAlias]" vervangen en "[YourInputAlias]" velden met de aliassen van de invoer en uitvoer die u wilt gebruiken eerst. Bovendien kunt u nog steeds de auteur en de query testen in de klassieke Azure-Portal zonder invoer en uitvoer die aan de taak.
    - Als u uitvoeren voor de verwerking van meer dan een eenvoudige Pass Through-query wilt, kunt u de definitie van de query bewerken. Als u wilt beginnen met het ontwerpen van de query, gaat u naar enkele veelvoorkomende query patronen zijn vastgelegd [hier](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Querygegevens venster](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Als u wilt querygegevens valideren werkt: ##

U kunt testen dat uw query naar verwachting door deze in de browser te voeren via een of meer lokale JSON bestanden met testgegevens werkt. Dit niet begint de taak of een factureringsbeheerder gevolgen hebben.

> [AZURE.NOTE] Browser-query testen is momenteel niet ondersteund in de Portal Azure.  

1.  Zorg ervoor dat er geen fouten in de query (anders kan de knop testen wordt uitgeschakeld) en klik vervolgens op de knop testen.  

    ![Querygegevens testen](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  U wordt gevraagd om op te geven van bestanden voor elk van de invoer in de query wordt verwezen. In dit voorbeeld de query sjabloon als er nog-is, zodat het dialoogvenster is waarin u wordt gevraagd om een invoer met de naam 'yourinputalias'.  

    ![Gegevens-testquery](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Blader naar een testbestand. Verschillende voorbeeldbestanden zijn beschikbaar op [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) en u kunt ook voorbeeldgegevens ophalen uit uw eigen gegevens stream invoeritems via de functie voorbeeldgegevens op het tabblad invoeritems.  

    ![Query invoer](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  Nadat het dialoogvenster te sluiten, de query wordt uitgevoerd via de testgegevens en u de resultaten onder aan de querypagina ziet.  

    ![Overzicht van de query](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
