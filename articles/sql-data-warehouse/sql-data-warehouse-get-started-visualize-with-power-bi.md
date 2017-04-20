<properties
   pageTitle="SQL Data Warehouse gegevens met Power BI Microsoft Azure visualiseren"
   description="SQL Data Warehouse gegevens met Power BI visualiseren"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Gegevens met Power BI visualiseren

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Deze zelfstudie wordt getoond hoe u verbinding maken met SQL Data Warehouse en een paar basisvisualisaties kunt maken met Power BI.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie stapsgewijs, hebt u het volgende nodig:

- Een SQL-Data Warehouse vooraf geladen met de AdventureWorksDW-database. Als u wilt inrichten dit, Zie [maken een SQL-Data Warehouse][] en kies de voorbeeldgegevens laden. Als u al een data-warehouse hebt, maar ik heb geen voorbeeldgegevens, kunt u [handmatig voorbeeldgegevens laden][].


## <a name="1-connect-to-your-database"></a>1. verbinding maken met uw database

Power BI openen en verbinding maken met uw AdventureWorksDW-database:

1. Meld u aan bij de [portal van Azure][].
2. Klik op **SQL-databases** en kies uw AdventureWorks SQL Data Warehouse-database.

    ![Zoek de database][1]

3. Klik op de knop 'Openen in Power BI'.

    ![Power BI-knop][2]

4. U ziet nu de pagina van het SQL Data Warehouse verbinding met het webadres van uw database. Klik op volgende.

    ![Power BI-verbinding][3]

6. Voer uw Azure SQL server-gebruikersnaam en wachtwoord en u wordt volledig niet worden verbonden met uw SQL Data Warehouse-database.

    ![Power BI aanmelden][4]

7. Zodra u hebt aangemeld bij Power BI, klikt u op de gegevensset AdventureWorksDW op het linker blad. Hiermee opent u de database.

    ![Open AdventureWorksDW van Power BI][5]



## <a name="2-create-a-report"></a>2. een rapport maken

U bent nu klaar voor gebruik van Power BI AdventureWorksDW steekproef gegevens te analyseren. Als u wilt uitvoeren in de analyse, heeft AdventureWorksDW een weergave genaamd AggregateSales. Deze weergave bevat een paar van de doelstellingen van de belangrijkste voor het analyseren van de omzet van het bedrijf.

1. Als u wilt een kaart van het verkoopbedrag op basis van de postcode, in het deelvenster rechts velden maken, klikt u op de weergave AggregateSales weer te geven. Klik op de postcode en SalesAmount kolommen om deze te selecteren.

    ![Power BI Selecteer AggregateSales][6]

    Power BI herkent automatisch dit is de geografische gegevens en zet dit in een kaart voor u.

    ![Power BI-kaart][7]

2. Deze stap maakt een staafdiagram waarin bedrag van verkoop per klant inkomsten. Dit Ga naar de uitgevouwen AggregateSales weergave wilt maken. Klik op het veld Verkoopbedrag. Sleep het veld klant inkomsten naar links en zet deze neer in as.

    ![Selecteer Power BI-as][8]

    We verplaatst het staafdiagram op links.

    ![Power BI-balk][9]

3. Deze stap maakt een lijndiagram bevat waarin de totale omzet per orderdatum ligt. Dit Ga naar de uitgevouwen AggregateSales weergave wilt maken. Klik op verkoopbedrag en Orderdatum. Klik in de visualisaties kolom op het pictogram lijndiagram. Dit is het eerste pictogram in de tweede regel onder visualisaties.

    ![Power BI select-lijndiagram][10]

    U hebt nu een rapport met drie verschillende visualisaties van de gegevens.

    ![Power BI-lijn][11]

U kunt de voortgang van uw op elk gewenst moment besparen door te klikken op **bestand** en **Opslaan**te selecteren.

## <a name="next-steps"></a>Volgende stappen
Nu we hebt u gegeven enige tijd om te warme omhoog met de voorbeeldgegevens, kunt u bekijken hoe u [ontwikkelen][], [laden][]of [migreren][]. Of gaat u naar de [Power BI-website][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migreren]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md
[laden]: sql-data-warehouse-overview-load.md
[voorbeeldgegevens handmatig laden]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Een SQL-datawarehouse maken]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure-portal]: https://portal.azure.com/
[Power BI-website]: http://www.powerbi.com/
