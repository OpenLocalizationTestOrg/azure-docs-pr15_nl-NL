
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>De verbindingsreeks vergelijken


De volgende tabel ziet de verbindingstekenreeksen dat u uw C#-programma wilt verbinden met uw lokale SQL Server versus uw Azure SQL-Database in de cloud. De verschillen zijn vet.


| Verbindingsreeks voor<br/>Azure SQL-Database | Verbindingsreeks voor<br/>Microsoft SQL Server |
| :-- | :-- |
| Server =**tcp:**{your_serverName_here}**. database.windows.net,1433**;<br/>Gebruikers-ID = {your_loginName_here}**@{your_serverName_here}**;<br/>Wachtwoord = {your_password_here};<br/>**Database = {your_databaseName_here};**<br/>**Time-out = 30**;<br/>**Versleutelen = True**;<br/>**TrustServerCertificate = False**; | Server = {your_serverName_here};<br/>Gebruikers-ID = {your_loginName_here};<br/>Wachtwoord = {your_password_here}; |


De **Database =** is optioneel voor SQL Server, maar is vereist voor SQL-Database.


[.NET ADO SqlConnectionStringBuilder eigenschappen](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) - worden alle parameters in detail beschreven.


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
