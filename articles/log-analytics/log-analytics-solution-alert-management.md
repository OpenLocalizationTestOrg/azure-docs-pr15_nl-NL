<properties
   pageTitle="Waarschuw beheeroplossing in bewerkingen Management Suite Kantoorbeheersysteem | Microsoft Azure"
   description="De waarschuwing beheeroplossing in Log Analytics kunt u alle waarschuwingen in uw omgeving analyseren.  Behalve waarschuwingen gegenereerd binnen OMS, importeert deze waarschuwingen uit verbonden systeem Center Operations Manager (SCOM) beheer van groepen in Log Analytics."
   services="log-analytics"
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
   ms.date="10/06/2016"
   ms.author="bwren" />

# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Waarschuwing beheeroplossing in bewerkingen Management Suite Kantoorbeheersysteem

![Waarschuwingspictogram Management](media/log-analytics-solution-alert-management/icon.png) De waarschuwing beheeroplossing kunt u alle waarschuwingen in uw omgeving analyseren.  Behalve waarschuwingen gegenereerd binnen OMS, importeert deze waarschuwingen uit verbonden systeem Center Operations Manager (SCOM) beheer van groepen in Log Analytics.  In een omgeving met meerdere management groepen, de waarschuwing beheeroplossing vindt u een gecombineerde weergave van waarschuwingen over alle management groepen.

## <a name="prerequisites"></a>Vereisten voor

- Als u wilt importeren waarschuwingen vanuit SCOM, is deze oplossing vereist een verbinding tussen uw werkruimte OMS en SCOM management groep met behulp van het proces wordt beschreven in [Verbinding maken met Operations Manager naar Log Analytics](log-analytics-om-agents.md).  


## <a name="configuration"></a>Configuratie

