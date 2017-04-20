<properties
 pageTitle="Scheduler hoge beschikbaarheid en betrouwbaarheid"
 description="Scheduler hoge beschikbaarheid en betrouwbaarheid"
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
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>Scheduler hoge beschikbaarheid en betrouwbaarheid

## <a name="azure-scheduler-high-availability"></a>Azure Scheduler hoge beschikbaarheid

Als een core Azure platform-service, Azure Scheduler ten zeerste beschikbaar is en zowel geografische-redundante service-implementatie- en geografische regio taak replicatie-functies.

### <a name="geo-redundant-service-deployment"></a>Geografische-redundante service-implementatie

Azure Scheduler is beschikbaar via de gebruikersinterface in bijna elke geografische gebied dat in Azure vandaag. De lijst met regio's die Azure Scheduler is beschikbaar in is [Hier wordt vermeld](https://azure.microsoft.com/regions/#services). Als een datacenter in een gehoste regio wordt opgebouwd niet beschikbaar zijn, zijn de mogelijkheden failover van Azure Scheduler zodat de service beschikbaar in een andere datacenter is.

### <a name="geo-regional-job-replication"></a>Geografische regio taak herhaling

Niet alleen wordt de planner Azure front beschikbaar voor management aanvragen, maar uw eigen taak is ook geografische gerepliceerd. Als er een storing in één regio, wordt Azure Scheduler wordt overgenomen en zorgt ervoor dat de taak wordt uitgevoerd vanuit een ander datacenter in de gepaarde geografische regio.

Als u een taak in Zuid centraal ons hebt gemaakt, wordt uw Azure Scheduler bijvoorbeeld automatisch gerepliceerd die taak in Noord centraal ons. Als er een fout in Zuid centraal ons, Azure Scheduler zorgt ervoor dat de taak wordt uitgevoerd van Noord centraal ons. 

![][1]

Hierdoor Azure Scheduler zorgt ervoor dat uw gegevens binnen dezelfde bredere geografische regio voor het geval een Azure is mislukt blijven. Hierdoor u hoeft niet uw taak alleen als u wilt toevoegen van beschikbaarheid dupliceren: Azure Scheduler biedt automatisch hoge beschikbaarheid mogelijkheden voor uw taken.

## <a name="azure-scheduler-reliability"></a>Azure Scheduler betrouwbaarheid

Azure Scheduler garandeert eigen hoge beschikbaarheid en een andere benadering gaat naar gebruiker gemaakte taken. Uw taak kan bijvoorbeeld een HTTP-eindpunt dat is niet beschikbaar aanroepen. Azure Scheduler wil toch uw taak is uitgevoerd doordat u alternatieve opties te handelen is mislukt. Azure Scheduler doet dit op twee manieren:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Configureerbare opnieuw beleid via "retryPolicy"

Azure Scheduler kunt u een beleid opnieuw configureren. Standaard als een taak mislukt, Scheduler wordt geprobeerd de taak opnieuw vier vaker, tijdsintervallen van 30 seconden. U kunt dit beleid opnieuw als u wilt meer agressieve (bijvoorbeeld tien keer tijdsintervallen van 30 seconden) opnieuw configureren of lossere (bijvoorbeeld twee keer met dagelijkse tussenpozen.)

Voorbeeld van wanneer Hiermee kunt kunt u een taak die wordt uitgevoerd eenmaal per week en roept een HTTP-eindpunt maken. Als het HTTP-eindpunt niet actief voor een paar uur wanneer uw taak wordt uitgevoerd is, wilt u mogelijk niet meer weekweergave voor de taak uit te voeren opnieuw omdat zelfs het standaardbeleid voor nieuwe pogingen, mislukt wacht. In dat geval u mogelijk de configuratie van het beleid standaard opnieuw om de drie uur (bijvoorbeeld) opnieuw uit te voeren in plaats van elke 30 seconden.

Als u wilt weten hoe u een beleid opnieuw configureren, raadpleegt u [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternatieve eindpunt configureerbaarheid via "errorAction"

Als het eindpunt doel voor uw taak Azure Scheduler niet bereikbaar blijft, valt Azure Scheduler terug naar het alternatieve foutverwerking eindpunt na het uitvoeren van haar beleid opnieuw. Als een alternatieve foutverwerking eindpunt is geconfigureerd, roept Azure Scheduler deze. Uw eigen taken zijn met een alternatieve eindpunt ten zeerste beschikbaar licht is mislukt.

Als u bijvoorbeeld in het onderstaande diagram volgt Azure Scheduler haar beleid opnieuw als u wilt een webservice New York raken. Nadat het nieuwe pogingen mislukt, er wordt gecontroleerd of er momenteel een alternatief is. Vervolgens gaat u verder en begint het aanvragen in de alternatieve met hetzelfde opnieuw beleid.

![][2]

Houd er rekening mee dat het hetzelfde opnieuw-beleid van toepassing op zowel de oorspronkelijke actie en de alternatieve fout. Het is ook mogelijk om van de alternatieve fout actie actietype afwijken van de belangrijkste actie actietype. Bijvoorbeeld terwijl de aanroepen van de belangrijkste actie mogelijk een HTTP-eindpunt, de actie fout in plaats daarvan mogelijk een opslag wachtrij, service bus wachtrij of service bus onderwerp actie die registratie van aanmeldingsfouten doet.

Als u wilt weten hoe u een alternatieve eindpunt configureren, raadpleegt u [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Zie ook

 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler concepten, terminologie en entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met Scheduler in de portal van Azure](scheduler-get-started-portal.md)

 [Abonnementen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Het maken van complexe planningen en geavanceerde terugkeerpatroon met Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler REST API-verwijzing](https://msdn.microsoft.com/library/mt629143)

 [Overzicht van Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler limieten, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
