<properties
    pageTitle="Active Directory herhaling Status oplossing in Log Analytics | Microsoft Azure"
    description="Het Active Directory herhaling Status oplossing taalpakket regelmatig uw Active Directory-omgeving voor eventuele fouten herhaling bewaakt en de resultaten op uw dashboard OMS-rapporten."
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

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Active Directory herhaling Status oplossing in Log Analytics

Active Directory is een belangrijk onderdeel van een IT-omgeving in het bedrijf. Beschikbaarheid en hoge prestaties te garanderen, heeft elke domeincontroller een eigen kopie van de Active Directory-database. Domeincontrollers gerepliceerd met elkaar om te kunnen wijzigingen doorvoeren in de organisatie. Fouten in dit proces herhaling kunnen leiden tot van tal van problemen in de organisatie.

Het pakket van de oplossing AD herhaling Status regelmatig uw Active Directory-omgeving voor eventuele fouten herhaling bewaakt en de resultaten op uw dashboard OMS-rapporten.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

- Agenten moeten zijn geïnstalleerd op domeincontrollers die deel uitmaken van het domein dat moet worden geëvalueerd of op lidservers is geconfigureerd voor het verzenden van AD herhaling gegevens naar OMS. Als u wilt weten over het koppelen van Windows-computers aan OMS, raadpleegt u [verbinding maken met Windows-computers Log Analytics](log-analytics-windows-agents.md). Als uw domeincontroller al deel van een systeem Center Operations Manager-omgeving die u wilt verbinden met OMS uitmaakt, raadpleegt u [Verbinding maken met Operations Manager naar Log Analytics](log-analytics-om-agents.md).
- De Status van Active Directory-replicatie-oplossing toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie vereist.


## <a name="ad-replication-status-data-collection-details"></a>AD herhaling Status detailgegevens siteverzameling

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor AD herhaling Status.

