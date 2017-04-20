<properties
    pageTitle="System Update Assessment-oplossing in Log Analytics | Microsoft Azure"
    description="U kunt de oplossing System-Updates in Log Analytics helpen u ontbrekende updates toepassen op servers in de infrastructuur van uw."
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
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="system-update-assessment-solution-in-log-analytics"></a>System Update Assessment-oplossing in Log Analytics

U kunt de oplossing systeemupdates in Log Analytics helpen u ontbrekende updates toepassen op servers in de infrastructuur van uw. Nadat u de oplossing hebt geïnstalleerd, kunt u de updates die in uw gecontroleerde servers ontbreken met behulp van de tegel **Systeem Update Assessment** op de pagina **Overzicht** in OMS weergeven.

Als ontbrekende updates worden gevonden, worden details worden weergegeven op het dashboard **Updates** . U kunt het dashboard **Updates** gebruiken om te werken met ontbrekende updates en ontwikkel een plan toepassen op de servers die ze nodig hebt.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

- De oplossing systeem Update Assessment toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie vereist.

## <a name="system-update-data-collection-details"></a>Systeem Update detailgegevens siteverzameling

Systeem Update Assessment verzamelt metagegevens en staat gegevens met behulp van de agenten die u hebt ingeschakeld.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor System Update Assessment.

| platform | Directe Agent | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Nee](./media/log-analytics-system-update/oms-bullet-red.png)|            ![Nee](./media/log-analytics-system-update/oms-bullet-red.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)| Ten minste 2 maal per dag en 15 minuten na de installatie van een update|

De volgende tabel ziet u voorbeelden van gegevenstypen die worden verzameld door systeem Update Assessment:

|**Gegevenstype**|**Velden**|
|---|---|
|Metagegevens|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, naam netwerk, IP-adres, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adres, NetbiosDomainName, LogicalProcessors, DNS-naam, weergavenaam, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|De staat|StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>Werken met updates

1. Klik op de tegel **Systeem Update Assessment** op de pagina **Overzicht** .  
    ![Systeem Update Assessment tegel](./media/log-analytics-system-update/sys-update-tile.png)
2. Klik op het dashboard **Updates** door de categorieën update te bekijken.  
    ![Updates dashboard](./media/log-analytics-system-update/sys-updates02.png)
3. Schuif naar rechts van de pagina om te bekijken van het blad **Windows kritiek/beveiligingsupdates** en klik onder **classificatie**, op **Beveiligingsupdates**.  
    ![Beveiligingsupdates](./media/log-analytics-system-update/sys-updates03.png)
4. Klik op de pagina Log zoeken wordt allerlei informatie weergegeven over de updates die niet aanwezig op servers zijn gevonden in de infrastructuur van uw. Klik op **lijst** om gedetailleerde informatie over de updates weer te geven.  
    ![Zoekresultaten - lijst](./media/log-analytics-system-update/sys-updates04.png)
5. Klik op de pagina Log zoeken wordt gedetailleerde informatie over elke update weergegeven. Klik op **weergave** als het bijbehorende artikel weergeven op de website van Microsoft Support naast het nummer KBID.  
    ![Meld u aan lijst met zoekresultaten de-weergave KB](./media/log-analytics-system-update/sys-updates05.png)
6. Uw webbrowser wordt geopend de webpagina van Microsoft ondersteuning voor de update in een nieuw tabblad. De informatie over de update ontbrekende weergeven.  
    ![Microsoft Support-webpagina](./media/log-analytics-system-update/sys-updates06.png)
7. Met het gebruik van de informatie die u hebt gevonden, kunt u een abonnement handmatig de ontbrekende update toepassen of u kunt doorgaan met de overige stappen om toe te passen automatisch de update volgen.
8. Als u de ontbrekende update automatisch toepassen wilt, gaat u terug naar het dashboard **Updates** en klikt u onder **Bijwerken wordt uitgevoerd**, **klikt u op het plannen van een update uitvoeren**.  
    ![Update wordt uitgevoerd - geplande tabblad](./media/log-analytics-system-update/sys-updates07.png)
9. Klik op **toevoegen** om te maken van een nieuwe update uitvoeren op de pagina **Bijwerken wordt uitgevoerd** op het tabblad **gepland** .  
    ![Gepland met de tab - toevoegen](./media/log-analytics-system-update/sys-updates08.png)
10. Klik op de pagina **Nieuwe bijwerken uitvoeren** , typ een naam voor de update uitvoeren, voeg afzonderlijke computers of computergroepen, definiëren van een planning en klik vervolgens op **Opslaan**.  
    ![Nieuwe Update uitvoeren](./media/log-analytics-system-update/sys-updates09.png)
11. Het tabblad **gepland** op de **Bijwerken wordt uitgevoerd** pagina ziet u de nieuwe update uitvoeren die u hebt gepland.  
    ![Geplande tabblad](./media/log-analytics-system-update/sys-updates10.png)
12. Wanneer de update uitvoeren wordt gestart, ziet u informatie op het tabblad **uitgevoerd** .  
    ![Actief tabblad](./media/log-analytics-system-update/sys-updates11.png)
13. Wanneer de update uitvoeren is voltooid, wordt het tabblad **voltooid** status.
14. Als er updates zijn toegepast van de update uitvoert, in het blad **Kritiek/beveiligingsupdates voor Windows Update** , ziet u dat het aantal updates wordt verminderd.  
    ![Windows-kritiek/beveiligingsupdates blade - update tellen beperkte](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) gedetailleerde systeem updategegevens moeten worden weergegeven.
