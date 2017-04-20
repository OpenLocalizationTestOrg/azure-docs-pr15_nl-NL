<properties
    pageTitle="Bewaken en beheren van een elastische database-toepassingen met de portal van Azure | Microsoft Azure"
    description="Informatie over het gebruik van de Azure-portal en ingebouwde intelligence SQL-Database om te beheren, bewaken en breedte een resourcegroep die scalable elastische database databaseprestaties optimaliseren en beheren van kosten."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/22/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="monitor-and-manage-an-elastic-database-pool-with-the-azure-portal"></a>Controleren en een elastische database-toepassingen met de Azure-portal beheren

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


U kunt de Azure-portal gebruiken om te controleren en een elastische database-toepassingen en de databases in de groep beheren. U kunt het gebruik van een elastische toepassingen en de databases binnen die groep controleren in de portal. U kunt ook een reeks wijzigingen aanbrengen in uw elastische groep en alle wijzigingen tegelijk verzenden. Deze wijzigingen omvatten toevoegen of verwijderen databases, uw elastische groep-instellingen wijzigen, of uw database-instellingen wijzigen.

De onderstaande afbeelding ziet u een voorbeeld van elastische toepassingen. De weergave bevat:

*  Grafieken voor het Resourcegebruik van zowel de elastische toepassingen en de databases die zijn opgenomen in de groep controle.
*  De knop van de groep **configureren** om de elastische groep te wijzigen.
*  De **database maken** -knop die een nieuwe database gemaakt en toegevoegd aan de huidige elastische toepassingen.
*  Grote hoeveelheden databases beheren elastische taken waarmee u door Transact-SQL-scripts uit te voeren op alle databases in een lijst.

![Groep weergeven][2]

Als u wilt werken via de stappen in dit artikel, moet u een SQL server in Azure wordt aangegeven met ten minste één database en een elastische toepassingen. Als u een elastische van toepassingen niet hebt, raadpleegt u [een resourcegroep maken](sql-database-elastic-pool-create-portal.md); Als u een database niet hebt, raadpleegt u de [zelfstudie voor SQL-database](sql-database-get-started.md).

## <a name="elastic-pool-monitoring"></a>Elastische groep controle

U kunt gaan naar een bepaalde groep om te zien van het Resourcegebruik. De groep is standaard geconfigureerd voor opslagruimte en eDTU gebruik weergeven voor het laatste uur. De grafiek kan worden geconfigureerd om weer te geven op diverse manieren worden via verschillende tijdvensters.

1. Selecteer een groep voor gebruik met.
2. Onder **Elastische groep Monitoring** heet een grafiek **Resourcegebruik**. Klik op de grafiek.

    ![Elastische groep controle][3]

    Het blad **Metrisch** wordt geopend, met een gedetailleerd overzicht van de doelstellingen van de opgegeven via het opgegeven tijdvenster valt.   

    ![Metrische blade][9]

### <a name="to-customize-the-chart-display"></a>De grafiekweergave aanpassen

U kunt de grafiek en het metrische blad om weer te geven aan de andere doelstellingen zoals processor percentage, gegevens IO percentage en log IO percentage gebruikt bewerken.

2. Klik op het blad metrische, klikt u op **bewerken**.

    ![Klik op bewerken][6]

- In het blad **Grafiek bewerken** , selecteert u een nieuwe tijdsbereik (voorbij het uur, tegenwoordig kunnen of vorige week) of klik op **aangepast** als u wilt een datumbereik te selecteren in de laatste twee weken. Selecteer het grafiektype (staaf of lijn) en selecteer vervolgens de bronnen om te controleren.

