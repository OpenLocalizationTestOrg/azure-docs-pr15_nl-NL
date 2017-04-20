<properties
    pageTitle="Log Analytics oplossingen toevoegen vanuit de galerie met oplossingen | Microsoft Azure"
    description="Log Analytics oplossingen zijn dat een verzameling logica, visualisatie en gegevensverzameling regels die aan de doelstellingen gedraaid rond een bepaalde probleemgebied bieden."
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

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Log Analytics oplossingen toevoegen vanuit de galerie met oplossingen

Log Analytics oplossingen zijn een verzameling **logica**, **visualisatie** en **gegevens acquisition regels** die aan de doelstellingen gedraaid rond een bepaalde probleemgebied opgeeft. In dit artikel lijsten oplossingen Log Analytics ondersteund en wordt uitgelegd hoe u toevoegen en deze verwijderen met de galerie met oplossingen.

Oplossingen kunnen meer inzicht naar:

- Help-onderzoeken en sneller operationele problemen oplossen
- verzamelen en diverse soorten machine gegevens relateren
- kunt u moet ondernemen met activiteiten zoals capaciteitsplanning, patch rapporteren en controleren.


>[AZURE.NOTE] Log Analytics bevat Log zoekfunctionaliteit, zodat u niet nodig hebt voor het installeren van een oplossing als u wilt deze inschakelen. U kunt echter gegevensvisualisaties, voorgestelde zoekopdrachten en inzichten openen door het toevoegen van oplossingen uit de galerie met oplossingen.

Nadat u een oplossing hebt toegevoegd, is de gegevens die van de servers in de infrastructuur van uw worden verzameld en verzonden naar de OMS-service. Verwerking van door de OMS duurt service meestal een paar minuten tot een uur. Nadat de gegevens worden verwerkt door de service, kunt u deze kunt weergeven in OMS.

Wanneer het niet meer nodig is, kunt u gemakkelijk een oplossing verwijderen. Wanneer u een oplossing verwijdert, wordt de gegevens wordt niet verzonden naar OMS, zodat u de hoeveelheid gegevens die worden gebruikt door de quota voor uw dagelijkse, bespaart als er een.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Oplossingen die worden ondersteund door de Microsoft-Agent bewaken

Op dit moment kunnen servers die verbonden zijn met OMS Microsoft Monitoring Agent met de oplossingen beschikbaar, waaronder de meeste gebruiken:

- Active Directory Assessment
- Beheer van waarschuwingen (zonder SCOM waarschuwingen)
- Antimalware
- Het bijhouden van wijzigingen
- Beveiliging
- SQL-Assessment
- Systeemupdates

De volgende oplossingen zijn echter *niet* ondersteund met de Microsoft-Agent cmdlets voor controle en System Center Operations Manager (SCOM)-agent vereisen.

- Beheer van waarschuwingen (inclusief SCOM waarschuwingen)
- Beheer van de capaciteit
- Configuratie bepalen

Raadpleeg [Verbinding Operations Manager naar Log Analytics](log-analytics-om-agents.md) voor informatie over de SCOM-agent verbinden met Log Analytics.

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Een oplossing die via de galerie met oplossingen toevoegen

1. Klik op de tegel van de **Galerie met oplossingen** op de pagina overzicht in OMS.    
    ![Galerie met oplossingen](./media/log-analytics-add-solutions/sol-gallery.png)
2. Klik op de pagina Galerie met oplossingen OMS meer informatie over elke oplossing beschikbaar. Klik op de naam van de oplossing aan die u wilt toevoegen aan OMS.
3. Klik op de pagina voor de oplossing aan die u hebt gekozen wordt gedetailleerde informatie over de oplossing weergegeven. Klik op **toevoegen**.
4. Een nieuwe titel voor de oplossing die u hebt toegevoegd wordt weergegeven op de pagina in OMS en u kunt gaan gebruiken nadat u uw gegevens worden verwerkt door de service OMS overzicht.

## <a name="to-configure-solutions"></a>Voor het configureren van oplossingen
1. U moet enkele oplossingen configureren. Bijvoorbeeld, moet u automatisering, herstel van Azure-Site en back-up configureren voordat u ze kunt gebruiken.
2. Voor een van deze oplossingen, klikt u op de tegel op de pagina overzicht.  
    ![oplossing configureren](./media/log-analytics-add-solutions/configure-additional.png)
