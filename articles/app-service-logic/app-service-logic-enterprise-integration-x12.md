<properties 
    pageTitle="Overzicht van X12 en de Enterprise-integratie Pack | App-Service van Microsoft Azure | Microsoft Azure" 
    description="Informatie over het gebruik van X12 overeenkomsten om logica apps te maken" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-x12"></a>Enterprise-integratie met X12 

>[AZURE.NOTE]Deze pagina omvat de X12 functies van logica-Apps. Klik voor informatie over EDIFACT [hier](app-service-logic-enterprise-integration-edifact.md).

## <a name="create-an-x12-agreement"></a>Maken van een X12 overeenkomst 
Voordat u kunt X12 uitwisselen berichten die u moet maken van een X12 overeenkomst en sla deze op uw account integratie. De volgende stappen begeleidt u bij het maken van een X12 overeenkomst.

### <a name="heres-what-you-need-before-you-get-started"></a>Hier ziet u wat u nodig hebt voordat u begint
- Een [account van de integratie](./app-service-logic-enterprise-integration-accounts.md) gedefinieerd in uw Azure-abonnement  
- Ten minste twee [partners](./app-service-logic-enterprise-integration-partners.md) al zijn gedefinieerd in uw account integratie  

>[AZURE.NOTE]Wanneer u een overeenkomst maakt, kan de inhoud in het bestand overeenkomst moet overeenkomen met het Overeenkomstsoort.    


Nadat u [gemaakt van een account integratie](./app-service-logic-enterprise-integration-accounts.md) en [partners toegevoegd hebt](./app-service-logic-enterprise-integration-partners.md), kunt u een X12 overeenkomst door deze stappen uit:  

### <a name="from-the-azure-portal-home-page"></a>Vanaf de startpagina voor Azure portal

