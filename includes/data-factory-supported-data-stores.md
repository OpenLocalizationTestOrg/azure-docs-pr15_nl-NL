Activiteit in Data Factory-kopieën opslaan van gegevens uit een brongegevens kopiëren naar een gegevensopslag sink. Gegevens Factory ondersteunt de volgende gegevens winkels. Gegevens uit elke bron kan naar een sink worden geschreven. Klik op een gegevensopslag om te leren hoe u gegevens kopieert en naar die winkel.

Categorie | Gegevensopslag | Ondersteund als een bron | Ondersteund als een sink
:------- | :--------- | :------------------ | :-----------------
Azure | [Azure-blobopslag](../articles/data-factory/data-factory-azure-blob-connector.md) <br/> [Azure Lake gegevensopslag](../articles/data-factory/data-factory-azure-datalake-connector.md) <br/> [Azure SQL-Database](../articles/data-factory/data-factory-azure-sql-connector.md) <br/> [Azure SQL datawarehouse](../articles/data-factory/data-factory-azure-sql-data-warehouse-connector.md) <br/> [Azure-tabelopslag](../articles/data-factory/data-factory-azure-table-connector.md) <br/> [Azure DocumentDB](../articles/data-factory/data-factory-azure-documentdb-connector.md) <br/> | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓
Databases | [SQL Server](../articles/data-factory/data-factory-sqlserver-connector.md)\* <br/> [Oracle](../articles/data-factory/data-factory-onprem-oracle-connector.md)\* <br/> [MySQL](../articles/data-factory/data-factory-onprem-mysql-connector.md)\* <br/> [DB2](../articles/data-factory/data-factory-onprem-db2-connector.md)\* <br/> [Teradata](../articles/data-factory/data-factory-onprem-teradata-connector.md)\* <br/> [PostgreSQL](../articles/data-factory/data-factory-onprem-postgresql-connector.md)\* <br/> [Sybase](../articles/data-factory/data-factory-onprem-sybase-connector.md)\* <br/>[Cassandra](../articles/data-factory/data-factory-onprem-cassandra-connector.md)\* <br/>[MongoDB](../articles/data-factory/data-factory-on-premises-mongodb-connector.md)\*<br/>[Amazon Redshift](../articles/data-factory/data-factory-amazon-redshift-connector.md) | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓<br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;<br/> &nbsp; <br/>&nbsp;
Bestand | [Bestandssysteem](../articles/data-factory/data-factory-onprem-file-system-connector.md)\* <br/> [HDFS](../articles/data-factory/data-factory-hdfs-connector.md)\* <br/> [Amazon S3](../articles/data-factory/data-factory-amazon-simple-storage-service-connector.md) <br/> [FTP](../articles/data-factory/data-factory-ftp-connector.md)| ✓ <br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> &nbsp;<br/>&nbsp;
Anderen | [SalesForce](../articles/data-factory/data-factory-salesforce-connector.md)<br/> [Algemene ODBC](../articles/data-factory/data-factory-odbc-connector.md)\* <br/> [Algemene OData](../articles/data-factory/data-factory-odata-connector.md) <br/> [Webtabel (tabel van HTML)](../articles/data-factory/data-factory-web-table-connector.md) <br/> [GE Historian](../articles/data-factory/data-factory-odbc-connector.md#ge-historian-store)* | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓  | &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;

> [AZURE.NOTE] Gegevens worden opgeslagen met * on-premises implementatie kan zijn of op Azure IaaS, en moet u [Data Management Gateway](../articles/data-factory/data-factory-data-management-gateway.md) installeren op een computer op-premises/Azure-IaaS.

