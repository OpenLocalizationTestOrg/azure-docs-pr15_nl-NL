<properties
 pageTitle="Scheduler limieten en standaardwaarden voor"
 description="Scheduler limieten en standaardwaarden voor"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-limits-and-defaults"></a>Scheduler limieten en standaardwaarden voor

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Quota voor Scheduler, beperkingen, standaardwaarden en Throttles

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>De kop van de x-ms-aanvraag-id

Elke aanvraag ten opzichte van de Scheduler-service retourneert een antwoord koptekst met de naam**x-ms-aanvraag-id**. Deze koptekst bevat een ondoorzichtig waarde die deze identificeert op het verzoek.

Als een verzoek voor een consistente is verbroken en u hebt gecontroleerd, dat het verzoek goed geformuleerd is, kunt u deze waarde om de fout naar Microsoft. In het rapport bevat de waarde van x-ms-aanvraag-id, bij benadering de tijd die het verzoek is gemaakt, de id van het abonnement, taak verzameling.een Help-verzameling en/of taak en het type bewerking die het verzoek probeert toe.

## <a name="see-also"></a>Zie ook


 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler concepten, terminologie en entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met Scheduler in de portal van Azure](scheduler-get-started-portal.md)

 [Abonnementen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler REST API-verwijzing](https://msdn.microsoft.com/library/mt629143)

 [Overzicht van Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)
