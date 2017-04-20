
<properties
    pageTitle="De HTTP + Swagger toevoegen actie in logica apps | Microsoft Azure"
    description="Overzicht van de HTTP + Swagger actie en bedrijfsactiviteiten"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http--swagger-action"></a>Aan de slag met de HTTP + Swagger actie

Met de HTTP + Swagger actie, kunt u een eersteklas connector aan een eindpunt REST tot en met een [Swagger document](https://swagger.io)maken. U kunt ook een app logica om te bellen van een REST-eindpunt een eersteklas logica App Designer-ervaring uitbreiden.

U kunt beginnen met de HTTP + Swagger actie in een logica-app, raadpleegt u [een nieuwe logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Gebruik HTTP + Swagger als een trigger of een actie

De HTTP + Swagger trigger en een actie werken op dezelfde manier als de [actie HTTP](connectors-native-http.md) maar om er een eenvoudiger ontwerp zelf door de vorm van de API en de uitvoer van in de ontwerpfunctie van de [metagegevens Swagger](https://swagger.io). Bovendien kunt u HTTP + Swagger als een trigger. Als u implementeren van een peiling-trigger wilt, zou dit het polling-patroon dat wordt beschreven in het [maken van een aangepaste API voor gebruik met logica apps](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers)volgen.

[Meer informatie over de logica app triggers en acties.](connectors-overview.md)

Hier volgt een voorbeeld van de manier waarop naar gebruik de HTTP + Swagger bewerking als een actie in een werkstroom in een app logica.

1. Selecteer de knop **Nieuwe stap** .
2. Selecteer **een actie toevoegen**.
3. Typ **swagger** aan de lijst de HTTP-Swagger actie in het zoekvak actie.

    ![Selecteer HTTP & Swagger actie](./media/connectors-native-http-swagger/using-action-1.png)

4. Typ de URL voor een document Swagger:
    - Als u wilt werken vanuit de logica App Designer, moet de URL een HTTPS-eindpunt en CORS hebt ingeschakeld.
    - Als het document Swagger niet deze vereiste voldoet, kunt u [Opslagruimte Azure met CORS ingeschakeld](#hosting-swagger-from-storage) voor de opslag van het document.
5. Klik op **volgende** om te lezen en weer te geven uit het document Swagger.
6. In alle parameters die vereist voor de HTTP-oproep zijn toevoegen.

    ![HTTP-bewerking voltooien](./media/connectors-native-http-swagger/using-action-2.png)

1. Klik op **Opslaan** in de linkerbovenhoek van de werkbalk en uw logica app is beide opslaan en publiceren (activeren).

### <a name="host-swagger-from-azure-storage"></a>Host Swagger van Azure opslag

Het is raadzaam om te verwijzen naar een document Swagger die niet wordt gehost, of die niet voldoet aan de veiligheid en cross-origin vereisten voor de ontwerpfunctie. U kunt u lost dit probleem op door het Swagger-document opslaan in Azure Storage en inschakelen van CORS om te verwijzen naar het document.  

Hier volgen de stappen voor het maken, configureren en Swagger documenten opslaan in Azure Storage:

1. [Een account Azure opslagruimte met Azure-blobopslag maken](../storage/storage-create-storage-account.md). (Klik hiertoe machtigingen instellen voor **toegang tot openbare**.)
2. CORS inschakelen in de blob. U kunt [deze PowerShell-script](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) configureren die instelling automatisch.
3. Upload het bestand Swagger naar de blob. U kunt dit doen vanuit de [Azure-portal](https://portal.azure.com) of een hulpmiddel zoals [Azure opslag Explorer](http://storageexplorer.com/).
1. Verwijzingen maken naar een HTTPS-koppeling naar het document in Azure-blobopslag. (De koppeling volgt op de indeling `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`.)



## <a name="technical-details"></a>Technische details

Hier volgen de details in voor de triggers en acties die deze HTTP + Swagger verbindingslijn ondersteunt.

## <a name="http--swagger-triggers"></a>HTTP + Swagger triggers

Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die gedefinieerd in een app logica te starten. [Meer informatie over triggers.](connectors-overview.md) De HTTP + Swagger verbindingslijn heeft één trigger.

|Trigger|Beschrijving|
|---|---|
|HTTP + Swagger|Een gesprek HTTP en de inhoud van de reactie terug te keren|

## <a name="http--swagger-actions"></a>HTTP + Swagger acties

Een actie is een bewerking die wordt uitgevoerd door de werkstroom die gedefinieerd in een app logica. [Meer informatie over acties.](connectors-overview.md) De HTTP + Swagger verbindingslijn heeft een mogelijke actie.

|Actie|Beschrijving|
|---|---|
|HTTP + Swagger|Een gesprek HTTP en de inhoud van de reactie terug te keren|

### <a name="action-details"></a>Actiedetails

De HTTP + Swagger verbindingslijn wordt geleverd met één mogelijke actie. Hier volgt informatie over elk van de acties, de vereiste en optionele invoervelden en de corresponderende gegevens voor uitvoer die zijn gekoppeld aan hun gebruik.

#### <a name="http--swagger"></a>HTTP + Swagger

Een uitgaande HTTP-verzoek met ondersteuning van Swagger metagegevens maken
Een sterretje (*) betekent dat er een vereist veld.

|Weergavenaam|Naam van eigenschap|Beschrijving|
|---|---|---|
|Methode *|methode|HTTP-opdracht om te gebruiken.|
|URI *|URI|URI voor de HTTP-verzoek.|
|Kopteksten|kopteksten|Een object JSON HTTP-headers om op te nemen.|
|Hoofdtekst|hoofdtekst|Het hoofdgedeelte van de aanvraag.|
|Verificatie|verificatie|Verificatie voor de aanvraag wilt gebruiken. [Zie voor meer informatie, HTTP](./connectors-native-http.md#authentication).|

**Gegevens voor uitvoer**

HTTP-antwoord

|Naam van eigenschap|Gegevenstype|Beschrijving|
|---|---|---|
|Kopteksten|object|Antwoord kopteksten|
|Hoofdtekst|object|Antwoordobject|
|Statuscode|int|HTTP-statuscode|

### <a name="http-responses"></a>HTTP-antwoorden

Wanneer u oproepen naar verschillende acties, krijgt u mogelijk bepaalde antwoorden. Hier volgt een tabel staat bijbehorende antwoorden en beschrijvingen aangegeven.

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout opgetreden.|

---

## <a name="next-steps"></a>Volgende stappen

Probeer het platform en [een app logica maakt](../app-service-logic/app-service-logic-create-a-logic-app.md) nu. Door te kijken onze [lijst met API's](apis-list.md)vindt u de andere beschikbare connectors in logica-apps.
