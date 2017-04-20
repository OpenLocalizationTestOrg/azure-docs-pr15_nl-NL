<properties 
    pageTitle="Meer informatie over Enterprise Integration Pack coderen X12 bericht Connctor | App-Service van Microsoft Azure | Microsoft Azure" 
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

# <a name="get-started-with-encode-x12-message"></a>Aan de slag met coderen X12 bericht

Bewerken en partner-specifieke eigenschappen is gevalideerd, XML-codering berichten worden geconverteerd naar bewerken transactie sets in de uitwisseling en een bevestiging technische en/of functionele aanvraagt

## <a name="create-the-connection"></a>De verbinding maken

### <a name="prerequisites"></a>Vereisten voor

* Een Azure-account; u kunt een [gratis account](https://azure.microsoft.com/free) maken

* Een Account-integratie is vereist voor het coderen x12 bericht connector gebruiken. Zie meer informatie over het maken van een [Account van de integratie](./app-service-logic-enterprise-integration-create-integration-account.md), [partners](./app-service-logic-enterprise-integration-partners.md) en [X12 overeenkomst](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Verbinding maken met coderen X12 bericht met de volgende stappen:

1. Wordt een voorbeeld van [een logica-App maken](./app-service-logic-create-a-logic-app.md)

2. Deze verbindingslijn is geen eventuele triggers. Gebruik andere triggers de logica-App, zoals een aanvraag-trigger te starten.  Klik in de ontwerpfunctie logica App een trigger toevoegen en een actie toevoegen.  Selecteer Microsoft beheerde API's in de vervolgkeuzelijst lijst en voer vervolgens "x12" weergeven in het zoekvak.  Selecteer een van beide X12-X12 coderen bericht door de Overeenkomstnaam van de of X12-X-12-bericht coderen met identiteiten.  

    ![x12 zoeken](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. Als u geen verbindingen met integratie Account nog niet eerder hebt gemaakt, wordt u gevraagd om de verbindingsgegevens

    ![verbinding voor integratie-account](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. Geef de details van de integratie van Account.  Eigenschappen met een sterretje zijn vereist

  	| Eigenschap | Meer informatie |
  	| -------- | ------- |
  	| Verbindingsnaam * | Voer een naam voor de verbinding |
  	| Integratie van Account * | Voer de naam van de integratie van Account. Zorg ervoor dat uw Account van de integratie en logica app zich in dezelfde Azure locatie |

    Als voltooid, de verbindingsdetails van uw aangepaste gegevens er ongeveer als volgt uit

    ![integratie van accountverbinding is gemaakt](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. Selecteer **maken**

6. Zoals u ziet dat de verbinding is gemaakt.

    ![integratie van verbinding accountgegevens](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12-coderen X12 bericht met de Overeenkomstnaam van de

7. Selecteer X12 overeenkomst uit de vervolgkeuzelijst en XML-berichten coderen.

    ![Geef verplichte velden](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12-coderen X12 bericht met een identiteiten

7.  Bieden afzender-id, afzender kwalificatie, ontvanger-id en ontvanger kwalificatie zoals geconfigureerd in de X12 overeenkomst.  XML-bericht selecteert om coderen

    ![Geef verplichte velden](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>X12 coderen doet volgen:

* De resolutie van de overeenkomst door overeenkomen met eigenschappen van verzenders en ontvangers context.
* De uitwisseling van bewerken, converteren van een XML-codering berichten in bewerken transactie sets in de uitwisseling serialiseert.
* Van toepassing transactie set kop- en aanhangwagen segmenten
* Genereert een nummer interchange-besturingselement, het nummer van een besturingselement en een transactie set besturingselement nummer voor elke uitgaande interchange
* Hiermee vervangt u scheidingstekens in de nettolading
* Bewerken en partner-specifieke eigenschappen is gevalideerd
    * Schemavalidatie van de transactie-set gegevenselementen ten opzichte van het bericht Schema
    * Bewerken gevalideerd gegevenselementen transactie-instellen.
    * Uitgebreide gevalideerd gegevenselementen transactie-instellen
* Vraagt om een bevestiging technische en/of functionele (indien geconfigureerd).
    * Een technische bevestiging genereert grond koptekst gegevensvalidatie. De status van de verwerking van een interchange kop- en aanhangwagen door de ontvanger adres van de technische bevestiging-rapporten
    * Een functionele bevestiging genereert grond hoofdtekst gegevensvalidatie. De functionele bevestiging rapporten elke fout opgetreden tijdens het verwerken van het document ontvangen

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack") 

