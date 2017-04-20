<properties 
    pageTitle="Enterprise-integratie met EDIFACT | Microsoft Azure" 
    description="Informatie over het gebruik van EDIFACT overeenkomsten om logica apps te maken" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="jonfan"/>

# <a name="enterprise-integration-with-edifact"></a>Enterprise-integratie met EDIFACT 

> [AZURE.NOTE] Deze pagina behandelt de functies EDIFACT van logica Apps. Klik voor informatie over X12 [hier](app-service-logic-enterprise-integration-x12.md).

## <a name="create-an-edifact-agreement"></a>Een overeenkomst EDIFACT maken 
Voordat u berichten EDIFACT uitwisselen kunt, moet u een overeenkomst EDIFACT maakt en opslaat in uw account integratie. De volgende stappen wordt u begeleid bij het proces van het maken van een overeenkomst EDIFACT.

### <a name="heres-what-you-need-before-you-get-started"></a>Hier ziet u wat u nodig hebt voordat u begint
- Een [account van de integratie](./app-service-logic-enterprise-integration-accounts.md) gedefinieerd in uw Azure-abonnement  
- Ten minste twee [partners](./app-service-logic-enterprise-integration-partners.md) al zijn gedefinieerd in uw account integratie  

>[AZURE.NOTE]Wanneer u een overeenkomst maakt, moet het Overeenkomstsoort overeenkomen met de inhoud in de berichten u wordt ontvangen/verzenden naar en vanuit de partner.    


Nadat u [gemaakt van een account integratie](./app-service-logic-enterprise-integration-accounts.md) en [partners toegevoegd hebt](./app-service-logic-enterprise-integration-partners.md), kunt u een overeenkomst EDIFACT maken door deze stappen uit:  

### <a name="from-the-azure-portal-home-page"></a>Vanaf de startpagina voor Azure portal

Nadat u zich aanmelden bij de [portal van Azure](http://portal.azure.com "Azure-portal"):  
1. Selecteer **Bladeren** in het menu aan de linkerkant.  

>[AZURE.TIP]Als u de koppeling **Bladeren** niet ziet, moet u mogelijk eerst het optiemenu uitvouwen. Dit doen door het selecteren van de koppeling voor **menu weergeven** die zich bevindt op links boven in het menu samengevouwen.  

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)    
2. *Integratie* Typ in het zoekvak van het filter en selecteer **Integratie Accounts** in de lijst met resultaten.       
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)    
3. Selecteer in het blad **Integratie Accounts** dat wordt geopend, de integratie-account waarin u de overeenkomst wilt maken. Als u niet ziet een integratie accounts lijsten, [maken een eerste](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)  
4.  Selecteer de tegel **overeenkomsten** . Als u de tegel overeenkomsten niet ziet, moet u dit eerst toevoegen.   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)     
5. Selecteer de knop **toevoegen** in het blad overeenkomsten dat wordt geopend.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)  
6. Voer een **naam** voor uw overeenkomst en selecteer vervolgens het **soort overeenkomst** voor EDIFACT, **Host-Partner**, **Host identiteit**, **Gast Partner**, **Gast identiteit**, in het blad overeenkomsten dat wordt geopend.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)  
7. Nadat u de eigenschappen van het overeenkomst hebt ingesteld, selecteert u **Ontvangen instellingen** te configureren hoe berichten ontvangen via deze overeenkomst moeten worden verwerkt.  
8. Het besturingselement ontvangen instellingen is in de volgende secties, inclusief-id's, bevestiging, schema's, besturingselement getallen, validatie, interne instellingen en batchbestand verdeeld. Deze eigenschappen op basis van uw overeenkomst met de partner die u berichten met uitwisselt configureren. Hier volgt een weergave van deze besturingselementen en ze op basis van de manier waarop deze overeenkomst herkennen en verwerken van binnenkomende berichten configureren:  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)  
9. Selecteer de knop **OK** uw instellingen op te slaan.  

### <a name="identifiers"></a>Id 's

|Eigenschap|Beschrijving |
|---|---|
|UNB6.1 (geadresseerden verwijzing wachtwoord)|Voer een alfanumerieke waarde in die tussen 1 en 14 tekens zijn variëren.|
|UNB6.2 (geadresseerden verwijzing kwalificatie)|Voer een alfanumerieke waarde met ten minste één teken en een maximum van twee tekens.|

### <a name="acknowledgments"></a>Bevestigingen 

