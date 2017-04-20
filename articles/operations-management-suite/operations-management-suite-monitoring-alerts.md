<properties 
   pageTitle="Waarschuw management in Microsoft-producten monitoring | Microsoft Azure"
   description="Een waarschuwing geeft aan dat sommige probleem dat de aandacht van een beheerder vereist.  In dit artikel worden de verschillen tussen de hoe waarschuwingen gemaakt en beheerd in System Center Operations Manager (SCOM) en Log Analytics beschreven en aanbevolen procedures in gebruikmaken van de twee producten voor een waarschuwing strategie hybride." 
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
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="managing-alerts-with-microsoft-monitoring"></a>Waarschuwingen met Microsoft monitoring beheren 

Een waarschuwing geeft aan dat sommige probleem dat de aandacht van een beheerder vereist.  Zijn er onderling nogal verschillen tussen systeem Center Operations Manager (SCOM) en analyses Log in bewerkingen Management Suite Kantoorbeheersysteem wat betreft hoe waarschuwingen worden gemaakt, hoe ze worden beheerd en geanalyseerd, en hoe u wordt geïnformeerd dat er een probleem met de kritieke is ontdekt.

## <a name="alerts-in-operations-manager"></a>Waarschuwingen in Operations Manager
Waarschuwingen in SCOM worden gegenereerd door afzonderlijke regels of beeldschermen om aan te geven van een specifiek probleem.  Een monitor kunt een waarschuwing wordt gegenereerd als deze in een foutstatus terwijl een regel mogelijk een melding in om aan te geven sommige kritieke-probleem dat is niet rechtstreeks zijn gerelateerd aan de status van een beheerde object genereren.  Management packs zijn tal van werkstromen die meldingen instellen voor de toepassing of service die ze beheren.  Deel van het proces van het configureren van een nieuw management pack is om te zorgen dat ontvangt u geen overtollige waarschuwingen voor problemen die u niet rekening met kritieke houden afstemmen.

