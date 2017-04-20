<properties
   pageTitle="Integratie met bewerkingen Management Suite (OMS) | Microsoft Azure"
   description="Naast de standaardfuncties van OMS, kunt u deze integreren met andere management-toepassingen en services te leveren van een hybride management omgeving, om aangepaste management scenario's die uniek zijn voor uw omgeving of op te geven van een aangepaste management functionaliteit voor uw klanten.  Dit artikel bevat een overzicht van verschillende opties voor integratie met OMS en koppelingen naar artikelen gedetailleerde technische informatie verstrekt."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Integratie met bewerkingen Management Suite (OMS)

Bewerkingen Management Suite is Microsofts cloudgebaseerde IT-oplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud infrastructuur.  Naast de standaardfuncties van OMS, kunt u deze integreren met andere management-toepassingen en services te leveren van een hybride management omgeving, om aangepaste management scenario's die uniek zijn voor uw omgeving of op te geven van een aangepaste management functionaliteit voor uw klanten.  Dit artikel bevat een overzicht van verschillende opties voor integratie met OMS services en koppelingen naar artikelen gedetailleerde technische informatie verstrekt. 



## <a name="log-analytics"></a>Log Analytics
Van beheergegevens die worden verzameld door Log Analytics wordt opgeslagen in een bibliotheek die wordt gehost in Azure wordt aangegeven.  Alle gegevens die zijn opgeslagen in de bibliotheek is beschikbaar in log zoekopdrachten die snelle analyse betrekking tot zeer grote hoeveelheden gegevens bieden.  Uw Integratievereisten voor mogelijk met het vullen van de bibliotheek met nieuwe gegevens beschikbaar maakt voor analyse en kunt ophalen van gegevens in de bibliotheek om een nieuwe visualisatie of integreren met een ander hulpmiddel voor het beheer.

Elk stukje gegevens in de bibliotheek is opgeslagen als een record.  Wanneer u de waarden voor de bibliotheek, moet u gebruikers opgeven met het recordtype dat uw oplossing gebruikt en een beschrijving van de eigenschappen.  Wanneer u gegevens ophaalt, moet u deze informatie over de gegevens waarmee u werkt.

