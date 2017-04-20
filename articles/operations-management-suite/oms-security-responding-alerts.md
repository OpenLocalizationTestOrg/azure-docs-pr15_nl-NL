<properties
   pageTitle="Cmdlets voor controle en reageren op beveiligingsmeldingen in bewerkingen Management Suite beveiligings- en controle oplossing | Microsoft Azure"
   description="In dit document helpt u bij het gebruik van de bedreiging intelligence optie die beschikbaar zijn in OMS beveiligings- en controle bewaken en reageren op beveiligingsmeldingen."
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

# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Cmdlets voor controle en reageren op beveiligingswaarschuwingen in bewerkingen Management Suite beveiligings- en controle-oplossing

In dit document kunt u via de optie beschikbaar in OMS beveiligings- en controle bedrijfsinformatie van bedreiging bewaken en reageren op beveiligingsmeldingen.

## <a name="what-is-oms"></a>Wat is OMS?

Microsoft bewerkingen Management Suite Kantoorbeheersysteem is van Microsoft cloud op basis van IT-oplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud infrastructuur. Lees het artikel [Bewerkingen Management Suite](https://technet.microsoft.com/library/mt484091.aspx)voor meer informatie over OMS.

## <a name="threat-intelligence"></a>Bedreiging bedrijfsinformatie

In een bedrijfsomgeving waar gebruikers globaal toegang hebt tot het netwerk en allerlei apparaten met verbinding maken met zakelijke gegevens, is het noodzakelijk die u kunt uw resources actief controleren en snel op beveiligingsincidenten reageren. Dit is name belangrijk vanuit het perspectief van de levenscyclus van de beveiliging omdat sommige cybersecurity threats kunnen niet verhogen meldingen of verdachte activiteiten die door de technische besturingselementen traditionele beveiliging kunnen worden geïdentificeerd. 

Met behulp van de optie **Bedreiging Intelligence** beschikbaar in OMS beveiligings- en controle, kunnen IT-beheerders beveiligingsrisico's ten opzichte van de omgeving, bijvoorbeeld identificeren, bepalen of een bepaalde computer maakt deel uit van een [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Computers kunnen worden omgezet in knooppunten in een botnet wanneer hackers installeert vastlegt schadelijke software die deze computer heimelijk met de opdracht en het besturingselement verbindt. Dit kan ook worden mogelijke threats die afkomstig zijn uit ondergrondse communicatiekanalen, zoals [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents)aangegeven. 

Gebruik gegevens die afkomstig zijn uit meerdere bronnen in Microsoft om te kunnen deze intelligence bedreiging samenstelt, OMS beveiligings- en controle. OMS beveiligings- en controle wordt gebruikmaken van deze gegevens om te identificeren mogelijke threats ten opzichte van uw omgeving.

Het deelvenster bedreiging Intelligence bestaat door de drie belangrijkste opties:
- Servers met uitgaande schadelijk verkeer
- Gevonden threats typen
- Bedreiging intelligence kaart

> [AZURE.NOTE] Lees meer [aan de slag met bewerkingen Management Suite beveiligings- en controle oplossing](oms-security-getting-started.md)voor een overzicht van alle deze opties.

### <a name="responding-to-security-alerts"></a>Reageren op beveiligingsmeldingen

Een van de stappen van een proces [incident antwoord beveiliging](https://technet.microsoft.com/library/cc512623.aspx) is de ernst van de systemen compromissen identificeren. In deze fase moet u de volgende taken uitvoeren:

- De aard van de aanval bepalen
- Bepaalt het punt aanval van oorsprong
- Hiermee bepaalt u het doel van de aanval. De aanval speciaal op uw organisatie zijn gericht aan te schaffen specifieke informatie is of is deze willekeurig?
- De systemen die is geknoeid identificeren
- De bestanden die zijn geopend en te bepalen de gevoeligheid van deze bestanden identificeren

U kunt gebruikmaken van **Bedreiging Intelligence** -gegevens in OMS beveiligings- en controle oplossing om u te helpen met de volgende taken kunnen uitvoeren. Volg de onderstaande stappen om toegang tot deze **Bedreiging Intelligence** -opties:

1. Klik op **beveiliging en controle** -tegel in het **Microsoft bewerkingen Management Suite** belangrijkste dashboard.

    ![Beveiligings- en controle](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. In het dashboard **beveiligings- en controle** ziet u de opties **Bedreiging bedrijfsinformatie** in rechts, zoals hieronder wordt weergegeven:

    ![Bedreiging intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Deze drie tegels krijgt u een overzicht van de huidige threats. In de **Server met uitgaande schadelijk verkeer** die is mogelijk om te bepalen of er een computer die u controleren is wilt (binnen of buiten uw netwerk) stuurt die schadelijk verkeer met Internet. 

De tegel **gedetecteerd bedreiging typen** wordt een overzicht van de risico's die huidige 'in de natuur zijn", als u klikt op op deze tegel ziet u meer informatie over deze threats zoals u hierna ziet:

![Gevonden bedreiging typen](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

U kunt meer informatie over elke bedreiging ophalen door erop te klikken. Het volgende voorbeeld ziet u meer informatie over Botnet:

![meer informatie over een bedreiging](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Zoals is beschreven in het begin van deze sectie, is deze informatie met name handig tijdens een incident antwoord hoofdletters/kleine letters. Dit kan ook worden belangrijke tijdens een forensisch onderzoek waar u wilt berekenen van de bron van de aanval, welke systeem is is gehackt en de tijdlijn. In dit rapport dat u enkele belangrijke informatie over de aanval, zoals gemakkelijk kunt herkennen: de bron van de aanval, de lokaal IP-dat is beschadigd en de status van de huidige sessie van de verbinding. 

De **bedreiging intelligence kaart** kunt u de huidige locaties overal ter wereld die schadelijk verkeer hebben. Er zijn oranje (binnenkomende) en rode (uitgaand) pijlen in deze map die de richting verkeer bepalen of u in een van deze pijlen klikt, wordt dit het type bedreiging en de richting verkeer zoals hieronder weergegeven:

![bedreiging intel-kaart](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [AZURE.NOTE] U kunt een demonstratie zien over het gebruik van deze mogelijkheid tijdens een incident antwoord proces door te bekijken van de presentatie [Mitigate datacenter beveiligingsrisico's met begeleide onderzoek met bewerkingen Management Suite](https://myignite.microsoft.com/videos/5000) bezorgd bij Microsoft Ignite.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe u de optie **Bedreiging bedrijfsinformatie** in OMS beveiligings- en controle oplossing gebruikt om te reageren op beveiligingsmeldingen. Meer informatie over OMS beveiliging, raadpleegt u de volgende artikelen:

- [Overzicht van de Management Suite Kantoorbeheersysteem bewerkingen](operations-management-suite-overview.md)
- [Aan de slag met bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-getting-started.md)
- [Resources voor controle in bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-monitoring-resources.md)
