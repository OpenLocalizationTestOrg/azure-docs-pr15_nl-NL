<properties 
    pageTitle="Meer informatie over Enterprise Integration Pack decoderen X12 bericht Connctor | App-Service van Microsoft Azure | Microsoft Azure" 
    description="Informatie over het gebruik van partners met de apps Enterprise Integration Pack en logica" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-x12-message"></a>Aan de slag met decoderen X12 bericht

Bewerken en partner-specifieke eigenschappen is gevalideerd, wordt gegenereerd XML-document voor elke transactie set en genereert bevestiging voor verwerkte transactie.

## <a name="create-the-connection"></a>De verbinding maken

### <a name="prerequisites"></a>Vereisten voor

* Een Azure-account; u kunt een [gratis account](https://azure.microsoft.com/free) maken

* Een Account-integratie is vereist voor het gebruiken van decoderen X12 bericht verbindingslijn. Zie meer informatie over het maken van een [Account van de integratie](./app-service-logic-enterprise-integration-create-integration-account.md), [partners](./app-service-logic-enterprise-integration-partners.md) en [X12 overeenkomst](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Verbinding maken met decoderen X12 bericht met de volgende stappen:

1. Wordt een voorbeeld van [een logica-App maken](./app-service-logic-create-a-logic-app.md)

2. Deze verbindingslijn is geen eventuele triggers. Gebruik andere triggers de logica-App, zoals een aanvraag-trigger te starten.  Klik in de ontwerpfunctie logica App een trigger toevoegen en een actie toevoegen.  Selecteer Microsoft beheerde API's in de vervolgkeuzelijst lijst en voer vervolgens "x12" weergeven in het zoekvak.  Selecteer X12 – X12 decoderen bericht

    ![x12 zoeken](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)  

3. Als u geen verbindingen met integratie Account nog niet eerder hebt gemaakt, wordt u gevraagd om de verbindingsgegevens

    ![verbinding voor integratie-account](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)    

4. Geef de details van de integratie van Account.  Eigenschappen met een sterretje zijn vereist

  	| Eigenschap | Meer informatie |
  	| -------- | ------- |
  	| Verbindingsnaam * | Voer een naam voor de verbinding |
  	| Integratie van Account * | Voer de naam van de integratie van Account. Zorg ervoor dat uw Account van de integratie en logica app zich in dezelfde Azure locatie |

    Als voltooid, de verbindingsdetails van uw aangepaste gegevens er ongeveer als volgt uit
    
    ![integratie van accountverbinding is gemaakt](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png) 

5. Selecteer **maken**
    
6. Zoals u ziet dat de verbinding is gemaakt.

    ![integratie van verbinding accountgegevens](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png) 

7. Selecteer X12 platte bestand bericht decoderen

    ![Geef verplichte velden](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>X12 decoderen doet volgen

* De envelop tegen handelsdag partnerovereenkomst is gevalideerd
* Genereert een XML-document voor elke transactie-set.
* Bewerken en partner-specifieke eigenschappen is gevalideerd
    * Structurele validatie bewerken en uitgebreide schema gegevensvalidatie
    * Validatie van de structuur van de envelop interchange.
    * Schemavalidatie van de envelop ten opzichte van het besturingselement schema.
    * Schemavalidatie van de gegevenselementen transactie-set ten opzichte van het berichtschema.
    * Bewerken gevalideerd gegevenselementen transactie-instellen 
* Controleert of de interchange, groep en transactie set besturingselement getallen zijn geen dubbele waarden
    * Hiermee wordt het getal voor het beheer van interchange ten opzichte van eerder ontvangen knooppunten gecontroleerd.
    * Hiermee wordt het getal voor het beheer van groep ten opzichte van andere groep besturingselement getallen in de uitwisseling gecontroleerd.
    * Hiermee wordt gecontroleerd op dat de transactie besturingselementnummer instellen ten opzichte van andere getallen transactie set besturingselement in die groep.
* De hele uitwisseling converteert naar XML 
    * Gesplitste Interchange als transactie sets - schorten transactie sets op fout: parseert u elke transactie instellen in een interchange in een afzonderlijke XML-document. Als een of meer transactie Hiermee stelt u in de uitwisseling validatie, X12 is mislukt decoderen onderbreekt alleen die sets transactie.
    * Gesplitste Interchange als transactie sets - schorten interchange op fout: parseert u elke transactie instellen in een interchange in een afzonderlijke XML-document.  Als een of meer transactie Hiermee stelt u in de uitwisseling validatie, X12 is mislukt decoderen onderbreekt de hele uitwisseling.
    * Behouden Interchange - transactie sets schorten op fout: Hiermee maakt u een XML-document voor de hele batch uitwisseling. X12 decoderen onderbreekt die transactie-sets waarvan de validatie is mislukt, terwijl u aan alle andere transactie verwerken sets
    * Behouden Interchange - interchange schorten op fout: Hiermee maakt u een XML-document voor de hele batch uitwisseling. Als een of meer transactie Hiermee stelt u in de uitwisseling validatie, X12 is mislukt decoderen onderbreekt de hele uitwisseling 
* Genereert een bevestiging technische en/of functionele (indien geconfigureerd).
    * Een technische bevestiging genereert grond koptekst gegevensvalidatie. De technische bevestiging rapporten de status van de verwerking van een interchange kop- en aanhangwagen door de ontvanger adres.
    * Een functionele bevestiging genereert grond hoofdtekst gegevensvalidatie. De functionele bevestiging rapporten elke fout opgetreden tijdens het verwerken van het document ontvangen

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack") 


