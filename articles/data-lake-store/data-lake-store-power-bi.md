<properties
   pageTitle="Gegevens analyseren in Lake gegevensopslag met behulp van Power BI | Microsoft Azure"
   description="Power BI gebruiken voor het analyseren van gegevens die zijn opgeslagen in Azure Lake gegevensopslag"
   services="data-lake-store" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Gegevens analyseren in Lake gegevensopslag met behulp van Power BI

In dit artikel leert u hoe u met Power BI Desktop analyseren en visualiseren van gegevens die zijn opgeslagen in de gegevensopslag Lake Azure.

## <a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **De gegevensopslag Lake azure-account**. Volg de instructies bij [aan de slag met Azure Lake gegevensopslag met behulp van de Azure-Portal](data-lake-store-get-started-portal.md). In dit artikel wordt ervan uitgegaan dat u hebt al een account voor gegevensopslag Lake, **mybidatalakestore**, genaamd gemaakt en die zijn geüpload, een voorbeeld van gegevensbestand (**Drivers.txt**) toe. In dit voorbeeld van een bestand is beschikbaar voor downloaden van [Azure gegevens Lake cijfer opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).

- **Power BI Desktop**. U kunt dit downloaden van het [Microsoft Downloadcentrum](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 


## <a name="create-a-report-in-power-bi-desktop"></a>Een rapport in Power BI Desktop maken

1. Start de Power BI Desktop op uw computer.

2. Klik op **Gegevens ophalen**uit op het lint **Start** en klik vervolgens op meer. In het dialoogvenster **Gegevens ophalen** , klikt u op **Azure**, klikt u op **Azure Lake gegevensopslag**en klik op **verbinding maken**.

    ![Verbinding maken met Lake gegevensopslag] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Verbinding maken met Lake gegevensopslag")

3. Als er een dialoogvenster over de verbindingslijn die in een ontwikkelingsfase, ook voor kiezen om door te gaan.

4. Klik in het dialoogvenster **Microsoft Azure gegevensopslag Lake** de URL naar uw account voor gegevensopslag Lake en klik vervolgens op **OK**.

    ![URL voor gegevensopslag Lake] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL voor gegevensopslag Lake")

5. Klik in het volgende dialoogvenster op **aanmelden** u zich aanmeldt bij Lake gegevensopslag-account. U wordt omgeleid naar de aanmeldingspagina van uw organisatie. Volg de aanwijzingen u zich aanmeldt bij het account.

    ![Meld u aan bij Lake gegevensopslag] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Meld u aan bij Lake gegevensopslag")

6. Nadat u bent aangemeld, klikt u op **verbinding maken**.

    ![Verbinding maken met Lake gegevensopslag] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Verbinding maken met Lake gegevensopslag")

7. Het volgende dialoogvenster ziet u het bestand dat u hebt geüpload naar uw account voor gegevensopslag Lake. Controleer of de info en klik vervolgens op **laden**.

    ![De gegevens laden uit Lake gegevensopslag] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "De gegevens laden uit Lake gegevensopslag")

8. Nadat de gegevens is geladen in Power BI, ziet u de volgende velden op het tabblad **velden** .

    ![Geïmporteerd velden] (./media/data-lake-store-power-bi/imported-fields.png "Geïmporteerd velden")

    Als u wilt visualiseren en de gegevens te analyseren, liever we echter de gegevens kan worden gecontroleerd per de volgende velden

    ![Gewenste velden] (./media/data-lake-store-power-bi/desired-fields.png "Gewenste velden")

    We zullen de query om de geïmporteerde gegevens in de gewenste indeling te converteren bijwerken in de volgende stappen.

9. Klik op **Query's bewerken**in het lint **Start** .

    ![Bewerken van query 's] (./media/data-lake-store-power-bi/edit-queries.png "Bewerken van query 's")

10. In Query-Editor, klik onder de kolom **inhoud** op **binaire**.

    ![Bewerken van query 's] (./media/data-lake-store-power-bi/convert-query1.png "Bewerken van query 's")

11. Hier ziet u een bestandspictogram, waarmee het **Drivers.txt** -bestand dat u hebt geüpload. Met de rechtermuisknop op het bestand en klik op **CSV**.  

    ![Bewerken van query 's] (./media/data-lake-store-power-bi/convert-query2.png "Bewerken van query 's")

12. U ziet een uitvoer zoals hieronder wordt weergegeven. Uw gegevens is nu beschikbaar in een indeling die u gebruiken kunt om te maken van visualisaties.

    ![Bewerken van query 's] (./media/data-lake-store-power-bi/convert-query3.png "Bewerken van query 's")

13. Het lint **Start** op **sluiten en toepassen**en klik vervolgens op **sluiten en toe te passen**.

    ![Bewerken van query 's] (./media/data-lake-store-power-bi/load-edited-query.png "Bewerken van query 's")

14. Wanneer de query wordt bijgewerkt, wordt het tabblad **velden** de nieuwe velden die beschikbaar zijn voor visualisatie weergegeven.

    ![Bijgewerkte velden] (./media/data-lake-store-power-bi/updated-query-fields.png "Bijgewerkte velden")

15. Laat ons een cirkeldiagram bevatten de stuurprogramma's in elke stad voor een bepaald land te maken. Hiervoor moet u de volgende selecties maken.

    1. Klik op het tabblad visualisaties op het symbool voor een cirkeldiagram.

        ![Cirkeldiagram maken] (./media/data-lake-store-power-bi/create-pie-chart.png "Cirkeldiagram maken")

    2. De kolommen die we gaat gebruiken zijn **kolom 4** (naam van de plaats) en **kolom 7** (naam van het land). Sleep deze kolommen van het tabblad **velden** naar het tabblad **Visualisaties** zoals hieronder wordt weergegeven.

        ![Visualisaties maken] (./media/data-lake-store-power-bi/create-visualizations.png "Visualisaties maken")

    3. Het cirkeldiagram ziet er nu zoals hieronder wordt weergegeven.

        ![Cirkeldiagram] (./media/data-lake-store-power-bi/pie-chart.png "Visualisaties maken")

16. Als u een bepaald land van de pagina niveau filters selecteert, kunt u nu het aantal stuurprogramma's in elke stad van het geselecteerde land zien. Bijvoorbeeld, klik op het tabblad **Visualisaties** onder het **niveau van de pagina gefilterd**, selecteert u **Brazilië**.

    ![Selecteer een land] (./media/data-lake-store-power-bi/select-country.png "Selecteer een land")

17. Het cirkeldiagram wordt automatisch bijgewerkt zodat de stuurprogramma's in de steden van Brazilië.

    ![Stuurprogramma's in een land] (./media/data-lake-store-power-bi/driver-per-country.png "Stuurprogramma's per land")

18. Klik op **Opslaan** om de visualisatie opslaan als een Power BI Desktop-bestand uit het menu **bestand** .

## <a name="publish-report-to-power-bi-service"></a>Rapport publiceren naar Power BI-service

Nadat u de visualisaties in Power BI Desktop hebt gemaakt, kunt u deze delen met anderen door deze te publiceren naar de Power BI-service. Zie voor instructies over hoe u dat doen, [publiceren van Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Zie ook

* [Gegevens analyseren in Lake gegevensopslag door middel van gegevens Lake analyses](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
