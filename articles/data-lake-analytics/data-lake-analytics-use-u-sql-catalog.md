<properties
   pageTitle="Azure gegevens Lake Analytics U SQL-catalogus introduceren | Azure"
   description="Introduceren Azure gegevens Lake Analytics U SQL-catalogus"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>I-SQL-catalogus gebruiken

De catalogus met I-SQL wordt gebruikt voor het structureren van gegevens en code, zodat ze kunnen worden gedeeld door U-SQL-scripts. De catalogus kunt de mogelijkheden van gegevens in Azure gegevens Lake beste prestaties.

Elke Azure gegevens Lake Analytics-account heeft precies één I-SQL-catalogus gekoppeld. U kunt de I-SQL-catalogus niet verwijderen. Momenteel worden niet I-SQL catalogi gedeeld tussen Lake gegevensopslag accounts.

Elke I-SQL-catalogus bevat een database met de naam van **model**. De basispagina-Database kan niet worden verwijderd.  Elke I-SQL-catalogus kan meer extra databases bevatten.

I-SQL-database bevat:

- .NET-code tussen I-SQL-scripts, stroombaan – delen.
- Tabelwaarden functies – deel I-SQL-code tussen I-SQL-scripts.
- Tabellen-gegevens delen tussen I-SQL-scripts.
- Schema's - delen tabel schema's tussen I-SQL-scripts.

## <a name="manage-catalogs"></a>Catalogussen beheren
Elke Azure gegevens Lake Analytics-account heeft een standaardaccount Azure Lake gegevensopslag gekoppeld. Dit account voor gegevensopslag Lake wordt verwezen als het standaardaccount voor gegevensopslag Lake. I-SQL-catalogus wordt opgeslagen in het standaardaccount voor gegevensopslag Lake onder de map/catalogus-map. Verwijder alle bestanden in de map/catalogus-map.

### <a name="use-azure-portal"></a>Azure-portal gebruiken

Zie [gegevens Lake-analyses beheren met behulp van portal](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Gebruik Data Lake Tools voor Visual Studio.

U kunt Data Lake Tools voor Visual Studio gebruiken voor het beheren van de catalogus.  Zie voor meer informatie over de hulpmiddelen voor [Data Lake Tools voor Visual Studio gebruiken](data-lake-analytics-data-lake-tools-get-started.md).

**Voor het beheren van de catalogus**

1. Open Visual Studio en verbinding maken met azure. Zie [verbinding maken met Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure)voor de instructies.
1. Druk op **CTRL + ALT + S** **Server Explorer** openen.
2. Van **Server Explorer** **Azure**uitvouwen, **Gegevens Lake Analytics**uitvouwen uitvouwen van uw gegevens Lake Analytics-account, vouw **Databases**uit en vouwt u **basispagina**.



    - Als u wilt een nieuwe Database hebt toegevoegd, met de rechtermuisknop op de **Database**en klik vervolgens op **Database maken**.
    - Een nieuwe constructie toevoegen, met de rechtermuisknop op **stroombaan**, en klik vervolgens op **Registreren constructie**.
    - Als u wilt een nieuwe schema toevoegt, met de rechtermuisknop op **schema's maken**en klik op "maken Schema **.
    - Als u wilt een nieuwe tabel hebt toegevoegd, met de rechtermuisknop op **tabellen**en klik op "" maken tabel **.
    - Zie een nieuwe tabelwaardefunctie toevoegen, [I-SQL ontwikkelen door de gebruiker gedefinieerde operatoren voor gegevens Lake Analytics-taken](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![I-SQL Visual Studio catalogi bladeren](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Zie ook

- Aan de slag
    - [Aan de slag met gegevens Lake analyses met behulp van Azure portal](data-lake-analytics-get-started-portal.md)
    - [Aan de slag met gegevens Lake Analytics via Azure PowerShell](data-lake-analytics-get-started-powershell.md)
    - [Aan de slag met gegevens Lake analyses met behulp van Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
    - [I-SQL-scripts met Data Lake Tools voor Visual Studio ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md)
    - [Aan de slag met Azure gegevens Lake Analytics U SQL-taal](data-lake-analytics-u-sql-get-started.md)

- I-SQL en ontwikkeling
    - [Aan de slag met Azure gegevens Lake Analytics U SQL-taal](data-lake-analytics-u-sql-get-started.md)
    - [I-SQL-venster-functies gebruiken voor Azure gegevens Lake Analytics-taken](data-lake-analytics-use-window-functions.md)
    - [I-SQL door de gebruiker gedefinieerde operatoren voor taken van de gegevens Lake analyses ontwikkelen](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Projectmanagement
    - [Azure gegevens Lake analyses met behulp van Azure portal beheren](data-lake-analytics-manage-use-portal.md)
    - [Azure gegevens Lake Analytics via Azure PowerShell beheren](data-lake-analytics-manage-use-powershell.md)
    - [Controleren en problemen met Azure gegevens Lake Analytics-taken met behulp van Azure portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- End-to-end zelfstudie
    - [Azure gegevens Lake Analytics interactieve handleiding gebruiken](data-lake-analytics-use-interactive-tutorials.md)
    - [Logboeken aan de Website door middel van Azure gegevens Lake analyses analyseren](data-lake-analytics-analyze-weblogs.md)
