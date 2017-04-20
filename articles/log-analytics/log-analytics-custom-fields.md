<properties
   pageTitle="Aangepaste velden in Log Analytics | Microsoft Azure"
   description="De functie aangepaste velden van Log Analytics kunt u uw eigen doorzoekbare velden maken van OMS-gegevens die aan de eigenschappen van een verzamelde record toevoegen.  In dit artikel worden beschreven van het proces voor het maken van een aangepast veld en een gedetailleerd overzicht met een voorbeeld van de gebeurtenis."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-fields-in-log-analytics"></a>Aangepaste velden in Log Analytics

De functie **Aangepaste velden** van Log Analytics kunt u bestaande records in de bibliotheek OMS uitbreiden door uw eigen doorzoekbare velden toe te voegen.  Aangepaste velden worden automatisch ingevuld van gegevens opgehaald uit andere eigenschappen in dezelfde record.

![Overzicht van aangepaste velden](media/log-analytics-custom-fields/overview.png)

Het onderstaande voorbeeldrecord bevat bijvoorbeeld handig gegevens ondergesneeuwd in de gebeurtenisbeschrijving van de.  Deze gegevens ophalen in afzonderlijke eigenschappen stelt beschikbaar van bijvoorbeeld sorteren en filteren.

![Meld u aan de knop Zoeken](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] Klik in het voorbeeld zijn u beperkt tot 100 aangepaste velden in de werkruimte.  Deze limiet worden uitgevouwen als deze functie beschikbaar is bereikt.

## <a name="creating-a-custom-field"></a>Een aangepast veld maken

Wanneer u een aangepast veld maakt, moet Log Analytics welke gegevens kunnen worden gebruikt om te vullen de waarde begrijpen.  Een technologie van Microsoft Research genoemd FlashExtract gebruikt de functie snel identificeren van deze gegevens.  In plaats van dat u hoeft te bieden expliciete instructies, leert Log Analytics over de gegevens die u wilt extraheren uit voorbeelden die u opgeeft.

De volgende secties bevatten de procedure voor het maken van een aangepast veld.  Onderaan in dit artikel is Stapsgewijze instructies voor de extractie van een steekproef.

> [!NOTE] Het aangepaste veld wordt gevuld records die overeenkomen met de opgegeven criteria zijn toegevoegd aan de gegevensopslag OMS, zodat deze alleen wordt weergegeven op de records die zijn verzameld nadat het aangepaste veld is gemaakt.  Het aangepaste veld worden niet toegevoegd aan de records die zich al in de gegevensopslag wanneer deze gemaakt.

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Stap 1 – records waarvoor u het aangepaste veld identificeren
De eerste stap is om aan te geven van de records die u het aangepaste veld krijgt.  U beginnen met een [standaard log zoeken](log-analytics-log-searches.md) en selecteer vervolgens een record fungeert als het model dat Log Analytics leert uit.  Wanneer u opgeeft dat u wilt extraheren van gegevens in een aangepast veld, wordt het **Veld extractie van Wizard** wordt geopend waar u valideren en Verfijn de criteria.

2. Ga naar **Log zoeken** en gebruiken van een [query voor het ophalen van de records](log-analytics-log-searches.md) die het aangepaste veld heeft.
2. Selecteer een record die Log Analytics fungeert als een model voor het extraheren van gegevens in te vullen van het aangepaste veld wilt gebruiken.  U geeft de gegevens die u wilt extraheren uit deze record en Log Analytics deze informatie wordt gebruikt om te bepalen de logica om te vullen van het aangepaste veld voor alle vergelijkbare records.
3. Klik op de knop aan de linkerkant van de teksteigenschap van een van de record en selecteer **extraheren velden uit**.
4. Het **veld extractie van Wizard wordt geopend**en de record die u hebt geselecteerd, wordt weergegeven in de kolom **Hoofdgegeven voorbeeld** .  Het aangepaste veld moet worden gedefinieerd voor deze records met dezelfde waarde in de eigenschappen die zijn geselecteerd.  
5. Als de selectie niet precies wat u wilt is, selecteert u extra velden om de criteria te beperken.  Om te wijzigen van de waarden van de velden voor de criteria, moet u annuleren en selecteer een andere record die overeenkomen met de gewenste criteria.

