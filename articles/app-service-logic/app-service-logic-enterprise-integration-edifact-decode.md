<properties 
    pageTitle="Meer informatie over Enterprise Integration Pack decoderen EDIFACT bericht verbindingslijn | App-Service van Microsoft Azure | Microsoft Azure" 
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

# <a name="get-started-with-decode-edifact-message"></a>Aan de slag met decoderen EDIFACT bericht

Bewerken en partner-specifieke eigenschappen is gevalideerd, wordt gegenereerd XML-document voor elke transactie set en genereert bevestiging voor verwerkte transactie.

## <a name="create-the-connection"></a>De verbinding maken

### <a name="prerequisites"></a>Vereisten voor

* Een Azure-account; u kunt een [gratis account](https://azure.microsoft.com/free) maken

* Een Account-integratie is vereist voor het decoderen EDIFACT bericht connector gebruiken. Zie meer informatie over het maken van een [Account van de integratie](./app-service-logic-enterprise-integration-create-integration-account.md), [partners](./app-service-logic-enterprise-integration-partners.md) en [EDIFACT overeenkomst](./app-service-logic-enterprise-integration-edifact.md)

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Verbinding maken met decoderen EDIFACT bericht met de volgende stappen:

1. [Een App logica maken](./app-service-logic-create-a-logic-app.md) wordt een voorbeeld.

2. Deze verbindingslijn is geen eventuele triggers. Gebruik andere triggers de logica-App, zoals een aanvraag-trigger te starten.  Klik in de ontwerpfunctie logica App een trigger toevoegen en een actie toevoegen.  Selecteer Microsoft weergeven beheerde API's in de vervolgkeuzelijst lijst en voer vervolgens 'EDIFACT' in het zoekvak.  Selecteer decoderen EDIFACT bericht

    ![EDIFACT zoeken](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Als u geen verbindingen met integratie Account nog niet eerder hebt gemaakt, wordt u gevraagd om de verbindingsgegevens

    ![integratie-account maken](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Geef de details van de integratie van Account.  Eigenschappen met een sterretje zijn vereist

  	| Eigenschap | Meer informatie |
  	| -------- | ------- |
  	| Verbindingsnaam * | Voer een naam voor de verbinding |
  	| Integratie van Account * | Voer de naam van de integratie van Account. Zorg ervoor dat uw Account van de integratie en logica app zich in dezelfde Azure locatie |

    Als voltooid, de verbindingsdetails van uw aangepaste gegevens er ongeveer als volgt uit

    ![integratie van account is gemaakt](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Selecteer **maken**

6. Zoals u ziet dat de verbinding is gemaakt

    ![integratie van verbinding accountgegevens](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. Selecteer EDIFACT plat bestand bericht decoderen

    ![Geef verplichte velden](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>EDIFACT decoderen heeft volgen

* De overeenkomst oplossen door het overeenkomen met de afzender kwalificatie & id en ontvanger kwalificatie & id
* Meerdere knooppunten in een enkel bericht in afzonderlijke gesplitst.
* De envelop tegen handelsdag partnerovereenkomst is gevalideerd
* De uitwisseling ontleed.
* Bewerken en partner-specifieke eigenschappen is gevalideerd bevat
    * Validatie van de structuur van de envelop interchange.
    * Schemavalidatie van de envelop ten opzichte van het besturingselement schema.
    * Schemavalidatie van de gegevenselementen transactie-set ten opzichte van het berichtschema.
    * Bewerken gevalideerd gegevenselementen transactie-instellen
* Controleert of de interchange, groep en transactie set besturingselement getallen zijn geen dubbele waarden (indien geconfigureerd) 
    * Hiermee wordt het getal voor het beheer van interchange ten opzichte van eerder ontvangen knooppunten gecontroleerd. 
    * Hiermee wordt het getal voor het beheer van groep ten opzichte van andere groep besturingselement getallen in de uitwisseling gecontroleerd. 
    * Hiermee wordt gecontroleerd op dat de transactie besturingselementnummer instellen ten opzichte van andere getallen transactie set besturingselement in die groep.
* Genereert een XML-document voor elke transactie-set.
* De hele uitwisseling converteert naar XML 
    * Gesplitste Interchange als transactie sets - schorten transactie sets op fout: parseert u elke transactie instellen in een interchange in een afzonderlijke XML-document. Als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens alleen die sets transactie EDIFACT decoderen onderbreekt. 
    * Gesplitste Interchange als transactie sets - schorten interchange op fout: parseert u elke transactie instellen in een interchange in een afzonderlijke XML-document.  Als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens de hele uitwisseling EDIFACT decoderen onderbreekt.
    * Behouden Interchange - transactie sets schorten op fout: Hiermee maakt u een XML-document voor de hele batch uitwisseling. EDIFACT decoderen onderbreekt die transactie-sets waarvan de validatie, terwijl u alle andere transactie-sets van proces is mislukt
    * Behouden Interchange - interchange schorten op fout: Hiermee maakt u een XML-document voor de hele batch uitwisseling. Als een of meer transactie Hiermee stelt u in de uitwisseling validatie is mislukt, en vervolgens de hele uitwisseling EDIFACT decoderen onderbreekt 
* Genereert een technische (besturingselement) en/of functionele bevestiging (indien geconfigureerd).
    * Een technische bevestiging of de CONTRL ACK-rapporten de resultaten van een syntactische controle van de volledige ontvangen uitwisseling.
    * Een functionele bevestiging bevestigt accepteren of negeren van een ontvangen interchange of een groep

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack") 