|Eigenschap|Beschrijving |
|----|----|
|Ontvangst van bericht (CONTRL)|Schakel dit selectievakje in dat een technische (CONTRL) bevestiging naar de afzender interchange. De bevestiging wordt verzonden naar de interchange-afzender op basis van de instellingen voor verzenden voor de overeenkomst.|
|Bevestiging (CONTRL)|Schakel dit selectievakje in om terug te keren een functionele (CONTRL) bevestiging naar de afzender interchange de bevestiging wordt verzonden naar de interchange-afzender op basis van de instellingen voor verzenden voor de overeenkomst.|

### <a name="schemas"></a>Schema's maken

|Eigenschap|Beschrijving |
|----|----|
|UNH2.1 (TYPE)|Selecteer een type instellen.|
|UNH2.2 (VERSIE)|Voer in het versienummer van het bericht. (Minimum, één teken; maximum, drie tekens).|
|UNH2.3 (DEFINITIEVE VERSIE)|Voer het nummer van het release-programma. (Minimum, één teken; maximum, drie tekens).|
|UNH2.5 (GEKOPPELD TOEGEWEZEN CODE)|De toegewezen code invoeren. (Maximaal zes tekens. Moet alfanumerieke).|
|UNG2.1 (APP AFZENDER ID)|Voer een alfanumerieke waarde met ten minste één teken en een maximum van 35 tekens.|
|UNG2.2 (APP AFZENDER CODE KWALIFICATIE)|Voer een waarde alfanumerieke met maximaal vier tekens.|
|SCHEMA|Selecteer het eerder geüploade schema dat u wilt gebruiken vanaf uw gekoppelde integratie-Account.|

### <a name="control-numbers"></a>Besturingselement getallen

|Eigenschap|Beschrijving |
|----|----|
|Controle-aantal Interchange duplicaten weigeren|Schakel dit selectievakje dubbele knooppunten blokkeren. Als geselecteerd, wordt op basis van de EDIFACT decoderen actie checkt dat het interchange besturingselement getal (UNB5) voor de uitwisseling ontvangen niet overeenkomen met een eerder verwerkte interchange besturingselement getal. Als er een overeenkomst wordt gevonden, wordt het de uitwisseling niet is verwerkt.
|Controleren op dubbele UNB5 elke (dagen)|Als u ervoor hebt gekozen om dubbele interchange besturingselement getallen weigeren, kunt u het aantal dagen waarop de controle wordt uitgevoerd door de juiste waarde voor de optie **controleren op dubbele UNB5 elke (dagen)** .|
|Groep besturingselement getal duplicaten weigeren|Schakel dit selectievakje wilt blokkeren van knooppunten met dubbele groep besturingselement getallen (UNG5).|
|Transactie set besturingselement getal duplicaten weigeren|Schakel dit selectievakje wilt blokkeren van knooppunten met dubbele transactie set besturingselement getallen (UNH1).|
|EDIFACT bevestiging besturingselement getal|Als wilt toewijzen de transactie set verwijzing getallen in een bevestiging wordt gebruikt, voert u een waarde voor het voorvoegsel, een getallenbereik verwijzing en achtervoegsel.|

### <a name="validations"></a>Validatie

|Eigenschap|Beschrijving |
|----|----|
|Berichttype|Geef het berichttype. Als elke rij validatie is voltooid, wordt een andere automatisch toegevoegd. Als er geen regels zijn opgegeven, klikt u vervolgens de rij gemarkeerd als standaard wordt gebruikt voor validatie.|
|Validatie bewerken|Schakel dit selectievakje in om te bewerken validatie voeren op gegevenstypen gedefinieerd door de eigenschappen van het bewerken van het schema, lengte beperkingen, lege gegevenselementen en volgspaties scheidingstekens.|
|Uitgebreide gegevensvalidatie|Schakel dit selectievakje in om in te schakelen uitgebreide (XSD) validatie van knooppunten van de afzender interchange hebt ontvangen. Dit geldt ook voor validatie van Veldlengte, optionaliteit en aantal herhalingen naast XSD-type gegevensvalidatie.|
|Regelafstand/volgspaties nullen toestaan|Selecteer **toestaan** toe te staan dat regelafstand/nullen achter de komma; Nul wordt **NotAllowed** dat regelafstand/volgspaties nullen of **Knippen** knippen van de regelafstand en volgspaties niet zijn toegestaan.|
|Regelafstand/volgspaties nullen knippen|Schakel dit selectievakje in voor het knippen van eventuele nullen voorloop- of volgspaties|
|Scheidingsteken beleid volgspaties|Selecteer **Niet toegestaan** als u niet wilt dat aan het einde scheidingstekens en scheidingstekens in een interchange ontvangen van de afzender interchange zijn toegestaan. Als de uitwisseling volgspaties scheidingstekens en scheidingstekens bevat, is deze ongeldige gedeclareerd. Selecteer **optioneel** uitgewisseld met of zonder volgspaties scheidingstekens en scheidingstekens accepteren. Selecteer in de **verplicht** als de ontvangen uitwisseling moet volgspaties scheidingstekens en scheidingstekens bevatten.|

