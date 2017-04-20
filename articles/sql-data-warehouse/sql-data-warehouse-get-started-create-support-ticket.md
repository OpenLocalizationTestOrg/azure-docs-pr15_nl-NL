<properties
   pageTitle="Het maken van een ondersteuningsticket voor SQL Data Warehouse | Microsoft Azure"
   description="Het maken van een ondersteuningsticket in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Het maken van een ondersteuningsticket voor SQL Data Warehouse
 
Als u problemen hebt met uw SQL Data Warehouse hoeft Maak voor een ondersteuning kaartverkoop zodat onze technische team kan u helpen.

## <a name="create-a-support-ticket"></a>Een ondersteuningsticket maken

1. Open de [portal van Azure][].

2. Klik op het beginscherm op de tegel **Help + ondersteuning** .

    ![Help + ondersteuning](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. In het menu Help + ondersteuning blade, klikt u op **verzoek voor ondersteuning van maken**.

    ![Nieuwe verzoek voor ondersteuning](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Selecteer het **Type aanvragen**.

    ![Type aanvragen](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Standaard bevat elk SQL-server (bijvoorbeeld myserver.database.windows.net) een **DTU quotum** van 45,000. Dit quotum is alleen een limiet veiligheid. U kunt de quota voor uw verhogen door te maken van een ondersteuningsticket en *Quotum* selecteren als het type verzoek. Voor het berekenen van uw DTU moeten, vermenigvuldigt u het 7.5 hebt met het totale [dat DWU][] nodig. Zoals u wilt hosten twee DW6000s op één SQL server en klik vervolgens moet u een quotum DTU van 90,000 aanvragen.  U kunt uw huidige DTU verbruik van het SQL server-blad weergeven in de portal. Onderbroken en voortgezet databases tellen naar het quotum voor DTU. 

5. Selecteer het **abonnement** waarop de database met het probleem dat u wilt melden.

    ![Abonnement](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. Selecteer **SQL datawarehouse** als de Resource.

    ![Resource](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Selecteer uw [Azure ondersteunen abonnement][].

    - Ondersteuning voor **Facturering quota en abonnementen management** is beschikbaar op alle niveaus van de ondersteuning.
    - **Einde-fix** ondersteuning is beschikbaar via [ontwikkelaars][], [standaard][], [Professional directe][] of [Premier][] support. Einde-problemen oplossen worden problemen waarmee klanten bij het gebruik van Azure wanneer er een redelijk verwachting dat het probleem wordt veroorzaakt door Microsoft.
    - **Ontwikkelaars begeleiding** en **advies services** zijn beschikbaar op het niveau van de ondersteuning voor [Directe Professional][] en [Premier][] . 
    
    Als u een Premier ondersteuning voor abonnement hebt, kunt u ook SQL Data Warehouse melden problemen op de [Microsoft Premier online-portal][].  Zie [Azure ondersteuning voor abonnementen]voor[Azure ondersteunen plannen] voor meer informatie over de verschillende abonnementen voor ondersteuning, waaronder bereik, antwoord tijden, prijzen, enzovoort.  Ondersteuning voor veelgestelde vragen over Azure: Zie van [Azure ondersteunen Veelgestelde vragen][].  

    ![Ondersteuning voor abonnement](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. Selecteer het **probleemtype** en de **categorie**.

    ![Probleem type categorie](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. Het probleem beschrijven en kiest u het niveau van de gevolgen voor bedrijven.

    ![Beschrijving van probleem](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. Uw **contactgegevens** voor deze ondersteuningsticket zijn vooraf ingevuld. Werk deze indien nodig.

    ![Contactgegevens](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Klik op **maken** om de aanvraag ondersteuning te verzenden.


## <a name="monitor-a-support-ticket"></a>Monitor met een ondersteuningsticket

Nadat u het verzoek voor ondersteuning hebt ingediend, het ondersteuningsteam van Azure wordt contact met u. Als u wilt controleren of uw aanvraagstatus en details, klikt u op **aanvragen voor ondersteuning van beheren** op het dashboard.

![Status controleren](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Overige informatiebronnen

Bovendien kunt u een verbinding maken met de SQL Data Warehouse-community op [Stapel overloop][] of op het [Azure SQL Data Warehouse MSDN-forum][].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Azure-portal]: https://portal.azure.com/
[Ondersteuning voor Azure-abonnement]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Ontwikkelaars]: https://azure.microsoft.com/support/plans/developer/  
[Standaard]: https://azure.microsoft.com/support/plans/standard/  
[Professionele Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure ondersteuning Veelgestelde vragen]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online-portal]: https://premier.microsoft.com/
[Stapel overloop]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

