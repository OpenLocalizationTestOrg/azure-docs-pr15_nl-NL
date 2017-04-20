<properties
    pageTitle="Optimalisatie van uw omgeving met de evaluatie van de SQL-oplossing in Log Analytics | Microsoft Azure"
    description="U kunt de evaluatie van de SQL-oplossing Beoordeel de risico's en de status van uw serveromgevingen met regelmatige tussenpozen."
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

# <a name="optimize-your-environment-with-the-sql-assessment-solution-in-log-analytics"></a>Optimalisatie van uw omgeving met de evaluatie van de SQL-oplossing in Log Analytics


U kunt de evaluatie van de SQL-oplossing Beoordeel de risico's en de status van uw serveromgevingen met regelmatige tussenpozen. In dit artikel kunt u de oplossing installeren, zodat u corrigerende acties voor potentiële problemen kunt uitvoeren.

Deze oplossing biedt een gesorteerde lijst met aanbevelingen die specifiek zijn voor de serverinfrastructuur van uw geïmplementeerd. De aanbevelingen zijn gecategoriseerd over zes gebieden met focus waarmee u snel informatie lezen over het risico en corrigerende maatregelen nemen.

De aanbevelingen zijn gebaseerd op de kennis en ervaring door Microsoft engineers uit duizenden klant bezoeken. Elke aanbeveling bevat richtlijnen over waarom een probleem mogelijk interessant voor u zijn en hoe u de voorgestelde wijzigingen implementeren.

Focus gebieden die voor uw organisatie belangrijkst zijn en de voortgang naar een risico gratis en orde omgeving uitgevoerd bijhouden, kunt u kiezen.

Nadat u de oplossing hebt toegevoegd en een beoordeling voltooid, samenvatting is worden gegevens over voor focus gebieden worden weergegeven op het dashboard **SQL Assessment** voor de infrastructuur in uw omgeving. De volgende secties wordt beschreven hoe de gegevens op het dashboard **SQL Assessment** , waar u kunt weergeven en vervolgens aanbevolen acties uitvoeren voor de infrastructuur van uw SQL-server gebruiken.