### <a name="internal-settings"></a>Interne instellingen

|Eigenschap|Beschrijving |
|----|----|
|Lege XML-codes maken als volgspaties scheidingstekens zijn toegestaan|Schakel dit selectievakje in om de interchange-afzender leeg XML-codes voor volgspaties scheidingstekens bevatten.|
|Binnenkomende batchen verwerking|Opties zijn:</br></br>**Gesplitste Interchange als transactie Sets - schorten transactie-Sets op fout**: parseert u elke transactie instellen in een interchange in een afzonderlijke XML-document door de juiste envelop toepassen op de set transactie. Met deze optie wordt als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens alleen die sets transactie zijn geschorst. Gesplitste Interchange als transactie Sets - schorten Interchange op fout: parseert u elke transactie instellen in een interchange in een afzonderlijke XML-document door de juiste envelop toe te voegen. Met deze optie wordt als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens de hele uitwisseling wordt geschorst.</br></br>**Behouden Interchange - transactie-Sets op fout schorten**: laat de uitwisseling intact, een XML-document voor de hele batch uitwisseling maken. Met deze optie wordt als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens alleen die sets transactie onderbroken, zolang alle andere transactie sets worden verwerkt.</br></br>**Behouden Interchange - schorten Interchange op fout**: laat de uitwisseling intact, een XML-document voor de hele batch uitwisseling maken. Met deze optie wordt als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens de hele uitwisseling is geschorst.|

Uw overeenkomst is klaar voor het verwerken van binnenkomende berichten die voldoen aan de instellingen die u hebt geselecteerd.

De instellingen die u naar partners verzendt e-mails configureren:  
10. Selecteer **Instellingen voor verzenden** naar configureren hoe berichten die zijn verzonden via deze overeenkomst moeten worden verwerkt.  

Het besturingselement instellingen voor verzenden is in de volgende secties, inclusief id's, bevestiging, schema's, enveloppen, tekensets en scheidingstekens, besturingselement getallen en gegevensvalidatie verdeeld. 

Hier ziet u een weergave van deze besturingselementen. De selecties op basis van wat u moet gebeuren van berichten die u naar partners via deze overeenkomst verzendt maken:   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)    
11. Selecteer de knop **OK** uw instellingen op te slaan.  

### <a name="identifiers"></a>Id 's
|Eigenschap|Beschrijving |
|----|----|
|UNB1.2 (syntaxis van de versie)|Selecteer een waarde tussen **1** en **4**.|
|UNB2.3 (afzender omgekeerde routeren adres)|Voer een alfanumerieke waarde met ten minste één teken en een maximum van 14 tekens.|
|UNB3.3 (adres geadresseerde omgekeerde routering)|Voer een alfanumerieke waarde met ten minste één teken en een maximum van 14 tekens.|
|UNB6.1 (geadresseerden verwijzing wachtwoord)|Voer een alfanumerieke waarde met minstens een en een maximum van 14 tekens.|
|UNB6.2 (geadresseerden verwijzing kwalificatie)|Voer een alfanumerieke waarde met ten minste één teken en een maximum van twee tekens.|
|UNB7 (toepassing verwijzing ID)|Voer een alfanumerieke waarde met ten minste één teken en een maximum van 14 tekens|

### <a name="acknowledgment"></a>Bevestiging
|Eigenschap|Beschrijving |
|----|----|
|Ontvangst van bericht (CONTRL)|Schakel dit selectievakje in als de gehoste partner verwacht te ontvangen als u wilt een technische (CONTRL) bevestiging ontvangen. Deze instelling wordt aangegeven dat de gehoste partner, die het bericht verzendt, om een bevestiging van de partner Gast vraagt.|
|Bevestiging (CONTRL)|Schakel dit selectievakje in als de gehoste partner verwacht een functionele (CONTRL) bevestiging ontvangen. Deze instelling wordt aangegeven dat de gehoste partner, die het bericht verzendt, om een bevestiging van de partner Gast vraagt.|
|Een loopback SG1/SG4 voor geaccepteerde transactie sets genereren|Als u ervoor kiest een functionele ontvangstbevestiging aanvragen, schakel dit selectievakje forceert generatie van SG1/SG4 lussen functionele CONTRL bevestigingen voor geaccepteerde transactie sets.|

