<properties
    pageTitle="Uitrekken Database uitschakelen en weer terughalen externe gegevens | Microsoft Azure"
    description="Leer hoe u uitrekken Database uitschakelen voor een tabel en desgewenst weer terughalen externe gegevens."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Uitrekken Database uitschakelen en weer terughalen externe gegevens

Als u wilt uitschakelen uitrekken Database voor een tabel, selecteert u **Uitrekken** voor een tabel in SQL Server Management Studio. Selecteer een van de volgende opties.

-   **Uitschakelen | Gegevens terughalen uit Azure**. Uitrekken Database kopiëren de externe gegevens voor de tabel van Azure terug naar SQL Server, klikt u vervolgens uitschakelen voor de tabel. Deze bewerking gegevens doorverbinden kosten bijhoudt en deze niet worden geannuleerd.

-   **Uitschakelen | Gegevens voor onbepaalde tijd in Azure**. Uitrekken Database uitschakelen voor de tabel.  De externe gegevens voor de tabel in Azure verlaten.

U kunt ook Transact\-SQL-Database uitrekken uitschakelen voor een tabel of voor een database.

Nadat u de Database uitrekken uitgeschakeld voor een tabel, bevatten gegevens migratie stopt en queryresultaten geen meer resultaten van de tabel met externe.

Als u gewoon onderbreken gegevensmigratie wilt, raadpleegt u [onderbreken en hervatten uitrekken Database](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Uitrekken Database uitschakelen voor een tabel of voor een database, worden het externe object niet verwijderd. Als u verwijderen van de externe tabel of de externe database wilt, hebt u zet deze neer met behulp van de Azure beheerportal. De externe objecten blijven Azure kosten totdat u deze verwijderen. Zie [Prijzen van SQL Server uitrekken Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer informatie.

## <a name="disable-stretch-database-for-a-table"></a>Uitrekken Database uitschakelen voor een tabel

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Gebruik van SQL Server Management Studio uitrekken Database uitschakelen voor een tabel

1.  Selecteer de tabel waarvan u wilt uitschakelen uitrekken Database in SQL Server Management Studio in Object Explorer.

2.  Rechts\-klikt u op en selecteer **Uitrekken**en selecteer vervolgens een van de volgende opties.

    -   **Uitschakelen | Gegevens terughalen uit Azure**. Uitrekken Database kopiëren de externe gegevens voor de tabel van Azure terug naar SQL Server, klikt u vervolgens uitschakelen voor de tabel. Deze opdracht kan niet worden geannuleerd.

        >   [AZURE.NOTE] Kopiëren van de externe gegevens voor de tabel van Azure maken naar SQL Server data doorverbinden kosten bijhoudt. Zie [Prijzen detailgegevens overdrachten](https://azure.microsoft.com/pricing/details/data-transfers/)voor meer informatie.

        Nadat alle de externe gegevens zijn gekopieerd van Azure terug naar SQL Server, uitrekken uitgeschakeld voor de tabel.

    -   **Uitschakelen | Gegevens voor onbepaalde tijd in Azure**. Uitrekken Database uitschakelen voor de tabel.  De externe gegevens voor de tabel in Azure verlaten.

    >   [AZURE.NOTE] Uitrekken Database uitschakelen voor een tabel, worden de externe gegevens of de tabel met externe niet verwijderen. Als u verwijderen van de tabel met externe wilt, hebt u zet deze neer met behulp van de Azure beheerportal. De tabel met externe blijft Azure kosten totdat u deze verwijderen. Zie [Prijzen van SQL Server uitrekken Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer informatie.

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Gebruik Transact\-SQL-Database uitrekken uitschakelen voor een tabel

-   Voer de volgende opdracht om te schakelen uitrekken voor een tabel en de externe gegevens voor de tabel van Azure terug naar SQL Server kopiëren. Nadat alle de externe gegevens zijn gekopieerd van Azure terug naar SQL Server, worden uitrekken is uitgeschakeld voor de tabel.

    Deze opdracht kan niet worden geannuleerd.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] Kopiëren van de externe gegevens voor de tabel van Azure maken naar SQL Server data doorverbinden kosten bijhoudt. Zie [Prijzen detailgegevens overdrachten](https://azure.microsoft.com/pricing/details/data-transfers/)voor meer informatie.

-   Als u wilt uitschakelen, uitrekken voor een tabel en de externe gegevens verlaten, kunt u de volgende opdracht uitvoeren.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Uitrekken Database uitschakelen voor een tabel, worden de externe gegevens of de tabel met externe niet verwijderen. Als u verwijderen van de tabel met externe wilt, hebt u zet deze neer met behulp van de Azure beheerportal. De tabel met externe blijft Azure kosten totdat u deze verwijderen. Zie [Prijzen van SQL Server uitrekken Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer informatie.

## <a name="disable-stretch-database-for-a-database"></a>Uitrekken Database uitschakelen voor een database
Voordat u uitrekken Database voor een database uitschakelen kunt, hebt u voor het uitschakelen van uitrekken Database op de afzonderlijke uitrekken\-ingeschakelde tabellen in de database.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Gebruik van SQL Server Management Studio uitrekken Database uitschakelen voor een database

1.  Selecteer de database waarvan u wilt uitschakelen uitrekken Database in SQL Server Management Studio in Object Explorer.

2.  Rechts\-klikt u op en selecteert u die **taken**, en selecteer vervolgens **Uitrekken**, en selecteer vervolgens **uitschakelen**.

>   [AZURE.NOTE] Uitrekken Database uitschakelen voor een database, worden de externe database niet verwijderd. Als u verwijderen van de externe database wilt, hebt u zet deze neer met behulp van de Azure beheerportal. De externe database blijft Azure kosten totdat u deze verwijderen. Zie [Prijzen van SQL Server uitrekken Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer informatie.

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Gebruik Transact\-SQL-Database uitrekken uitschakelen voor een database
Voer de volgende opdracht.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Uitrekken Database uitschakelen voor een database, worden de externe database niet verwijderd. Als u verwijderen van de externe database wilt, hebt u zet deze neer met behulp van de Azure beheerportal. De externe database blijft Azure kosten totdat u deze verwijderen. Zie [Prijzen van SQL Server uitrekken Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)voor meer informatie.

## <a name="see-also"></a>Zie ook

[Opties voor instellen van ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[Onderbreken en hervatten uitrekken Database](sql-server-stretch-database-pause.md)