### <a name="step-2---perform-initial-extract"></a>Stap 2: eerste uitpakken uitvoeren.
Nadat u de records die het aangepaste veld hebt geïdentificeerd, kunt u de gegevens die u wilt extraheren identificeren.  Log Analytics wordt deze informatie gebruikt om te identificeren vergelijkbare patronen in vergelijkbare records.  In de stap na dit is mogelijk om te valideren van de resultaten en geef meer details voor Log analyses voor gebruik in de analyse.

1. Markeer de tekst in de steekproef-record die u wilt vullen van het aangepaste veld.  U krijgt vervolgens het dialoogvenster Geef een naam voor het veld en uitvoeren van de eerste uitpakken.  De tekens ** \_CF** wordt automatisch toegevoegd.
2. Klik op **uitpakken** om in een analyse van verzamelde records.  
3. De secties **samenvattings** - en **Zoekresultaten** weergeven de resultaten van het uitpakken, zodat u kunt de nauwkeurigheid controleren.  **Overzicht van** de criteria die gebruikt om te identificeren records en een aantal voor elk van de gegevenswaarden die zijn geïdentificeerd worden weergegeven.  **Lijst met zoekresultaten** biedt een uitgebreide lijst met records die de criteria voldoen.


### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Stap 3: Controleer of de nauwkeurigheid van de uitpakken en aangepast veld maken

Als u de eerste uitpakken hebt uitgevoerd, weergegeven Log Analytics de resultaten op basis van gegevens die zijn verzameld.  Als de resultaten correct kunt u het aangepaste veld maken met geen verdere werk.  Als dat niet zo is, klikt u vervolgens kunt u de resultaten verfijnen zodat Log Analytics de logica kunt verbeteren.

2.  Als geen waarden in de eerste uitpakken niet juist, klikt u vervolgens op het pictogram **bewerken** naast een onjuiste record en selecteer **wijzigen deze markeren** om te wijzigen van de selectie.
3.  De invoer wordt gekopieerd naar de sectie **aanvullende voorbeelden** onder in het **Hoofdmenu voorbeeld**.  U kunt de markering hier aanpassen zodat Log Analytics begrijpen van de selectie die moet hebt aangebracht.
4.  Klik op **uitpakken** om deze nieuwe informatie te evalueren van alle bestaande records gebruiken.  De resultaten kunnen worden gewijzigd voor records behalve het gedeelte die u zojuist hebt gewijzigd, op basis van deze nieuwe intelligence.
5.  Ga verder met het toevoegen van correcties totdat alle records in de uitpakken herkennen de gegevens in te vullen de nieuw aangepast veld.
6. Klik op **Opslaan extraheren** wanneer u tevreden met de resultaten bent.  Het aangepaste veld is nu gedefinieerd, maar deze niet toegevoegd aan records nog.
7.  Wacht totdat de nieuwe records die de opgegeven criteria voldoen om te worden verzameld en voer de zoekopdracht log opnieuw. Nieuwe records moeten het aangepaste veld hebben.
8.  Het aangepaste veld zoals elke andere record, eigenschap gebruiken.  U kunt gebruiken om te gegevens aggregeren en groeperen en deze zelfs gebruiken om te maken van nieuwe inzichten te verkrijgen.


## <a name="viewing-custom-fields"></a>Aangepaste velden weergeven
U kunt een lijst met alle aangepaste velden weergeven in uw groep management via de tegel van de **Instellingen** van het dashboard OMS.  Selecteer de **gegevens** en klik vervolgens op **aangepaste velden** voor een lijst met alle aangepaste velden in uw werkruimte.  