![De bibliotheek OMS vullen](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>De bibliotheek Log Analytics vullen
Er zijn meerdere methoden voor het vullen van de bibliotheek OMS.  De methode die u gebruikt, is afhankelijk van de factoren zoals waarin de brongegevens zich bevindt, de opmaak van de gegevens en welke clients die u nodig hebt om te ondersteunen.  Nadat de gegevens worden opgeslagen in de bibliotheek, maakt het niet uit hoe deze zijn verzameld.

De volgende secties worden de verschillende opties voor de bibliotheek OMS vullen.

#### <a name="connected-sources-and-data-sources"></a>Verbonden bronnen en gegevensbronnen 
Verbonden bronnen zijn de locaties waar de gegevens voor de bibliotheek OMS kunnen worden opgehaald.  Gegevensbronnen en -oplossingen die zijn uitgevoerd op bronnen verbonden en definieer de specifieke gegevens die worden verzameld.  Als uw toepassing gegevens naar een van deze gegevensbronnen schrijft, kunt klikt u vervolgens u deze door het configureren van de gegevensbron.  Bijvoorbeeld als de toepassing Syslog gebeurtenissen maakt, kunnen klikt u vervolgens ze worden verzameld door de gegevensbron Syslog op een agent Linux.

- [Gegevensbronnen in Log Analytics](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Oplossingen

Oplossingen van de mogelijkheden van OMS.  Een oplossing kan gegevens verzamelen uit de bron verbonden of er mogelijk analyse op records al die worden verzameld in de bibliotheek worden uitgevoerd.  Elke oplossing geleverd door Microsoft heeft een afzonderlijke artikel die de details over de gegevens die worden verzameld.

- [Oplossingen in Log Analytics](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>HTTP-gegevens verzamelen API

De Log Analytics HTTP gegevens verzamelen API is een REST API waarmee u JSON gegevens toevoegen aan de bibliotheek Log Analytics.  U kunt gebruikmaken van deze API wanneer u een toepassing die gegevens via een van de andere gegevensbronnen of oplossingen niet zelf hebt.  Dit kan worden gebruikt om te vullen de bibliotheek van een client die de API kunt bellen, en niet afhankelijk is van de planning van de siteverzameling met een bepaalde gegevensbron of de oplossing.

- [Meld u aan analyses HTTP-gegevens verzamelen API](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Gegevens ophalen uit de Log Analytics-bibliotheek

Er zijn meerdere methoden voor het ophalen van gegevens uit de bibliotheek OMS.  U kunt gebruikers moeten het ophalen van gegevens met behulp van de OMS-console en geef de verschillende soorten visualisaties en analyse.  U kunt ook de gegevens ophalen uit een externe proces zoals beheeroplossing voor een andere.

#### <a name="log-searches"></a>Log zoekopdrachten

Alle gegevens die zijn opgeslagen in de bibliotheek OMS is beschikbaar via log zoekopdrachten.  Gebruikers kunnen hun eigen ad-hoc analyses uitvoeren in de OMS-console of een dashboard maken met een visualisatie voor een bepaalde log zoeken.  Oplossingen kunnen aangepaste weergaven bevatten met visualisaties op basis van vooraf gedefinieerde zoekopdrachten.  U kunt de zoeken-API Log access-gegevens in de bibliotheek OMS uit een externe toepassing of management hulpmiddel.  

- [Log zoekopdrachten in Log Analytics](../log-analytics/log-analytics-log-searches.md)
- [Log Analytics log zoeken REST API](../log-analytics/log-analytics-log-search-api.md)
- [Meld u Analytics-cmdlets](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Aangepaste weergaven 
De ontwerpfunctie voor weergaven kunt u aangepaste weergaven maken in de OMS-console die gebruikers visualisatie en analyse van de gegevens in uw oplossing bieden.  Elke weergave bevat een tegel die wordt weergegeven op de hoofdpagina van de console en een willekeurig aantal visualisatie-onderdelen die zijn gebaseerd op log zoekopdrachten die u definieert.
  
- [Ontwerpfunctie voor weergaven log-analyses](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>Power BI

Log Analytics kunt automatisch gegevens exporteren uit de bibliotheek OMS in Power BI zodat u van de visualisaties en hulpprogramma's voor gegevensanalyse gebruikmaken kunt.  Uitvoeren deze exportbewerking volgens een schema zodat de gegevens up-to-date wordt bewaard. 

- [Log Analytics-gegevens exporteren naar Power BI](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Automatisering

OMS kunt automatiseren processen te reageren op verzamelde gegevens of andere beheerfuncties uitvoeren.  Mogelijk gegevens verzamelen van uw toepassing en invoegen in de bibliotheek OMS of u de correctie van een bekend probleem in antwoord op de gegevens die zijn gevonden in de bibliotheek mogelijk automatiseren. 

![Automatisering](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks

Runbooks in Azure automatisering PowerShell-scripts en werkstromen worden uitgevoerd in de cloud Azure.  U kunt deze gebruiken voor het beheren van resources in Azure wordt aangegeven of andere bronnen die kunnen worden geopend vanuit de cloud.  Runbooks kan ook worden uitgevoerd in een lokale datacenter met hybride Runbook werknemer.  U kunt een runbook starten vanuit de portal van Azure of externe processen op een aantal manieren zoals PowerShell of de API automatisering.

- [Een runbook starten in Azure automatisering](../automation/automation-starting-a-runbook.md)
- [Azure automatisering-cmdlets](https://msdn.microsoft.com/library/dn690262.aspx)
- [Automatisering REST API](https://msdn.microsoft.com/library/mt662285.aspx)
- [Automatisering .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Waarschuwingen

Log zoekopdrachten op basis van een planning wordt automatisch uitgevoerd door waarschuwingsregels.  Als de resultaten voldoen aan bepaalde criteria kan worden de resulterende melding voor een runbook in Azure Automatisering gestart of bellen van een webhook waarin een externe kunt starten.  Beide van deze antwoorden kunnen details van de melding dat de gegevens als resultaat gegeven in het logboek zoeken op te nemen opnemen.

- [Waarschuwingen in Log Analytics](../log-analytics/log-analytics-alerts.md)
- [Waarschuwing log Analytics-API](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Back-up en herstellen van de Site

Services voor het beveiligen van uw bedrijfsgegevens en ervoor zorgen dat de beschikbaarheid van servers en toepassingen bieden, Azure back-up en sites worden hersteld.  U kunt gebruikmaken van deze services als u wilt uitvoeren van scenario's zoals back-services voor uw toepassing publiceren of een overname van een virtuele machine wordt gestart.

- [Azure back-up-cmdlets](https://msdn.microsoft.com/library/mt619253.aspx)
- [Azure Site herstel REST API](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Cmdlets voor het herstellen van Azure-Site](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Aangepaste oplossingen

U kunt de logica integratie verpakken in een aangepaste oplossing in uw werkruimte of in de werkruimte van een klant uit te voeren.  Uw oplossing kan elk van de integratie methoden bevatten in dit artikel naast andere bronnen op te geven van een scenario voor volledig beheer.  De resources in de oplossing worden geleverd, zodat wanneer de oplossing wordt verwijderd, alle bronnen die deze gemaakt zijn verwijderd uit het Kantoorbeheersysteem werkruimte en Azure-abonnement.

Uw oplossing kan bijvoorbeeld een runbook automatisering als u wilt verzamelen en verwerken van gegevens en vul de Log Analytics-bibliotheek met de HTTP-API voor het verzamelen van gegevens bevatten.  U kunt ook een aangepaste weergave die worden weergegeven en geanalyseerd de verzamelde gegevens opnemen.  

- Maken van aangepaste oplossingen (binnenkort)    

## <a name="next-steps"></a>Volgende stappen
- Verwijzen naar de [OMS SDK](operations-management-suite-sdk.md) voor technische informatie over het automatiseren van OMS services.  
