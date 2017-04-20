<properties
   pageTitle="Beveiliging van gegevens van Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document wordt uitgelegd hoe gegevens wordt beheerd en gewaarborgd in Beveiligingscentrum Azure."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Beveiliging van Beveiligingscentrum Azure-gegevens
Vastlopen zodat klanten voorkomen, detecteren in en reageren op threats, Azure Beveiligingscentrum verzamelt en verwerkt gegevens over uw Azure resources, configuratiegegevens, metagegevens, gebeurtenislogboeken, inclusief bestanden en meer. We geven sterke verplichtingen de privacy en beveiliging van deze gegevens te beschermen. Microsoft volgt strikte richtlijnen voor naleving en beveiliging, uit codering aan een dienst. 

In dit artikel wordt uitgelegd hoe gegevens wordt beheerd en gewaarborgd in Beveiligingscentrum Azure.

## <a name="data-sources"></a>Gegevensbronnen

Azure Beveiligingscentrum analyseert gegevens uit de volgende bronnen:

- Azure Services: Leest informatie over het configureren van Azure services die u hebt geïmplementeerd door de provider van de resource van die service communiceert.
- Netwerkverkeer: Lees partijen verkeer de metagegevens van de infrastructuur van Microsoft, zoals bron/bestemming IP-/ poort, grootte-pakketten, netwerk- en -protocol van.
- Partneroplossingen: Verzamelt beveiligingsmeldingen van geïntegreerde partneroplossingen, zoals firewalls en antimalware oplossingen. Deze gegevens worden opgeslagen in Beveiligingscentrum Azure-opslag, op de huidige locatie in de Verenigde Staten.
- Uw virtuele Machines: Azure Beveiligingscentrum kunt verzamelen configuratiegegevens en informatie over beveiligingsgebeurtenissen, zoals Windows gebeurtenis en controle Logboeken, IIS logboeken syslog berichten en vastlopen bestanden vanaf uw virtuele machines met gegevens agenten. Zie het gedeelte "Gegevensverzameling beheren" hieronder voor meer informatie.  

Daarnaast wordt informatie over beveiligingsmeldingen, aanbevelingen en beveiliging systeemstatus status opgeslagen in Beveiligingscentrum Azure-opslag, op de huidige locatie in de Verenigde Staten. Deze informatie bevatten gerelateerde configuratiegegevens en beveiligingsgebeurtenissen verzameld uit uw virtuele machines zo nodig om u te geven met de beveiligingswaarschuwing, aanbeveling of beveiliging systeemstatus status.

## <a name="data-protection"></a>Gegevensbescherming

**Scheiding van gegevens**: gegevens worden bijgehouden logisch afzonderlijk op elk onderdeel overal in de service. Alle gegevens is gelabeld per organisatie. Deze labelen zich blijft voordoen gedurende de levenscyclus van de gegevens en deze op elke laag die u van de service wordt afgedwongen. Bovendien wordt gegevens die worden verzameld uit uw virtuele machines opgeslagen in uw opslagruimte (s).

**Toegang tot gegevens**: te bieden aanbevelingen voor beveiliging en mogelijke beveiligingsrisico's onderzoeken, personeel van Microsoft mogelijk gegevens weer te geven die zijn verzameld of geanalyseerd door Azure services, met inbegrip van bestanden vastlopen. Bestanden vastlopen en proces maken gebeurtenissen mogelijk per ongeluk omvatten klantgegevens of persoonlijke gegevens uit uw virtuele machines. We ons houden aan de [Microsoft Online Services-voorwaarden](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), welke staat dat Microsoft niet klantgegevens gebruiken of gegevens uit dit voor commerciële doeleinden reclame of soortgelijke worden afgeleid. We alleen gebruiken klantgegevens zo nodig kunt u opgeven met Azure services, met inbegrip van de toepassing die compatibel is met het leveren van deze services. U bewaren alle rechten tot klantgegevens.

**Gebruik van gegevens**: Microsoft gebruikt patronen en bedrijfsinformatie van bedreiging gezien over meerdere tenants ter verbetering van de mogelijkheden van onze voorkomen en detectie; dit doen we overeenkomstig de privacyafspraken in onze [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx)beschreven.