> Opmerking: Alleen aan de doelstellingen met de dezelfde maateenheid kan worden weergegeven in de grafiek op hetzelfde moment. Bijvoorbeeld als u "eDTU percentage" selecteert is vervolgens alleen mogelijk andere aan de doelstellingen met percentage selecteren als de maateenheid.

    ![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Klik vervolgens op **OK**.


## <a name="elastic-database-monitoring"></a>Elastische database bewaken

Afzonderlijke databases kunnen ook worden bewaakt voor mogelijke problemen.

1. Klik onder **Elastische Database Monitoring**is er een grafiek met doelstellingen voor vijf databases. Standaard de grafiek worden weergegeven de bovenste 5-databases in de groep gemiddelde eDTU gebruik in de afgelopen uur. Klik op de grafiek.

    ![Elastische groep controle][4]

2. Het blad **Database Resourcegebruik** wordt weergegeven. Hier vindt u een gedetailleerd overzicht van het gebruik van de database in de groep. Het raster in het onderste gedeelte van het blad gebruikt, kunt u een database (s) selecteren in de groep om weer te geven van het gebruik ervan in de grafiek (maximaal 5 databases). U kunt ook het venster met het aan de doelstellingen en de tijd weergegeven in de grafiek door te klikken op **grafiek bewerken**.

    ![Database resource gebruik blade][8]

### <a name="to-customize-the-view"></a>De weergave aanpassen

1. Klik in het blad **Resourcegebruik Database** op de **grafiek bewerken**.

    ![Klik op de grafiek bewerken](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. In het blad grafiek **bewerken** , selecteert u een nieuwe tijdsbereik (voorbij uur of afgelopen 24 uur) of klik op **aangepast** om een andere dag in de afgelopen twee weken om weer te geven.

    ![Klik op aangepast](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klik op de vervolgkeuzelijst **databases door te vergelijken** om te selecteren van een andere meting gebruiken bij het vergelijken van databases.

    ![De grafiek bewerken](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Databases selecteren om te controleren

In de databaselijst in het blad **Database Resourcegebruik** , kunt u bepaalde databases vinden door ze opzoeken via de pagina's in de lijst of te typen in de naam van een database. Gebruik het selectievakje in om de database te selecteren.

![Zoeken naar databases om te controleren][7]


## <a name="add-an-alert-to-a-pool-resource"></a>Een waarschuwing toevoegen aan een resourceweergave van toepassingen

U kunt regels toevoegen aan de resources die e-mail naar personen of waarschuwing tekenreeksen naar URL eindpunten verzenden wanneer de resource raakt een gebruik drempel die u hebt ingesteld.

> [AZURE.IMPORTANT]Resourcegebruik monitoring voor elastische toepassingen heeft een vertraging van ten minste 20 minuten. Instellen van waarschuwingen van minder dan 30 minuten voor elastische toepassingen wordt momenteel niet ondersteund. Waarschuwingen instellen voor elastische toepassingen op een punt (parameter met de naam '-venstergrootte "in PowerShell-API) van minder dan 30 minuten niet geactiveerd. Zorg ervoor dat die u voor elastische toepassingen definieert waarschuwingen een bepaalde periode (venstergrootte) 30 minuten of langer gebruikt.

**Een waarschuwing toevoegen aan een resource:**

1. Klik op de grafiek **Resourcegebruik** het blad **Metrisch** openen, klikt u op **melding toevoegen**en vul vervolgens de gegevens in het blad **een waarschuwing regel toevoegen** (**Resource** wordt automatisch een instellen de toepassingen die u met werkt).
2. Typ een **naam** en **Beschrijving** ter aanduiding van de melding naar u en de geadresseerden.
3. Kies een **meetwaarde** die u wilt Waarschuw in de lijst.

    De grafiek wordt dynamisch Resourcegebruik voor die metrisch kunt u een drempelwaarde kiezen.

4. Kies een **voorwaarde** (groter dan, minder dan, enz.) en een **drempelwaarde**.
5. Klik op **OK**.



## <a name="move-a-database-into-an-elastic-pool"></a>Een database verplaatsen naar een elastische toepassingen

U kunt toevoegen of verwijderen van databases uit een bestaande groep. De database kunnen zijn in andere toepassingen. U kunt echter alleen databases die zich op dezelfde logische server toevoegen.

1. Klik in het blad voor de toepassingen, onder **elastische databases** op **configureren van toepassingen**.

    ![Klik op configureren van toepassingen][1]

2. Klik op **toevoegen aan groep**in het blad **configureren van toepassingen** .

    ![Klik op toevoegen aan groep](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. Selecteer de database of databases toevoegen aan de groep in het blad **toevoegen-databases** . Klik vervolgens op **selecteren**.

    ![Selecteer databases om toe te voegen](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Het **configureren van toepassingen** blad bevat nu de database die u hebt geselecteerd om te worden toegevoegd, klikt u met de status ervan **status in behandeling**.

    ![In behandeling groep toevoegingen](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. Klik in het **configureren van toepassingen blade**, klikt u op **Opslaan**.

    ![Klik op Opslaan](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Een database verplaatsen uit een elastische toepassingen

1. Selecteer de database of databases verwijderen in het blad **configureren van toepassingen** .

    ![databases vermelding](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klik op **verwijderen uit groep**.

    ![databases vermelding](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    Het **configureren van toepassingen** blad bevat nu de database die u hebt geselecteerd om te worden verwijderd met de status ervan **status in behandeling**.

    ![voorbeeld database toegevoegd of verwijderd](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. Klik in het **configureren van toepassingen blade**, klikt u op **Opslaan**.

    ![Klik op Opslaan](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-a-pool"></a>Instellingen van de prestaties van een groep wijzigen

Als u het Resourcegebruik van een resourcegroep controleren, is het mogelijk dat bepaalde aanpassingen nodig zijn. De groep moet wellicht een wijziging in de limieten prestaties of opslag. Mogelijk wilt u de database-instellingen in de groep wijzigen. U kunt de instellingen van de groep wijzigen op elk gewenst moment om de beste balans van prestaties en kosten. Zie [Wanneer moet een elastische database-toepassingen worden gebruikt?](sql-database-elastic-pool-guidance.md) voor meer informatie.

**De maximale eDTUs of opslagruimte per toepassingen en eDTUs per database wijzigen:**

1. Open het **configureren van toepassingen** blad.

    Klik onder **Instellingen voor elastische database-toepassingen**gebruiken beide schuifregelaar om de instellingen van de groep te wijzigen.

    ![Resourcegebruik elastische van toepassingen](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Wanneer de instelling verandert, ziet u de weergave de geschatte maandelijkse kosten van de wijziging.

    ![Een nieuwe maandelijkse kosten en een groep bijwerken](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## <a name="create-and-manage-elastic-jobs"></a>Maken en beheren van elastische taken

Elastische taken kunnen u Transact-SQL-scripts uitvoeren op een willekeurig aantal databases in de groep. Als u nieuwe taken maken, of bestaande projecten met behulp van de portal beheren.

![Maken en beheren van elastische taken][5]


Voordat u taken hebt gebruikt, elastische taken-onderdelen installeren en uw referenties invoeren. Zie [overzicht van elastische database](sql-database-elastic-jobs-overview.md)voor meer informatie.

Zie [schaal af met Azure SQL-Database](sql-database-elastic-scale-introduction.md): elastische Databasehulpmiddelen gebruiken als u wilt schalen, verplaats gegevens, query, of transacties maken.



## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database elastische toepassingen](sql-database-elastic-pool.md)
- [Een toepassingen elastische database maken met de portal](sql-database-elastic-pool-create-csharp.md)
- [Een toepassingen elastische database maken met PowerShell](sql-database-elastic-pool-create-powershell.md)
- [Een toepassingen elastische database maken met C#](sql-database-elastic-pool-create-csharp.md)
- [Aandachtspunten voor de prijs en prestaties voor elastische database van toepassingen](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
