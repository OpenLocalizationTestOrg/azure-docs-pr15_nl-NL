
<properties
    pageTitle="Vereisten voor de App voor Azure RemoteApp | Microsoft Azure"
    description="Meer informatie over de vereisten voor apps die u wilt gebruiken in Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="app-requirements"></a>Vereisten voor de App

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp ondersteunt streaming 32-bits of 64-bits Windows-toepassingen uit een afbeelding van Windows Server 2012 R2. De meeste bestaande 32-bits of 64-bits Windows-toepassingen uitvoeren 'als' in Azure RemoteApp-(Remote Desktop Services of voorheen bekend als Terminal Services) omgeving. Maar er is een verschil tussen uitgevoerd en goed werkt: sommige toepassingen correct, maar kunnen ook worden uitgevoerd en andere niet. De volgende informatie bevat richtlijnen voor het ontwikkelen van toepassingen in een omgeving met Remote Desktop Services en testen van compatibiliteit.

Tip: We werken aan het maken van enkele voorbeelden van het werken met apps voor u. Hier ziet u nieuwe onderwerpen die betrekking hebben met Microsoft Access, QuickBooks en App-V in RemoteApp.

## <a name="requirements"></a>Vereisten
Deze drie vereisten, helpen als opgevolgd, uw toepassing goed in RemoteApp uitvoeren:

1.  Toepassingen die voldoen aan alle [certificeringsvereisten voor Windows-bureaublad-apps](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) te houden aan [Remote Desktop Services programming richtlijnen](https://msdn.microsoft.com/library/aa383490.aspx) heeft voltooid compatibiliteit met RemoteApp.
2.  Toepassingen moeten nooit gegevens lokaal opslaan op de afbeelding of een RemoteApp-exemplaren die verloren gaan.  Nadat u een RemoteApp-siteverzameling hebt gemaakt, de exemplaren worden gekopieerd en stateless zijn en mogen alleen toepassingen. Opslag van gegevens in een externe bron of in het gebruikersprofiel.
3.  Aangepaste afbeeldingen moeten nooit bevatten gegevens die verloren gaan.  

## <a name="testing-your-apps"></a>Uw apps testen
Gebruik deze stappen om te testen van toepassingen:

1.  Installeer Windows Server 2012 R2 en uw toepassing
2.  Extern bureaublad inschakelen
3.  Maak twee gebruikersaccounts, gebruiker a en verleent, beide gebruikersaccounts toevoegen aan de beveiligingsgroep van extern bureaublad.
4.  Meerdere sessie compatibiliteit controleren door in te stellen van twee gelijktijdige RDS sessies naar de PC tijdens het starten van de toepassing.
5.  Valideer de app gedrag

## <a name="application-development-guidelines"></a>Richtlijnen voor het ontwikkelen van toepassing
Gebruik de volgende richtlijnen voor het ontwikkelen van toepassingen voor RemoteApp.

### <a name="multiple-users"></a>Meerdere gebruikers

- Installatie van een [toepassing voor één gebruiker ](https://msdn.microsoft.com/library/aa380661.aspx)kan problemen veroorzaken in een omgeving.
- Toepassingen moeten de [gebruiker gedefinieerde informatie opslaan](https://msdn.microsoft.com/library/aa383452.aspx) op locaties van de gebruiker gedefinieerde, afzonderlijk van globale informatie die van toepassing op alle gebruikers.
- RemoteApp gebruikmaakt van meerdere [naamruimten voor kernel objecten](https://msdn.microsoft.com/library/aa382954.aspx); een globale naamruimte wordt hoofdzakelijk door services in client/server-toepassingen gebruikt.
- Het is niet veilig wordt ervan uitgegaan dat de naam van de computer of het [IP-adres](https://msdn.microsoft.com/library/aa382942.aspx) toegewezen aan de computer zijn gekoppeld aan één gebruiker omdat meerdere gebruikers kunnen tegelijk zijn aangemeld met een extern bureaublad (sessiehost)-server.

### <a name="performance"></a>Prestaties
- [Grafische effecten](https://msdn.microsoft.com/library/aa380822.aspx) voordat u uw app aan RemoteApp toevoegen uitschakelen
- Als u wilt maximaliseren CPU beschikbaarheid voor alle gebruikers, schakel [achtergrondtaken](https://msdn.microsoft.com/library/aa380665.aspx) uit of efficiënt achtergrondtaken die niet systeembronnen zijn maken.
- U moet af te stellen en saldo vanaf [thread gebruik](https://msdn.microsoft.com/library/aa383520.aspx) van toepassingen voor een database voor meerdere gebruikers, met meerdere processors-omgeving.
- Optimale prestaties is het verstandig voor toepassingen om te [bepalen](https://msdn.microsoft.com/library/aa380798.aspx) of ze worden uitgevoerd in een clientsessie.