3. Vervolgens de oplossing configureren met de benodigde gegevens en klik vervolgens op **Opslaan**.  
    ![oplossing configureren](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Verwijderen van een oplossing die via de galerie met oplossingen

1. Klik op de tegel van de **Instellingen** op de pagina overzicht in OMS.
2. Klik op de pagina instellingen, klik op het tabblad oplossingen klikt u op **verwijderen** voor de oplossing aan die u wilt verwijderen.
3. Klik in het bevestigingsvenster op **Ja** als u wilt verwijderen van de oplossing.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Gegevens verzamelen details voor OMS functies en oplossingen

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor OMS functies en oplossingen. Directe agenten en SCOM agenten zijn in principe hetzelfde, maar de directe agent extra functionaliteit bevat om het verbinding maken met de werkruimte OMS en routeren via een proxy. Als u een agent SCOM gebruikt, moet deze als een agent OMS om te communiceren met OMS worden gericht. SCOM agenten in deze tabel zijn OMS agenten die met SCOM verbonden zijn. Zie [Verbinding maken met Operations Manager naar Log Analytics](log-analytics-om-agents.md) voor informatie over het verbinden van uw bestaande SCOM-omgeving naar OMS.

>[AZURE.NOTE] Het type agent waarmee u bepaalt hoe gegevens worden verzonden naar OMS, met de volgende voorwaarden:

- U de directe agent of gebruiken een agent OMS SCOM gekoppeld.
- Als SCOM moet ondernomen worden, is SCOM agent gegevens voor de oplossing altijd verzonden naar OMS met de SCOM management group. Daarnaast kunnen als SCOM vereist is, wordt alleen de SCOM-agent gebruikt door de oplossing.
- Als SCOM niet vereist is en de tabel ziet u dat SCOM agent gegevens wordt verstuurd naar OMS met de management group, is klikt u vervolgens SCOM agent gegevens altijd verzonden naar OMS management groepen gebruiken. Directe agenten de management group overslaan en hun gegevens rechtstreeks naar OMS verzenden.
- Wanneer SCOM agent gegevens niet voor een groep management gebruiken verzonden wordt, klikt u vervolgens de gegevens worden verzonden rechtstreeks naar OMS — de management group overslaan.


|gegevenstype| platform | Directe Agent | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|---|
|AD Assessment|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 dagen|
|AD herhaling Status|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 dagen|
|Waarschuwingen (Nagios)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|bij ontvangst|
|Waarschuwingen (Zabbix)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 minuut|
|Waarschuwingen (Operations Manager)|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 minuten|
|Antimalware|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| per uur|
|Beheer van de capaciteit|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| per uur|
|Het bijhouden van wijzigingen|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| per uur|
|Het bijhouden van wijzigingen|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|per uur|
|Configuratie Assessment (oudere Advisor)|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| twee keer per dag|
|ETW|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minuten|
|IIS-logboeken|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minuten|
|Belangrijke kluizen|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuten|
|Netwerk Toepassingsgateways|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuten|
|Beveiligingsgroepen netwerk|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuten|
|Office 365|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|Klik op melding|
|Prestatie-items|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|Als gepland, minimum van 10 seconden|
|Prestatie-items|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|Als gepland, minimum van 10 seconden|
|Service stof|Windows|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minuten|
|SQL-Assessment|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 dagen|
|SurfaceHub|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|bij ontvangst|
|Syslog|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|van Azure opslag: 10 minuten; van-agent: bij ontvangst|
|Systeemupdates|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| ten minste 2 maal per dag en 15 minuten na de installatie van een update|
|Gebeurtenislogboeken van Windows-beveiliging|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)| voor Azure opslag: 10 min; voor de agent: bij ontvangst|
|Windows firewall-Logboeken|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)| bij ontvangst|
|Gebeurtenislogboeken van Windows|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| voor Azure opslag: 1 min; voor de agent: bij ontvangst|
|Gegevens van de kabel|Windows (2012 R2 / 8.1 of hoger)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nee](./media/log-analytics-add-solutions/oms-bullet-red.png)| elke 1 minuut|

## <a name="log-analytics-preview-solutions-and-features"></a>Meld u aan analyses Preview oplossingen en functies

Door te voeren van een service en devops procedures volgen kunnen we samenwerken met klanten functies en oplossingen ontwikkelen.

In privé voorbeeldweergave geven we een kleine groep klanten toegang tot een vroege implementatie van de functie of oplossing krijgt feedback en kunt verbeteren. Deze vroege implementatie heeft minimale functies en mogelijkheden voor operationele.

Ons doel is om te proberen dingen snel zodat we vinden kunnen wat werkt en wat nog niet werkt. We doorlopen dit proces totdat de feedback van klanten privé preview ons meldt dat we gereed is voor een openbare preview bent.

Tijdens de public preview maken wordt de functie of oplossing beschikbaar voor alle gebruikers meer feedback vragen en valideren onze schaalbaarheid en efficiëntie. Tijdens deze fase:

