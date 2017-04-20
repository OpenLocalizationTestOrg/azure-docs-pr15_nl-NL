<properties
   pageTitle="Het registreren van gegevensbronnen | Microsoft Azure"
   description="Hoe kan ik artikel markeren hoe u gegevensbronnen met de gegevenscatalogus Azure, zoals de metagegevensvelden voor uitgepakt tijdens de registratie hebt geregistreerd."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-register-data-sources"></a>Het registreren van gegevensbronnen

## <a name="introduction"></a>Inleiding
**Microsoft Azure-gegevenscatalogus** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van discovery voor enterprise-gegevensbronnen fungeert. **Azure-gegevenscatalogus** is met andere woorden, helpen bij het personen ontdekken, inzichtelijk maken en gegevensbronnen, en helpt organisaties gebruikt om meer waarde van de bestaande gegevens. En de eerste stap voor het afsluiten van een gegevensbron detecteerbaar via **Azure-gegevenscatalogus** is om te registreren die gegevensbron.
## <a name="registering-data-sources"></a>Gegevensbronnen registreren
Registratie is het proces van het ophalen van metagegevens uit de gegevensbron en die gegevens te kopiëren naar de **Gegevenscatalogus van Azure** -service. De gegevens blijft waar het momenteel zich bevindt en klik onder het besturingselement in de groep administrators en het beleid van het huidige systeem blijft.

Als u wilt een gegevensbron hebt geregistreerd, start u gewoon het hulpprogramma **Azure-gegevenscatalogus** gegevens source registratie van de **Gegevenscatalogus van Azure** -portal. Aanmelden met uw werk- of schoolaccount (de dezelfde Azure Active Directory-referenties dat u gebruikt voor aanmelding bij de portal) en selecteer vervolgens de gegevensbron die u wilt registreren.
Zie de zelfstudie [Aan de slag met Azure-gegevenscatalogus](data-catalog-get-started.md) meer vindt u meer details.

Nadat de gegevensbron is geregistreerd, wordt de catalogus wordt bijgehouden de locatie en de metagegevens, indexeert, zodat gebruikers kunnen zoeken, zoeken, en kennismaken met de gegevensbron en gebruikt u de locatie te brengen via de toepassing of het hulpmiddel van hun keuze.

## <a name="sources-supported"></a>Gegevensbronnen die worden ondersteund
Zie [Gegevenscatalogus DSA](data-catalog-dsr.md) voor de lijst met ondersteunde gegevensbronnen.
<br/>


## <a name="structural-metadata"></a>Structurele metagegevens
Wanneer u een gegevensbron registreert bent, wordt de registratie-functie wordt opgehaald door de informatie over de structuur van de objecten die u selecteert: dit is bedoeld als structurele metagegevens.

Voor alle objecten bevat deze structurele metagegevens van het object locatie, zodat gebruikers die de gegevens te ontdekken die gegevens verbinding maken met het object in de clienthulpprogramma's van hun keuze kunnen gebruiken. Andere structurele metagegevens bevat de objectnaam van het en type en kenmerk/kolomnaam en gegevens te typen.

## <a name="descriptive-metadata"></a>Beschrijvende metagegevens
Naast de core structurele metagegevens opgehaald uit de gegevensbron, wordt het hulpprogramma voor de registratie gegevens ook beschrijvende metagegevens ook uitgepakt. Voor SQL Server Analysis Services en SQL Server Reporting Services, is dit afkomstig van de eigenschappen van de beschrijving die worden aangeboden door deze services. Voor SQL Server, worden de waarden die met de uitgebreide eigenschap ms_description uitgepakt. Voor Oracle-Database, wordt het hulpprogramma voor de registratie gegevens de kolom Opmerkingen ophalen uit de weergave ALL_TAB_COMMENTS.

Naast de beschrijvende metagegevens opgehaald uit de gegevensbron, kunnen gebruikers ook beschrijvende metagegevens met de functie gegevensbron registratie invoeren. Gebruikers kunnen labels toevoegen en experts voor de objecten wordt geregistreerd kunnen identificeren. Alle deze beschrijvende metagegevens wordt gekopieerd naar de **Gegevenscatalogus van Azure** -service samen met de structurele metagegevens.

