<properties
    pageTitle="Azure SQL Database JSON-functies | Microsoft Azure"
    description="Azure SQL-Database kunt u parseren, query en opmaak van gegevens in de notatie voor JavaScript-Object (JSON) notatie weer."
    services="sql-database"
    documentationCenter=""
    authors="jovanpop-msft"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/17/2016"
    ms.author="jovanpop"
   ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Aan de slag met JSON-functies in Azure SQL-Database

Azure SQL-Database kunt u parseren en uw relationele gegevens exporteren als JSON tekst query gegevens weergegeven in de notatie voor JavaScript-Object [(JSON)](http://www.json.org/) notatie.

JSON is een populaire gegevensindeling gebruikt voor de uitwisseling van gegevens in moderne web en mobiele toepassingen. JSON wordt ook gebruikt voor het opslaan van gedeeltelijk gestructureerde gegevens in de logboekbestanden of in NoSQL-databases zoals [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Veel REST-webservices resultaten zijn opgemaakt als tekst JSON of gegevens accepteren opgemaakt als JSON. Meest Azure services zoals [Azure zoeken](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/)en [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) hebben REST eindpunten die terug of JSON gebruiken.

Azure SQL-Database kunt u werken met JSON gegevens eenvoudig en uw database integreren met moderne services.

## <a name="overview"></a>Overzicht

Azure SQL-Database biedt de volgende functies voor het werken met gegevens van JSON:

![JSON-functies](./media/sql-database-json-features/image_1.png)

Als u JSON tekst hebt, kunt u gegevens ophalen uit JSON of controleert dat de JSON correct is opgemaakt met behulp van de ingebouwde functies [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx)en [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). De functie [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) kunt u de waarde in JSON tekst bijwerken. Voor meer geavanceerde query's uitvoeren en analyse, kunt [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) functie een matrix van JSON objecten transformeren in een aantal rijen. Een SQL-query kan worden uitgevoerd op de geretourneerde resultatenset. Tot slot er is een [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) -component waarmee u kunt gegevens die zijn opgeslagen in uw relationele tabellen als JSON tekst opmaken.

## <a name="formatting-relational-data-in-json-format"></a>Opmaak van relationele gegevens in de indeling van JSON
Als u een webservice die wordt gegevens uit de database lagen en vindt u een reactie in JSON opmaken, of aan de clientzijde JavaScript kaders of bibliotheken waarin de gegevens die zijn opgemaakt als JSON hebt, kunt u uw database-inhoud opmaken als JSON rechtstreeks in een SQL-query. U niet meer hoeft te schrijven van toepassingscode die de resultaten van Azure SQL-Database als JSON-indelingen of enkele bibliotheek JSON serialisatie om te converteren in tabelvorm queryresultaten en vervolgens serialiseren objecten op de indeling van JSON opnemen. In plaats daarvan kunt u de component FOR JSON wilt opmaken van de resultaten van de SQL-query als JSON in Azure SQL-Database en deze rechtstreeks in uw toepassing te gebruiken.

Rijen uit de tabel Sales.Customer zijn in het volgende voorbeeld wordt met behulp van de component FOR JSON opgemaakt als JSON:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

De resultaten van de query de component voor de JSON-pad opgemaakt als tekst JSON. Kolomnamen worden gebruikt als sleutels, terwijl de celwaarden worden gegenereerd als JSON waarden:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

De resultatenset is opgemaakt als een JSON-matrix waarbij elke rij is opgemaakt als een afzonderlijk JSON-object.

PAD geeft aan dat u de opmaak van het resultaat JSON met stip-notatie in de Kolomaliassen kunt aanpassen. De volgende query wordt de naam van de sleutel 'CustomerName' in de indeling van de JSON uitvoer, en telefoon-en fax plaatst in het submenu object 'Contactpersonen':

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

De uitvoer van deze query ziet er zo uit:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

In dit voorbeeld wordt één object JSON in plaats van een matrix geretourneerd door het opgeven van de optie [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) . U kunt deze optie als u weet dat u één object als gevolg van de query retourneert.

De belangrijkste waarde van de FOR JSON-component is dat deze optie kunt u complexe hiërarchische gegevens uit de database die is opgemaakt als geneste JSON-objecten of matrices geretourneerd. Het volgende voorbeeld wordt getoond hoe Orders die deel uitmaakt van de klant een geneste matrix van Orders opnemen:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

In plaats van verzenden scheiden van query's naar de klantgegevens ophalen en klik vervolgens om een lijst met verwante Orders ophalen, krijgt u de benodigde gegevens met een enkele query zoals wordt weergegeven in het volgende voorbeelduitvoer:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>Werken met JSON-gegevens

Als u geen strikt gestructureerde gegevens, hebt u complexe onderliggend objecten, matrices of hiërarchische gegevens of als uw gegevensstructuren ontwikkelen na verloop van tijd, kunt de indeling van JSON u om aan te geven van de structuur van een complexe gegevens.

JSON is een tekstuele indeling die kan worden gebruikt als een andere tekenreekstype in Azure SQL-Database. U kunt verzenden of JSON-gegevens opslaan als een standaard NVARCHAR:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

De JSON-gegevens die worden gebruikt in dit voorbeeld wordt weergegeven met behulp van het type NVARCHAR(MAX). JSON kan worden ingevoegd in deze tabel of die is opgegeven als een argument van de opgeslagen procedure standaard Transact-SQL-syntaxis gebruiken, zoals in het volgende voorbeeld:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Een aan de clientzijde taal of -bibliotheek die met tekenreeksgegevens in Azure SQL-Database werkt ook werken met JSON-gegevens. JSON kan worden opgeslagen in een tabel die ondersteuning biedt voor het type NVARCHAR, zoals een tabel geheugen geoptimaliseerde of systeem versienummer. JSON voorziet niet in een beperking in de code aan de clientzijde of in de databaselaag.

## <a name="querying-json-data"></a>Query's uitvoeren JSON-gegevens

Als u gegevens die zijn opgemaakt als JSON die zijn opgeslagen in SQL Azure-tabellen hebt, wordt er JSON-functies kunnen u deze gegevens gebruiken in een SQL-query.

JSON-functies die beschikbaar in SQL Azure-database kunt zijn u gegevens die zijn opgemaakt als JSON als een ander SQL-gegevenstype behandelen. U kunt eenvoudig waarden ophalen uit de JSON-tekst en JSON gegevens gebruiken in een query:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

De functie JSON_VALUE haalt een waarde uit de JSON-tekst die zijn opgeslagen in de kolom met gegevens. Deze functie wordt een pad JavaScript-achtige gebruikt om te verwijzen naar een waarde in de tekst JSON worden geëxtraheerd. De opgehaalde waarde kan worden gebruikt in een willekeurig deel van de SQL-query.

De functie JSON_QUERY is vergelijkbaar met JSON_VALUE. In tegenstelling tot JSON_VALUE haalt deze functie complexe onderliggend object zoals matrices of objecten die in JSON tekst worden geplaatst.

De functie JSON_MODIFY kunt u het pad van de waarde in de JSON-tekst die moet worden bijgewerkt, evenals een nieuwe waarde die het oude account overschreven opgeven. Deze manier kunt u eenvoudig JSON tekst bijwerken zonder de hele structuur reparsing.

Aangezien JSON is opgeslagen in een standaardtekst, zijn er niet garanderen dat de waarden die zijn opgeslagen in tekstkolommen correct zijn opgemaakt. U kunt controleren of tekst die zijn opgeslagen in de kolom JSON juist is ingedeeld met behulp van standaard Azure SQL Database-check-beperkingen en de functie ISJSON:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Als de ingevoerde tekst correct is opgemaakt JSON, de functie ISJSON retourneert de waarde 1. Controleer op elke invoegen of het bijwerken van JSON kolom, deze beperking wordt of nieuwe tekenreeks niet onjuiste JSON.

## <a name="transforming-json-into-tabular-format"></a>JSON omzetten in tabelvorm

Azure SQL-Database kunt u ook JSON siteverzamelingen in de indeling en laden of query JSON tabelgegevens transformeren.

OPENJSON is een tabel-waarde-functie dat de JSON tekst parseert, zoekt een matrix van JSON-objecten, de elementen van de matrix doorlopen en retourneert één rij in het resultaat van de uitvoer voor elk element van de matrix.

![JSON in tabelvorm](./media/sql-database-json-features/image_2.png)

In het voorbeeld hierboven, we waar u de JSON-matrix die moet worden geopend (in de $ kunt opgeven. Orders pad), welke kolommen moeten worden geretourneerd als resultaat en waar vind ik de JSON-waarden die wordt geretourneerd als cellen.

We een matrix JSON in kunt transformeren de @orders variabele in een reeks rijen, deze resultaatset analyseren of rijen in een standaardtabel invoegen:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
De verzameling orders die zijn opgemaakt als een matrix JSON en weergegeven als een parameter voor de opgeslagen procedure kan worden geparseerd en ingevoegd in de tabel Orders.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het integreren van JSON in uw toepassing, voert u deze resources:

- [TechNet-Blog](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
- [MSDN-documentatie](https://msdn.microsoft.com/library/dn921897.aspx)
- [Kanaal video 9](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Meer informatie over de verschillende scenario's voor het integreren van JSON in uw toepassing, raadpleegt u de demo in dit [kanaal 9 video's](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) of een scenario die overeenkomt met uw use-case in [JSON weblogberichten](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/)zoeken.