![SCOM waarschuwing weergeven](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

Volledig beheer van waarschuwingen biedt SCOM waarschuwingen met een status die kan worden gewijzigd door beheerders terwijl ze werkt om het probleem te verhelpen.  Als het probleem is opgelost, de beheerder Hiermee stelt u de melding naar gesloten die deze niet meer wordt weergegeven in weergaven actieve meldingen weergeven.  Waarschuwingen die zijn gegenereerd op basis van beeldschermen kunnen automatisch worden opgelost wanneer het beeldscherm keert terug naar orde zijn.

![Waarschuwing status](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Waarschuwingen in Log Analytics
Een waarschuwing in Log Analytics wordt gemaakt van een query log die automatisch wordt uitgevoerd met regelmatige tussenpozen.  U kunt een waarschuwing regel voor het maken van een query log.  Als de query resultaten die voldoen aan de criteria die u opgeeft retourneert, wordt een melding gemaakt.  Dit kan een specifieke query die Hiermee maakt u een waarschuwing als een bepaalde gebeurtenis wordt aangetroffen, of u kunt een meer algemene query die Hiermee wordt gezocht naar een willekeurige foutgebeurtenis betrekking hebben op een bepaalde toepassing.

Meld u Analytics waarschuwingen worden geschreven naar de bibliotheek OMS als een gebeurtenis en kunnen worden opgehaald met een query log.  Ze hebben geen een status zoals SCOM gebeurtenissen zodat u aangeven kunt wanneer het probleem is opgelost.

![OMS waarschuwing](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Wanneer SCOM wordt gebruikt als gegevensbron voor Log analyses, worden SCOM waarschuwingen naar de bibliotheek OMS geschreven als ze worden gemaakt of gewijzigd.  

![SCOM waarschuwing](media/operations-management-suite-monitoring-alerts/scom-alert.png)

De [Waarschuwing beheeroplossing](http://technet.microsoft.com/library/mt484092.aspx) biedt een overzicht van actieve waarschuwingen en verschillende algemene query's om op te halen verschillende sets waarschuwingen.  Hiermee kunt u efficiënter analyse van uw waarschuwingen dan een rapport in SCOM.  U kunt inzoomen op van de samenvattingen naar gedetailleerde gegevens en ad hoc-query's voor het ophalen van verschillende soorten waarschuwingen maken.

![Waarschuwing beheeroplossing](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Meldingen
Meldingen in SCOM verzendt u een e-mail of tekst in antwoord op berichten die aan bepaalde criteria voldoen.  U kunt de verschillende abonnementen met verschillende personen een melding ontvangen afhankelijk van criteria zoals het object wordt gecontroleerd, de ernst van de melding, het soort probleem dat gedetecteerd of de tijd van de dag maken.

Enkele abonnementen kunnen worden gebruikt om een strategie voltooid melding voor een groot aantal management packs te implementeren.

![SCOM waarschuwingen](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Log Analytics kan een melding via e-mail een melding dat is gemaakt door het instellen van een e-mailbericht melding actie op elke [waarschuwingsregels](http://technet.microsoft.com/library/mt614775.aspx).  Er geen de mogelijkheid van SCOM het abonneren op meerdere waarschuwingen met een één regel.  U moet ook uw eigen waarschuwingsregels maken aangezien OMS geen vooraf geconfigureerde biedt.

![Meld u aan analyses waarschuwingen](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

U kunt geen SCOM waarschuwingen in Log Analytics volledig door beheren aangezien u deze alleen in de bewerkingen-Console wijzigen kunt.  Log Analytics is nuttig als onderdeel van een waarschuwing management echter van proces voor het leveren van hulpprogramma's voor gegevensanalyse die alleen SCOM geen.

## <a name="alert-remediation"></a>Waarschuwing Remediation
[Remediation](http://technet.microsoft.com/library/mt614775.aspx) verwijst naar een poging die wordt aangeduid met een melding in om het probleem automatisch worden gecorrigeerd.
  
SCOM kunt u Diagnostisch hulpprogramma en herstel uitvoeren in antwoord op een monitor invoeren van een beschadigde status.  Dit gebeurt tegelijk naar de monitor maken van de melding.  Diagnostisch hulpprogramma en herstel worden meestal geïmplementeerd als een script die wordt uitgevoerd op de agent.  Een diagnose probeert te verzamelen meer informatie over het probleem met de gevonden terwijl een herstel Hiermee kunt u proberen het probleem te verhelpen.

Log Analytics kunt u een [Azure automatisering runbook](https://azure.microsoft.com/documentation/services/automation/) starten of een webhook in antwoord op de melding voor een logboek Analytics bellen.  Runbooks kan complexe logica geïmplementeerd in PowerShell bevatten.  Het script wordt uitgevoerd in Azure wordt aangegeven en toegang tot een Azure resources of een externe bronnen die beschikbaar zijn vanuit de cloud.  Azure automatisering beschikt over de mogelijkheid runbooks uitvoeren op een server in uw lokale datacenter, maar deze functie is momenteel niet beschikbaar bij het starten van het runbook in antwoord op Log Analytics waarschuwingen.

Herstel in SCOM zowel runbooks in OMS PowerShell-scripts bevatten, maar herstel zijn moeilijker te maken en beheren, omdat ze moeten worden opgenomen tussen een management pack.  Runbooks worden opgeslagen in Azure automatisering waarmee functies voor schrijven, testen en runbooks beheren.

Als u SCOM als gegevensbron voor Log analyses gebruikt, kunt u een melding voor een logboek Analytics SCOM waarschuwingen die zijn opgeslagen in de bibliotheek OMS ophalen met de query van een logboek maken.  Deze manier kunt u een runbook Azure automatisering uitvoeren in antwoord op de melding voor een SCOM.  Aangezien het runbook kan worden uitgevoerd in Azure wordt aangegeven, zou dit natuurlijk niet een goede strategie voor het herstellen van on-premises implementatie problemen zijn.

## <a name="next-steps"></a>Volgende stappen

- Informatie over de details van [waarschuwingen in het systeem Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).