<properties
   pageTitle="Beveiliging op gebruikersniveau rij met Power BI ingesloten"
   description="Meer informatie over de beveiliging op gebruikersniveau rij met Power BI ingesloten"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="row-level-security-with-power-bi-embedded"></a>Beveiliging op rij met Power BI ingesloten

Beveiliging op rij (RLS) kan de toegang van gebruikers beperken tot bepaalde gegevens vanuit een rapport of de gegevensset, zodat voor meerdere verschillende gebruikers gebruik hetzelfde rapport terwijl ziet dat alle andere gegevens worden gebruikt. Power BI ingesloten ondersteunt nu gegevenssets geconfigureerd met RLS.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Om optimaal te profiteren van RLS, is het belangrijk dat u basisbegrippen drie belangrijkste; Gebruikers, rollen en regels. Laten we eens op elk:

**Gebruikers** – hierna ziet u de werkelijke eindgebruikers rapporten weergeven. In Power BI ingesloten, worden gebruikers geïdentificeerd door de eigenschap username in een Token-App.

**Rollen** – gebruikers deel uitmaakt van rollen. Een rol is een container voor regels en kunt de naam iets zoals "Verkoopmanager" of "Verkoper". In Power BI ingesloten, worden gebruikers geïdentificeerd door de eigenschap rollen in een Token-App.

**Regels** – rollen regels hebt en deze regels worden de werkelijke filters die moeten worden toegepast om de gegevens. Dit kan zijn net zo eenvoudig als "land VS = ' of iets veel meer dynamische.

### <a name="example"></a>Voorbeeld

Wij bieden een voorbeeld van RLS creatie en die vervolgens door andere binnen een ingesloten toepassing voor de rest van dit artikel. Ons voorbeeld gebruikt het [Voorbeeld detailhandel-analyse](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX-bestand.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

Ons voorbeeld detailhandel analyse verkoopcijfers voor alle winkels in een bepaalde detailhandelketen. Zonder RLS, ongeacht welk gebied manager zich aanmeldt en -weergaven van het rapport ziet ze dezelfde gegevens. Effectieve leiding heeft elke manager gebied ziet alleen de omzet voor de winkels die ze beheren en hiervoor gebruiken we RLS bepaald.

RLS is in Power BI Desktop gemaakt. Wanneer de gegevensset en het rapport worden geopend, kunnen we overschakelen naar de diagramweergave te klikken om het schema weer te geven:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Hier volgen een paar dingen om zoals u ziet, met dit schema:

-   Alle eenheden, zoals **Totale verkoop**, worden opgeslagen in de tabel **Sales** faculteit.
-   Er zijn vier extra dimensie voor gerelateerde tabellen: **Item**, **tijd**, **Store**en **gebied**.
-   De pijlen langs de relatielijnen aangeven welke filters kunnen flow van de ene tabel naar een andere manier. Bijvoorbeeld als een filter op **tijd [datum]**is geplaatst, filteren in het huidige schema deze alleen omlaag waarden in de tabel **Sales** . Geen andere tabellen geldt dit filter omdat alle pijlpunten op de relatielijnen wijst u naar de tabel sales en niet afwezig.
-   De tabel **gebied** geeft aan wie de manager is bedoeld voor elk:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Op basis van deze schema, als er een filter op de kolom **Gebied Manager** in de tabel gebied toepassen en als die overeenkomsten op de gebruiker die het rapport bekijkt filteren, dat filter ook omlaag de **Store** en **verkoop** tabellen alleen om gegevens te geven voor dat bepaalde gebied manager worden gefilterd.

Hier ziet u hoe:

1.  Klik op het tabblad modellering op **Rollen beheren**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Maak een nieuwe rol **Manager**genoemd.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  Voer de volgende DAX-expressie in de tabel **gebied** : **[gebied Manager] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Klik op **weergave als rollen**om ervoor te zorgen dat de regels zijn werkt, klikt u op het tabblad **Modeling** en voer het volgende:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    De rapporten wordt nu gegevens weergegeven als wanneer u zijn aangemeld als **Andrew Ma**.

Het filter toepassen op, worden zoals we hier hebben gedaan omlaag alle records in het **gebied**, **Store**en **verkoop** tabellen gefilterd. Echter vanwege de filter richting hebben op de relaties tussen de **verkoop** - en **tijdfunctie**, **verkoop** - en **Item**- en **Item** - en **tijd** tabellen niet gefilterd omlaag.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Die mogelijk ok voor deze vereiste, echter als we managers waarvoor ze een verkoop geen items weergeven die niet wilt, we kan inschakelen bidirectionele kruisgewijze filtering voor de relatie en het beveiligingsfilter in beide richtingen flow. Dit kan worden uitgevoerd door het bewerken van de relatie tussen de **verkoop** - en **Item**, zoals hier:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Filters kunnen nu ook doorlopen van de tabel Sales naar de tabel **Item** :

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Opmerking** Als u DirectQuery-modus voor uw gegevens gebruikt, moet u inschakelen bidirectionele kruisgewijs filteren door deze twee opties te selecteren:

1.  **Bestand** -> **Opties en instellingen** -> **Functies** -> **inschakelen cross filteren in beide richtingen voor DirectQuery**.
2.  **Bestand** -> **Opties en instellingen** -> **DirectQuery** -> **toestaan onbeperkte maateenheid in de DirectQuery-modus**.


Meer informatie over het gebruiken van bidirectionele kruisgewijze filtering, downloadt u de whitepaper [bidirectionele kruisgewijze filtering in SQL Server Analysis Services-2016 en Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) .

Hiermee wordt afgerond het werk dat moet worden uitgevoerd in Power BI Desktop, maar er is een meer stuk werk dat moet worden uitgevoerd als u wilt maken van de RLS regels dat we werk gedefinieerd in Power BI ingesloten. Gebruikers worden geverifieerd en geautoriseerd door de toepassing en App tokens die gebruiker toegang tot een specifiek Power BI ingesloten rapport worden gebruikt. Power BI ingesloten heeft geen specifieke gegevens op die de gebruiker is. Voor RLS om te werken, moet u enkele extra context bieden doorgeven als onderdeel van uw app-sleutel:
-   **gebruikersnaam** (optioneel) – gebruikt met RLS dit is een tekenreeks die kan worden gebruikt om te bepalen van de gebruiker wanneer RLS regels toepassen. Zie Werken met rij beveiliging met Power BI ingesloten
-   **rollen** – een tekenreeks met de rollen Selecteer wanneer rij-beveiliging op gebruikersniveau regels toepassen. Als meer dan één functie doorgeeft, moeten ze worden doorgegeven als een tekenreeksmatrix.

Als de eigenschap username aanwezig is, moet u ten minste één waarde ook doorgeven in rollen.

Het volledige app token ziet er ongeveer zo uitziet:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Nu, met alle onderdelen samen, wanneer iemand die zich aanmeldt bij de toepassing om weer te geven van dit rapport, ze wordt alleen mogelijk om de gegevens die ze zien, kunnen zoals gedefinieerd door de beveiliging van onze rij op gebruikersniveau weer te geven.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Zie ook
[Rij beveiliging op gebruikersniveau van (RLS) met Power](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)
