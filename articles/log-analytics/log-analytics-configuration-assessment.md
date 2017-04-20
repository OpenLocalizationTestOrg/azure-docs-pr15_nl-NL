<properties
    pageTitle="Configuratie Assessment oplossing in Log Analytics | Microsoft Azure"
    description="De configuratie Assessment-oplossing in Log Analytics biedt u gedetailleerde informatie over de huidige status van de infrastructuur van uw systeem Center Operations Manager-server wanneer u Operations Manager agenten of een groep voor het beheer van Operations Manager gebruikt."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="configuration-assessment-solution-in-log-analytics"></a>Configuratie Assessment oplossing in Log Analytics

De configuratie Assessment-oplossing in Log Analytics kunt u mogelijke problemen met de server configuration door middel van waarschuwingen en knowledge aanbevelingen vinden.

Deze oplossing vereist systeem Center Operations Manager. Configuratie beoordeling is niet beschikbaar als u alleen direct verbonden agenten gebruiken.

Bepaalde gegevens weergeven in een oplossing Assessment configuratie, is de Silverlight-invoegtoepassing voor uw browser vereist.

>[AZURE.NOTE] Vanaf 5 juli 2016, de oplossing configuratie Assessment niet meer kan worden toegevoegd aan Log Analytics werkruimten en deze oplossing wordt niet langer beschikbaar voor bestaande gebruikers na 1 augustus 2016. Voor klanten met behulp van deze oplossing voor SQL Server- of Active Directory, wordt u aangeraden dat u gebruiken in plaats daarvan de [SQL Server Assessment](log-analytics-sql-assessment.md), [Active Directory-Assessment](log-analytics-ad-assessment.md) en de [Status van de herhaling Active Directory](log-analytics-ad-replication-status.md) -oplossingen. Voor klanten die werken met configuratie bepalen voor Windows, Hyper-V en System Center VM Manager, raden u gebruikt het verzamelen van de gebeurtenis en wijzigingen bijhouden mogelijkheden om te krijgen een overzichtsweergave van eventuele problemen binnen uw omgevingen.

