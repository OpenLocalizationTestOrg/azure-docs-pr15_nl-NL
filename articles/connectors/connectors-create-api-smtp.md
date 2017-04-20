<properties
pageTitle="SMTP | Microsoft Azure"
description="Logica apps maken met de App Azure-service. Verbinding maken met SMTP om e-mail te verzenden."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>Aan de slag met de SMTP-connector

Verbinding maken met SMTP om e-mail te verzenden.

Als u wilt gebruiken op [een verbindingslijn](./apis-list.md), moet u eerst een logica-app maakt. U kunt aan de slag door nu een logica-app te [maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>Verbinding maken met SMTP

Voordat uw app logica toegang elke service tot, moet u eerst een *verbinding* met de service te maken. Een [verbinding](./connectors-overview.md) biedt connectiviteit tussen een logica-app en een andere service. Bijvoorbeeld wilt verbinden met SMTP, moet u eerst een SMTP- *verbinding*. Een verbinding wilt maken, moet u de referenties die u normaal gesproken gebruiken voor toegang tot de service die u wilt verbinden met op te geven. Zo is, in het voorbeeld SMTP moet u de referenties voor uw verbindingsnaam, adres van de SMTP-server en foutieve aanmeldinformatie die gebruiker om te maken van de verbinding met SMTP. [Meer informatie over verbindingen]()  

### <a name="create-a-connection-to-smtp"></a>Een verbinding maken met SMTP

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>Gebruik een SMTP-trigger

Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een app logica te starten. [Meer informatie over activering](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

In dit voorbeeld omdat SMTP geen een trigger zelf, gebruiken we de trigger **Salesforce - wanneer een object wordt gemaakt** . Deze trigger wordt geactiveerd wanneer een nieuw object wordt gemaakt in Salesforce. In ons voorbeeld kunt we instellen dat telkens wanneer een nieuwe potentiële klant is gemaakt in Salesforce, de actie voor een *e-mailbericht verzenden* deze gebeurtenis vindt plaats via de SMTP-connector met een melding van de nieuwe potentiële klant wordt gemaakt.

1. *Salesforce* invoeren in het zoekvak op de ontwerpfunctie voor logica-apps en selecteer vervolgens de trigger **Salesforce - wanneer een object wordt gemaakt** .  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. Het besturingselement **als een object is gemaakt** , wordt weergegeven.
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. Selecteer het **Type Object** en selecteer vervolgens *potentiële klant* in de lijst met objecten. In deze stap geeft u aan dat u een trigger die uw app logica gewaarschuwd als een nieuwe potentiële klant is gemaakt in Salesforce maakt.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. De trigger is gemaakt.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Gebruik een SMTP-actie

Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een app logica. [Meer informatie over acties](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Nu dat de trigger is toegevoegd, volgt u deze stappen als een SMTP-actie dat plaatsvinden zal wanneer een nieuwe potentiële klant is gemaakt in Salesforce wilt toevoegen.

1. Selecteer **+ nieuwe stap** om toe te voegen van de actie die u uitvoeren wilt wanneer aan een nieuwe potentiële klant is gemaakt.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. Selecteer **een actie toevoegen**. Dit wordt geopend zodra het zoekvak waarin u voor een actie u zoeken kunt wilt uitvoeren.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. Voer *smtp* om te zoeken naar acties die betrekking hebben op SMTP.  

4. Selecteer **SMTP - e-mailbericht verzenden** als de actie moet uitvoeren wanneer aan de nieuwe potentiële klant is gemaakt. Hiermee opent u de actie in het Configuratiescherm. U hebt uw smtp om verbinding te maken in de ontwerpfunctie blokkeren als u niet hebt gedaan eerder.  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. Input uw gewenste e-mailgegevens in het blok **SMTP - e-mailbericht verzenden** .  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. Sla uw werk om te activeren van de werkstroom.  

## <a name="technical-details"></a>Technische details

Hier volgen de details van de triggers, acties en -antwoorden die ondersteuning biedt voor deze verbinding:

## <a name="smtp-triggers"></a>SMTP-triggers

SMTP heeft geen triggers. 

## <a name="smtp-actions"></a>SMTP-acties

SMTP heeft de volgende actie:


|Actie|Beschrijving|
|--- | ---|
|[E-mailbericht verzenden](connectors-create-api-smtp.md#send-email)|Deze bewerking stuurt een e-mailbericht naar een of meer geadresseerden.|

### <a name="action-details"></a>Actiedetails

Hier volgen de details in voor de actie van deze verbindingslijn, samen met de bijbehorende antwoorden:


### <a name="send-email"></a>E-mailbericht verzenden
Deze bewerking stuurt een e-mailbericht naar een of meer geadresseerden. 


|Naam van eigenschap| Weergavenaam|Beschrijving|
| ---|---|---|
|Aan|Aan|E-mailadressen gescheiden door puntkomma's zoals opgevenrecipient1@domain.com;recipient2@domain.com|
|CC|CC|E-mailadressen gescheiden door puntkomma's zoals opgevenrecipient1@domain.com;recipient2@domain.com|
|Onderwerp|Onderwerp|E-onderwerp|
|Hoofdtekst|Hoofdtekst|Hoofdtekst van e-mail|
|Van|Van|E-mailadres van de afzender zoalssender@domain.com|
|IsHtml|Waarschuwing|Het e-mailbericht verzenden als HTML (waar/onwaar)|
|BCC|BCC|E-mailadressen gescheiden door puntkomma's zoals opgevenrecipient1@domain.com;recipient2@domain.com|
|Urgentie|Urgentie|Belang van het e-mailbericht (hoog, normaal of laag)|
|ContentData|Bijlagen inhoud gegevens|Inhoud van de gegevens (base64 gecodeerd voor gegevensstromen en als-is bedoeld voor tekenreeks)|
|ContentType|Inhoudstype van bijlagen|Inhoudstype|
|ContentTransferEncoding|Bijlagen inhoud codering voor overdracht|Inhoud overbrengen codering (base64 of geen)|
|FileName|Bijlagen-bestandsnaam|Bestandsnaam|
|ContentId|Bijlagen inhoud-ID|Inhoud-id|

Een * geeft aan dat een eigenschap vereist is


## <a name="http-responses"></a>HTTP-antwoorden

De acties en triggers bovenstaande kunnen retourneren een of meer van de volgende codes van de HTTP-status: 

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout opgetreden.|
|Standaard|Bewerking is mislukt.|

## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)