**Locatie van gegevens**: een opslag-account is opgegeven voor de regio waar virtuele machines worden uitgevoerd. Hiermee kunt u voor de opslag van gegevens in de regio hetzelfde als de virtuele machine waaruit de gegevens worden verzameld. Deze gegevens, inclusief bestanden vastlopen, persistent opgeslagen in uw account opslag. De service bevat ook informatie over beveiligingsmeldingen, inclusief meldingen van geïntegreerde partneroplossingen, aanbevelingen en beveiliging systeemstatus status in Beveiligingscentrum Azure-opslag, op de huidige locatie in de Verenigde Staten.

## <a name="managing-data-collection-from-virtual-machines"></a>Het beheren van gegevens verzamelen via virtuele machines

Als u besluit om het inschakelen van Azure Beveiligingscentrum, wordt de gegevensverzameling ingeschakeld voor elk van uw abonnementen. U kunt gegevens verzamelen in de sectie "Beveiligingsbeleid" van het Dashboard van het Azure beveiliging beheercentrum uitschakelen. Wanneer het verzamelen van gegevens is ingeschakeld, wordt Beveiligingscentrum Azure bepalingen de Azure Monitoring Agent op alle bestaande ondersteund virtuele machines en nieuwe bestanden die zijn gemaakt. De extensie Azure beveiliging Monitoring zoekt naar verschillende beveiligingsupdates configuraties en gebeurtenissen wordt getraceerd deze bij [Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Daarnaast wordt het besturingssysteem gebeurtenislogboek gebeurtenissen verhogen in de loop van de uitvoering van de computer. Voorbeelden van dergelijke gegevens zijn: besturingssysteemtype en versie, systeemlogboeken (gebeurtenislogboeken van Windows,) met processen, de computernaam van de, IP-adressen, besturingsomgeving aangemeld gebruiker en tenant-ID. De controle-Agent van Azure logboekvermeldingen leest en ETW wordt getraceerd en kopieert u deze aan uw account opslag voor analyse. 

Een account opslag is opgegeven voor de regio waarin er virtuele machines uitgevoerd, waar gegevens verzameld uit virtuele machines die dezelfde regio is opgeslagen. Hiermee kunt u gemakkelijk voor u om de gegevens in hetzelfde geografische gebied voor privacy en gegevens te garanderen doeleinden. U kunt opslagruimte accounts voor elk gebied configureren in de sectie "Beveiligingsbeleid" van het Dashboard van het Azure beveiliging-beheercentrum.

Bestanden vastlopen de Azure Monitoring Agent ook gekopieerd bij uw account opslag.  Azure Beveiligingscentrum kortstondige kopieën van uw bestanden vastlopen verzameld en geanalyseerd ze van misbruik pogingen en slaagt compromissen.  Azure Beveiligingscentrum voert deze analyse binnen hetzelfde geografische gebied, als de opslagruimte-account, en verwijdert u de kortstondige exemplaren wanneer analyse voltooid is.

Gegevens verzamelen van virtuele machines op elk gewenst moment, waarbij elke Monitoring agenten eerder geïnstalleerd door Azure Beveiligingscentrum wordt verwijderd, kunt u uitschakelen.


## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe gegevens wordt beheerd en gewaarborgd in Beveiligingscentrum Azure. Meer informatie over Azure Beveiligingscentrum, raadpleegt u:

- [Bewerkingen handleiding en Azure beveiliging Center plannen](security-center-planning-and-operations-guide.md) , informatie over het plannen en meer informatie over de ontwerpoverwegingen Azure Beveiligingscentrum vast te stellen.
- [Beveiliging servicestatus bewaken in Azure Beveiligingscentrum](security-center-monitoring.md) , informatie over het controleren van de status van uw Azure resources
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) , informatie over het beheren en reageren op beveiligingsmeldingen
- [Partneroplossingen met Azure Beveiligingscentrum Monitoring](security-center-partner-solutions.md) , informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over beheercentrum voor Azure-beveiliging](security-center-faq.md) : veelgestelde vragen over het gebruik van de service zoeken
- [Azure Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) , zoeken weblogberichten over Azure beveiliging en naleving
