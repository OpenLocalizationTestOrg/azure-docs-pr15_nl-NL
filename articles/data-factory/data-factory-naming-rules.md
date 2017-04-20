<properties 
    pageTitle="Gegevens Factory - naamgeving | Microsoft Azure" 
    description="Beschrijft naming regels voor gegevens Factory entiteiten." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure gegevens Factory - naamgeving 
De volgende tabel vindt naming regels voor Data Factory-onderdelen.



Naam | Naam uniekheid | Controles voor gegevensvalidatie
:--- | :-------------- | :----------------
Gegevens Factory | Uniek zijn voor Microsoft Azure. Namen zijn niet hoofdlettergevoelig, dat wil zeggen MyDF en mydf verwijzen naar dezelfde gegevens fabriek. |<ul><li>Elke factory gegevens is gekoppeld aan één Azure-abonnement.</li><li>Objectnamen moeten beginnen met een letter of een getal en kunnen alleen letters, cijfers en het streepje (-) bevatten.</li><li>Elke streepje (–) moet direct worden voorafgegaan en gevolgd door een letter of een getal. Opeenvolgende streepjes zijn niet toegestaan in de container namen.</li><li>Naam kan 3-63 tekens lang zijn.</li></ul>
Gekoppelde tabellen/Services/pijpleidingen | Unieke met in een fabriek gegevens. Namen zijn niet hoofdlettergevoelig. | <ul><li>Maximum aantal tekens in naam van een tabel: 260.</li><li>Objectnamen moeten beginnen met een letter getal of een onderstrepingsteken (_).</li><li>Volgende tekens zijn niet toegestaan: ". ', 'en',"?","/"," < "," > "," * ","%","&",": ","\\"</li></ul>
Resourcegroep | Uniek zijn voor Microsoft Azure. Namen zijn niet hoofdlettergevoelig. | <ul><li>Maximum aantal tekens: 1000.</li><li>Naam kan letters, cijfers en de volgende tekens bevatten: "-", "_",","en".".</li></ul>