<properties 
    pageTitle="Meer informatie over Enterprise Integration Pack decoderen AS2 bericht Connctor | App-Service van Microsoft Azure | Microsoft Azure" 
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

# <a name="get-started-with-decode-as2-message"></a>Aan de slag met decoderen AS2 bericht

Verbinding maken met decoderen AS2 bericht tot stand brengen van beveiliging en betrouwbaarheid tijdens het overbrengen van berichten. Dit zorgt voor digitale ondertekening, decoderen en bevestigingen via een bericht voor bestemming meldingen (MDN).

## <a name="create-the-connection"></a>De verbinding maken

### <a name="prerequisites"></a>Vereisten voor

* Een Azure-account; u kunt een [gratis account](https://azure.microsoft.com/free) maken

* Een Account-integratie is vereist voor het decoderen AS2 bericht connector gebruiken. Zie meer informatie over het maken van een [Account van de integratie](./app-service-logic-enterprise-integration-create-integration-account.md), [partners](./app-service-logic-enterprise-integration-partners.md) en een [AS2 overeenkomst](./app-service-logic-enterprise-integration-as2.md)

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Verbinding maken met decoderen AS2 bericht met de volgende stappen:

1. [Een App logica maken](./app-service-logic-create-a-logic-app.md) wordt een voorbeeld.

2. Deze verbindingslijn is geen eventuele triggers. Gebruik andere triggers de logica-App, zoals een aanvraag-trigger te starten.  Klik in de ontwerpfunctie logica App een trigger toevoegen en een actie toevoegen.  Selecteer Microsoft weergeven beheerde API's in de vervolgkeuzelijst lijst en voer vervolgens 'AS2' in het zoekvak.  Selecteer AS2 – decoderen AS2 bericht

    ![AS2 zoeken](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Als u geen verbindingen met integratie Account nog niet eerder hebt gemaakt, wordt u gevraagd om de verbindingsgegevens

    ![Integratie van verbinding maken](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Geef de details van de integratie van Account.  Eigenschappen met een sterretje zijn vereist

  	| Eigenschap   | Meer informatie |
  	| --------   | ------- |
  	| Verbindingsnaam *    | Voer een naam voor de verbinding |
  	| Integratie van Account * | Voer de naam van de integratie van Account. Zorg ervoor dat uw Account van de integratie en logica app zich in dezelfde Azure locatie |

    Als voltooid, de verbindingsdetails van uw aangepaste gegevens er ongeveer als volgt uit

    ![integratie van verbinding](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Selecteer **maken**
    
6. Zoals u ziet dat de verbinding is gemaakt.  Nu gaat u verder met de overige stappen in de logica-App

    ![integratie van verbinding is gemaakt](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. Selecteer hoofdtekst en koppen in de uitvoer van de aanvraag

    ![Geef verplichte velden](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>De AS2 decoderen gebeurt het volgende

* AS2/HTTP-headers verwerkt
* Verifieert de handtekening (indien geconfigureerd)
* De berichten ontsleutelt (indien geconfigureerd)
* Het bericht decomprimeert (indien geconfigureerd)
* Een ontvangen MDN met het oorspronkelijke uitgaande bericht Verenigd
* Updates en verbindt records in de database niet afwijzen
* Records voor het melden van AS2 status schrijft
* De inhoud van de nettolading uitvoer zijn base64 codering
* Bepaalt of een MDN vereist is, en of de MDN synchroon moet of asynchroon op basis van configuratie in AS2 overeenkomst
* Genereert een synchroon of asynchroon MDN (gebaseerd op overeenkomst configuraties)
* De eigenschappen en correlatietokens ingesteld op de MDN

##<a name="try-it-for-yourself"></a>Probeer het zelf

Waarom zou u niet zelf proberen. Klik [hier](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) om te implementeren van een volledig operationele logica-app van uw eigen via de logica Apps AS2-functies 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het taalpakket Enterprise-integratie] (./app-service-logic-enterprise-integration-overview.md "Meer informatie over Enterprise Integration Pack") 