<properties
    pageTitle="Wire-oplossing in Log Analytics | Microsoft Azure"
    description="Gegevens van de kabel is samengevoegde netwerk- en prestatieproblemen gegevens uit computers met OMS agenten, inclusief Operations Manager en Windows-verbonden agenten. Netwerkgegevens wordt gecombineerd met uw logboekgegevens kunt u gegevens relateren."
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

# <a name="wire-data-solution-in-log-analytics"></a>Wire-oplossing in Log Analytics

Gegevens kabel is samengevoegde netwerk- en prestatieproblemen gegevens uit computers met OMS agenten, inclusief Operations Manager en Windows-verbonden agenten. Netwerkgegevens wordt gecombineerd met uw logboekgegevens kunt u gegevens relateren. OMS agenten installeren op computers in uw IT-infrastructuur monitor netwerkgegevens verzonden naar en van die computers voor netwerk niveaus 2-3 in het [model OSI](https://en.wikipedia.org/wiki/OSI_model) , met inbegrip van de verschillende protocollen en poorten die worden gebruikt.

>[AZURE.NOTE] De oplossing kabel is momenteel niet beschikbaar wordt toegevoegd aan de werkruimten. Klanten die al op de kabel gegevensoplossing ingeschakeld kunnen blijven gebruiken van de oplossing kabel gegevens.

Standaard verzamelt OMS logboekgegevens voor CPU, geheugen, schijf en prestaties van netwerkgegevens van items die zijn ingebouwd in Windows. Netwerk- en andere gegevens verzamelen wordt uitgevoerd in realtime voor elke agent, met inbegrip van subnetten en niveau van de webtoepassing protocollen wordt gebruikt door de computer. U kunt andere items op de pagina instellingen op het tabblad logboeken toevoegen.

Als u [sFlow](http://www.sflow.org/) of andere software met [Cisco NetFlow protocol](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html)hebt gebruikt, de statistieken en gegevens u van kabel gegevens ziet worden vertrouwd voor u.

Enkele van de soorten ingebouwde Log zoekquery's zijn:

- Agenten die kabel gegevens bevatten
- IP-adres van agenten kabel gegevens leveren
- Uitgaande communicatie op IP-adressen
- Het aantal bytes dat is verzonden door de toepassingsprotocollen
- Het aantal bytes dat is verzonden door een toepassingsservice
- Bytes ontvangen door verschillende protocollen
- Totaal aantal bytes door IP verzonden en ontvangen
- IP-adressen die met agenten op 10.0.0.0/8 subnet hebt doorgegeven
- Latentie van gemiddeld voor verbindingen die werden betrouwbaar gemeten
- Computerprocessen die gestart of netwerkverkeer ontvangen
- Hoeveelheid netwerkverkeer voor een proces

Als u het gebruik van kabel gegevens zoekt, kunt u filteren en gegevens groeperen om informatie over de bovenste agenten en de bovenste protocollen te bekijken. Of u kunt zoeken naar wanneer bepaalde computers (IP-adressen/MAC adressen) met elkaar, doorgegeven voor hoe lang en hoeveel gegevens is verzonden--in principe u metagegevens over netwerkverkeer, dat wil op basis van zoekactie zeggen weergeven.

Aangezien u metagegevens bekijkt, is het echter niet per se handig voor uitgebreidere probleemoplossing uitvoert. Gegevens van de kabel in OMS is niet een volledige opname netwerkgegevens. Dit dus niet bedoeld voor het oplossen van diepblauwe pakket niveau.
Het voordeel van het gebruik van de agent, vergeleken met andere methoden siteverzameling, is dat er geen apparaten installeren, configureren, de schakelopties voor uw netwerk of ingewikkelde configuraties uitvoeren. Gegevens van kabel is gewoon agent gebaseerde--u de agent installeert op een computer en deze wordt een eigen netwerkverkeer controleren. Een ander voordeel is wanneer u controleren werkbelasting uitgevoerd in de cloud-providers of hostingprovider service of Microsoft Azure wilt, waar de gebruiker niet de eigenaar van de laag stof.

U hoeft niet daarentegen compleet overzicht van wat gebeurt er in het netwerk als u niet kunt vinden op alle computers in de infrastructuur van uw netwerk installeren.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

- De oplossing kabel gegevens krijgt gegevens uit computers met Windows Server 2012 R2, Windows 8.1 en latere besturingssystemen.
- Microsoft .NET Framework 4.0 of hoger is vereist op computers waarop in het bezit van kabel gegevens uit.
- De oplossing kabel gegevens toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie vereist.
- Als u wilt kabel gegevens voor een specifieke oplossing moeten worden weergegeven, moet u de oplossing die al zijn toegevoegd aan uw werkruimte OMS hebt.

## <a name="wire-data-data-collection-details"></a>Gegevens verzamelen detailgegevens Wire

Kabel gegevens verzamelen van metagegevens over netwerkverkeer met de agenten die u hebt ingeschakeld.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor kabel gegevens.


| platform | Directe Agent | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Windows (2012 R2 / 8.1 of hoger)|![Ja](./media/log-analytics-wire-data/oms-bullet-green.png)|![Ja](./media/log-analytics-wire-data/oms-bullet-green.png)|![Nee](./media/log-analytics-wire-data/oms-bullet-red.png)|            ![Nee](./media/log-analytics-wire-data/oms-bullet-red.png)|![Nee](./media/log-analytics-wire-data/oms-bullet-red.png)| elke 1 minuut|


## <a name="combining-wire-data-with-other-solution-data"></a>Combineren kabel gegevens met andere oplossingsgegevens

Gegevens als resultaat gegeven van de ingebouwde query's die hierboven mogelijk interessant zijn door zelf. Het nut van gegevens van de kabel is echter gerealiseerd wanneer u deze met de informatie van andere oplossingen OMS combineren. U kunt bijvoorbeeld beveiliging gebeurtenisgegevens die worden verzameld door de oplossing beveiligings- en controle gebruiken en deze combineren met gegevens van de kabel om ongebruikelijke netwerk aanmeldingspogingen voor benoemde processen te vinden.  In dit voorbeeld gebruikt u de operatoren IN en DISTINCT deel te nemen aan gegevenspunten in uw query.

Vereisten: Om te kunnen gebruiken in het volgende voorbeeld, moet u er de beveiligings- en controle oplossing is geïnstalleerd. U kunt echter gegevens uit andere oplossingen combineren met gegevens van de kabel om soortgelijke resultaten te bereiken.

### <a name="to-combine-wire-data-with-security-events"></a>Kabel om gegevens te combineren met beveiligingsgebeurtenissen

1. Klik op de pagina overzicht op de tegel **WireData** .
2. Klik in de lijst met **Algemene WireData-query's**, op **Bedrag van netwerkverkeer (in Bytes) door proces** om de lijst van resulterende processen.
    ![wiredata query 's](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Als de lijst met processen te lang gemakkelijk weergeven is, kunt u de zoekopdracht in die lijkt op kunt wijzigen:

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    Weergegeven in het onderstaande voorbeeld is een proces met de naam DancingPigs.exe, die kan worden verdachte weergegeven.
    ![lijst met zoekresultaten de wiredata](./media/log-analytics-wire-data/oms-wiredata-02.png)

4. Gebruik van gegevens in de lijst met geretourneerd, klikt u op een benoemd proces. In dit voorbeeld is DancingPigs.exe geklikt. De resultaten die hieronder wordt het type netwerkverkeer zoals uitgaande communicatie via verschillende protocollen beschreven.
    ![wiredata resultaten met een benoemde proces](./media/log-analytics-wire-data/oms-wiredata-03.png)

5. Omdat de oplossing beveiligings- en controle is geïnstalleerd, kunt u zoeken naar de beveiligingsgebeurtenissen die dezelfde procesnaam veldwaarde hebben doordat uw query met de IN- en DISTINCT zoeken voor query's. U kunt die vervolgens doen als uw gegevens kabel zowel andere logboeken oplossing waarden in dezelfde opmaak bevatten. Wijzig uw zoekopdracht in die lijkt op:

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![wiredata resultaten gecombineerd waarin gegevens](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. In de bovenstaande resultaten ziet u dat accountgegevens worden weergegeven. Nu kunt u uw zoekopdracht om vast te stellen hoe vaak de account, met de beveiligings- en controle gegevens, is gebruikt door het proces met een query die lijkt op verfijnen:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![wiredata resultaten met accountgegevens](./media/log-analytics-wire-data/oms-wiredata-05.png)



## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) om gedetailleerde kabel gegevens zoeken records te bekijken.
- Zie dat de Tjeerd [kabel gegevens gebruiken in de blog bewerkingen Management Suite Log zoeken post](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) biedt extra informatie over hoe vaak de gegevens worden verzameld en hoe u de eigenschappen van siteverzameling voor Operations Manager gebruikersagenten kunt wijzigen.