- Functies wordt weergegeven op het tabblad instellingen en kunnen worden ingeschakeld door een gebruiker
- Voorbeeld oplossingen kunnen worden toegevoegd via de galerie of met een gepubliceerde script

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Wat moet ik weten over de Preview-functies en oplossingen?

We enthousiast over de nieuwe functies en oplossingen en we graag mee werkt werken met u ze ontwikkelen.

Functies en oplossingen zijn niet geschikt voor iedereen, dus voordat het vragen deel te nemen aan een privé voorbeeld of het inschakelen van een openbare voorbeeld ervoor zorgen dat u OK werkt met een ander nummer dat wordt gewerkt.

Als u een voorbeeldfunctie via de portal u inschakelt ziet u een waarschuwing die herinneren dat de functie in de proefversie is.

#### <a name="for-both-private-and-public-preview"></a>Voor *persoonlijke* en *openbare* preview

De volgende van toepassing op openbare en persoonlijke voorbeelden:

- Dingen werken mogelijk alleen altijd correct.
  - Problemen bereik worden lichte ergernis veroorzaken via te vestigen niet helemaal werkt
- Er is voor het voorbeeld naar een negatieve invloed hebben op uw systemen mogelijke / omgeving
  - We proberen te voorkomen dat negatieve wat er met de systemen u gebruikt met OMS maar soms onverwachte dingen optreden
- Gegevensverlies / beschadigd raken
- We gewoonlijk u gevraagd te verzamelen diagnostische logboeken of andere gegevens voor het oplossen van problemen
- De functie of een oplossing kan worden verwijderd (tijdelijk of permanent)
  - Op basis van onze geleerde lessen in de voorbeeldweergave die we besluiten niet los van de functie of oplossing
- Voorbeelden werkt mogelijk niet of niet getest met alle configuraties en we kunnen beperken:
  - De besturingssystemen die kunnen worden gebruikt (bijvoorbeeld een functie mogelijk alleen van toepassing op Linux terwijl u in de Preview-versie)
  - Het type agent (MMA, SCOM) die kan worden gebruikt (bijvoorbeeld een functie werkt niet met SCOM terwijl u in de Preview-versie)  
- Voorbeeld oplossingen en functies worden niet bedekt door de Service Level Agreement
- Gebruik van functies wiens rekening gebruikskosten
- Functies of mogelijkheden dat u nodig voor de functie hebt / oplossing nuttig ontbreken mogelijk of onvolledige
- Functies / oplossingen mogelijk niet beschikbaar in alle regio's
- Functies / oplossingen mogelijk niet gelokaliseerd
- Functies / oplossingen mogelijk een limiet van het aantal klanten of apparaten die deze kunnen gebruiken
- Mogelijk moet u scripts gebruiken voor het uitvoeren van de configuratie en de oplossing/functie inschakelen
- De gebruikersinterface (UI) onvolledig en dag kan veranderen
- Openbare voorbeelden mogelijk niet geschikt te maken voor uw productie / kritieke systemen

#### <a name="for-private-preview"></a>Voor *privé* preview

Hieronder ziet u voorbeelden van privé-specifieke naast de bovenstaande items.

- We verwachten u bieden ons feedback over het gebruik van SharePoint zodat we de functie/oplossing nog beter kunt maken
- We kunnen contact met u opnemen voor feedback met enquêtes, telefoongesprekken of e-mail
- Dingen altijd manier van werken
- We mogelijk een niet-Uitlekt overeenkomst (NDA) voor deelname of vertrouwelijke inhoud kan bevatten
  - Controleer met het programma Manager die verantwoordelijk is voor de Preview-versie voor meer informatie over welke beperkingen er gelden op uitlekt voor het blog, tweeting of anderszins communiceren met derden
- Worden niet uitgevoerd in productie- / kritieke systemen


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Hoe krijg ik toegang tot privé preview-functies en oplossingen?

We uitnodigen klanten privé voorbeelden tot en met verschillende manieren afhankelijk van de voorbeeldversie.

- De maandelijkse enquête te beantwoorden en ons toestemming dat moet worden opgevolgd met u geven verbetert de kans groter dat wordt uitgenodigd voor een persoonlijke voorbeeld.
- Uw team van Microsoft-account, kunt u benoemen.
- U kunt zich aanmeldt op basis van details op twitter [msopsmgmt](https://twitter.com/msopsmgmt) geplaatst
- U kunt zich aanmeldt op basis van details gedeelde community gebeurtenissen – Bekijk voor ons vergaderen ups, vergaderingen en klik in de online community's.


## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) om gedetailleerde informatie verzameld door oplossingen te bekijken.
