<properties
    pageTitle="Sessie affiniteit met behulp van de Azure-Toolkit voor Eclips inschakelen"
    description="Informatie over het inschakelen van sessie affiniteit met behulp van de Azure-Toolkit voor Eclips."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Sessie affiniteit inschakelen #

Binnen de Toolkit Azure voor Eclips, kunt u HTTP-sessie affiniteit of "Plaktoetsen sessies', voor uw rollen inschakelen. De volgende afbeelding ziet u de eigenschappen van **Taakverdeling** dialoogvenster gebruikt voor de sessie affiniteit inschakelen:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Aan de sessie affiniteit voor uw rol inschakelen ##

1. Met de rechtermuisknop op de rol in Projectverkenner Eclips van **Azure**op en klik vervolgens op **Taakverdeling**.
1. In het dialoogvenster **Eigenschappen voor WorkerRole1 van taakverdeling** :
    1. Controleer **inschakelen HTTP-sessie affiniteit (Plaktoetsen sessies) voor deze rol.**
    1. Voor **invoer eindpunt te gebruiken**, selecteert u een invoer eindpunt te gebruiken, bijvoorbeeld **http (openbare: 80, privé: 8080)**. Uw toepassing moet dit eindpunt gebruiken als het HTTP-eindpunt. U kunt meerdere eindpunten inschakelen voor uw functie, maar u kunt slechts één van deze ter ondersteuning van Plaktoetsen sessies selecteren.
    1. Bouw die tabellen opnieuw uw toepassing.

Eenmaal is ingeschakeld, hebt u meer dan één exemplaar van de rol, blijft HTTP-aanvragen die afkomstig zijn uit een bepaalde client worden verwerkt door het exemplaar van dezelfde rol.

De Toolkit Eclips maakt dit mogelijk door de installatie van een speciaal IIS-module toepassing aanvragen routering (ARR) in elk van uw rol exemplaren genoemd. ARR HTTP-aanvragen in het exemplaar van de juiste rol wordt omgeleid. De toolkit opnieuw automatisch geconfigureerd voor het geselecteerde eindpunt, zodat de binnenkomende HTTP-verkeer eerst gerouteerd naar de software ARR. De toolkit ook Hiermee maakt u een nieuw interne eindpunt die uw Java-server is geconfigureerd om te beluisteren. Dit is het eindpunt dat wordt gebruikt door ARR voor de HTTP-verkeer in het exemplaar van de juiste rol omleiden. Op deze manier elk exemplaar van de rol in uw implementatie meerdere exemplaren fungeert als omgekeerde proxy voor alle andere exemplaren, Plaktoetsen sessies inschakelen.

## <a name="notes-about-session-affinity"></a>Opmerkingen over sessie affiniteit ##

* Sessie affiniteit werkt niet in de emulator berekeningscluster. De instellingen in de emulator berekeningscluster kunnen worden toegepast zonder uw proces opbouwen verhinderd of berekenen emulator worden uitgevoerd, maar de functie zelf werkt niet binnen de emulator berekeningscluster.
* Inschakelen van sessie affiniteit leidt tot een toename van de schijfruimte van uw implementatie in Azure wordt aangegeven, zoals extra software wordt gedownload en geïnstalleerd in uw rol exemplaren tijdens uw service wordt gestart in de cloud Azure.
* De tijd aan elke rol duurt langer.
* Een interne eindpunt, als een rerouter verkeer bovengenoemde, worden added.ss

Lees [hoe u gegevens op de sessie voor het behoud van met sessie affiniteit][]voor een voorbeeld van de manier waarop sessiegegevens voor het behoud van wanneer sessie affiniteit is ingeschakeld.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Maken van een toepassing van de wereld Hallo voor Azure in Eclips][]

[Installatie van de Azure Toolkit voor Eclips][] 

[Het beheren van sessiegegevens met sessie affiniteit][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Het beheren van sessiegegevens met sessie affiniteit]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png
