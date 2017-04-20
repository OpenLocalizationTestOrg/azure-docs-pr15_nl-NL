<properties 
   pageTitle="Gebruik van de weergave van de uitvoering hoekpunt in hulpmiddelen voor gegevens Lake voor Visual Studio | Microsoft Azure" 
   description="Informatie over het werken met de weergave van de uitvoering hoekpunt aan certificeringsexamens gegevens Lake Analytics projecten." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Gebruik van de weergave van de uitvoering hoekpunt in hulpmiddelen voor gegevens Lake voor Visual Studio

Informatie over het werken met de weergave van de uitvoering hoekpunt aan certificeringsexamens gegevens Lake Analytics projecten.

## <a name="prerequisites"></a>Vereisten voor

- Basiskennis op het gebied van het gebruik van Data Lake Tools voor Visual Studio I-SQL-script ontwikkelen.  Zie [Zelfstudie: I-SQL-scripts met Data Lake Tools voor Visual Studio ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Open de weergave van de uitvoering hoekpunt

Voor een bepaalde functie, kunt u de koppeling 'Hoekpunt Execution weergeven' in de linkerbenedenhoek. Mogelijk moet u eerst profielen laden en het enige tijd afhankelijk van uw netwerkconnectiviteit kan duren.

![Gegevens Lake Analytics extra hoekpunt Execution weergave](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Hoekpunt Execution weergave begrijpen

Na het invoeren van de weergave van de uitvoering hoekpunt, zijn er drie onderdelen:

![Gegevens Lake Analytics extra hoekpunt Execution weergave](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- Hoekpunt Gegevenskiezer: aan de linkerkant is het hoekpunt Gegevenskiezer.  U kunt de hoekpunten selecteren door functies (zoals top 10-gegevens lezen, of kies fase).

    Een van de meest voorkomende gebruikte filters is de hoekpunten op kritieke pad. Kritieke pad is de langste pad van een taak I-SQL. Het is handig voor het optimaliseren van uw taken door te schakelen welke hoekpunt de langste duurt.

- Het deelvenster middenboven:

    ![Gegevens Lake Analytics extra hoekpunt Execution weergave](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    Deze weergave worden ook de huidige status van alle hoekpunten. Deze converteert de tijd dienovereenkomstig gewijzigd naar uw lokale computer, en ziet u verschillende status in verschillende kleuren.

- Het middelste deelvenster van onder:

    ![Gegevens Lake Analytics extra hoekpunt Execution weergave](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - De procesnaam van het: De naam van het hoekpunt exemplaar. Deze bestaat uit verschillende delen in StageName | VertexName | VertexRunInstance. Het SV7_Split [62] .v1 hoekpunt staat bijvoorbeeld voor het tweede actieve exemplaar (.v1, beginnend bij 0 index) van hoekpunt getal 62 in fase SV7_Split.
    - Totale gegevens alleen-lezen/schrijven: De gegevens zijn gelezen/geschreven door deze hoekpunt.
    - De staat/afsluiten Status: De uiteindelijke status wanneer het hoekpunt wordt beëindigd.
    - Type van de Code/fout afsluiten: De fout bij het hoekpunt is mislukt.
    - Reden maken: Waarom het hoekpunt is gemaakt.
    - Resource latentie/proces latentie/PN wachtrij latentie: de tijd die u hebt gemaakt voor het hoekpunt moet wachten om door resources, om gegevens te verwerken en behouden in de wachtrij aan.
    - GUID van proces/Creator: GUID voor het huidige actieve hoekpunt of de maker.
    - Versie: de N-de exemplaar van het actieve hoekpunt (het systeem kunt plannen nieuwe exemplaren van een hoekpunt voor groot aantal redenen, bijvoorbeeld failover redundantie, enz berekenen.)
    - Versie gemaakt wanneer.
    - Proces maken Start tijd/proces in wachtrij tijd/proces Start tijd/proces voltooid tijd: wanneer het hoekpunt-proces wordt gestart maken; Als het hoekpunt-proces wordt gestart in de wachtrij; Als het bepaalde hoekpunt-proces wordt gestart; Wanneer het bepaalde hoekpunt is voltooid.

## <a name="next-steps"></a>Volgende stappen

- Als u een overzicht van gegevens Lake analyses, Zie [overzicht van de Azure gegevens Lake Analytics](data-lake-analytics-overview.md).
- Zie [ontwikkelen I--SQL-scripts met gegevens Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)om te beginnen ontwikkelen van toepassingen I-SQL.
- Zie [aan de slag met Azure gegevens Lake Analytics U SQL - taal](data-lake-analytics-u-sql-get-started.md)voor meer I-SQL.
- Zie [Azure gegevens Lake Analytics beheren met behulp van Azure portal](data-lake-analytics-manage-use-portal.md)voor beheertaken.
- U wilt vastleggen van diagnostische informatie, raadpleegt u [toegang tot diagnostische logboeken van Azure gegevens Lake analysegegevens](data-lake-analytics-diagnostic-logs.md)
- Een complexe query's, raadpleegt u [analyseren Website Logboeken door middel van Azure gegevens Lake analyses](data-lake-analytics-analyze-weblogs.md).
- Taakdetails van een, Zie [Gebruik taak Browser en taak voor Azure gegevens lake Analytics-taken weergeven](data-lake-analytics-data-lake-tools-view-jobs.md)