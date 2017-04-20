<properties
   pageTitle="Overzicht van bewerkingen Management Suite Kantoorbeheersysteem | Microsoft Azure"
   description="Microsoft bewerkingen Management Suite Kantoorbeheersysteem is Microsofts cloudgebaseerde IT-oplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud infrastructuur.  In dit artikel worden de verschillende services van opgenomen in OMS geïdentificeerd en bevat koppelingen naar hun gedetailleerde inhoud."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Wat is bewerkingen Management Suite Kantoorbeheersysteem?

Microsoft bewerkingen Management Suite Kantoorbeheersysteem is Microsofts cloudgebaseerde IT-oplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud infrastructuur.  Aangezien OMS is geïmplementeerd als een cloudservice, kunt u deze hebt uitgevoerd en snel met minimale investering in infrastructuurservices.  Nieuwe functies worden automatisch, bezorgd zodat u doorlopend onderhoud en upgraden kosten.

Naast waardevolle services op een eigen biedt kunt OMS integreren met System Center onderdelen zoals System Center Operations Manager om uit te breiden uw investeringen in bestaande management in de cloud.  System Center en OMS kunnen samenwerken om een volledige hybride management ervaring beschikken.

De volgende secties bevatten een algemene beschrijving van de andere waarde gebieden van OMS en de services die ze implementeren.  U kunt OMS architectuur voor een overzicht van de verschillende onderdelen van de OMS verwijzen voordat u de gedetailleerde documentatie voor elk reviseren.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Inzicht en analyse](media/operations-management-suite-overview/icon-insight-analytics.png) Inzicht en analyse

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) helpt u verzamelen, relateren, zoeken en handelen logboek en prestaties gegevens die zijn gegenereerd door besturingssystemen en toepassingen. Hebt u realtime operationele inzichten geïntegreerde zoeken en aangepaste dashboards gebruiken om te analyseren gemakkelijk miljoenen records in al uw werkbelasting en servers ongeacht hun fysieke locatie.

U kunt eenvoudig oplossingen toevoegen aan Log-analyses waarmee gegevens worden verzameld en de logica voor de analyse.  Oplossingen mogelijk extra functionaliteit die automatisch wordt bezorgd in agenten met minimale of geen configuratie opnemen.  U kunt naast hulpprogramma's voor gegevensanalyse verstrekt door afzonderlijke oplossingen, aangepaste zoekopdrachten uitvoeren in de hele gegevensset als u wilt relateren van gegevens tussen systemen en toepassingen.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatisering & besturingselement](media/operations-management-suite-overview/icon-automation-control.png) Automatisering & besturingselement

Azure automatisering automatiseert administratieve processen met [runbooks](../automation/automation-runbook-types.md) die zijn gebaseerd op PowerShell en uitvoeren in de cloud Azure.  Runbooks hebben toegang tot een product of service die kan worden beheerd met PowerShell resources op te nemen in andere wolken zoals Amazon Web Services (AWS).  Runbooks kunnen ook worden uitgevoerd op een server in uw lokale Datacenter voor het beheren van lokale bronnen.

Azure automatisering biedt beheer van de configuratie met [PowerShell DSC](../automation/automation-dsc-overview.md).  U kunt maken en beheren van DSC resources die zijn ingesloten in een Azure en deze toepassen op de cloud en on-premises systemen definieert en automatisch afdwingen hun configuratie.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Beveiliging en herstellen](media/operations-management-suite-overview/icon-protection-recovery.png) Beveiliging en herstel

[Azure back-up](http://azure.microsoft.com/documentation/services/backup) uw toepassingsgegevens beschermen en het jaar zonder een hoofdletter investering en met minimale gebruikskosten behoudt.  Dit kan back-up maken van gegevens uit de fysieke en virtuele Windows-servers naast toepassing-werkbelastingen, zoals SQL Server en SharePoint.  Deze kan ook worden gebruikt door System Center Data Protection Manager (DPM) beveiligde gegevens repliceren naar Azure voor redundantie en opslag van de lange termijn.

[Azure Site herstel](http://azure.microsoft.com/documentation/services/site-recovery) draagt bij aan uw bedrijfscontinuïteit en een noodherstelplan (BCDR) door orchestrating herhaling, failover en herstel van on-premises implementatie Hyper-V virtuele machines, VMware virtuele machines en fysieke Windows/Linux-servers. U kunt machines repliceren naar een secundaire Datacenter of uw datacenter uitbreiden door ze worden gerepliceerd naar Azure. Herstel van de site bevat ook eenvoudige failover- en herstelbestanden voor werkbelasting. Dit wordt geïntegreerd met noodgevallen herstel regelingen zoals SQL Server AlwaysOn en herstel-abonnementen voor eenvoudig overname van werkbelasting die worden doorverbonden op meerdere computers bevat.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS beveiliging en naleving](media/operations-management-suite-overview/icon-security-compliance.png) Beveiliging en naleving
Beveiliging en naleving helpt u bij het opsporen, beoordelen en beperken beveiligingsrisico's voor de infrastructuur van uw.  Deze functies van OMS zijn geïmplementeerd door meerdere oplossingen in Log Analytics die analyseren logboekgegevens en configuratie van agent systems waarmee u ervoor zorgen dat de lopende beveiliging van uw omgeving.

- De [beveiliging en controle oplossing](oms-security-getting-started.md ) worden verzameld en geanalyseerd beveiligingsgebeurtenissen op beheerde systemen om verdachte activiteit identificeren.
- De [oplossing Antimalware](log-analytics-malware.md ) rapporten over de status van de beveiliging van antimalware op beheerde systemen.  
- De oplossing systeemupdates voert een analyse van de beveiligingsupdates en andere updates op uw beheerde systemen, zodat u gemakkelijk systemen herkennen vereisen van patches.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
- Meer informatie over [Azure automatisering](../automation/automation-intro.md).
- Meer informatie over [Azure back-up](http://azure.microsoft.com/documentation/services/backup).
- Meer informatie over het [herstellen van Azure-Site](http://azure.microsoft.com/documentation/services/site-recovery).
