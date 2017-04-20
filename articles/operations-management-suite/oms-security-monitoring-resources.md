<properties
   pageTitle="Resources voor controle in bewerkingen Management Suite beveiligings- en controle oplossing | Microsoft Azure"
   description="In dit document helpt u bij het gebruiken van OMS beveiliging en controle mogelijkheden voor het controleren van uw resources en beveiligingskwesties identificeren."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Bronnen in bewerkingen Management Suite beveiligings- en controle oplossing bewaken

Dit document kunt u OMS beveiligings- en controle mogelijkheden gebruiken om te controleren van uw resources en beveiligingskwesties identificeren.

## <a name="what-is-oms"></a>Wat is OMS?

Microsoft bewerkingen Management Suite Kantoorbeheersysteem is van Microsoft cloud op basis van IT-oplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud infrastructuur. Lees het artikel [Bewerkingen Management Suite](https://technet.microsoft.com/library/mt484091.aspx)voor meer informatie over OMS.

## <a name="monitoring-resources"></a>Controle van resources

Wanneer is het mogelijk, dat u wilt voorkomen dat-incidenten probleem in eerste instantie worden optreedt. Het is echter niet mogelijk om te voorkomen dat alle-incidenten. Wanneer een beveiligingsincident gebeurt, moet u om ervoor te zorgen dat het effect ervan is geminimaliseerd.  Er zijn drie kritieke suggesties die kunnen worden gebruikt om het nummer en de invloed van beveiligingsincidenten minimaliseren:

- Regelmatig beoordelen problemen in uw omgeving.
- Controleer regelmatig alle computersystemen en netwerkapparaten om ervoor te zorgen dat van de meest recente patches is geïnstalleerd zijn.
- Controleer regelmatig alle logboeken en logboekregistratie regelingen, inclusief gebeurtenislogboeken besturingssysteem, toepassingen en inbraakdetectiesysteem.

OMS beveiligings- en controle oplossing kunnen IT om te controleren actief alle bronnen, zodat minimaliseren de invloed die beveiligingsincidenten. Heeft beveiligingsdomeinen die kunnen worden gebruikt voor het controleren van resources OMS beveiligings- en controle. De beveiligingsdomeinen biedt snel toegang tot een opties voor het beveiliging controleren van de volgende domeinen worden behandeld in daarna nog meer informatie:

- Malware assessment
- Assessment bijwerken
- Identiteits- en Access

> [AZURE.NOTE] Lees meer [aan de slag met bewerkingen Management Suite beveiligings- en controle oplossing](oms-security-getting-started.md)voor een overzicht van alle deze opties.

### <a name="monitoring-system-protection"></a>Monitoring systeembeveiliging

In een bescherming bij benadering van de diepteas is elke laag van beveiliging belangrijk voor de algemene beveiligingsstatus van uw activa. Computers met gevonden threats en computers met onvoldoende beveiliging worden weergegeven in de tegel Malware Assessment onder beveiligingsdomeinen. Met behulp van de informatie op de schadelijke software beoordeling, kunt u een abonnement beveiliging toepassen op de servers die deze nodig hebt identificeren. Toegang tot deze optie Volg de onderstaande stappen:

1. Klik op **beveiliging en controle** -tegel in het **Microsoft bewerkingen Management Suite** belangrijkste dashboard.

    ![Beveiligings- en controle](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Klik op **Antimalware Assessment** onder **Beveiliging Domains**in het dashboard **beveiligings- en controle** . Het dashboard **Antimalware Assessment** wordt weergegeven, zoals hieronder wordt weergegeven:

![Malware assessment](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

U kunt het dashboard **Malware Assessment** gebruiken om aan te geven van de volgende beveiligingskwesties:

- **Actieve threats**: computers die zijn is gehackt en actieve threats in het systeem hebben.
- **Remediated threats**: computers waarop is gehackt zijn, maar de threats zijn verholpen.
- **Handtekening verouderd**: computers waarop beveiliging tegen malware ingeschakeld, maar de handtekening is verlopen.
- **Geen realtime-beveiliging**: computers waarop niet is geïnstalleerd antimalware.

### <a name="monitoring-updates"></a>Updates voor controle 

Veiligheidsoverwegingen toepassen van de meest recente beveiligingsupdates is en deze moet worden opgenomen in uw strategie bijwerken. Cmdlets voor controle Agent van Microsoft-service (HealthService.exe) leest bijwerkgegevens van gecontroleerde computers en stuurt vervolgens deze bijgewerkte gegevens naar de OMS-service in de cloud voor verwerking. De service Microsoft Monitoring Agent is geconfigureerd als een automatische service en deze altijd moet worden uitgevoerd in de doel-pc.

![updates voor controle](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logica wordt toegepast op de updategegevens en de gegevens voor de cloudservice-records. Als ontbrekende updates worden gevonden, wordt deze worden weergegeven op het dashboard **Updates** . U kunt het dashboard **Updates** gebruiken om te werken met ontbrekende updates en ontwikkel een plan toepassen op de servers die ze nodig hebt. Volg de onderstaande stappen om toegang tot het dashboard **Updates** :

1. Klik op **beveiliging en controle** -tegel in het **Microsoft bewerkingen Management Suite** belangrijkste dashboard.
2. Klik op **Update Assessment** onder **Beveiliging Domains**in het dashboard **beveiligings- en controle** . Het Update-dashboard wordt weergegeven, zoals hieronder wordt weergegeven:

![assessment bijwerken](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

In dit dashboard kunt u een beoordeling bijwerken als u meer informatie over de huidige status van uw computers en zijn gerelateerd aan de belangrijkste threats wilt uitvoeren. Met behulp van de tegel **Kritiek of beveiligingsupdates** kunnen IT-beheerders gedetailleerde gegevens over de updates die verdwenen zijn zoals hieronder weer te geven:

![zoekresultaat](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Dit rapport kritieke gegevens bevatten die kan worden gebruikt om het aangegeven welk type bedreiging dit systeem vatbaar voor, waaronder de Microsoft KB-artikelen die is gekoppeld aan de beveiligingsupdate en het MS-Bulletin met meer informatie over het beveiligingsprobleem is.

### <a name="monitoring-identity-and-access"></a>Cmdlets voor controle identiteits- en access

Met gebruikers die werken vanaf elke locatie, met verschillende apparaten en toegang tot een enorme hoeveelheid cloud en on-premises apps, is het noodzakelijk dat hun referenties zijn beveiligd. Referentie diefstal aanvallen zijn waarin onbevoegden in eerste instantie toegang tot de referenties van een gewone gebruiker krijgt voor toegang tot een systeem binnen het netwerk. In veel gevallen deze aanvankelijke aanval is alleen een manier om toegang tot het netwerk, het uiteindelijke doel is om te ontdekken bevoegdheidsniveau accounts. 

Hackers blijven aanwezig in het netwerk, met gratis gereedschap referenties extraheren uit de sessies van andere accounts aangemeld. Afhankelijk van de systeemconfiguratie, kunnen deze referenties worden geëxtraheerd in de vorm van hashes, tickets of zelfs leesbare wachtwoorden.  

> [AZURE.NOTE] machines die rechtstreeks zijn blootgesteld aan Internet zien groot aantal mislukte pogingen die wil aanmelden met alle soort bekende gebruikersnamen (bijvoorbeeld beheerder). In de meeste gevallen is het OK als de bekende gebruikersnamen niet worden gebruikt en als het wachtwoord sterk genoeg is.

Is het mogelijk deze indringers identificeren voordat ze een account met bevoegdheden manipuleren. U kunt gebruikmaken van **OMS beveiligings- en controle oplossing** om de identiteit en toegang te houden. Volg de onderstaande stappen om toegang tot het dashboard **identiteits- en Access** :

1. Klik in het **Microsoft bewerkingen Management Suite** belangrijkste dashboard op beveiliging en controle tegel.
2. Klik op **identiteit en toegang** onder **Beveiliging Domains**in het dashboard **beveiligings- en controle** . Het dashboard **identiteits- en Access** wordt weergegeven, zoals hieronder wordt weergegeven:

![identiteits- en access](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Als onderdeel van uw strategie voor het controleren van gewone, moet u opnemen identiteit bewaken. IT-beheerder ziet er als er een specifieke geldige gebruikersnaam die veel pogingen heeft. Dit mogelijk aangeven beide aanvaller die de reële gebruikersnaam opgehaald en probeer te felle of een automatische hulpmiddel dat wordt gebruikt hard gecodeerde wachtwoord dat is verlopen.

In dit dashboard inschakelen IT snel identificeren van mogelijke threats die betrekking hebben op de identiteit en toegang tot bedrijfsresources. Bepaalde is belangrijk ook mogelijke trends identificeren, bijvoorbeeld de tegel aanmeldingen verloop van tijd, kunt u zien over tijdsperiode hoe vaak een mislukte aanmeldingspoging is uitgevoerd. In dit geval ontvangen de computer **bestandsserver** 35 aanmeldingen. U vindt meer informatie over deze computer door erop te klikken. 

![meer informatie](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Het rapport gegenereerd voor deze computer Hiermee waardevol meer informatie over dit patroon. Zoals gezien, dat de kolom **ACCOUNT** u het gebruikersaccount dat is gebruikt kunt voor toegang tot het systeem, de kolom **TIMEGENERATED** geeft u het tijdsinterval waarin de poging werden geëffend en de kolom **LOGONTYPENAME** wordt de locatie waar deze poging werden geëffend. Als deze pogingen zijn lokaal in het systeem uitgevoerd door een programma, zou de kolom **proces** van het proces naam worden weergegeven. Beschikbaar op de procesnaam van het hebt u al in scenario's waarin de aanmeldingspoging afkomstig zijn uit een programma, en nu kunt u uitvoeren verder onderzoek in het doelsysteem.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd het gebruik van OMS beveiliging en controle oplossing om te controleren uw bronnen. Meer informatie over OMS beveiliging, raadpleegt u de volgende artikelen:

- [Overzicht van de Management Suite Kantoorbeheersysteem bewerkingen](operations-management-suite-overview.md)
- [Aan de slag met bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-getting-started.md)
- [Cmdlets voor controle en reageren op beveiligingsmeldingen in bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-responding-alerts.md)