## <a name="including-previews"></a>Inclusief voorbeelden

Standaard alleen metagegevens worden geëxtraheerd uit gegevensbronnen en gekopieerd naar de **Gegevenscatalogus van Azure** -service, maar informatie over dat een gegevensbron wordt vaak eenvoudiger gemaakt door een voorbeeld van de gegevens zien die deze bevat.

Het hulpprogramma **Azure-gegevenscatalogus** gegevens source registratie kan gebruikers een momentopname voorbeeld van de gegevens opnemen in elke tabel en de weergave die is geregistreerd. Als de gebruiker kiest in om op te nemen voorbeelden tijdens de registratie, bevat het hulpmiddel registratie maximaal 20 records uit elke tabel en de weergave. Deze momentopname vervolgens gekopieerd naar de catalogus samen met de structurele en beschrijvende metagegevens.


> [AZURE.NOTE]  Breed tabellen met een groot aantal kolommen wellicht minder dan 20 records die zijn opgenomen in de Preview-versie.


## <a name="including-data-profiles"></a>Inclusief gegevens profielen

Net zoals inclusief voorbeelden waardevol context voor gebruikers zoeken naar gegevensbronnen in de **Catalogus van Azure-gegevens bieden kunnen**, kan met inbegrip van een profiel gegevens ook gemakkelijker te begrijpen ontdekt gegevensbronnen.

Het hulpprogramma **Azure-gegevenscatalogus** gegevens source registratie kan gebruikers om het opnemen van een profiel gegevens voor elke tabel en de weergave die is geregistreerd. Als de gebruiker wil opnemen van een profiel gegevens tijdens de registratie, het hulpmiddel registratie statistische gegevens over de gegevens worden opgenomen in elke tabel en de weergave, waaronder:

* Het aantal rijen en grootte van de gegevens in het object
* De datum voor de meest recente update van de gegevens en het schema object
* Het aantal null records en unieke waarden voor kolommen
* Het minimum, maximum, gemiddelde en standaarddeviatie waarden voor kolommen

Deze statistieken worden vervolgens in de catalogus samen met de metagegevens structurele en beschrijvende gekopieerd.

> [AZURE.NOTE]  Tekst en datum kolommen zal geen gemiddelde of standaarddeviatie statistieken in haar profiel gegevens bevatten.

## <a name="updating-registrations"></a>Bijwerken van registraties

Registreren van een gegevensbron, kunt u deze gevonden in de **Catalogus van Azure-gegevens** met de metagegevens en optionele preview uitgepakt tijdens de registratie. Als de gegevensbron worden bijgewerkt in de catalogus moet (bijvoorbeeld als het schema van een object is gewijzigd, of tabellen die oorspronkelijk zijn uitgesloten opgenomen worden moeten of een gebruiker wil bijwerken van de gegevens die zijn opgenomen in de voorbeelden) kan registratiehulpprogramma voor de gegevens opnieuw worden uitgevoerd.

Een samenvoegen "upsert" opnieuw registreren van een al geregistreerde gegevensbron uitvoert: bestaande objecten wordt bijgewerkt, terwijl de nieuwe objecten wordt gemaakt. Alle metagegevens die door gebruikers via de portal van de **Catalogus van Azure-gegevens** blijven behouden.

## <a name="summary"></a>Overzicht
Een gegevensbron met **De gegevenscatalogus Azure** registreren vergemakkelijkt die gegevensbron om te detecteren en begrijpen, door structurele en beschrijvende metagegevens van de gegevensbron te kopiëren naar de catalogus-service. Als een gegevensbron is geregistreerd, deze kunt vervolgens aantekeningen worden voorzien, beheerde en ontdekt met behulp van de portal **Azure-gegevenscatalogus** .

## <a name="see-also"></a>Zie ook
- [Aan de slag met Azure-gegevenscatalogus](data-catalog-get-started.md) zelfstudie voor meer informatie over het registreren van gegevensbronnen.
