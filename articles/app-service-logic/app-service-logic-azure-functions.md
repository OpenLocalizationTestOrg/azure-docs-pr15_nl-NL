<properties
   pageTitle="Gebruik van Azure functies met logica Apps | Microsoft Azure"
   description="Lees hoe u met Azure functies met logica Apps"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Gebruik van Azure functies met logica Apps

U kunt aangepaste codefragmenten C# of node.js uitvoeren met behulp van Azure-functies uit binnen een logica-app.  [Azure-functies](../azure-functions/functions-overview.md) biedt server-vrije computing in Microsoft Azure. Dit is handig voor de uitvoering van de volgende taken uitvoeren:

* Geavanceerde opmaak of berekenen van velden in een App logica
* Berekeningen uitvoeren in een werkstroom
* De functionaliteit van de logica Apps met de functies die worden ondersteund in C# of node.js uitbreiden

## <a name="create-a-function-for-logic-apps"></a>Een functie voor logica-Apps maken

Het is raadzaam dat u een nieuwe functie in de portal Azure functies wilt maken met behulp van de **Algemene Webhook - knooppunt** of **Algemene Webhook - C#** -sjablonen. Hierdoor automatisch-wordt gevuld een sjabloon die accepteert `application/json` via een logica-app.  Als u het tabblad **integreren** in Azure-functies kunt deze **modus** ingesteld op **Webhook** en **Webhook type** **Algemene JSON**moet hebben.  Functies die gebruikmaken van deze sjablonen automatisch gedetecteerd en weergegeven in de ontwerpfunctie logica Apps onder **Azure-functies in mijn land.**

Webhook functies een verzoek accepteren en binnenkomen de methode via een `data` variabele. U kunt de eigenschappen van uw nettolading openen met de stip notatie zoals `data.foo`.  Een eenvoudige JavaScript-functie die een DateTime-waarde naar een datumtekenreeks converteert ziet er bijvoorbeeld als volgt uit:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Azure-functies aanroepen vanuit een logica-app

Als u op het menu **Acties** , kunt u in de ontwerpfunctie **Azure-functies in mijn regio**selecteren.  Hiermee Hier staan de containers in uw abonnement en kunt u kiezen van de functie die u wilt bellen.  

Nadat u de functie selecteert, wordt u gevraagd om op te geven van een object invoer nettolading. Dit is het bericht dat de logica-app wordt verzonden naar de functie en een object JSON moet worden. Bijvoorbeeld als u doorgeven in de datum **Laatst gewijzigd** van een Salesforce-trigger wilt, de functie nettolading als volgt uitzien:

![Laatste modfied datum][1]

## <a name="trigger-logic-apps-from-a-function"></a>Trigger logica apps van de functie

Het is ook mogelijk om te activeren van een app logica uit binnen een functie.  Hiervoor moet u gewoon een logica-app maken met een handmatige trigger. Zie [logica apps als aanroepbare eindpunten](app-service-logic-http-endpoint.md)voor meer informatie.  Genereer vervolgens binnen uw functie, een HTTP POST naar de URL handmatig trigger met het pakket dat u wilt verzenden naar de logica-app.

### <a name="create-a-function-from-the-designer"></a>Maken van een functie van de ontwerpfunctie

U kunt ook een webhook functie node.js binnen de ontwerpfunctie van maken. Eerst **Azure-functies in mijn regio,** selecteren en kies vervolgens een container voor uw functie.  Als u een container nog niet hebt, moet u maken via de [portal van Azure-functies](https://functions.azure.com/signin). Selecteer **Nieuw**.  

Als u wilt genereren van een sjabloon op basis van de gegevens die u wilt berekenen, geef het contextobject dat u van plan bent om door te geven in een functie. Dit moet een JSON-object. Als u in de inhoud van het bestand uit een FTP-actie doorgeeft, kunt u voor de nettolading context, wordt er zo uit:

![Context nettolading][2]

>[AZURE.NOTE] Omdat dit object is niet beschouwd als een tekenreeks, wordt de inhoud die rechtstreeks naar de JSON-nettolading worden toegevoegd. Dit wordt echter fout af als deze is niet een token JSON (dat wil zeggen, een tekenreeks of een JSON object/matrix). Als u wilt deze als een tekenreeks beschouwd, toe te voegen offertes zoals weergegeven in de eerste afbeelding in dit artikel.

De ontwerpfunctie genereert vervolgens een functie sjabloon dat u inline kunt maken. Variabelen zijn vooraf gemaakt op basis van de context die u van plan bent om door te geven in de functie.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