![Aangepaste velden](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Een aangepast veld verwijderen
Er zijn twee manieren een aangepast veld verwijderen.  De eerste is de optie **verwijderen** voor elk veld dat bij het weergeven van de volledige lijst, zoals hierboven is beschreven.  De andere methode is een record ophalen en klik op de knop aan de linkerkant van het veld.  Het menu heeft een optie voor het verwijderen van het aangepaste veld.

## <a name="sample-walkthrough"></a>Voorbeeld Stapsgewijze instructies

De volgende sectie begeleidt bij een volledig voorbeeld van een aangepast veld maken.  In dit voorbeeld haalt de servicenaam van de in Windows-gebeurtenissen die een service wijzigt status aangeven.  Dit is afhankelijk van gebeurtenissen die zijn gemaakt door servicebeheer in het logboek op een Windows-computer.  Als u volgen in dit voorbeeld wilt, moet u [voor het verzamelen van gebeurtenissen voor het logboek](log-analytics-data-sources-windows-events.md).

We Voer de volgende query om terug te keren alle gebeurtenissen van servicebeheer met een gebeurtenis-ID van 7036 in, dat wil zeggen de gebeurtenis die aangeeft van een service starten of stoppen.

![Query](media/log-analytics-custom-fields/query.png)

We Selecteer vervolgens een record met gebeurtenis-ID 7036 in.

![Bronrecord](media/log-analytics-custom-fields/source-record.png)

We wilt de servicenaam van de die wordt weergegeven in de eigenschap **RenderedDescription** en selecteer de knop naast deze eigenschap.

![Velden ophalen](media/log-analytics-custom-fields/extract-fields.png)

Het **Veld extractie van Wizard** wordt geopend en de velden **gebeurtenislogboek** en **EventID** zijn geselecteerd in de kolom **Hoofdgegeven voorbeeld** .  Hiermee wordt aangeduid dat het aangepaste veld moet worden gedefinieerd voor gebeurtenissen van het logboek met een gebeurtenis-ID van 7036 in.  Dit is voldoende zodat we niet nodig hebt om een andere velden te selecteren.

![Voorbeeld van de belangrijkste](media/log-analytics-custom-fields/main-example.png)

We de naam van de service in de eigenschap **RenderedDescription** markeren en **Service** gebruiken om de servicenaam te identificeren.  Het aangepaste veld naam **Service_CF**.

![Veldtitel](media/log-analytics-custom-fields/field-title.png)

U ziet dat de servicenaam van de correct wordt geïdentificeerd voor sommige records, maar niet voor anderen.   De **Lijst met zoekresultaten** blijkt dat gedeelte van de naam voor de **WMI-prestatieadapter** is niet geselecteerd.  Het **Overzicht** ziet u dat vier records met **DPRMA** service onjuist een extra word opgenomen en twee records die zijn geïdentificeerd **Modules Installer** in plaats van **Windows Modules Installer**.  

![Zoekresultaten](media/log-analytics-custom-fields/search-results-01.png)

We beginnen met de record **WMI prestatieadapter** .  We op het bewerkingspictogram en klik vervolgens op **wijzigen in dit markeren**.  

![Markering wijzigen](media/log-analytics-custom-fields/modify-highlight.png)

We vergroten de markering als u wilt opnemen van het woord **WMI** en voer de uitpakken.  

![Aanvullende voorbeeld](media/log-analytics-custom-fields/additional-example-01.png)

U ziet dat de items voor **WMI prestatieadapter** hebt gecorrigeerd en Log Analytics die gegevens ook gebruikt voor het corrigeren van de records voor **Windows Module Installer**.  We kunnen zien in de sectie **Samenvatting** wel die **DPMRA** is nog steeds niet wordt geïdentificeerd correct.

![Zoekresultaten](media/log-analytics-custom-fields/search-results-02.png)

We Schuif naar een record met de service DPMRA en gebruikt u dezelfde stappen die record worden gecorrigeerd.

![Aanvullende voorbeeld](media/log-analytics-custom-fields/additional-example-02.png)

 Als we de extractie van uitvoer, zien we dat al onze resultaten nu correct zijn.

![Zoekresultaten](media/log-analytics-custom-fields/search-results-03.png)

U ziet dat **Service_CF** wordt gemaakt, maar nog niet is toegevoegd aan records.

![Eerste tellen](media/log-analytics-custom-fields/initial-count.png)

Nadat het enige tijd is verstreken, dus nieuwe gebeurtenissen die zijn verzameld, zien we dat het veld **Service_CF** is nu wordt toegevoegd aan records die overeenkomen met onze criteria.

![Definitieve resultaten](media/log-analytics-custom-fields/final-results.png)

We kunnen nu het aangepaste veld zoals elke andere record eigenschap gebruiken.  Ter illustratie maken we een query die door het nieuwe veld aan **Service_CF** om te controleren welke services de Actiefste zijn van groepen.

![Groeperen op query](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [zoekopdrachten in het logboek](log-analytics-log-searches.md) maken van query's met aangepaste velden voor de criteria.
- Controle van [aangepaste logboekbestanden](log-analytics-data-sources-custom-logs.md) die u parseren met aangepaste velden.