Nadat u zich aanmelden bij de [portal van Azure](http://portal.azure.com "Azure-portal"):  
1. Selecteer **Bladeren** in het menu aan de linkerkant.  

>[AZURE.TIP]Als u de koppeling **Bladeren** niet ziet, moet u mogelijk eerst het optiemenu uitvouwen. Dit doen door het selecteren van de koppeling voor **menu weergeven** die zich bevindt op links boven in het menu samengevouwen.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integratie* Typ in het zoekvak van het filter en selecteer **Integratie Accounts** in de lijst met resultaten.       
![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)    
3. Selecteer in het blad **Integratie Accounts** dat wordt geopend, de integratie-account waarin u de overeenkomst wilt maken. Als u niet ziet een integratie accounts lijsten, [maken een eerste](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)  
4.  Selecteer de tegel **overeenkomsten** . Als u de tegel overeenkomsten niet ziet, moet u dit eerst toevoegen.   
![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)     
5. Selecteer de knop **toevoegen** in het blad overeenkomsten dat wordt geopend.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Voer een **naam** voor uw overeenkomst en selecteer het **soort overeenkomst**, **Host-Partner**, **Host identiteit**, **Gast Partner**, **Gast identiteit**, in het blad overeenkomsten dat wordt geopend.  
![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)  
7. Nadat u hebt de ontvangen eigenschappen ingesteld, selecteert u de knop **OK**  
Gaat u verder:  
8. Selecteer **Ontvangen instellingen** te configureren hoe berichten ontvangen via deze overeenkomst moeten worden verwerkt.  
9. Het besturingselement ontvangen instellingen is in de volgende secties, inclusief id's, bevestiging, schema's, enveloppen, besturingselement getallen, validatie en instellingen voor interne verdeeld. Deze eigenschappen op basis van uw overeenkomst met de partner die u berichten met uitwisselt configureren. Hier volgt een weergave van deze besturingselementen en ze op basis van de manier waarop deze overeenkomst herkennen en verwerken van binnenkomende berichten configureren:  
![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)  
10. Selecteer de knop **OK** uw instellingen op te slaan.  

### <a name="identifiers"></a>Id 's

|Eigenschap|Beschrijving |
|---|---|
|ISA1 (autorisatie kwalificatie)|Selecteer in de vervolgkeuzelijst de waarde van de kwalificatie autorisatie.|
|ISA2|Optioneel. Voer autorisatie informatie waarde. Als de waarde die u hebt opgegeven voor ISA1 dan 00 is, voert u ten minste één alfanumeriek teken en een maximum van 10.|
|ISA3 (beveiliging kwalificatie)|Selecteer in de vervolgkeuzelijst de waarde van de kwalificatie beveiliging.|
|ISA4|Optioneel. Voer de waarde voor informatie. Als de waarde die u hebt opgegeven voor ISA3 dan 00 is, voert u ten minste één alfanumeriek teken en een maximum van 10.|

### <a name="acknowledgments"></a>Bevestigingen 

|Eigenschap|Beschrijving |
|----|----|
|TA1 verwacht|Schakel dit selectievakje in dat een technische (TA1) bevestiging naar de afzender interchange. Deze bevestigingen worden verzonden naar de interchange-afzender op basis van de instellingen voor verzenden voor de overeenkomst.|
|FA verwacht|Schakel dit selectievakje in dat een functionele (FA) bevestiging naar de afzender interchange. Geef aan of u wilt dat de bevestigingen 997 of 999, op basis van de schema-versies die u met werkt. Deze bevestigingen worden verzonden naar de interchange-afzender op basis van de instellingen voor verzenden voor de overeenkomst.|
|Een loopback AK2/IK2 opnemen|Schakel dit selectievakje in te schakelen generatie van AK2 lussen functionele bevestigingen voor geaccepteerde transactie sets. Opmerking: Dit selectievakje is alleen beschikbaar als u het selectievakje FA verwacht hebt geselecteerd.|

### <a name="schemas"></a>Schema's maken

Kies een schema voor elke transactietype (ST1) en de afzender-toepassing (GS2). De pijplijn ontvangen ontleed het binnenkomende bericht door de waarden voor ST1 en GS2 in het binnenkomende bericht met de waarden die u hier instellen en het schema van het binnenkomende bericht met het schema dat u hier instelt.

|Eigenschap|Beschrijving |
|----|----|
|Versie|Selecteer de X12 versie|
|Transactietype (ST01)|Selecteer het transactietype|
|Afzender-toepassing (GS02)|Selecteer de afzender-toepassing|
|Schema|Selecteer het schemabestand dat u ons wilt. Schemabestanden bevinden zich in uw account integratie.|

### <a name="envelopes"></a>Enveloppen

|Eigenschap|Beschrijving |
|----|----|
|ISA11 gebruik|Gebruik dit veld om op te geven het scheidingsteken in een set transactie:</br></br>Selecteer de standaard-id voor het gebruik van de notatie van '. " ontvangen verkooppijplijn in plaats van de notatie van het binnenkomende document in de bewerken.</br></br>Selecteer terugkeerpatroon scheidingsteken om op te geven het scheidingsteken herhaalde exemplaren van een eenvoudige gegevenselement of een herhaalde gegevensstructuur. (^) Wordt bijvoorbeeld meestal gebruikt als scheidingsteken terugkeerpatroon. Voor HIPAA schema's, kunt u alleen (^) gebruiken.|

### <a name="control-numbers"></a>Besturingselement voor getallen

|Eigenschap|Beschrijving |
|----|----|
|Controle-aantal Interchange duplicaten weigeren|Schakel deze optie als u wilt blokkeren dubbele knooppunten. Als geselecteerd, wordt in de Portal van de Services BizTalk gecontroleerd dat het interchange besturingselement getal (ISA13) voor de uitwisseling ontvangen niet overeenkomen met het nummer interchange-besturingselement. Als er een overeenkomst wordt gevonden, worden de uitwisseling niet verwerkt door de pijplijn ontvangen.<br/>Als u ervoor hebt gekozen om dubbele interchange besturingselement getallen weigeren, kunt u het aantal dagen waarop de controle wordt uitgevoerd door middel van de juiste waarde voor het controleren op dubbele ISA13 elke x dagen opgeven.|
|Groep besturingselement getal duplicaten weigeren|Schakel deze optie als u wilt blokkeren van knooppunten met dubbele groep besturingselement getallen.|
|Transactie set besturingselement getal duplicaten weigeren|Schakel deze optie als u wilt blokkeren van knooppunten met dubbele transactie set besturingselement getallen.|

### <a name="validations"></a>Validatie

|Eigenschap|Beschrijving |
|----|----|
|Berichttype|Bewerken bericht type, zoals 850-inkooporder of bevestiging 999-implementatie.|
|Validatie bewerken|Worden gegevenstypen gedefinieerd door de eigenschappen van het bewerken van het schema, lengte beperkingen, lege gegevenselementen en volgspaties scheidingstekens bewerken gevalideerd.|
|Uitgebreide gegevensvalidatie|Als het gegevenstype niet bewerken is, gegevensvalidatie de gegevensvereiste-element is ingeschakeld en toegestane terugkeerpatroon, opsommingen en element lengte gegevensvalidatie (min/max).|
|Regelafstand/volgspaties nullen toestaan|Geen extra ruimte en nul tekens die zijn voorloop- of volgspaties blijven behouden. Ze worden niet verwijderd.|
|Scheidingsteken beleid volgspaties|Genereert volgspaties scheidingstekens op de uitwisseling ontvangen. Opties zijn NotAllowed, optioneel en verplicht.|

### <a name="internal-settings"></a>Interne instellingen

|Eigenschap|Beschrijving |
|----|----|
|Impliciete decimale notatie Nn wilt baseren 10 numerieke waarde converteren|Converteert een getal bewerken die is opgegeven met de notatie Nn in een grondtal 10 numerieke waarde in de tussenliggende XML in de Portal van BizTalk-Services.|
|Lege XML-codes maken als volgspaties scheidingstekens zijn toegestaan|Schakel dit selectievakje in om de interchange-afzender leeg XML-codes voor volgspaties scheidingstekens bevatten.|
|Binnenkomende batchen verwerking|Gesplitste Interchange als transactie sets - schorten transactie sets op fout: parseert u elke transactie instellen in een interchange in een afzonderlijke XML-document door de juiste envelop toepassen op de set transactie. Met deze optie wordt als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens BizTalk Services alleen die sets transactie onderbreekt. </br></br>Gesplitste Interchange als transactie sets - schorten interchange op fout: parseert u elke transactie instellen in een interchange in een afzonderlijke XML-document door de juiste envelop toe te voegen. Met deze optie wordt als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens BizTalk Services wordt de hele uitwisseling uitgesteld.</br></br>Behouden Interchange - transactie sets schorten op fout: laat de uitwisseling intact, een XML-document voor de hele batch uitwisseling maken. Met deze optie wordt als onAe of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens BizTalk Services alleen die sets van het transactie onderbreekt, terwijl u alle andere transactie sets verwerken.</br></br>Behouden Interchange - interchange schorten op fout: laat de uitwisseling intact, een XML-document voor de hele batch uitwisseling maken. Met deze optie wordt als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens BizTalk Services wordt de hele uitwisseling uitgesteld.</br></br>|

Uw overeenkomst is klaar voor het verwerken van binnenkomende berichten die voldoen aan het schema dat u hebt geselecteerd.

De instellingen die u naar partners verzendt e-mails configureren:  
11. Selecteer **Instellingen voor verzenden** naar configureren hoe berichten die zijn verzonden via deze overeenkomst moeten worden verwerkt.  

Het besturingselement instellingen voor verzenden is in de volgende secties, inclusief id's, bevestiging, schema's, enveloppen, besturingselement getallen, tekensets en scheidingstekens en gegevensvalidatie verdeeld. 

Hier ziet u een weergave van deze besturingselementen. De selecties op basis van wat u moet gebeuren van berichten die u naar partners via deze overeenkomst verzendt maken:   
![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)  
12. Selecteer de knop **OK** uw instellingen op te slaan.  

### <a name="identifiers"></a>Id 's
|Eigenschap|Beschrijving |
|----|----|
|Autorisatie kwalificatie (ISA1)|Selecteer in de vervolgkeuzelijst de waarde van de kwalificatie autorisatie.|
|ISA2|Voer autorisatie informatie waarde. Als deze waarde dan 00 is, en voer vervolgens een minimum van één alfanumeriek teken en een maximum van 10.|
|Beveiliging kwalificatie (ISA3)|Selecteer in de vervolgkeuzelijst de waarde van de kwalificatie beveiliging.|
|ISA4|Voer de waarde voor informatie. Als deze waarde is dan 00, voor het vak waarde (ISA4), en voer vervolgens een minimum van één alfanumerieke waarde en een maximum van 10.|

### <a name="acknowledgment"></a>Bevestiging
|Eigenschap|Beschrijving |
|----|----|
|TA1 verwacht|Schakel dit selectievakje in dat een technische (TA1) bevestiging naar de afzender interchange. Deze instelling wordt aangegeven dat de host-partner die het bericht verzendt om een bevestiging van de partner Gast in de overeenkomst vraagt. Deze bevestigingen verwachting door de host-partner op basis van de instellingen voor het ontvangen van de overeenkomst.|
|FA verwacht|Schakel dit selectievakje een functionele (FA) bevestiging teruggaan naar de afzender interchange, en geef aan of u wilt dat de bevestigingen 997 of 999, op basis van de schema versies die u met werkt. Deze bevestigingen verwachting door de host-partner op basis van de instellingen voor het ontvangen van de overeenkomst.|
|VA-versie|Selecteer de FA-versie|

### <a name="schemas"></a>Schema's maken
|Eigenschap|Beschrijving |
|----|----|
|Versie|Selecteer de X12 versie|
|Transactietype (ST01)|Selecteer het transactietype|
|SCHEMA|Selecteer het schema te gebruiken. Schema's bevinden zich in uw account integratie. Voor toegang tot uw schema's maken, moet u eerst uw integratie-account koppelen aan uw logica-app.|

### <a name="envelopes"></a>Enveloppen
|Eigenschap|Beschrijving |
|----|----|
|ISA11 gebruik|Gebruik dit veld om op te geven het scheidingsteken in een set transactie:</br></br>Selecteer de standaard-id voor het gebruik van de notatie van '. " ontvangen verkooppijplijn in plaats van de notatie van het binnenkomende document in de bewerken.</br></br>Selecteer terugkeerpatroon scheidingsteken om op te geven het scheidingsteken herhaalde exemplaren van een eenvoudige gegevenselement of een herhaalde gegevensstructuur. (^) Wordt bijvoorbeeld meestal gebruikt als scheidingsteken terugkeerpatroon. Voor HIPAA schema's, kunt u alleen (^) gebruiken.</br>|
|Scheidingsteken voor terugkeerpatroon|Voer het scheidingsteken voor terugkeerpatroon|
|Het versienummer besturingselement (ISA12)|Selecteer de versie van de standaard X12 die wordt gebruikt door de BizTalk Services-Portal voor het genereren van een uitgaande interchange.|
|Gebruiksindicator (ISA15)|Geef op of de context van een interchange vindt u informatie over (I), productiegegevens (P) of (T) gegevens testen. De bewerken ontvangen verkooppijplijn verhoogt het niveau van deze eigenschap op de context.|
|Schema|U kunt opgeven hoe de GS en ST lijnsegmenten voor een X12-codering uitwisseling dat deze wordt verzonden naar de pijplijn verzenden in de Portal van de Services BizTalk wordt gegenereerd.</br></br>U kunt waarden van de GS1, GS2 GS3, GS4, GS5, GS7 en GS8 gegevenselementen met waarden van het Type transactie en versie/gegevenselementen koppelen. Als de Portal van de Services BizTalk dat bepaalt een XML-bericht heeft de waarden instellen voor het Type transactie en versie-elementen in een rij van het raster, klikt u het vult de elementen van de gegevens GS1, GS2 GS3, GS4, GS5, GS7 en GS8 in de envelop van de uitgaande uitwisseling met de waarden uit dezelfde rij van het raster. De waarden van het Type transactie en versie-elementen moet uniek zijn.</br></br>Optioneel. Voor GS1, selecteer u een waarde voor de functionele code in de vervolgkeuzelijst.</br></br>Vereist. Voer voor GS2, een alfanumerieke waarde in voor de toepassing afzender met ten minste twee tekens en maximaal 15 tekens.</br></br>Vereist. Voer voor GS3, een alfanumerieke waarde in voor de ontvanger toepassing met ten minste twee tekens en maximaal 15 tekens.</br></br>Optioneel. Selecteer voor GS4, CCYYMMDD of JJMMDD.</br></br>Optioneel. Selecteer voor GS5, HHMM, UUMMSS of HHMMSSdd.</br></br>Optioneel. Voor GS7, selecteer u een waarde voor het verantwoordelijk Bureau in de vervolgkeuzelijst.</br></br>Optioneel. Voer voor GS8, een alfanumerieke waarde in voor het document die wordt aangeduid met ten minste één teken en een maximum van 12 tekens.</br></br>**Opmerking**: dit zijn de waarden die de Portal van de Services BizTalk ingevoerd in de velden GS van de uitwisseling deze bouwt als de transactie typt en versie-elementen in dezelfde rij zijn een overeenkomst met die zijn gekoppeld aan de uitwisseling.|

### <a name="control-numbers"></a>Besturingselement voor getallen
|Eigenschap|Beschrijving |
|----|----|
|Interchange-besturingselement getal (ISA13)|Vereist. Voer in een bereik van waarden voor de interchange besturingselement dat wordt gebruikt door de Portal BizTalk Services bij het genereren van een uitgaande interchange. Geef een numerieke waarde met minimaal 1 en een maximum van 999999999.|
|Groep besturingselement getal (GS06)|Vereist. Voer in het bereik van getallen die de Portal van de Services BizTalk voor het nummer van het besturingselement gebruiken moet. Geef een numerieke waarde met ten minste één teken en een maximum van negen tekens.|
|Transactie besturingselementnummer (ST02) instellen|Voer voor transactie instellen besturingselement getal (ST02), een bereik van numerieke waarden voor de vereiste middelste velden en alfanumerieke waarden voor de optioneel voor- en achtervoegsel. De maximumlengte van alle vier de velden is negen tekens.|
|Voorvoegsel|Als wilt toewijzen het bereik van transactie set besturingselement getallen in een bevestiging gebruikt, voert u waarden in de velden van ACK-besturingselement getal (ST02). Voer een numerieke waarde voor de middelste twee velden en een alfanumerieke waarde (indien gewenst) voor de velden voor- en achtervoegsel. De middelste velden zijn vereist en bevatten de laagste en hoogste waarde voor het besturingselementnummer van het; de voor- en achtervoegsel zijn optioneel. De maximumlengte voor alle drie de velden is negen tekens.|
|Achtervoegsel|Als wilt toewijzen het bereik van transactie set besturingselement getallen in een bevestiging gebruikt, voert u waarden in de velden van ACK-besturingselement getal (ST02). Voer een numerieke waarde voor de middelste twee velden en een alfanumerieke waarde (indien gewenst) voor de velden voor- en achtervoegsel. De middelste velden zijn vereist en bevatten de laagste en hoogste waarde voor het besturingselementnummer van het; de voor- en achtervoegsel zijn optioneel. De maximumlengte voor alle drie de velden is negen tekens.|

### <a name="character-sets-and-separators"></a>Teken Sets en scheidingstekens
Andere dan het teken dat is ingesteld, kunt u een andere set scheidingstekens moet worden gebruikt voor elk berichttype invoeren. Als een tekenset niet voor een bepaald berichtschema is opgegeven, wordt de standaard-tekenset gebruikt.

|Eigenschap|Beschrijving |
|----|----|
|Tekenset om te worden gebruikt|Selecteer de X12 tekenset voor het valideren van de eigenschappen die u voor de overeenkomst invoert.</br></br>**Opmerking**: de BizTalk Services Portal alleen deze instelling wordt gebruikt voor het valideren van de waarden voor de gerelateerde overeenkomst-eigenschappen. De verkooppijplijn ontvangen of verzenden verkooppijplijn negeert deze eigenschap tekenset bij het uitvoeren van runtime-verwerking.|
|Schema|Selecteer het (+)-symbool en selecteer een schema in de vervolgkeuzelijst. Selecteer de scheidingstekens te kunnen gebruiken voor het geselecteerde schema:</br></br>Onderdeel element scheidingsteken – Enter een willekeurig teken voor het scheiden van samengestelde gegevenselementen.</br></br>Gegevens Element scheidingsteken – Enter een willekeurig teken voor het scheiden van eenvoudige gegevenselementen binnen samengestelde gegevenselementen.</br></br></br></br>Vervangende teken: Schakel dit selectievakje in als de gegevens bevatten nettolading tekens die ook worden gebruikt als gegevens, segment of onderdeel scheidingstekens. Vervolgens kunt u een vervangend teken. Bij het genereren van de uitgaande bericht X12, alle exemplaren van scheidingstekens in de gegevens worden vervangen door het opgegeven teken nettolading.</br></br>Scheidingslijn Segment: Voer een willekeurig teken om aan te geven het einde van een segment bewerken.</br></br>Achtervoegsel: Selecteer het teken dat wordt gebruikt met het segment-id. Als u een achtervoegsel opgeeft, kan het segment scheidingslijn element leeg zijn. Als de scheidingslijn segment leeg laat, moet u een achtervoegsel aanwijzen.|

### <a name="validation"></a>Gegevensvalidatie
|Eigenschap|Beschrijving |
|----|----|
|Berichttype|U deze optie selecteert, kunt de ontvanger interchange worden gevalideerd. Deze validatie worden transactie-set gegevenselementen, valideren gegevenstypen, lengte beperkingen en lege gegevenselementen en scheidingstekens volgspaties bewerken gevalideerd.|
|Validatie bewerken||
|Uitgebreide gegevensvalidatie|Deze optie maakt uitgebreide validatie van knooppunten van de afzender interchange hebt ontvangen. Dit geldt ook voor validatie van Veldlengte, optionaliteit en aantal herhalingen naast XSD-type gegevensvalidatie. U kunt extensie validatie inschakelen zonder het activeren van validatie bewerken, of vice versa.|
|Begin / einde nullen toestaan|Deze optie wordt aangegeven dat een bewerken interchange hebt ontvangen van de partij wordt niet validatie is mislukt als een gegevenselement in een interchange bewerken niet aan de vereiste lengte vanwege of volgspaties voldoet, maar met de vereiste lengte overeen komt wanneer ze worden verwijderd.|
|Scheidingsteken volgspaties|Als wordt deze optie een bewerken interchange hebt ontvangen van de partij wordt validatie niet is mislukt als een gegevenselement in een interchange bewerken niet aan de vereiste lengte vanwege voorloop-(of volgspaties) nullen of volgspaties voldoet, maar met de vereiste lengte overeen komt wanneer ze worden verwijderd.</br></br>Selecteer niet toegestaan als u niet wilt dat aan het einde scheidingstekens en scheidingstekens in een interchange ontvangen van de afzender interchange zijn toegestaan. Als de uitwisseling volgspaties scheidingstekens en scheidingstekens bevat, is deze ongeldige gedeclareerd.</br></br>Selecteer optioneel uitgewisseld met of zonder volgspaties scheidingstekens en scheidingstekens accepteren.</br></br>Selecteer verplicht als de ontvangen uitwisseling moet volgspaties scheidingstekens en scheidingstekens bevatten.|

Selecteer **OK** in het geopende bladen nadat u hebt:  
13. Selecteer de tegel **overeenkomsten** op het blad integratie-Account en ziet u de zojuist toegevoegde overeenkomst wordt vermeld.  
![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>Meer informatie
- [Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack")  
