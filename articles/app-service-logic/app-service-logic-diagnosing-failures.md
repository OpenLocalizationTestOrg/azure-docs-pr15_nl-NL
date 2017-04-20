<properties
   pageTitle="Logica apps fouten diagnose | Microsoft Azure"
   description="Algemene manieren lidmaatschap waar logica apps worden verbroken"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="diagnosing-logic-app-failures"></a>Diagnose logica app fouten

Als u problemen of fouten met de functie logica Apps van Azure App Service ondervindt, kunt een paar manieren u beter te begrijpen waar de fouten die zijn afkomstig zijn uit.  

## <a name="azure-portal-tools"></a>Azure portal hulpmiddelen

De Azure-portal bevat veel functies om een diagnose stellen bij elke app logica bij elke stap.

### <a name="trigger-history"></a>Trigger geschiedenis

Elke app logica heeft ten minste één trigger. Als u merkt dat apps worden niet gebruikt, is de eerste plaats om aanvullende informatie te vinden die de geschiedenis trigger. U kunt de geschiedenis trigger op het logica app belangrijkste blad openen.

![De geschiedenis trigger zoeken][1]

Hier vindt u alle van de trigger pogingen die uw app logica heeft aangebracht. U kunt klikken op elke trigger poging om het volgende detailniveau (name invoer of uitvoer van die de trigger poging gegenereerd). Als u eventuele mislukte triggers ziet, klikt u op de trigger poging en inzoomen op de koppeling **uitvoer** om foutberichten die mogelijk zijn gegenereerd (bijvoorbeeld voor ongeldige FTP-referenties) weer te geven.

De verschillende statussen ziet u mogelijk zijn:

* **Overgeslagen**. Deze doorzocht van het eindpunt om gegevens te controleren en een antwoord hebt ontvangen dat geen gegevens beschikbaar is.
* **Is voltooid**. De trigger ontvangen een antwoord dat er gegevens beschikbaar is. Dit kan zijn van een handmatige trigger, een trigger terugkeerpatroon of een peiling-trigger. Dit waarschijnlijk moet worden voorzien met de status van **Fired**, maar deze mogelijk niet als u een voorwaarde of SplitOn opdracht in de weergave van een code die niet is voldaan.
* **Is mislukt**. Er is een fout gegenereerd.

#### <a name="starting-a-trigger-manually"></a>Een trigger starten handmatig

Als u de app logica om te controleren voor een beschikbaar trigger direct (zonder wachten op het volgende terugkeerpatroon) wilt, kunt u **Trigger selecteren** klikken op het belangrijkste blad afdwingen van een selectievakje. Bijvoorbeeld op deze koppeling met een Dropbox-trigger klikt, wordt de werkstroom onmiddellijk poll uitvoeren onder Dropbox voor nieuwe bestanden.

### <a name="run-history"></a>Geschiedenis uitvoeren

Elke trigger die is gestart resultaten in een uitvoeren. U kunt informatie over openen vanaf het belangrijkste blad, met een groot aantal gegevens die mogelijk beter begrijpen wat is er gebeurd tijdens de werkstroom.

![Zoeken naar de uitvoeren geschiedenis][2]

Een uitvoert, verschijnt er een van de volgende statussen:

* **Is voltooid**. Alle acties is geslaagd of, als er een fout opgetreden is, deze door een actie die zijn aangebracht later in de werkstroom is verwerkt. Dit is dat wil zeggen verwerkt door een actie die is ingesteld op na een mislukte actie uitvoeren.
* **Is mislukt**. Ten minste één actie hebt u er een fout die niet door een actie later in de werkstroom is verwerkt.
* **Geannuleerd**. De werkstroom is uitgevoerd, maar ontvangen een verzoek om te annuleren.
* **Uitgevoerd**. De werkstroom wordt uitgevoerd. Dit kan gebeuren voor werkstromen die zijn wordt vertraagd of vanwege de huidige App Service-abonnement. Zie limieten voor actie op de [pagina prijzen](https://azure.microsoft.com/pricing/details/app-service/plans/) voor meer informatie. Diagnostische gegevens configureren kunnen (de grafieken de uitvoeren geschiedenis onderstaande) ook bevatten informatie over eventuele vertraging gebeurtenissen die optreden.

Wanneer u een uitvoeren geschiedenis bekijkt, kunt u uitzoomen voor meer informatie.  

#### <a name="trigger-outputs"></a>Uitvoer van trigger

Uitvoer van trigger weergeven de gegevens die is ontvangen van de trigger. Hiermee kunt u bepalen of alle eigenschappen weergegeven zoals verwacht.

>[AZURE.NOTE] Handig het kan zijn om te begrijpen hoe de logica Apps aanbevelen [verschillende inhoudstypen verwerkt](app-service-logic-content-type.md) als u alle inhoud die u niet begrijpt ziet.

![Trigger uitvoer voorbeelden][3]

#### <a name="action-inputs-and-outputs"></a>Actie invoer en uitvoer

U kunt inzoomen op de invoer en uitvoer die een actie ontvangen. Dit is handig voor informatie over de grootte en de vorm van de uitvoer, ook als om te zien foutberichten die mogelijk zijn gegenereerd.

![Actie invoer en uitvoer][4]

## <a name="debugging-workflow-runtime"></a>Voor foutopsporing in werkstroom runtime

Naast het controleren van de invoer, uitvoer en triggers van een reeks, kan het handig Voeg enkele stappen binnen een werkstroom om u te helpen met foutopsporing zijn. [RequestBin](http://requestb.in) is een krachtige hulpprogramma die u als een stap in een werkstroom kunt toevoegen. Met behulp van RequestBin, kunt u een controle van de aanvraag HTTP instellen om te bepalen de exacte grootte, de vorm en de opmaak van een HTTP-aanvraag. U kunt maken van een nieuwe RequestBin en plak de URL in een app logica HTTP POST actie samen met de inhoud van hoofdtekst die u wilt testen (bijvoorbeeld een expressie of een andere stap uitvoer). Nadat u de app logica uitvoert, kunt u uw RequestBin om te zien hoe de aanvraag is gevormd vernieuwen als deze is gegenereerd op basis van de logica Apps-engine.




<!-- image references -->
[1]: ./media/app-service-logic-diagnosing-failures/triggerHistory.PNG
[2]: ./media/app-service-logic-diagnosing-failures/runHistory.PNG
[3]: ./media/app-service-logic-diagnosing-failures/triggerOutputsLink.PNG
[4]: ./media/app-service-logic-diagnosing-failures/ActionOutputs.PNG
