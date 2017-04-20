<properties
   pageTitle="EAI logica App VETR gebruiken in logica apps in Azure App Service maken | Microsoft Azure"
   description="Valideer de, coderen en transformeren van functies van BizTalk XML-services"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>EAI logica App met VETR maken

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

De meeste Enterprise Application Integration (EAI)-scenario's worden gegevens tussen een bron- en een bestemming. Scenario's bestaan vaak uit een gemeenschappelijke set met vereisten:

- Zorg ervoor dat de gegevens uit andere systemen voor correct zijn opgemaakt.
- "Zoekopties" uitvoeren op binnenkomende gegevens om de beslissingen nemen.
- Gegevens van de ene indeling geconverteerd naar een andere. Gegevens uit de gegevensindeling van een systeem CRM bijvoorbeeld converteren naar de gegevensindeling van een ERP-systeem.
- De gegevens van de routeren naar de gewenste toepassing of systeem.

In dit artikel leest u een algemene integratie patroon: 'Eén bericht bemiddeling' of VETR (valideren, toegang bieden, transformeren, doorsturen). Het patroon VETR doorgeeft gegevens tussen een bronentiteit en een entiteit bestemming. De bron- en doeltabellen zijn meestal gegevensbronnen.

Houd rekening met een website waarop orders accepteert. Gebruikers berichten plaatsen orders aan het systeem met behulp van HTTP. Achter de schermen, het systeem is gevalideerd met de binnenkomende gegevens voor correct normaliseert deze en deze in een Service Bus wachtrij voor nadere verwerking zich blijft voordoen. Het systeem gaat orders uit de wachtrij, verwacht in een bepaalde indeling. De stroom end-to-end is dus:

**HTTP** → **valideren** → **Transformeren** → **Service Bus**

![Eenvoudige VETR stroom][1]

De volgende BizTalk-API-Apps te maken van dit patroon:

* **HTTP-Trigger** - bron naar activeringsgebeurtenis bericht
* **Valideren** - is gevalideerd met binnenkomende gegevens
* **Transformeren** - transformaties gegevens uit de volgende systeem vereist voor inkomende format-indeling
* **Service-busconnector** - bestemming entiteit waar gegevens worden verzonden


## <a name="constructing-the-basic-vetr-pattern"></a>Het bouwen van VETR patroon
### <a name="the-basics"></a>De basisbeginselen

Klik in de portal Azure Selecteer **+ Nieuw** **Web + Mobile**, en selecteer **Logica-App**. Kies een naam, locatie, abonnement, resourcegroep en locatie die werkt. Resourcegroepen fungeren als containers voor uw apps; alle bronnen voor uw app Ga naar dezelfde resourcegroep.

Vervolgens triggers en acties toevoegen.


## <a name="add-http-trigger"></a>HTTP-Trigger toevoegen
1. Selecteer in de **Logica App-sjablonen** **maken op basis van maken**.
1. Selecteer **HTTP luisteraar ervan af** vanuit de galerie maken van een nieuwe luisteraar ervan af. Roep deze **HTTP1**.
2. Stel de **automatisch antwoord verzenden?** stellen op onwaar. De triggeractie door de instelling _HTTP-methode_ op _bericht_ en instelling _Relatieve URL_ naar _/OneWayPipeline_configureren:  
    ![HTTP-Trigger][2]
3. Selecteer het groene vinkje om te voltooien van de trigger.

## <a name="add-validate-action"></a>Toevoegen actie valideren

Nu kunnen we invoeren die worden uitgevoerd wanneer de trigger wordt uitgevoerd, dat wil zeggen, wanneer een oproep ontvangt op het HTTP-eindpunt.

1. **BizTalk XML-validatie** toevoegen vanuit de galerie en noem deze _(Validate1)_ een exemplaar te maken.
2. Een XSD-schema voor het valideren van de binnenkomende XML-berichten configureren. Selecteer de actie _valideren_ en selecteer _triggers('httplistener').outputs. Inhoud_ als de waarde voor de parameter _inputXml_ .

De actie valideren is nu de eerste actie na de HTTP luisteraar ervan af: 

![XML-BizTalk validatie][3]

Laten we toevoegen op dezelfde manier de rest van de acties. 

## <a name="add-transform-action"></a>Transformeren actie toevoegen
Laten we configureren transformaties om de binnenkomende gegevens te normaliseren.

1. **BizTalk transformeren Service** uit de galerie toevoegen.
2. Als u wilt configureren om te transformeren van de binnenkomende berichten van de XML-transformatie, selecteert u de actie **Transformeren** als de actie moet worden uitgevoerd als deze API wordt genoemd. Selecteer ```triggers(‘httplistener’).outputs.Content``` als de waarde voor _inputXml_. *Toewijzing* is een optionele parameter sinds de binnenkomende gegevens zijn gekoppeld aan alle geconfigureerde transformaties en die voldoen aan het schema zijn toegepast.
3. Ten slotte de transformeren uitgevoerd alleen als de valideren is geslaagd. Als u wilt configureren met deze voorwaarde, selecteer het tandwielpictogram rechtsboven in het scherm en selecteert u _een voorwaarde moet worden voldaan toevoegen_. Stel op ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![BizTalk transformaties][4]


## <a name="add-service-bus-connector"></a>Service-busconnector toevoegen
Vervolgens de bestemming toevoegen, een Service Bus wachtrij, gegevens om te schrijven.

1. Een **Service busconnector** toevoegen vanuit de galerie. De **naam** ingesteld op _Servicebus1_, **Verbindingsreeks** ingesteld op de verbindingsreeks op uw exemplaar van de bus service, **Naam van entiteit** naar _wachtrij_instellen en **de naam van abonnement**overslaan.
2. Selecteer de actie **Bericht verzenden** en stel het veld **inhoud** voor de actie op _actions('transformservice').outputs. OutputXml_.
3. Het veld **Inhoudstype** *toepassing/xml*instellen:  

![Service Bus][5]


## <a name="send-http-response"></a>HTTP-antwoord verzenden
Als verkooppijplijn verwerking is voltooid, terug een HTTP-antwoord verzenden voor zowel geslaagde en mislukte met de volgende stappen uit:

1. Het toevoegen van een **HTTP luisteraar ervan af** vanuit de galerie en selecteert u de actie **HTTP-antwoord verzenden** .
2. Stel **antwoord-ID** om *bericht*te verzenden.
2. Stel **Antwoord inhoud** op *verkooppijplijn verwerking is voltooid*.
3. **Statuscode antwoord** op *200* om aan te geven HTTP 200 OK.
4. Selecteer de vervolgkeuzelijst rechtsboven in het scherm en selecteert u **een voorwaarde moet worden voldaan toevoegen**.  Stel op de volgende expressie:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Herhaal deze stappen als u wilt een HTTP-antwoord verzenden op ook is mislukt. **Voorwaarde** wijzigen in de volgende expressie:  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Selecteer **OK** en vervolgens **maken**.



## <a name="completion"></a>Voltooid
Telkens wanneer iemand een bericht naar het eindpunt HTTP stuurt, activeert de app en voert de acties die u zojuist hebt gemaakt. Voor het beheren van dergelijke logica apps maken, selecteert u **Bladeren** in de Portal Azure en selecteer **Logica Apps**. Selecteer uw app voor meer informatie.

Deze onderwerpen nuttig:

[Beheren en controleren van uw API-Apps en verbindingslijnen](app-service-logic-monitor-your-connectors.md)  <br/>
[Uw Apps logica controleren](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