De waarschuwing beheeroplossing toevoegen aan uw OMS werkruimte met het proces wordt beschreven in [toevoegen oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie vereist.

## <a name="management-packs"></a>Management packs

Als uw SCOM management groep is verbonden met uw werkruimte OMS, wordt klikt u vervolgens de volgende management packs geïnstalleerd in SCOM wanneer u deze oplossing toevoegt.  Er is geen configuratie of onderhoud van deze management packs vereist.  

- Microsoft System Center Advisor waarschuwing Management (Microsoft.IntelligencePacks.AlertManagement)

Zie voor meer informatie over hoe de oplossing management packs worden bijgewerkt, [Verbinding Operations Manager naar Log Analytics](log-analytics-om-agents.md).

## <a name="data-collection"></a>Gegevens verzamelen

### <a name="agents"></a>Agenten

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

| Verbonden bron | Ondersteuning | Beschrijving |
|:--|:--|:--|
| [Windows-agenten](log-analytics-windows-agents.md) | Nee | Directe Windows agenten genereren geen SCOM waarschuwingen. |
| [Linux agenten](log-analytics-linux-agents.md) | Nee | Directe Linux agenten genereren geen SCOM waarschuwingen. |
| [SCOM management groep](log-analytics-om-agents.md) | Ja | Waarschuwingen die worden gegenereerd op SCOM agenten zijn afgeleverd in de groep management en vervolgens doorgestuurd naar Log Analytics.<br><br>Een directe verbinding van de SCOM-agent met Log Analytics is niet vereist. Waarschuwing gegevens uit de groep management doorgestuurd naar de opslagplaats OMS. |
| [Azure opslag-account](log-analytics-azure-storage.md) | Nee | SCOM meldingen worden niet opgeslagen in Azure opslag-accounts. |

### <a name="collection-frequency"></a>Frequentie van de siteverzameling

Waarschuwingen gegenereerd binnen OMS zijn onmiddellijk beschikbaar voor de oplossing.  Waarschuwing gegevens is verzonden vanuit de SCOM management group op Log Analytics elke 3 minuten.  

## <a name="using-the-solution"></a>Gebruik van de oplossing

Wanneer u de waarschuwing beheeroplossing aan uw werkruimte OMS toevoegt, wordt de tegel **Beheer van waarschuwingen** worden toegevoegd aan uw dashboard OMS.  Een aantal en grafische weergave van het aantal actieve waarschuwingen die zijn gegenereerd in de afgelopen 24 uur, worden deze tegel weergegeven.  U kunt dit tijdsbereik niet wijzigen.

![Waarschuwing Management-tegel](media/log-analytics-solution-alert-management/tile.png)

Klik op de tegel **Beheer van waarschuwingen** openen van het dashboard van het **Beheer van waarschuwingen** .  Het dashboard bevat de kolommen in de volgende tabel.  Elke kolom toont de bovenste tien waarschuwingen met tellen van die kolom vergelijkingscriteria voor het opgegeven bereik en tijdsbereik.  U kunt een zoekopdracht log vindt u de hele lijst door te klikken op **alle** onderaan in de kolom of door te klikken op de kolomkop waarop u kunt uitvoeren.

| Kolom| Beschrijving |
|:--|:--|
| Kritieke waarschuwingen | Alle waarschuwingen met een ernst van kritiek gegroepeerd op de naam van de waarschuwing.  Klik op de naam van een waarschuwing om uit te voeren een logboek zoekopdracht alle records voor deze waarschuwing als resultaat. |
| Waarschuwing waarschuwingen | Alle waarschuwingen met een ernst van de waarschuwing die zijn gegroepeerd op de naam van de waarschuwing.  Klik op de naam van een waarschuwing om uit te voeren een logboek zoekopdracht alle records voor deze waarschuwing als resultaat. |
| Actieve SCOM waarschuwingen | Alle SCOM meldingen met een staat andere dan *gesloten* , gegroepeerd op bron die het bericht heeft gegenereerd. |
| Alle actieve meldingen | Alle waarschuwingen met een gegroepeerd op de naam van de waarschuwing ernst. Alleen bevat SCOM waarschuwingen met een staat die geen *gesloten*.|

Als u naar rechts schuift, wordt het dashboard, een lijst maken met diverse algemene query's die u kunt op een [logboek zoeken](log-analytics-log-searches.md) voor waarschuwingen gegevens uitvoeren.

![Waarschuwing Beheerdashboard](media/log-analytics-solution-alert-management/dashboard.png)

## <a name="scope-and-time-range"></a>Scope- en tijdbereik op

Standaard is het bereik van de geanalyseerd in de waarschuwing beheeroplossing voor meldingen van alle verbonden management groepen gegenereerd binnen de laatste zeven dagen.  

![Waarschuwing Management bereik](media/log-analytics-solution-alert-management/scope.png)

- De groepen management is opgenomen in de analyse, klikt u op **bereik** aan de bovenkant van het dashboard.  U kunt selecteren **globale** voor alle verbonden management groepen of **Door Management Group** om te selecteren van een groep één management.

- Als het tijdsbereik van waarschuwingen, selecteert u de **gegevens op basis van** aan de bovenkant van het dashboard.  U kunt meldingen gegenereerd binnen de laatste zeven dagen, 1 dag of 6 uur selecteren.  Of u kunt **aangepaste** selecteren en geef een aangepast datumbereik.

## <a name="log-analytics-records"></a>Logboekrecords Analytics

De waarschuwing beheeroplossing analyseert geen record met een soort **Waarschuwing**.  Er wordt ook waarschuwingen importeren uit SCOM en maken van een record voor elk voorzien van een type **Waarschuwing** en een SourceSystem van **OpsManager**.  Deze records hebt de eigenschappen in de volgende tabel.  

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *Waarschuwen* |
| SourceSystem | *OpsManager* |
| AlertContext | Details van het gegevensitem waardoor de melding naar u worden gegenereerd in XML-indeling. |
| AlertDescription | Gedetailleerde beschrijving van de melding. |
| AlertId | GUID van de melding. |
| AlertName | De naam van de waarschuwing. |
| AlertPriority | Het prioriteitsniveau van de waarschuwing. |
| AlertSeverity | Het prioriteitsniveau van de waarschuwing. |
| AlertState | Status van de meest recente resolutie van de waarschuwing. |
| LastModifiedBy | Naam van de gebruiker van wie de melding voor het laatst is gewijzigd. |
| ManagementGroupName | De naam van de management group waar de waarschuwing is gegenereerd. |
| Aantalherhalingen | Aantal keer die de melding voor een hetzelfde is gegenereerd voor dezelfde gecontroleerd object sinds wordt opgelost. |
| ResolvedBy | Naam van de gebruiker van wie de melding opgelost. Leeg zijn als de melding nog niet opgelost is. |
| SourceDisplayName | De weergavenaam van de controle object dat het bericht heeft gegenereerd. |
| SourceFullName | Volledige naam van de controle object dat het bericht heeft gegenereerd. |
| TicketId | Tickets-ID voor de waarschuwing als de omgeving SCOM is geïntegreerd met een proces voor het toewijzen van tickets voor waarschuwingen.  Lege van geen tickets ID wordt toegewezen. |
| TimeGenerated | Datum en tijd waarop de melding is gemaakt. |
| TimeLastModified | Datum en tijd waarop de melding voor het laatst is gewijzigd. |
| TimeRaised | Datum en tijd waarop de melding is gegenereerd. |
| TimeResolved | Datum en tijd waarop de melding is opgelost. Leeg zijn als de melding nog niet opgelost is. |

## <a name="sample-log-searches"></a>Voorbeeld log zoekopdrachten

De volgende tabel vindt steekproef log wordt gezocht naar waarschuwing records die zijn verzameld met deze oplossing.  

| Query | Beschrijving |
|:--|:--|
| Type = waarschuwing SourceSystem = OpsManager AlertSeverity fout TimeRaised = > nu 24 uur | Kritieke waarschuwingen verheven tijdens de afgelopen 24 uur |
| Typ = waarschuwing AlertSeverity waarschuwing TimeRaised = > nu 24 uur | Waarschuwing waarschuwingen verheven tijdens de afgelopen 24 uur  |
| Typ = waarschuwing SourceSystem OpsManager AlertState =! gesloten TimeRaised = > nu 24 uur & #124; count() als aantal door SourceDisplayName meten | Gegevensbronnen met actieve waarschuwingen verheven tijdens de afgelopen 24 uur |
| Type = waarschuwing SourceSystem = OpsManager AlertSeverity fout TimeRaised = > nu 24 uur AlertState! = gesloten | Kritieke waarschuwingen verheven tijdens de afgelopen 24 uur die nog steeds actief |
| Typ = waarschuwing SourceSystem OpsManager TimeRaised = > nu 24 uur AlertState = gesloten | Waarschuwingen verheven tijdens de afgelopen 24 uur die nu zijn gesloten |
| Typ = waarschuwing SourceSystem OpsManager TimeRaised = > nu - 1 dag & #124; count() als aantal door AlertSeverity meten | Waarschuwingen verheven tijdens de afgelopen 1 dag gegroepeerd op hun ernst |
| Typ = waarschuwing SourceSystem OpsManager TimeRaised = > nu - 1 dag & #124; aantalherhalingen desc sorteren | Waarschuwingen verheven tijdens de afgelopen 1 dag gesorteerd op hun aantal herhalingen-waarde |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [waarschuwingen in Log Analytics](log-analytics-alerts.md) voor meer informatie over het genereren van waarschuwingen van Log analyseprogramma.
