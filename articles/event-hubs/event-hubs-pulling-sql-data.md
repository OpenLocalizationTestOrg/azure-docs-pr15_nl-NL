<properties
   pageTitle="SQL-gegevens in Azure gebeurtenis Hubs trekken | Microsoft Azure"
   description="Overzicht van de gebeurtenis Hubs importeren uit SQL-voorbeeld"
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor=""/>

<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Bepaalde gegevens van SQL op een gebeurtenis-Hub van Azure

Een typische architectuur voor een toepassing voor het verwerken van realtimegegevens heeft betrekking op uploaden daarvan eerst naar een Hub Azure-gebeurtenis. Het is mogelijk een mogelijkheid IoT, zoals het verkeer op verschillende onderweg een weg, cmdlets voor controle of een scenario voor spellen monitoring van de acties van een Houd van frenzied contestants, of een enterprise-scenario, zoals building welke cmdlets voor controle. In dat geval de producenten gegevens zijn meestal externe objecten produceren tijdreeks gegevens die u moet verzamelen, analyse, opslaan en gereageerd en u mogelijk hebben geïnvesteerd een groot aantal hoeveelheid gebouw uit de infrastructuur voor de volgende processen in. Wat u doen als u gebruikmaken van gegevens uit een ander nummer, zoals een database in plaats van een bron wilt van gegevensstromen, en deze gebruiken in combinatie met uw andere streaming gegevens? Houd rekening met de hoofdletters/kleine letters waarin u gebruiken van Azure Stream analyses, externe gegevens Explorer (RDX) of enkele andere hulpmiddel wilt te analyseren en reageren op gegevens van Microsoft Dynamics AX traag wijzigen of een aangepaste faciliteiten personeel-systeem? U lost dit probleem, hebt we geschreven en openen-die afkomstig zijn een kleine cloud-steekproef die u kunt wijzigen en implementeren die de gegevens ophalen uit een SQL-tabel en druk op een Hub Azure gebeurtenis gebruiken als invoer in de volgende analytische toepassingen. En zich daarna realiseert dat dit is een scenario niet vaak voorkomen en het tegenovergestelde van wat u gewend bent met de Hub van een gebeurtenis. Wanneer u vind uzelf in de situatie waar dit is wat u moet doen, kunt u de code vinden in de voorbeelden van Azure galerie [hier](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/).  

Houd er rekening mee dat de code in dit voorbeeld alleen die een steekproef is. Het is **niet** bedoeld voor een productietoepassing, en geen is geprobeerd zodat u deze geschikt is voor gebruik in een dergelijke omgeving: is stricly een DIY, ontwikkelaars gerichte voorbeeld. U moet controleren allerhande beveiliging, prestaties en functionaliteit en factoren voordat op een end-to-end-architectuur kosten.

## <a name="application-structure"></a>Toepassingsstructuur

De toepassing is geschreven in C# en het bestand readme.md in de steekproef bevat alle gegevens die u wilt wijzigen, maken en publiceren van de toepassing. De volgende secties bevatten een hoog niveau overzicht van de werking van de toepassing.

We beginnen met ervan uitgegaan dat u toegang tot een SQL Azure-tabel hebt. U moet ook hebt ingesteld met een Azure gebeurtenis Hub en de verbinding weet tekenreeks nodig toegang toe.

Wanneer u de oplossing SqlToEventHub start, wordt een configuratiebestand (App.config) om een aantal dingen, zoals wordt beschreven in het bestand readme.md gelezen. Hierna ziet u een heel geen uitleg, zoals de naam van de gegevenstabel, enzovoort, en geen nodig is om te rehash de uitleg hier. 

Zodra de toepassing het configuratiebestand lezen heeft, het gaat in een lus, het lezen van de SQL-tabel en zet nieuwe records aan de gebeurtenis-hub, en vervolgens wachten op een interval naar bed gaan door gebruiker gedefinieerd voordat u deze helemaal opnieuw. Een paar zaken zijn handig om te weten:

1. De toepassing is gebaseerd op aangenomen dat de SQL-tabel wordt bijgewerkt door enkele externe proces en u wilt verzenden alle en alleen de updates op een Hub voor de gebeurtenis.
2. De SQL-tabel moet een veld met een unieke en toeneemt getal, bijvoorbeeld een record numer. Dit is net zo eenvoudig als een veld met de naam "-Id" of andere items die wordt verhoogd als datgene die database bijgewerkt records zoals "Creation_time" of "Sequence_number" toevoegt. De toepassing notities en slaat de waarde van dat veld in elke iteratie. In elke volgende pass through-missen, door de toepassing in feite de tabel query's voor alle records waar de waarde van dat veld groter is dan de waarde van deze via de lus voor het laatst hebt gezien. We belt deze laatste waarde de "verschuiving".
3. De toepassing maakt een tabel "TableOffsets" bij het starten van, voor de opslag van de verschuivingen. De tabel wordt gemaakt met de query "CreateOffsetTableQuery" gedefinieerd in het configuratiebestand. 
4. Er zijn verschillende query's gebruikt voor het werken met de verschuiving tabel gedefinieerd in het configuratiebestand als "OffsetQuery", "UpdateOffsetQuery" en "InsertOffsetQuery". U moet deze niet wijzigen.
5. De query "DataQuery" gedefinieerd in het configuratiebestand is ten slotte de query die wordt uitgevoerd om op te halen van de records van uw SQL-tabel. Dit is momenteel alleen aan de bovenkant 1000 records in elke keer via de lus voor optimalisatie doeleinden - als, bijvoorbeeld 25.000 records zijn toegevoegd aan de database sinds de laatste query, deze kan het enige tijd duren de query uit te voeren. Door te beperken query toevoegen aan 1000 records telkens wanneer, zijn de query's sneller. De bovenkant 1.000 eenvoudige selecteren opeenvolgende batches van 1000 records worden op de hub van de gebeurtenis.    

## <a name="next-steps"></a>Volgende stappen

Als u wilt de oplossing implementeert, klonen of downloaden van de toepassing SqlToEventHub, bewerk het bestand App.config bouwen en ten slotte publiceert. Wanneer u de toepassing hebt gepubliceerd, kunt u deze uitgevoerd in de portal van Azure klassieke onder Cloudservices zien en controleren van de gebeurtenissen die eraan de hub van de gebeurtenis. Houd er rekening mee dat de frequentie wordt bepaald door twee zaken: de frequentie van de updates van de SQL-tabel en de slaapstand interval dat u hebt opgegeven in het configuratiebestand voor de toepassing.