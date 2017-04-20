<properties
   pageTitle="Aan de slag met bewerkingen Management Suite beveiligings- en controle oplossing | Microsoft Azure"
   description="In dit document helpt u bij aan de slag met bewerkingen Management Suite beveiligings- en controle oplossingsmogelijkheden om uw hybride cloud de te houden."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="get-started-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>
 
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Aan de slag met bewerkingen Management Suite beveiligings- en controle-oplossing
In dit document, kunt u snel aan de slag met bewerkingen Management Suite (OMS) beveiliging en controle oplossingsmogelijkheden doordat die u door elke optie.

## <a name="what-is-oms"></a>Wat is OMS?
Microsoft bewerkingen Management Suite Kantoorbeheersysteem is Microsofts cloudgebaseerde IT-oplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud infrastructuur. Lees het artikel [Bewerkingen Management Suite](https://technet.microsoft.com/library/mt484091.aspx)voor meer informatie over OMS.

## <a name="oms-security-and-audit-dashboard"></a>OMS beveiligings- en controle dashboard

De oplossing OMS beveiligings- en controle biedt een uitgebreide weergave in van uw organisatie IT beveiliging houding met ingebouwde zoekquery's voor aantal aanzienlijke problemen die uw aandacht nodig. Het dashboard **beveiligings- en controle** is het beginscherm voor alles met betrekking tot de beveiligingsfuncties in OMS. Deze biedt uitgebreide inzicht in de beveiligingsstatus van uw computers. Het bevat ook de mogelijkheid om alle gebeurtenissen in de afgelopen 24 uur, 7 dagen, of een andere aangepaste tijdsbestek te bekijken. Voor toegang tot het dashboard **beveiligings- en controle** , als volgt te werk:

1. Klik op **Instellingen** -tegel in links in het **Microsoft bewerkingen Management Suite** belangrijkste dashboard.
2. Klik in het blad **Instellingen** onder **oplossingen** op de optie **beveiliging en controle** .
3. Het dashboard **beveiligings- en controle** worden weergegeven:

    ![OMS beveiligings- en controle dashboard](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Als u toegang krijgt dit dashboard voor het eerst tot en u niet gecontroleerd door OMS apparaten hoeft, wordt de tegels niet worden gevuld met gegevens uit de agent. Als u de agent zijn geïnstalleerd, kan het enige tijd om te vullen duren, dus wat u te zien in eerste instantie mogelijk ontbreken er bepaalde gegevens zoals ze zijn nog steeds uploaden naar de cloud.  In dit geval is het normale om enkele tegels zonder concrete informatie weer te geven. [Verbinding maken met Windows-computers rechtstreeks OMS](https://technet.microsoft.com/library/mt484108.aspx) Lees voor meer informatie over het installeren van OMS agent in een Windows-systeem en de [verbinding Linux computers OMS](https://technet.microsoft.com/library/mt622052.aspx) voor meer informatie over hoe u deze taak uitvoert in een Linux-systeem.

> [AZURE.NOTE] De agent verzamelen de gegevens op basis van de huidige gebeurtenissen die zijn ingeschakeld, bijvoorbeeld de computernaam van de, IP-adres en de gebruiker. Echter geen/documentbestanden, de databasenaam of de persoonlijke gegevens zijn verzameld.   

Oplossingen zijn een verzameling gegevens, visualisatie en logica acquisition regels die belangrijke klant uitdagingen. Beveiligings- en controle is één oplossing, anderen afzonderlijk kunnen worden toegevoegd. Lees het artikel [toevoegen oplossingen](https://technet.microsoft.com/library/mt674635.aspx) voor meer informatie over het toevoegen van een nieuwe oplossing.

Het dashboard OMS beveiligings- en controle is ingedeeld in vier belangrijke categorieën:

- **Beveiligingsdomeinen**: in dit gedeelte is mogelijk naar verder verkennen beveiliging records na verloop van tijd, toegang tot malware assessment, assessment, netwerkbeveiliging, informatie over identiteit en toegang, computers met beveiligingsgebeurtenissen bijwerken en snel toegang hebben tot Azure Beveiligingscentrum dashboard.
- **Aantal aanzienlijke problemen**: met deze optie kunt u snel identificeren van het aantal actieve kwesties en de ernst van deze problemen.
- **Detectie (Preview)**: Hiermee kunt u patronen aanval op beveiligingsmeldingen zomersportevenementen als zij ten opzichte van uw resources plaatsvinden identificeren.
- **Bedreiging Intelligence**: aanval patronen identificeren door het totale aantal servers met uitgaande schadelijk IP-verkeer, het type schadelijke bedreiging en een map die waar deze IP-adressen vandaan visualiseren. 
- **Algemene waardepapier query's**: deze optie biedt u een lijst met de meest voorkomende waardepapier query's die u gebruiken kunt om te controleren van uw omgeving. Wanneer u in een van deze query's klikt, wordt het blad **Zoeken** met de resultaten voor die query geopend.

> [AZURE.NOTE] Lees dat hoe OMS beveiligt uw gegevens voor meer informatie over hoe OMS uw gegevens veilig houdt.

## <a name="security-domains"></a>Beveiligingsdomeinen

Als u resources controleren, is het belangrijk om te kunnen voor snelle toegang tot de huidige status van uw omgeving. Het is echter ook belangrijk moeten kunnen bijhouden achterste gebeurtenissen die hebben plaatsgevonden in het verleden dat leiden kan tot een beter begrip van wat in uw omgeving op een bepaald moment in tijd gebeurt er. 

> [AZURE.NOTE] gegevensretentie is volgens de OMS prijzen van abonnement. Ga naar de [Microsoft bewerkingen Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) prijzen van de pagina voor meer informatie.

Incident antwoord en forensics onderzoek-scenario's profiteren rechtstreeks van de resultaten die beschikbaar zijn in de tegel **Beveiliging Records na verloop van tijd** .

![Beveiliging records na verloop van tijd](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Wanneer u op deze tegel, klikt u op het blad **Zoeken** wordt geopend, met de resultaten van een query voor **Beveiligingsgebeurtenissen** (Type = SecurityEvents) met gegevens op basis van de afgelopen zeven dagen, zoals hieronder wordt weergegeven:

![Beveiliging records na verloop van tijd](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Het zoekresultaat is verdeeld in twee deelvensters: het linkerdeelvenster biedt u een overzicht van het aantal beveiligingsgebeurtenissen die zijn gevonden, de computers waarin deze gebeurtenissen zijn gevonden, het aantal accounts die zijn gevonden in deze computers en de soorten activiteiten. Het rechterdeelvenster vindt u de totale resultaten en een chronologische weergave van de beveiligingsgebeurtenissen met de naam en gebeurtenis activiteit van de computer. U kunt ook klikken op **Meer weergeven** om meer informatie over deze gebeurtenis, zoals gegevens van de gebeurtenis, de ID van de gebeurtenis en de gebeurtenisbron weer te geven.

> [AZURE.NOTE] Lees de [OMS verwijzing zoeken](https://technet.microsoft.com/library/mt450427.aspx)voor meer informatie over OMS zoekquery's.

### <a name="antimalware-assessment"></a>Antimalware assessment

Deze optie kunt u snel identificeren van computers met onvoldoende beveiliging en computers waarop zijn is gehackt door een deel van een schadelijke software. Malware assessment status en gevonden threats op de gecontroleerde servers worden opgelezen en klikt u vervolgens de gegevens wordt verzonden naar de OMS-service in de cloud voor verwerking. Servers met gedetecteerd threats en servers met onvoldoende beveiliging vindt u in het dashboard assessment schadelijke software, die toegankelijk is wanneer u op in de tegel **Antimalware Assessment** . 

![malware assessment](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Net als een andere live-tegel beschikbaar in OMS Dashboard, dat wanneer u erop, klikt u op het blad **Zoeken** wordt geopend met het queryresultaat. Voor deze optie als u in de optie **Niet worden gerapporteerd** onder **Beveiliging Status klikt**, hebt u het queryresultaat waarin deze enkelvoudige invoer met de naam van de computer en de rangschikking, zoals hieronder wordt weergegeven:

![zoekresultaat](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [AZURE.NOTE] *rang* is een cijfer geeft aan de status van de beveiliging (op, uitgeschakeld bijgewerkt, enzovoort) en's die zijn gevonden. Ondervindt die als een getal kan helpen u aggregaties.

Als u op in de naam van de computer, hebt u de chronologische weergave van de beveiligingsstatus van deze computer. Dit is bijzonder nuttig zijn voor scenario's in die u nodig hebt om te begrijpen als de antimalware nadat Outlook is geïnstalleerd en op een gegeven moment die is verwijderd.   

### <a name="update-assessment"></a>Assessment bijwerken 

Deze optie kunt u snel de algehele belichting aan potentiële beveiligingsproblemen, kunnen bepalen en of of hoe kritieke deze updates zijn voor uw omgeving. OMS beveiligings- en controle oplossing alleen de visualisatie van deze updates bieden, de reële gegevens afkomstig van [System Updates-oplossingen](https://technet.microsoft.com/library/mt484096.aspx), dat wil een verschillende module binnen OMS zeggen. Een voorbeeld van de updates als volgt:

![systeemupdates](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [AZURE.NOTE] Lees de [servers met de oplossing systeemupdates bijwerken](https://technet.microsoft.com/library/mt484096.aspx)voor meer informatie over Updates oplossing.

### <a name="identity-and-access"></a>Identiteits- en Access

Identiteit moet het besturingselement vlak voor uw onderneming, uw identiteit beschermen moet worden de hoogste prioriteit. Terwijl in het verleden verbindingen rond organisaties zijn en deze verbindingen een van de primaire defensief grenzen zijn, wordt tegenwoordig met meer gegevens en meer apps verplaatsen naar de cloud de identiteit de nieuwe omtrek. 

> [AZURE.NOTE] op dit moment de gegevens zijn gebaseerd op beveiligingsgebeurtenissen login gegevens (gebeurtenis-ID 4624) in de toekomstige Office365-aanmeldingen en Azure AD-gegevens ook worden opgenomen.

Door controleren of uw identiteit activiteiten is mogelijk proactief acties uitvoeren voordat een incident duurt locatie of reactieve acties een poging aanval stoppen. Het dashboard **identiteits- en toegangsbeheer** wordt een overzicht gegeven van de status van uw identiteit, inclusief het aantal mislukte pogingen aan te melden, account van de gebruiker die zijn gebruikt tijdens deze pogingen, accounts die zijn uitgesloten, accounts met gewijzigd of opnieuw wachtwoord en momenteel aantal accounts die zijn aangemeld. 

Wanneer u in de tegel **identiteits- en toegangsbeheer klikt** ziet u het volgende dashboard:

![identiteits- en access](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

De informatie in dit dashboard kunt, onmiddellijk helpen u aan te geven van een mogelijke verdachte activiteiten. Er zijn bijvoorbeeld 338 pogingen aan te melden als **beheerder** en 100% van de volgende pogingen is mislukt. Dit kan worden veroorzaakt door een brutekrachtaanval ten opzichte van dit account. Als u op dit account klikt, verkrijgt u meer informatie die u helpen kan bij het bepalen van de resource doel voor deze mogelijke aanval:

![zoekresultaten](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

De gedetailleerde lijst bevat belangrijke informatie over deze gebeurtenis, met inbegrip van: de doelcomputer, het type aanmelden (in dit geval aanmelding bij het netwerk), de activiteit (in dit geval gebeurtenis 4625) en een uitgebreide tijdlijn van elke poging. 

### <a name="computers"></a>Computers

Deze tegel kan worden gebruikt voor toegang tot alle computers met actief beveiligingsgebeurtenissen. Wanneer u op deze tegel klikt ziet u de lijst met computers met beveiligingsgebeurtenissen en het aantal gebeurtenissen op elke computer:

![Computers](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

U kunt doorgaan met uw onderzoek door te klikken op elke computer en controleer de beveiligingsgebeurtenissen die zijn gemarkeerd.

### <a name="azure-security-center"></a>Azure Beveiligingscentrum

Deze tegel is in feite een snelkoppeling voor toegang tot Azure Beveiligingscentrum dashboard. Lees meer [aan de slag met Azure Beveiligingscentrum](../security-center/security-center-get-started.md) voor meer informatie over deze oplossing.

## <a name="notable-issues"></a>Aantal aanzienlijke problemen

De hoofddoelstelling van deze groep met opties is bedoeld als een snelle weergave van de problemen die u in uw omgeving, hebt door ze te categoriseren in kritiek, waarschuwing en ter informatie. De actieve kwestie type tegel is een visualisatie van deze problemen, maar deze kunt niet u meer informatie over deze verkennen voor dat u het onderste gedeelte van deze tegel met de naam van het probleem (naam moet), hoeveel objecten had dit gebeurt (aantal) en hoe kritieke is (ERNST) gebruiken.

![Aantal aanzienlijke problemen](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

U kunt zien dat deze problemen zijn gedekt in andere gebieden van de groep **Beveiligingsdomeinen** , die de bedoeling deze weergave versterkt: visualiseren van de belangrijkste punten in uw omgeving vanaf één plek.

## <a name="detections-preview"></a>Detectie (Preview)

De hoofddoelstelling van deze optie is toe te staan dat IT snel identificeren van mogelijke threats naar hun omgeving via en de ernst van deze bedreiging.

![Bedreiging Intel](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Deze optie kunt ook tijdens een incident antwoord onderzoek worden gebruikt om de assessment en meer informatie over de aanval verkrijgen.

> [AZURE.NOTE] Bekijk [hoe om te profiteren van de Azure Beveiligingscentrum & Microsoft bewerkingen Management Suite voor een Incident antwoord wordt verzonden](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703)voor meer informatie over het gebruik van OMS voor Incident antwoord.

## <a name="threat-intelligence"></a>Bedreiging bedrijfsinformatie

Het nieuwe bedreiging intelligence gedeelte van de oplossing beveiligings- en controle gevisualiseerd de patronen mogelijke aanval op verschillende manieren: het totale aantal servers met uitgaande schadelijk IP-verkeer, het type schadelijke bedreiging en een map die waar deze IP-adressen die zijn afkomstig zijn uit. U kunt werken met de kaart en klik op het IP-adressen voor meer informatie.

Gele punaises op de kaart geven binnenkomende verkeer van schadelijke IP-adressen. Het is niet ongebruikelijk servers die zijn blootgesteld aan internet om binnenkomende schadelijk verkeer weer te geven, maar het is raadzaam te controleren van de volgende pogingen om ervoor te zorgen dat geen van deze is geslaagd. Deze indicatoren zijn gebaseerd op IIS-logboeken, WireData en logboeken van Windows Firewall.  

![Bedreiging Intel](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Algemene waardepapier query 's

De lijst met algemene beveiliging-query's beschikbaar zijn handig voor u kunt snel gegevens weer te geven van de resource en pas deze op basis van de behoeften van uw omgeving. Deze algemene query's zijn:

- Alle beveiligingsactiviteiten
- Beveiligingsactiviteiten op de computer "computer01.contoso.com" (vervangen door de naam van uw eigen computer)
- Beveiligingsactiviteiten op de computer "computer01.contoso.com" voor account "Beheerder" (vervangen door de namen van uw eigen computer en account)
- Meld u aan de activiteit op de Computer
- Accounts die Microsoft antimalware op elke computer beëindigd
- Computers waarop het Microsoft antimalware-proces is beëindigd
- Computers waarop "hash.exe" is uitgevoerd (vervangen door de procesnaam van de verschillende)
- Alle procesnamen die zijn uitgevoerd
- Meld u aan de activiteit op Account
- Accounts die extern aangemeld op de computer "computer01.contoso.com" (vervangen door de naam van uw eigen computer)

## <a name="see-also"></a>Zie ook

In dit document, zijn u kennis met OMS beveiligings- en controle oplossing. Meer informatie over OMS beveiliging, raadpleegt u de volgende artikelen:

- [Overzicht van de Management Suite Kantoorbeheersysteem bewerkingen](operations-management-suite-overview.md)
- [Cmdlets voor controle en reageren op beveiligingsmeldingen in bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-responding-alerts.md)
- [Resources voor controle in bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-monitoring-resources.md)
