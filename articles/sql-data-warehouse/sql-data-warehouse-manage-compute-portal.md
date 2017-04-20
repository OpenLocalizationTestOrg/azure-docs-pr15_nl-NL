<properties
   pageTitle="Beheren rekenkracht in Azure SQL Data Warehouse (Azure portal) | Microsoft Azure"
   description="Azure portal taken voor het beheren van rekenkracht. Schaal berekenen resources DWUs aanpassen. Of, onderbreken en hervatten resources om op te slaan kosten berekenen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Rekenkracht in Azure SQL Data Warehouse (Azure portal) beheren

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Prestaties van de schaal door schalen berekenen bronnen en het geheugen om te voldoen aan de veranderende vereisten van uw werkzaamheden. Bespaar kosten door schaal achterste resources tijdens de niet-tijden of onderbreken berekeningscluster helemaal af. 

Deze verzameling van taken wordt gebruikt voor de Azure-portal naar:

- Schaal berekenen
- Een pauze invoegen berekenen
- Cv berekenen

Zie [beheren berekenen overzicht][]voor meer informatie.

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Rekenkracht schaal

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Berekeningscluster resources wijzigen:

1. Open de [portal van Azure][], open de database en klik op **schaal**.

    ![Klik op schaal][1]

1. In het blad schalen, verplaatst u de schuifregelaar naar links of rechts om de DWU-instelling te wijzigen.

    ![Schuifregelaar][2]

1. Klik op **Opslaan**. Er wordt een bevestigingsbericht weergegeven. Klik op **Ja** om te bevestigen of **geen** om te annuleren.

    ![Klik op Opslaan][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Een pauze invoegen berekenen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Een database onderbreken:

1. Open de [portal van Azure][] en open de database. Zoals u ziet dat de Status ervan **Online is**. 

    ![Onlinestatus][6]

1. Geheugen en rekenkracht resources uitstellen, klikt u op **Pauze**, waarna een bevestigingsbericht weergegeven. Klik op **Ja** om te bevestigen of **geen** om te annuleren.

    ![Onderbreken bevestigen][7]

1. Terwijl SQL Data Warehouse kan de database wordt gestart, is de status **onderbreken**.
2. Wanneer de status ervan **onderbroken is**, de bewerking onderbreken klaar is en u zijn niet meer in rekening worden gebracht DWUs.

    ![Status van een pauze invoegen][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Cv berekenen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Een database hervatten:

1. Open de [portal van Azure][] en open de database. Zoals u ziet dat de Status ervan **onderbroken is**. 

    ![Een pauze invoegen-database][4]

1. U wilt doorgaan met de database op **Start**en vervolgens wordt een bevestigingsbericht weergegeven. Klik op **Ja** om te bevestigen of **geen** om te annuleren.

    ![Cv bevestigen][5]

1. Terwijl SQL Data Warehouse kan de database wordt gestart, is de status "Resuming".
2. Wanneer de status ervan **online is**, is de database is klaar.

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen
Zie [overzicht van de Management][]voor meer informatie.

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Beheer van overzicht]: ./sql-data-warehouse-overview-manage.md
[Overzicht van de berekeningscluster beheren]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure-portal]: http://portal.azure.com/