![afbeelding van de evaluatie van de SQL-tegel](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![afbeelding van de SQL-Assessment dashboard](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
SQL-Assessment werkt met alle ondersteunde versies van SQL Server voor de standaard, ontwikkelaars en Enterprise-edities.

Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

- Agenten moeten worden geïnstalleerd op servers SQL Server is geïnstalleerd.
- De evaluatie van de SQL-oplossing vereist .NET Framework 4 op elke computer waarop een agent OMS heeft geïnstalleerd.
- Wanneer u de Operations Manager-agent met SQL-Assessment, moet u een account Operations Manager Run-As gebruiken. Zie [Operations Manager starten als '-accounts voor OMS](#operations-manager-run-as-accounts-for-oms) hieronder voor meer informatie.

    >[AZURE.NOTE] De MMA-agent biedt geen ondersteuning voor Operations Manager Run-As accounts.

- De evaluatie van de SQL-oplossing toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md). Er is geen verdere configuratie vereist.

>[AZURE.NOTE] Nadat u de oplossing hebt toegevoegd, wordt het bestand AdvisorAssessment.exe wordt toegevoegd aan de servers met agenten. Configuratiegegevens lezen en vervolgens naar de OMS-service in de cloud voor verwerking verzonden. Logica wordt toegepast op de ontvangen gegevens en de gegevens voor de cloudservice-records.

## <a name="sql-assessment-data-collection-details"></a>SQL-Assessment siteverzameling detailgegevens

SQL-Assessment verzamelt WMI-gegevens, register prestatiegegevens en resultaten SQL Server management dynamische weergeven met de agenten die u hebt ingeschakeld.

De volgende tabel ziet u methoden voor het verzamelen van gegevens voor kunt vinden, of Operations Manager (SCOM) is vereist, en hoe vaak gegevens worden verzameld door een agent.

| platform | Directe Agent | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Nee](./media/log-analytics-sql-assessment/oms-bullet-red.png)|    ![Nee](./media/log-analytics-sql-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png)|   7 dagen|

## <a name="operations-manager-run-as-accounts-for-oms"></a>Operations Manager starten als '-accounts voor OMS

Log Analytics in OMS gebruikt de groep voor de Operations Manager-agent en beheer voor het verzamelen en gegevens verzenden naar de OMS-service. OMS builds na het management packs voor werkbelasting kunt opgeven toevoegen waarde-services. Elke werkbelasting vereist werkbelasting-specifieke machtigingen voor het uitvoeren van management packs in de beveiligingscontext van een andere, zoals een domeinaccount. U moet referentiegegevens door het configureren van een account Manager bewerkingen die uitvoeren als.

Raadpleeg de volgende informatie voor het instellen van het Operations Manager starten als '-account voor SQL-Assessment.

### <a name="set-the-run-as-account-for-sql-assessment"></a>Het account uitvoeren als voor SQL-assessment instellen

 Als u het SQL Server management pack al gebruikt, kunt u dat account uitvoeren als moet gebruiken.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>De SQL uitvoeren als-account configureren in de bewerkingen-console

>[AZURE.NOTE] Als u de directe OMS-agent, in plaats van de SCOM-agent gebruikt, wordt het management pack altijd worden uitgevoerd in de beveiligingscontext van de lokale systeemaccount. Stap 1-5 hieronder overslaan en uitvoeren van de T-SQL- of Powershell steekproef, systeemaccount geven als de gebruikersnaam in te voeren.

1. In Operations Manager, open de bewerkingen-console en klik vervolgens op **beheer**.

2. Klik op **profielen**onder **Uitvoeren als configuratie**, en open **OMS SQL Assessment uitvoeren als profiel**.

3. Klik op de pagina **Uitvoeren als Accounts** op **toevoegen**.

4. Selecteer een account Windows uitvoeren als met de referenties die u nodig hebt voor SQL Server, of klik op **Nieuw** om een.
    >[AZURE.NOTE] Het accounttype uitvoeren als moet Windows. Het account uitvoeren als moet eveneens deel uit van de lokale beheerdersgroep op alle Windows-Servers hostingprovider exemplaren van SQL Server.

5. Klik op **Opslaan**.

6. Klik in het onderstaande voorbeeld van de T-SQL uitvoeren op elke SQL Server-instantie minimale machtigingen vereist voor uitvoeren als Account om uit te voeren SQL Assessment en wijzigen. U moet echter niet dit doen als een uitvoeren als de Account al deel van de rol van de server systeembeheerder in SQL Server-exemplaren uitmaakt.

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>De SQL uitvoeren als-account via Windows PowerShell te configureren

Open een PowerShell-venster en het volgende script uitvoeren nadat u deze hebt bijgewerkt door uw gegevens:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"
     
    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Informatie over hoe aanbevelingen zijn prioriteit

Elke aanbeveling krijgt een weging waarde die het relatieve belang van de aanbeveling aangeeft. Alleen de tien belangrijkste aanbevelingen worden weergegeven.

### <a name="how-weights-are-calculated"></a>Hoe lijndikten worden berekend

Gewicht zijn statistische waarden op basis van drie belangrijke factoren:

- De *kans* dat een probleem vastgesteld wordt problemen veroorzaken. Er is een hogere kans gelijk aan een grotere algemene score voor de aanbeveling.

- De *gevolgen* van het probleem op uw organisatie als dit leidt een probleem tot. Er is een hogere impact gelijk aan een grotere algemene score voor de aanbeveling.

- De *hoeveelheid* vereist voor het implementeren van de aanbeveling. Er is een hogere inspanning gelijk aan een kleinere algemene score voor de aanbeveling.

Het gewicht voor elke aanbeveling wordt uitgedrukt als een percentage van de totale score die beschikbaar zijn voor elk focusgebied. Bijvoorbeeld als een aanbeveling in het gebied van de focus beveiliging en naleving heeft een score van 5%, die aanbeveling implementeren, neemt de algehele beveiliging en naleving score door 5%.

### <a name="focus-areas"></a>Focus gebieden

**Beveiliging en naleving** - dit focusgebied ziet u aanbevelingen voor mogelijke beveiligingsrisico's en inbreuk op, bedrijfsbeleid en nalevingsvereisten van technische, wet en regelgeving.

**Beschikbaarheid en bedrijfscontinuïteit** - dit focusgebied ziet u aanbevelingen voor de servicebeschikbaarheid van de, tolerantie infrastructuur en bedrijven beveiliging.

**Prestaties en schaalbaarheid** - dit focusgebied laat aanbevelingen om te helpen van uw organisatie IT-infrastructuur vergroten, zorg ervoor dat uw IT-omgeving huidige prestaties voldoet, en kan reageren op de behoeften van de infrastructuur wijzigen.

**Een upgrade uitvoert, migratie en implementatie** - dit focusgebied, ziet u aanbevelingen om te upgraden, migreren en implementeren van SQL Server naar uw bestaande infrastructuur.

**Bewerkingen en Monitoring** - dit focusgebied laat aanbevelingen om te helpen uw IT-activiteiten stroomlijnen, implementeren preventief onderhoud en prestaties.

**Beheer van de configuratie van wijzigingen en** -dit focusgebied laat aanbevelingen om te helpen beveiligen dagelijks, ervoor te zorgen dat wijzigingen niet een negatieve invloed op de infrastructuur van uw wijziging besturingselement procedures, vast en voor het bijhouden en systeemconfiguraties controleren.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Moet u voor het verkrijgen van 100% in elk focusgebied gericht?

Niet per se. De aanbevelingen zijn gebaseerd op de kennis en ervaringen ervaring door Microsoft engineers in duizenden klant bezoeken. Echter geen infrastructuur twee server zijn hetzelfde en specifieke aanbevelingen zijn mogelijk meer of minder relevant zijn voor u. Sommige aanbevelingen voor beveiliging mogelijk bijvoorbeeld minder relevante als uw virtuele machines zijn niet zichtbaar op Internet. Het is mogelijk dat sommige aanbevelingen beschikbaarheid minder relevant zijn voor services waarmee lage prioriteit ad-hoc gegevens verzamelen en rapporteren. Problemen die belangrijk voor een volwaardige bedrijf zijn mogelijk minder belangrijk is voor een opstart. U kunt herkennen welke gebieden focus zijn uw prioriteiten en kijkt u naar hoe uw scores stadium worden gewijzigd.

Elke aanbeveling bevat richtlijnen over waarom het belangrijk is. U moet deze instructies gebruiken om te bepalen of het implementeren van de aanbeveling geschikt voor u is, gegeven de aard van uw IT-services en de zakelijke behoeften van uw organisatie.

## <a name="use-assessment-focus-area-recommendations"></a>Assessment focus gebied aanbevelingen gebruiken

Voordat u een beoordeling-oplossing in OMS gebruiken kunt, moet u de oplossing is geïnstalleerd. Meer informatie over het installeren van oplossingen, raadpleegt u [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md). Nadat dit is geïnstalleerd, kunt u het overzicht van aanbevelingen weergeven met behulp van de evaluatie van de SQL-tegel op de pagina overzicht in OMS.

Bekijk de samengevatte naleving beoordelingen voor uw infrastructuur en klik vervolgens op aanbevelingen voor meer details in.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aanbevelingen voor een focusgebied weergeven en corrigerende maatregelen nemen

1. Klik op de tegel **SQL Assessment** op de pagina **Overzicht** .
2. Klik op de pagina **SQL Assessment** de samenvatting van de gegevens in een van de focus gebied bladen controleren en klik op een om aanbevelingen voor dit focusgebied weer te geven.
3. Klik op een van de focus gebied pagina's, kunt u de prioriteit aanbevelingen voor uw omgeving weergeven. Klik op een aanbeveling onder **Objecten beïnvloed** details over waarom de aanbeveling bestaat uit de weergeven.  
    ![afbeelding van de SQL-Assessment aanbevelingen](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. U kunt corrigerende maatregelen voorgesteld in **Voorgestelde acties**uitvoeren. Wanneer het item is opgelost, record later beoordelingen aanbevolen handelingen die zijn uitgevoerd en uw score naleving, neemt. Gecorrigeerde items worden weergegeven als **Objecten doorgegeven**.

## <a name="ignore-recommendations"></a>Aanbevelingen negeren

Hebt u aanbevelingen die u wilt negeren, kunt u een tekstbestand dat OMS wordt gebruikt om te voorkomen dat aanbevelingen wordt weergegeven in de resultaten van uw beoordeling.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Aanbevelingen die u negeert identificeren

1.  Meld u aan bij uw werkruimte en open Log zoeken. Gebruik de volgende query toevoegen aan lijst aanbevelingen die niet zijn voor computers in uw omgeving.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Hier ziet u een schermafbeelding met de query Log: ![is mislukt aanbevelingen](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)

2.  Kies aanbevelingen die u wilt negeren. Gebruikt u de waarden voor RecommendationId in de volgende procedure.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Maken en gebruiken van een tekstbestand IgnoreRecommendations.txt

1.  Een bestand met de naam IgnoreRecommendations.txt maken.
2.  Plak of typ elke RecommendationId voor elke aanbeveling dat u wilt dat OMS negeren op een aparte regel en klik vervolgens opslaan en sluit het bestand.
3.  Het bestand heb opgeslagen in de volgende map op elke computer waarop OMS aanbevelingen genegeerd.
    - Op computers met Microsoft Monitoring Agent (verbonden rechtstreeks of via Operations Manager) - *systeemstation*: \Program Files\Microsoft controle Agent\Agent
    - Klik op de server Operations Manager - *systeemstation*: \Program Files\Microsoft systeem Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Om te bevestigen dat aanbevelingen worden genegeerd

1.  Nadat u de volgende geplande assessment wordt uitgevoerd, al dan niet standaard 7 dagen, wordt de opgegeven aanbevelingen zijn gemarkeerd genegeerd en wordt niet weergegeven op het dashboard assessment.
2.  U kunt de volgende Log zoekopdrachten gaat uitvoeren voor een overzicht van de genegeerde aanbevelingen.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
3.  Als u later besluit dat u wilt zien van genegeerde aanbevelingen, IgnoreRecommendations.txt bestanden verwijderen of kunt u RecommendationIDs hiertegen verwijderen.

## <a name="sql-assessment-solution-faq"></a>SQL-Assessment oplossing Veelgestelde vragen

*Hoe vaak wordt een beoordeling uitgevoerd?*
- De beoordeling elke 7 dagen wordt uitgevoerd.

*Is er een manier om te configureren hoe vaak de beoordeling wordt uitgevoerd?*
- Niet op dit moment.

*Als een andere server wordt vastgesteld nadat ik de SQL-assessment-oplossing hebt toegevoegd, wordt deze worden beoordeeld?*
- Ja, zodra deze is beoordeeld van typt u vervolgens op, 7 dagen wordt ontdekt.

*Als een server is buiten gebruik gesteld, wanneer deze verwijderd uit de beoordeling?*
- Als de gegevens worden niet door een server voor 3 weken worden verzonden, wordt het verwijderd.

*Wat is de naam van het proces dat het verzamelen van gegevens wordt?*
- AdvisorAssessment.exe

*Hoe lang duurt het voor de gegevens te verzamelen?*
- Het verzamelen van de werkelijke gegevens op de server duurt ongeveer 1 uur. Het duurt langer op servers met een groot aantal SQL-exemplaren of databases.

*Welk type gegevens worden verzameld?*
- De volgende soorten gegevens zijn verzameld:
    - WMI
    - Register
    - Prestatie-items
    - SQL dynamische management weergaven (DMV).

*Is er een manier om te configureren als gegevens worden verzameld?*
- Niet op dit moment.

*Waarom heb ik een uitvoeren als de Account configureren*
- Voor SQL Server, worden een klein aantal SQL-query's uitgevoerd. In de volgorde worden uitgevoerd, een uitvoeren als Account met machtigingen voor VIEW SERVER STATE naar SQL moet worden gebruikt.  Daarnaast zijn query's op WMI, lokale beheerdersreferenties zijn vereist.

*Waarom worden alleen de top 10-aanbevelingen weergegeven?*
- In plaats van zodat u een volledige problematisch lijst van taken, is het raadzaam dat u zich richten op de prioriteit aanbevelingen eerst adressering. Nadat u deze oplossen, komen extra aanbevelingen beschikbaar. Als u liever gedetailleerde lijst wilt weergeven, kunt u alle aanbevelingen voor het zoeken OMS log kunt weergeven.

*Is er een manier om een aanbeveling negeren?*
- Ja, Zie [negeren aanbevelingen](#ignore-recommendations) sectie hierboven.



## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) naar gedetailleerde gegevens van de SQL-Assessment en aanbevelingen weergeven.