### <a name="schemas"></a>Schema's maken
|Eigenschap|Beschrijving |
|----|----|
|UNH2.1 (TYPE)|Selecteer een type instellen.|
|UNH2.2 (VERSIE)|Voer in het versienummer van het bericht.|
|UNH2.3 (DEFINITIEVE VERSIE)|Voer het nummer van het release-programma.|
|SCHEMA|Selecteer het schema te gebruiken. Schema's bevinden zich in uw account integratie. Voor toegang tot uw schema's maken, moet u eerst uw integratie-account koppelen aan uw logica-app.|

### <a name="envelopes"></a>Enveloppen
|Eigenschap|Beschrijving |
|----|----|
|UNB8 (Processing prioriteitscode)|Voer een waarde in alfabetische dat wil niet meer dan één teken lang zeggen.|
|UNB10 (communicatie overeenkomst)|Voer een alfanumerieke waarde met ten minste één teken en maximaal 40 tekens.|
|UNB11 (testen Indicator)|Schakel dit selectievakje in om aan te geven dat de uitwisseling gegenereerd testgegevens|
|UNA Segment (Service tekenreeks advies) toepassen|Schakel dit selectievakje om te genereren van een segment UNA voor de uitwisseling te kunnen verzenden.|
|UNG segmenten (functie groepskoptekst) toepassen|Schakel dit selectievakje groeperen om segmenten te maken in de koptekst functionele groep in de berichten die zijn verzonden naar de partner Gast. De volgende waarden worden gebruikt om de UNG segmenten te maken:</br></br>Voer voor **UNG1**, een alfanumerieke waarde met ten minste één teken en een maximum van zes tekens.</br></br>Voer voor **UNG2.1**, een alfanumerieke waarde met ten minste één teken en een maximum van 35 tekens.</br></br>Voer voor **UNG2.2**, een alfanumerieke waarde, met maximaal vier tekens.</br></br>Voer voor **UNG3.1**, een alfanumerieke waarde met ten minste één teken en een maximum van 35 tekens.</br></br>Voer voor **UNG3.2**, een alfanumerieke waarde, met maximaal vier tekens.</br></br>Voer voor **UNG6**, een alfanumerieke waarde met minimaal één en maximaal drie tekens.</br></br>Voer voor **UNG7.1**, een alfanumerieke waarde met ten minste één teken en maximaal drie tekens.</br></br>Voer voor **UNG7.2**, een alfanumerieke waarde met ten minste één teken en maximaal drie tekens.</br></br>Voer voor **UNG7.3**, een alfanumerieke waarde met een minimum van 1 teken en een maximum van 6 tekens.</br></br>Voer voor **UNG8**, een alfanumerieke waarde met ten minste één teken en een maximum van 14 tekens.|

### <a name="character-sets-and-separators"></a>Teken Sets en scheidingstekens
Andere dan het teken dat is ingesteld, kunt u een andere set scheidingstekens moet worden gebruikt voor elk berichttype invoeren. Als een tekenset niet voor een bepaald berichtschema is opgegeven, wordt de standaard-tekenset gebruikt.

|Eigenschap|Beschrijving |
|----|----|
|UNB1.1 (systeem-id)|Selecteer de EDIFACT tekenset moeten worden toegepast op de uitgaande uitwisseling.|
|Schema|Selecteer een schema in de vervolgkeuzelijst. Een nieuwe rij wordt toegevoegd als elke rij is voltooid. Selecteer de scheidingstekens te kunnen gebruiken voor het geselecteerde schema:</br></br>**Onderdeel element scheidingsteken** – Enter een willekeurig teken voor het scheiden van samengestelde gegevenselementen.</br></br>**Gegevens Element scheidingsteken** – Enter een willekeurig teken voor het scheiden van eenvoudige gegevenselementen binnen samengestelde gegevenselementen.</br></br></br></br>**Vervangend teken** : Schakel dit selectievakje in als de gegevens bevatten nettolading tekens die ook worden gebruikt als gegevens, segment of onderdeel scheidingstekens. Vervolgens kunt u een vervangend teken. Als het uitgaande EDIFACT bericht worden gegenereerd, worden alle instanties van scheidingstekens in de nettolading gegevens worden vervangen door het opgegeven teken.</br></br>**Segment scheidingslijn** – Enter een willekeurig teken om aan te geven aan het einde van een segment bewerken.</br></br>**Achtervoegsel** – Selecteer het teken dat wordt gebruikt met het segment-id. Als u een achtervoegsel opgeeft, kan het segment scheidingslijn element leeg zijn. Als de scheidingslijn segment leeg laat, moet u een achtervoegsel aanwijzen.|

