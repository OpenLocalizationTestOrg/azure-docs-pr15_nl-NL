<properties
   pageTitle="Wat is Log Analytics? | Microsoft Azure"
   description="Log Analytics is een service in bewerkingen Management Suite Kantoorbeheersysteem waarmee u kunt verzamelen en analyseren operationele gegevens die zijn gegenereerd door alle resources in de cloud en on-premises omgeving.  In dit artikel biedt een beknopt overzicht van de verschillende onderdelen van Log analyses en koppelingen naar gedetailleerde inhoud."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="what-is-log-analytics"></a>Wat is Log Analytics?
Log Analytics is een service in [bewerkingen Management Suite \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) die helpt u bij het verzamelen en analyseren van gegevens die zijn gegenereerd door alle resources in de cloud en on-premises omgevingen. Hebt u realtime inzichten geïntegreerde zoeken en aangepaste dashboards gebruiken om te analyseren gemakkelijk miljoenen records in al uw werkbelasting en servers ongeacht hun fysieke locatie.


## <a name="log-analytics-components"></a>Meld u Analytics-onderdelen
Bij het beheercentrum van Log Analytics is de OMS opslagplaats waarin wordt gehost in de cloud Azure.  Gegevens worden verzameld in de bibliotheek uit verbonden bronnen door configureren gegevensbronnen en oplossingen toe te voegen aan uw abonnement.  Gegevensbronnen en oplossingen maakt elk verschillende recordtypen die u hebt een eigen set met eigenschappen, maar kunnen nog steeds worden geanalyseerd samen in query's naar de bibliotheek.  Hiermee kunt u dezelfde hulpmiddelen en methoden gebruiken om te werken met verschillende soorten gegevens die worden verzameld door verschillende bronnen.


![OMS opslagplaats](media/log-analytics-overview/overview.png)


Verbonden bronnen zijn de computers en andere bronnen die gegevens die worden verzameld door Log Analytics genereren.  Dit kunt agenten geïnstalleerd op [Windows](log-analytics-windows-agents.md) en [Linux](log-analytics-linux-agents.md) computers die rechtstreeks verbinding te maken of agenten opnemen in een [verbonden systeem Center Operations Manager management groep](log-analytics-om-agents.md).  Log Analytics kunt ook gegevens verzamelen van [Azure opslag](log-analytics-azure-storage.md).

[Gegevensbronnen](log-analytics-data-sources.md) zijn de verschillende soorten gegevens die zijn verzameld uit elke verbonden bron.  Dit geldt ook voor gebeurtenissen en [prestatiegegevens](log-analytics-data-sources-performance-counters.md) van [Windows](log-analytics-data-sources-windows-events.md) en Linux agenten naast bronnen zoals [IIS-logboeken](log-analytics-data-sources-iis-logs.md)en [Logboeken aan de aangepaste tekst](log-analytics-data-sources-custom-logs.md).  U configureren elke gegevensbron die u wilt verzamelen en de configuratie automatisch wordt bezorgd in elke verbonden bron.


## <a name="analyzing-log-analytics-data"></a>Log Analytics-gegevens analyseren
De meeste van uw interactie met Log Analytics is via de portal OMS dat wordt uitgevoerd in een browser en u met toegang tot configuratie-instellingen en meerdere hulpprogramma's als u wilt analyseren en reageren op verzamelde gegevens.  In de portal kunt u gebruikmaken van [zoekopdrachten in het logboek](log-analytics-log-searches.md) waarin het maken van query's kunt analyseren verzamelde gegevens, [dashboards](log-analytics-dashboards.md) die u kunt aanpassen met grafische weergaven van uw meest waardevolle zoekopdrachten en [oplossingen](log-analytics-add-solutions.md) die extra functionaliteit en hulpprogramma's voor gegevensanalyse bieden.

![OMS-portal](media/log-analytics-overview/portal.png)


