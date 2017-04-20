<properties
   pageTitle="Transparante gegevenscodering (TDE) voor SQL Server-uitrekken Database op Azure inschakelen | Microsoft Azure"
   description="Transparante gegevenscodering (TDE) voor SQL Server-uitrekken Database op Azure inschakelen"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Transparante gegevensversleuteling (TDE) inschakelen voor uitrekken op Azure-Database
> [AZURE.SELECTOR]
- [Azure-portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Transparante gegevens versleuteling (TDE) helpt beschermen tegen schadelijke activiteit door realtime coderen en decoderen van de database, gekoppeld back-ups en transactie-logbestanden in rust zonder wijzigingen in de toepassing in te voeren.

TDE versleutelt de opslag van een volledige database met behulp van een symmetric sleutel database versleutelingssleutel genoemd. De databasesleutel is beveiligd met een ingebouwde servercertificaat. Het ingebouwde servercertificaat is uniek voor elke Azure server. Microsoft automatisch Hiermee draait u deze certificaten op minstens elke 90 dagen duurt. Zie voor een algemene beschrijving van TDE, [Transparante gegevens versleuteling (TDE)].

##<a name="enabling-encryption"></a>Codering inschakelen

Als u wilt TDE inschakelen voor een Azure database die de gegevens worden opgeslagen gemigreerd uit een uitrekken ingeschakelde SQL Server-database, het volgende doen:

1. Open de database in de [portal van Azure](https://portal.azure.com)
2. Klik in het blad database op de knop **Instellingen**
3. Selecteer de optie **transparante gegevensversleuteling**![][1]
4. Selecteer de instelling **op** en selecteer vervolgens **Opslaan**
![][2]


##<a name="disabling-encryption"></a>Codering uitschakelen

Om te schakelen TDE voor een Azure de database die de gegevens worden opgeslagen die zijn gemigreerd van een uitrekken ingeschakelde SQL Server-database, het volgende doen:

1. Open de database in de [portal van Azure](https://portal.azure.com)
2. Klik in het blad database op de knop **Instellingen**
3. Selecteer de optie **transparante gegevensversleuteling**
4. Selecteer de instelling **uitschakelen** en selecteer vervolgens **Opslaan**




<!--Anchors-->
[Transparante gegevensversleuteling (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
