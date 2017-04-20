<properties
   pageTitle="Schaalbaarheid van webservice | Microsoft Azure"
   description="Leer hoe u een webservice met groter wordende gelijktijdigheid en het toevoegen van nieuwe eindpunten wilt verkleinen."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure machine learning-, webservices, uitoefening, schaalbaarheid, eindpunt, bij gelijktijdigheid"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Schaalbaarheid van een webservice

>[AZURE.NOTE] In dit onderwerp wordt beschreven technieken van toepassing op een klassieke Machine Learning-Web-service. 

Standaard wordt elke gepubliceerde Web-service is geconfigureerd voor ondersteuning van 20 gelijktijdige aanvragen en mag maximaal 200 gelijktijdige aanvragen. Terwijl de Azure klassieke portal een manier biedt om deze waarde, Azure Machine Learning optimaliseert automatisch de instelling op te geven van de beste prestaties voor uw webservice en de portal waarde wordt genegeerd. 

Als u de API met een hogere laden dan de waarde van een Max gelijktijdige oproepen van 200 aan te roepen abonnement ondersteunt, moet u meerdere eindpunten maken op dezelfde webservice. U kunt uw laden willekeurig verdelen over alle labels.

## <a name="add-new-endpoints-for-same-web-service"></a>Nieuwe eindpunten voor dezelfde webservice toevoegen

De schaal van een webservice is een algemene taak. Er zijn enkele redenen aan de nieuwe schaal ondersteuning voor meer dan 200 gelijktijdige aanvragen, beschikbaarheid tot en met meerdere eindpunten vergroten of afzonderlijke eindpunten bieden voor de webservice. U kunt de schaal vergroten door het toevoegen van extra eindpunten voor dezelfde webservice via [Azure klassieke portal](https://manage.windowsazure.com/) of de [Webservice van Azure Machine Learning](https://services.azureml.net/) -portal.

Zie voor meer informatie over het toevoegen van nieuwe eindpunten [Eindpunten maken](machine-learning-create-endpoint.md).

Houd er rekening mee dat met een hoge gelijktijdigheid telling schadelijk als u niet de API met een overeenkomstig hoge snelheid belt. Ziet u mogelijk enkel geval-outs en/of pieken in de latentie als u een lage laden in een API die is geconfigureerd voor hoge belasting plaatsen.

De synchroon API's worden meestal gebruikt in situaties waarin een lage latentie gewenst is. Latentie hier houdt de tijd die het duurt voordat de API om een aanvraag te voltooien en niet-account voor eventuele vertragingen in het netwerk. Stel dat u hebt een API met een latentie 50-ms. Volledig in beslag neemt de beschikbare capaciteit met vertraging niveau Hoog en Max gelijktijdige gesprekken = 20, moet u deze API 20 * 1000 bellen / 50 = 400 times per seconde. Een Max gelijktijdige oproepen van 200 kunt dit verder uitgebreid, u de tijden API 4000 per seconde, ervan uitgaande dat er een latentie 50-ms bellen.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
