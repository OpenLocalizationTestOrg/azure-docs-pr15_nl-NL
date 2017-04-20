<properties
   pageTitle="Hoe u de schaal van Azure functies | Microsoft Azure"
   description="Begrijpen hoe de schaal van Azure functies aanpassen aan de behoeften van uw werkbelasting gebeurtenisgestuurde."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure-functies, functies, verwerking van de gebeurtenis, webhooks, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Hoe u de schaal van Azure-functies

## <a name="introduction"></a>Inleiding

Een voordeel van Azure functies is berekeningscluster resources worden alleen verbruikt als dat nodig is. Dit betekent dat u niet voor niet-actieve VMs betalen of reserveren capaciteit voor wanneer u deze mogelijk nodig hebt. In plaats daarvan het platform rekenkracht toegewezen wanneer uw code wordt uitgevoerd, wordt aangepast omhoog zo nodig worden afgehandeld laden en vervolgens omlaag wordt aangepast wanneer code niet wordt uitgevoerd.

De om deze nieuwe functionaliteit is de dynamische serviceplan.  

Dit artikel bevat een overzicht van de werking van de dynamische serviceplan en hoe het platform worden op aanvraag uw code uit te voeren.

Als u nog niet bekend bent met Azure-functies, zorg ervoor dat u het artikel [overzicht van Azure-functies](functions-overview.md) voor een beter begrip van de mogelijkheden controleren.

## <a name="configure-azure-functions"></a>Azure functies configureren

Instellingen voor twee belangrijkste hebben betrekking op schaal:

* [Azure-App serviceplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) of dynamische serviceplan
* Grootte van geheugen voor de uitvoeringsomgeving

De kosten van een functie veranderen afhankelijk van het abonnement dat die u selecteert. Kosten zijn met een dynamische serviceplan, een functie van de tijd, de geheugengrootte en aantal executions. Kosten toenemen alleen wanneer uw code daadwerkelijk uitgevoerd.

Een App-serviceplan host uw functies op bestaande VMs, die ook andere code uit te voeren kunnen worden gebruikt. Nadat u voor deze VMs elke maand betaalt, moet u er gratis voor uitvoering-functies worden is.

## <a name="choose-a-service-plan"></a>Kies een abonnement dat is

Wanneer u functies maakt, kunt u deze niet uitvoeren op een dynamische serviceplan of een [App-abonnement](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
In het App-abonnement, kunnen uw functies worden uitgevoerd op een speciale VM, net als de web-apps werken vandaag voor Basic, Standard of Premium SKU's.
Deze speciale VM is toegewezen aan uw apps en -functies en is altijd beschikbaar of code wordt actief uitgevoerd of niet. Dit is een goede optie als u bestaande, onder gebruikt VMs waarop andere code al hebt of als u verwacht of uit te voeren functies continu vrijwel continu. Een VM decouples kosten van zowel runtime en geheugen grootte. Hierdoor kunt u de kosten van veel langdurige functies en de kosten van de een of meer VMs die ze worden uitgevoerd op beperken.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