| platform | Directe Agent | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Ja](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Nee](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Nee](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Ja](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| elke 5 dagen|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>(Optioneel) een niet-domeincontroller AD-gegevens te verzenden naar OMS inschakelen
Als u niet verbinden met een domeincontroller rechtstreeks OMS wilt, kunt u elke andere OMS verbonden computer in uw domein gebruiken voor het verzamelen van gegevens voor het pakket van de oplossing AD herhaling Status en het verzenden van de gegevens hebt.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Een niet-domeincontroller AD-gegevens te verzenden naar OMS inschakelen
1.  Controleer of de computer is een lid van het domein dat u wilt controleren met de Status van AD-replicatie-oplossing.
2.  [Verbinding maken met het Windows-computer naar OMS](log-analytics-windows-agents.md) of [verbinding maken met uw bestaande Operations Manager-omgeving wilt OMS](log-analytics-om-agents.md), als dit nog niet is aangesloten.
3.  Stel op deze computer, de volgende registersleutel:
    - Sleutel: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management groepen\<ManagementGroupName > \Solutions\ADReplication**
    - Value: **IsTarge**
    - **Waar** Waardegegevens:

    >[AZURE.NOTE]Deze wijzigingen worden pas effect opnieuw te starten de service Microsoft Monitoring Agent (HealthService.exe).

## <a name="understanding-replication-errors"></a>Lidmaatschap herhaling fouten
Nadat u verzonden naar OMS replicatie-statusgegevens voor AD hebt, ziet u een tegel ongeveer als volgt uit op het Kantoorbeheersysteem dashboard dat aangeeft hoeveel replicatiefouten die u momenteel hebt.  
![AD herhaling Status tegel gemarkeerd](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritieke herhaling fouten** zijn die tegen of boven 75% van de [tombstone-levensduur](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) van uw Active Directory-bos.

Wanneer u op de tegel hebt geklikt, ziet u meer informatie over de fouten.
![AD herhaling Status dashboard](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>De Status van de bestemming-Server en de Status van de bron-Server
Deze bladen weergeven de status van de bestemming en bronservers die problemen met replicatie zijn. Het nummer na elke naam van de domeincontroller geeft het aantal herhaling fouten op die domeincontroller.

De fouten voor zowel de doelserver als de bronservers worden weergegeven omdat er zijn enkele problemen eenvoudiger om op te lossen vanuit het perspectief van de server bron en anderen vanuit het perspectief van de server bestemming.

In dit voorbeeld ziet u dat veel doelserver heb ongeveer hetzelfde aantal fouten, maar er is een bronserver (ADDC35) met veel meer fouten dan alle andere. Is het mogelijk dat er is een probleem op ADDC35 dat wordt veroorzaakt door deze gegevens te verzenden naar haar replicatiepartners is mislukt. De problemen op te lossen op ADDC35 worden waarschijnlijk opgelost veel van de fouten die worden weergegeven in het blad bestemming-server.

### <a name="replication-error-types"></a>Replicatie fouttypen
Deze blade vindt u informatie over de verschillende typen fouten in uw onderneming. Elke fout heeft een unieke numerieke code en een bericht dat u de hoofdmap van de fout oorzaak kan helpen.

De ring aan de bovenkant krijgt u een idee van welke fouten worden weergegeven informatie en minder vaak in uw omgeving.

Dit kunt u zien wanneer domein controller dezelfde herhaling fout optreden. In dit geval u mogelijk om te ontdekken aanduiden van een oplossing op één domeincontroller en klik op andere domeincontrollers beïnvloed door het probleem wilt herhalen.

### <a name="tombstone-lifetime"></a>Tombstone-levensduur
De tombstone-levensduur bepaalt hoe lang een verwijderde object, waarnaar wordt verwezen als een tombstone, blijft behouden in de Active Directory-database. Wanneer u een verwijderde object worden doorgegeven de tombstone-levensduur, wordt deze door een proces van de siteverzameling permanent automatisch verwijderd uit de Active Directory-database.

De standaard tombstone-levensduur is 180 dagen voor de meest recente versie van Windows, maar deze was 60 dagen voor oudere versies en deze expliciet kan worden gewijzigd door een Active Directory-beheerder.

Het is belangrijk te weten als u ondervindt herhaling fouten die zijn bijna is bereikt of voorbij de tombstone-levensduur. Als twee domeincontrollers een replicatiefout die zich blijft voorbij de tombstone-levensduur ondervindt voordoen, wordt replicatie uitgeschakeld tussen deze twee domeincontroller, zelfs als de onderliggende Replicatiefout is vastgezet.

Het blad Tombstone-levensduur kunt u herkennen locaties waar dit in gevaar van foutbericht gegeven is. Elke fout in de **dan 100% TSL** categorie vertegenwoordigt een partition die niet heeft gerepliceerd tussen de bron- en doeltabellen server voor ten minste de tombstone-levensduur voor de bos.

In dit geval gewoon de Replicatiefout op te lossen worden niet genoeg. Minimaal moet u handmatig onderzoeken om te identificeren en opschonen zich objecten voordat u kunnen replicatie opnieuw. Mogelijk moet u nog een domeincontroller nemen.

Naast eventuele herhaling fouten die blijven bestaan voorbij de tombstone-levensduur identificeren, ook wilt u aandacht besteden aan eventuele fouten die de categorieën **TSL 50-75%** of **75-100% TSL** bereik vallen.

Dit zijn de fouten die zijn duidelijk zich en niet kortstondige, zodat zij nodig hebben waarschijnlijk uw tussenkomst om op te lossen. Het goede nieuws is dat ze nog niet is bereikt de tombstone-levensduur. Als u deze problemen direct oplossen en *voordat* die ze de levensduur van de tombstone bereikt, herhaling met minimale tussenkomst kunt opnieuw starten.

Zoals hierboven is, bevat de tegel dashboard voor de oplossing AD herhaling Status het nummer van *kritieke* herhaling fouten in uw omgeving, die is gedefinieerd als fouten die meer dan 75% van tombstone-levensduur (met inbegrip van fouten die meer dan 100% van TSL zijn). Streven te bewaren van dit nummer bij 0.

>[AZURE.NOTE] Alle tombstone levensduur percentage berekeningen zijn gebaseerd op de werkelijke tombstone-levensduur voor uw Active Directory-bos, zodat u vertrouwen kunt dat deze percentages juist zijn, zelfs als u een aangepaste tombstone levensduur waarde instellen.

### <a name="ad-replication-status-details"></a>Meer informatie de status van de AD herhaling
Als u een item in een van de lijsten klikt, ziet u meer informatie over deze via de zoekfunctie. De resultaten zijn gefilterd, zodat alleen de fouten met betrekking tot dat item. Bijvoorbeeld als u op de eerste domeincontroller vermeld onder **Bestemming Server Status (ADDC02)**klikt, ziet u zoekresultaten gefilterd op fouten weergeven met die domeincontroller vermeld als de doelserver:

![AD herhaling status fouten in zoekresultaten](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Van hieruit kunt u verder filteren, de query wijzigen en enzovoort. Zie voor meer informatie over het gebruik van de zoekopdracht Log [Log zoekopdrachten](log-analytics-log-searches.md).

Het veld **HelpLink** ziet u de URL van een pagina TechNet met meer informatie over deze specifieke fout. U kunt kopiëren en plak deze koppeling in het browservenster om informatie over probleemoplossing en herstellen van de fout weer te geven.

U kunt ook klikken op **exporteren** om de resultaten exporteren naar Excel. Hiermee kunt u voor replicatie foutgegevens op geen enkele manier die u wilt bekijken.

![geëxporteerde AD herhaling status fouten in Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD herhaling Status Veelgestelde vragen
**V: hoe vaak is AD herhaling statusgegevens bijgewerkt?**
A: de gegevens wordt elke 5 dagen bijgewerkt.

**V: is er een manier om te configureren hoe vaak deze gegevens worden bijgewerkt?**
A: niet op dit moment.

**V: moet ik mijn domeincontrollers toevoegen aan mijn werkruimte OMS om te zien herhaling status?**
A: Nee, kan slechts één domeincontroller moet worden toegevoegd. Als u uw werkruimte OMS controller domein hebt, wordt gegevens uit alle labels verzonden naar OMS.

**V: ik wil niet alle domeincontrollers toevoegen aan mijn OMS-werkruimte. Kan ik de oplossing AD herhaling Status nog steeds gebruiken?**
A: Ja. U kunt de waarde van een registersleutel deze instellen. Zie [een niet - domeincontroller aan AD gegevens verzenden naar OMS inschakelen](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**V: Wat is de naam van het proces dat het verzamelen van gegevens wordt?**
A: AdvisorAssessment.exe

**V: hoe lang duurt het voor de gegevens te verzamelen?**
A: gegevens verzamelen tijd is afhankelijk van de grootte van de omgeving voor Active Directory, maar het duurt meestal minder dan 15 minuten.

**V: welk type gegevens worden verzameld?**
A: herhaling gegevens worden verzameld via LDAP.

**V: is er een manier om te configureren als gegevens worden verzameld?**
A: niet op dit moment.

**V: welke machtigingen heb ik nodig om gegevens te verzamelen?**
A: normale gebruikersmachtigingen met Active Directory zijn meestal voldoende.

## <a name="troubleshoot-data-collection-problems"></a>Gegevens verzamelen problemen oplossen
Het verzamelen van gegevens, moet het pakket van de oplossing AD herhaling Status ten minste één domeincontroller niet worden verbonden met uw werkruimte OMS. Totdat u dat doet, ziet u een bericht aangegeven dat **nog steeds gegevens worden verzameld**.

Als u hulp verbinden van een van uw domein nodig hebt, kunt u documentatie bij [verbinding maken met Windows-computers Log Analytics](log-analytics-windows-agents.md)weergeven. U kunt ook als uw domeincontroller al is verbonden met een bestaande systeem Center Operations Manager-omgeving, kunt u documentatie weergeven op [Verbinding maken met System Center Operations Manager naar Log Analytics](log-analytics-om-agents.md).

Als u niet dat een domeincontroller verbinding rechtstreeks met OMS of SCOM wilt, Zie [inschakelen van een niet - domeincontroller AD-gegevens naar OMS te verzenden](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Volgende stappen

- Gebruik [zoekopdrachten in het logboek in Log Analytics](log-analytics-log-searches.md) gedetailleerde Active Directory-replicatie statusgegevens moeten worden weergegeven.