Log Analytics biedt een querysyntaxis van de om snel te halen en gegevens in de bibliotheek samenvoegen.  U kunt maken en opslaan van [Log zoekopdrachten](log-analytics-log-searches.md) rechtstreeks om gegevens te analyseren in de portal OMS of hebt log zoekopdrachten automatisch wordt uitgevoerd als u wilt een waarschuwing maken als u de resultaten van de query geven een belangrijke voorwaarde.

![Log zoeken](media/log-analytics-overview/log-search.png)

Om een snelle grafische weergave van de status van uw algehele-omgeving, kunt u visualisaties voor opgeslagen logboek zoekacties toevoegen aan uw [dashboard](log-analytics-dashboards.md).   

![Dashboard](media/log-analytics-overview/dashboard.png)

Als u wilt analyseren van gegevens buiten Log analyses, kunt u de gegevens uit de bibliotheek OMS exporteren naar hulpprogramma's zoals [Power BI](log-analytics-powerbi.md) of Excel.  U kunt ook gebruikmaken van de [Zoek-API Log](log-analytics-log-search-api.md) aangepaste oplossingen bouwen die gebruikmaken van Log Analytics-gegevens te integreren met andere systemen.

## <a name="solutions"></a>Oplossingen
Functionaliteit toevoegen oplossingen aan Log Analytics.  Ze hoofdzakelijk uitvoeren in de cloud en analyseren van gegevens die worden verzameld in de bibliotheek OMS bieden. Ze kunnen ook nieuwe recordtypen te verzamelen die kunnen worden geanalyseerd met Log zoekopdrachten of een extra gebruikersinterface van de oplossing in het dashboard OMS definiëren.  

![Oplossing voor het bijhouden wijzigen](media/log-analytics-overview/change-tracking.png)


Oplossingen voor een groot aantal functies beschikbaar zijn en kunt u eenvoudig bladeren beschikbare oplossingen en [Voeg deze toe aan uw werkruimte OMS](log-analytics-add-solutions.md) vanuit de galerie met oplossingen.  Veel automatisch wordt geïmplementeerd en werk direct terwijl de anderen kunnen sommige configuratie vereist.

![Galerie met oplossingen](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Log Analysearchitectuur
De implementatievereisten van Log Analytics zijn minimale omdat de centrale onderdelen worden gehost in de cloud Azure.  Dit geldt ook voor de bibliotheek naast de services die u voor het analyseren van verzamelde gegevens toestaan.  De portal zijn toegankelijk vanaf elke gewenste browser zodat er geen vereiste voor clientsoftware.

U moet agenten installeren op computers met [Windows](log-analytics-windows-agents.md) en [Linux](log-analytics-linux-agents.md) , maar er is geen extra agent die is vereist voor computers die al lid van een [verbonden SCOM management groep zijn](log-analytics-om-agents.md).  SCOM agenten blijft communiceren met management-servers die hun gegevens naar Log Analytics doorstuurt.  Sommige oplossingen echter moeten worden agenten om rechtstreeks met Log Analytics te communiceren.  De documentatie van elke oplossing, wordt de vereisten van de communicatie opgeven.

Wanneer u [zich aanmeldt voor Log analyses](log-analytics-get-started.md), maakt u een werkruimte OMS.  U kunt de werkruimte zien als een unieke OMS-omgeving met een eigen gegevensopslagplaats, gegevensbronnen en oplossingen. U kunt meerdere werkruimten maken in uw abonnement ondersteuning voor meerdere omgevingen zoals productie en test.

![Log Analysearchitectuur](media/log-analytics-overview/architecture.png)


## <a name="next-steps"></a>Volgende stappen

- [Registreren voor een gratis Log Analytics-account](log-analytics-get-started.md) in uw eigen omgeving testen.
- De verschillende [Gegevensbronnen](log-analytics-data-sources.md) beschikbaar zijn voor het verzamelen van gegevens naar de bibliotheek OMS weergeven.
- [De beschikbare oplossingen in de galerie met oplossingen Blader](log-analytics-add-solutions.md) naar het functionaliteit toevoegen aan Log Analytics.
