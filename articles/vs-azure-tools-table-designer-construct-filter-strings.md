<properties
   pageTitle="Het bouwen van tekenreeksen voor de ontwerpfunctie voor tabellen | Microsoft Azure"
   description="Het bouwen van tekenreeksen voor de ontwerpfunctie voor tabellen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Het bouwen van tekenreeksen voor de ontwerpfunctie voor tabellen

## <a name="overview"></a>Overzicht

Als u wilt filteren van gegevens in een Azure tabel die wordt weergegeven in de Visual Studio- **Ontwerpfunctie voor tabellen**, een filtertekenreeks maken en voert u het naar het filterveld. De syntaxis van de tekenreeks filter is gedefinieerd door de WCF Data Services en is vergelijkbaar met een SQL WHERE-component, maar wordt verzonden naar de tabel-service via een HTTP-aanvraag. De **Ontwerpfunctie voor tabellen** omgaat met de juiste codering voor u, dus kunt u filteren op een gewenste waarde, hoeft u alleen de naam van eigenschap, vergelijkingsoperator criteriumwaarde en desgewenst Booleaanse waarde invoeren operator in het filterveld. U hoeft niet de optie $filter query opnemen, zoals u zou doen als het bouwen van een URL in de tabel via de [Opslagruimte Services REST API Reference](http://go.microsoft.com/fwlink/p/?LinkId=400447)query zijn.

Het WCF Data Services zijn gebaseerd op de [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Zie voor meer informatie over de optie filter systeem query (**$filter**), de [specificatie van de OData-URI conventies](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Vergelijkingsoperatoren

De volgende logische operatoren worden voor alle eigenschaptypen ondersteund:

|Logische operators|Beschrijving|Voorbeeld van een filtertekenreeks|
|---|---|---|
|EQ|Gelijk aan|Plaats eq 'Redmond'|
|BT|Groter dan|Prijs BT 20|
|ge|Groter dan of gelijk aan|Prijs ge 10|
|lt|Minder dan|Prijs lt 20|
|bestand|Kleiner dan of gelijk|Prijs-bestand 100|
|nieuwe|Niet gelijk aan|Plaats nieuwe "Amsterdam"|
|en|En|Prijs-bestand 200 en prijs BT 3.5|
|of|Of|Prijs-bestand 3.5 of prijs BT 200|
|niet|Niet|niet isAvailable|

Wanneer het bouwen van een filtertekenreeks, wordt de volgende regels belangrijk zijn:

- De logische operators gebruiken om te vergelijken van een eigenschap waarmee een waarde. Houd er rekening mee dat het is niet mogelijk om te vergelijken van een eigenschap waarmee een dynamische waarde; een-kant van de expressie moet een constante zijn.

- Alle onderdelen van de filtertekenreeks zijn hoofdlettergevoelig.

- Waarde van de constante moeten hetzelfde gegevenstype als de eigenschap in volgorde voor het filter om geldige resultaten te retourneren. Zie [informatie over het gegevensmodel van de tabel-Service](http://go.microsoft.com/fwlink/p/?LinkId=400448)voor meer informatie over ondersteunde eigenschaptypen.

## <a name="filtering-on-string-properties"></a>Filteren op tekenreekseigenschappen van een

Wanneer u op tekenreekseigenschappen van een filteren, moet u de tekenreeksconstante tussen enkele aanhalingstekens.

Het volgende voorbeeld filters voor de eigenschappen **PartitionKey** en **RowKey** ; aanvullende eigenschappen van niet-toets kunnen ook worden toegevoegd aan de filtertekenreeks:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

U kunt haakjes elke filterexpressie, hoewel het niet nodig is:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Houd er rekening mee dat de tabel-service biedt geen ondersteuning voor query's met jokertekens en ze worden niet ondersteund in de ontwerpfunctie voor tabellen op. U kunt echter voorvoegsel overeenkomende via vergelijkingsoperatoren op het gewenste voorvoegsel uitvoeren. Het volgende voorbeeld retourneert entiteiten met een eigenschap achternaam die begint met de letter 'A':

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filteren op numerieke eigenschappen

Als u wilt filteren op een geheel getal of Drijvendekommagetal, geef het getal zonder de aanhalingstekens.

In dit voorbeeld retourneert alle entiteiten met een leeftijd eigenschap waarvan de waarde groter dan 30 is:

    Age gt 30

In dit voorbeeld retourneert alle entiteiten met een achterstallig bedrag eigenschap waarvan de waarde kleiner dan of gelijk aan 100.25 is:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filteren op Booleaanse eigenschappen

Geef als u wilt filteren op een Booleaanse waarde **true** of **false** zonder de aanhalingstekens.

Het volgende voorbeeld retourneert alle entiteiten waarvan de eigenschap IsActive is ingesteld op **waar**:

    IsActive eq true

U kunt ook deze filterexpressie zonder de logische operator schrijven. In het volgende voorbeeld wordt retourneert de tabel-service ook alle entiteiten **waarvoor IsActive geldt**:

    IsActive

Om terug te keren alle entiteiten waar IsActive onwaar is, kunt u niet operator:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filteren op DateTime-eigenschappen

Als u wilt filteren op een DateTime-waarde, geef het **datetime** -sleutelwoord, gevolgd door de datum/tijd-constante tussen enkele aanhalingstekens. De datum/tijd-constante moet in gecombineerde UTC-indeling, zoals wordt beschreven in [DateTime-eigenschapswaarden opmaak](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Het volgende voorbeeld retourneert entiteiten waar de eigenschap CustomerSince is gelijk aan 10 juli 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