![Configuratie Assessment tegel](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Configuratiegegevens is verzameld uit gecontroleerde servers en vervolgens naar de OMS-service in de cloud voor verwerking worden verzonden. Logica wordt toegepast op de ontvangen gegevens en de gegevens voor de cloudservice-records. Verwerkte gegevens voor de servers voor wordt weergegeven voor de volgende stadia:

- **Waarschuwingen:** Ziet u de proactief, waarschuwingen die zijn gegenereerd voor uw gecontroleerde servers. Deze worden geproduceerd door regels geschreven door Microsoft Customer en ondersteuningspersoneel (CSS) met aanbevolen procedures uit het veld.
- **Knowledge aanbevelingen:** Ziet u de Microsoft Knowledge Base-artikelen die worden aanbevolen voor werkbelasting die zijn gevonden in uw infrastructuur; deze automatisch gesuggereerd op basis van uw configuratie via het gebruik van machine learning.
- **Servers en geanalyseerd werkbelasting:** Ziet u de servers en de werkbelasting die door OMS zijn worden gecontroleerd.

![Configuratie Assessment dashboard](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Technologieën die u kunt analyseren met configuratie bepalen

OMS configuratie Assessment analyseert de werklast van de volgende:

- Windows Server 2012 en Microsoft Hyper-V Server 2012
- Windows Server 2008 en Windows Server 2008 R2, waaronder:
    - Active Directory
    - Hyper-V host
    - Algemene besturingssysteem
- SQL Server 2008 en hoger
    - SQL Server-Database Engine
- Microsoft SharePoint 2010
- Microsoft Exchange Server 2010 en Microsoft Exchange Server 2013
- Microsoft Lync Server 2013 en Lync Server 2010
- Systeem Center 2012 SP1 – Virtual Machine Manager

Voor SQL Server, worden de volgende 32-bits en 64-bits versies voor analyse ondersteund:

- SQL Server 2016 - alle edities
- SQL Server 2014 - alle edities
- SQL Server 2008 en 2008 R2 - alle edities

De SQL Server-Database Engine geanalyseerd op alle ondersteunde edities. De 32-bits versie van SQL Server wordt ook ondersteund wanneer u zich in de WOW64-implementatie.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

- Operations Manager is vereist voor de configuratie Assessment-oplossing.
- U moet een agent Operations Manager op elke computer waarop u wilt beoordelen van de configuratie.
- De configuratie Assessment-oplossing toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie vereist.

## <a name="configuration-assessment-data-collection-details"></a>Configuratiegegevens Assessment gegevens verzamelen

Configuratie Assessment verzamelt configuratiegegevens, metagegevens en staat gegevens met de agenten die u hebt ingeschakeld.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor beoordeling van de configuratie.

| platform | Directe Agent | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Windows|![Nee](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Nee](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| twee keer per dag|

De volgende tabel ziet u voorbeelden van gegevenstypen die worden verzameld door de configuratie bepalen:

|**Gegevenstype**|**Velden**|
|---|---|
|Configuratie|Klant-id, AgentID, id van de entiteit, ManagedTypeID, ManagedTypePropertyID, HuidigeWaarde, ChangeDate|
|Metagegevens|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, naam netwerk, IP-adres, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adres, NetbiosDomainName, LogicalProcessors, DNS-naam, weergavenaam, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|De staat|StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Configuratie Assessment waarschuwingen
U kunt weergeven en waarschuwingen van de configuratie beoordeling met de pagina Waarschuwingen beheren. Meldingen geven aan het probleem die is gedetecteerd, de oorzaak en hoe het probleem. Ze bevatten ook informatie over configuratie-instellingen in uw omgeving die leiden prestatieproblemen tot kunnen.

![waarschuwingen weergeven](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] De configuratie Assessment waarschuwingen verschillen van de andere meldingen in Log Analytics. Waarschuwingen weergeven, is een Silverlight-invoegtoepassing voor uw browser vereist.

Wanneer u een item of de categorie van items op de pagina waarschuwingen selecteert, ziet u een lijst met servers of werkbelasting met waarschuwingen die van toepassing op elk item.

![Waarschuwing lijst](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Elke waarschuwing bevat een koppeling naar een artikel in het Microsoft Knowledge Base. Deze artikelen bevatten aanvullende informatie over de melding.

>[AZURE.TIP] Standaard is het maximum aantal waarschuwingen weergegeven 2.000. Meer als waarschuwingen wilt bekijken, klikt u op de melding balk boven aan de lijst van waarschuwingen.

U kunt klikken op een item in de lijst wilt bekijken van het KB-artikel waarmee u de oorzaak van het probleem dat het bericht heeft gegenereerd verhelpen.

![KB-artikel](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

U kunt waarschuwingsregels om te negeren voor specifieke regels of een soorten regels beheren.

![Waarschuwingsregels beheren](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Knowledge aanbevelingen
Wanneer u knowledge aanbevelingen weergeeft, bent u log vermelding Microsoft KB artikelen aanbevolen voor de werkbelasting en computers waarop aanvullende informatie over de melding voor een bieden lijst met zoekresultaten weergegeven.

![zoekresultaten voor knowledge aanbevelingen](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Servers en geanalyseerd werkbelasting
Wanneer u knowledge aanbevelingen weergeeft, bent u log lijst met zoekresultaten de vermelding van alle servers en werkbelasting waarvan OMS bekend uit Operations Manager gepresenteerd.

![Servers en werkbelasting](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Volgende stappen

- Gebruik [zoekopdrachten in het logboek in Log Analytics](log-analytics-log-searches.md) gedetailleerde configuratie assessment gegevens moeten worden weergegeven.
