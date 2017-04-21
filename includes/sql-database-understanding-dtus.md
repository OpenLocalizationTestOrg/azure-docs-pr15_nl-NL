De Database transactie eenheid (DTU) is de maateenheid in SQL-Database die de relatieve kracht van databases op basis van een maateenheid echte vertegenwoordigt: de databasetransactie. Een reeks bewerkingen die vaak worden voor een online transactie verwerken (OLTP)-aanvraag en vervolgens gemeten hoeveel transacties kunnen worden voltooid per seconde onder volledig voorwaarden geladen werd (die is de korte versie, vindt u de categorieas details in het [overzicht van de vergelijkende](../articles/sql-database/sql-database-benchmark-overview.md)). 

Een database maken Premium P11 met 1750 DTUs bevat bijvoorbeeld 350 x meer DTU rekenkracht dan een eenvoudige database maken met 5 DTUs. 

![Inleiding tot SQL-Database: één database DTUs door laag en niveau.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Als u een bestaande SQL Server-database migreert, kunt u een derde partij hulpmiddel, [De Azure SQL Database DTU Rekenmachine](http://dtucalculator.azurewebsites.net/), gebruiken voor het ophalen van een schatting van het prestatieniveau van de en service-laag die uw database nodig bij het Azure SQL-Database hebben.

### <a name="dtu-vs-edtu"></a>DTU versus eDTU

De DTU voor één databases equivalent rechtstreeks naar de eDTU voor elastische databases. Een database in een groep met eenvoudige elastische database biedt bijvoorbeeld maximaal 5 eDTUs. Dit is dezelfde prestaties als een enkele eenvoudige database. Het verschil is dat de elastische database eventuele eDTUs uit de groep gebruiken Won't totdat deze aan. 

![Inleiding tot SQL-Database: elastische pools door laag.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Een eenvoudig voorbeeld helpt. Volg een resourcegroep die eenvoudige elastische database met 1000 DTUs te 800 databases weghalen erin. Zo lang maken als enige 200 van de 800 databases worden gebruikt op een willekeurige plaats in tijd (5 DTU X 200 = 1000), u de capaciteit van de groep won't raken en de databaseprestaties won't afnemen. In dit voorbeeld is voor de duidelijkheid vereenvoudigd. De reële wiskundige gaat minder snel. De portal kost de wiskundige voor u en zorgt ervoor dat een aanbeveling op basis van historische database gebruik. Zie [Aandachtspunten voor de prijs en prestaties voor een toepassingen elastische database](../articles/sql-database/sql-database-elastic-pool-guidance.md) voor meer informatie over de werking van de aanbevelingen of de wiskundige zelf doen. 