### <a name="control-numbers"></a>Besturingselement voor getallen
|Eigenschap|Beschrijving |
|----|----|
|UNB5 (Interchange besturingselement getal)|Voer een voorvoegsel, een bereik van waarden voor het getal interchange-besturingselement en achtervoegsel. Deze waarden worden gebruikt voor het genereren van een uitgaande interchange. De voor- en achtervoegsel zijn optioneel. het getal besturingselement is vereist. Het besturingselementnummer wordt verhoogd voor elk nieuw bericht; de voor- en achtervoegsel blijven hetzelfde.|
|UNG5 (groeperen besturingselement getal)|Voer een voorvoegsel, een bereik van waarden voor het getal interchange-besturingselement en achtervoegsel. Deze waarden worden gebruikt voor het genereren van de groep controle-aantal. De voor- en achtervoegsel zijn optioneel. het getal besturingselement is vereist. Het besturingselementnummer wordt verhoogd voor elk nieuw bericht totdat de maximumwaarde is bereikt. de voor- en achtervoegsel blijven hetzelfde.|
|UNH1 (Message referentienummer koptekst)|Voer een voorvoegsel, een bereik van waarden voor het getal interchange-besturingselement en achtervoegsel. Deze waarden worden gebruikt voor het genereren van het bericht koptekst nummer. De voor- en achtervoegsel zijn optioneel. het nummer is vereist. Het referentienummer wordt verhoogd voor elk nieuw bericht; de voor- en achtervoegsel blijven hetzelfde.|

### <a name="validations"></a>Validatie
|Eigenschap|Beschrijving |
|----|----|
|Berichttype|U deze optie selecteert, kunt de ontvanger interchange worden gevalideerd. Deze validatie worden bewerken gevalideerd transactie-set gegevenselementen, valideren gegevenstypen, lengte beperkingen en lege gegevenselementen en training scheidingstekens.|
|Validatie bewerken|Schakel dit selectievakje in om te bewerken validatie voeren op gegevenstypen gedefinieerd door de eigenschappen van het bewerken van het schema, lengte beperkingen, lege gegevenselementen en volgspaties scheidingstekens.|
|Uitgebreide gegevensvalidatie|Deze optie maakt uitgebreide validatie van knooppunten van de afzender interchange hebt ontvangen. Dit geldt ook voor validatie van Veldlengte, optionaliteit en aantal herhalingen naast XSD-type gegevensvalidatie. U kunt extensie validatie inschakelen zonder het activeren van validatie bewerken, of vice versa.|
|Nullen regelafstand/volgspaties toestaan|Deze optie wordt aangegeven dat een bewerken interchange hebt ontvangen van de partij wordt niet validatie is mislukt als een gegevenselement in een interchange bewerken niet aan de vereiste lengte vanwege of volgspaties voldoet, maar met de vereiste lengte overeen komt wanneer ze worden verwijderd.|
|Regelafstand/volgspaties nullen knippen|Als u deze optie selecteert, wordt de nullen voorloop- en volgspaties knippen.|
|Scheidingsteken volgspaties|Als wordt deze optie een bewerken interchange hebt ontvangen van de partij wordt validatie niet is mislukt als een gegevenselement in een interchange bewerken niet aan de vereiste lengte vanwege voorloop-(of volgspaties) nullen of volgspaties voldoet, maar met de vereiste lengte overeen komt wanneer ze worden verwijderd.</br></br>Selecteer **Niet toegestaan** als u niet wilt dat aan het einde scheidingstekens en scheidingstekens in een interchange ontvangen van de afzender interchange zijn toegestaan. Als de uitwisseling volgspaties scheidingstekens en scheidingstekens bevat, is deze ongeldige gedeclareerd.</br></br>Selecteer **optioneel** uitgewisseld met of zonder volgspaties scheidingstekens en scheidingstekens accepteren.</br></br>Selecteer in de **verplicht** als de ontvangen uitwisseling moet volgspaties scheidingstekens en scheidingstekens bevatten.|

Klik op **OK** nadat u op het blad geopend:  
12. Selecteer de tegel **overeenkomsten** op het blad integratie-Account en ziet u de zojuist toegevoegde overeenkomst wordt vermeld.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)   

## <a name="learn-more"></a>Meer informatie
- [Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack")  
