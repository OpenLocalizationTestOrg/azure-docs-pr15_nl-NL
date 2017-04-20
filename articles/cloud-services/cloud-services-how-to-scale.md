<properties
    pageTitle="Automatisch de schaal van een cloudservice in de portal | Microsoft Azure"
    description="(klassieke) Informatie over het gebruik van de klassieke portal voor het configureren van regels voor automatisch schaal voor een cloud-service Webrol of werknemer rol in Azure wordt aangegeven."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Hoe u automatisch schalen een cloudservice

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-scale-portal.md)
- [Azure klassieke portal](cloud-services-how-to-scale.md)

Klik op de pagina schaal van de Azure klassieke portal u kunt handmatig de rol van de werknemer van uw Webrol of schalen of kunt u de automatische schaling op basis van CPU laden of een berichtenwachtrij inschakelen.

>[AZURE.NOTE] In dit artikel ligt de nadruk op het web en werknemer rollen Cloudservice. Wanneer u een virtuele machine (klassieke) rechtstreeks maakt, wordt deze gehost in een cloudservice. Sommige van deze informatie geldt voor deze soorten virtuele machines. Schaalbaarheid van een set beschikbaarheid van virtuele machines is echt alleen afgesloten ze in- of uitschakelen op basis van de schaal regels die u wilt configureren. Zie voor meer informatie over virtuele Machines en beschikbaarheid sets [beheren de beschikbaarheid van virtuele Machines](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

Voordat u de schaal voor uw toepassing configureren, moet u de volgende informatie overwegen:

- Schaalbaarheid wordt beïnvloed door core gebruik. Grotere rol exemplaren gebruiken meer cores. U kunt de schaal van een toepassing alleen binnen de limiet van cores voor uw abonnement. Als uw abonnement geldt een limiet van twintig gietkernen en u een toepassing met twee gemiddeld grootte uitvoeren bijvoorbeeld cloud services (een totaal van vier cores), kunt u alleen de schaal van de implementaties van andere cloud-service in uw abonnement door 16 cores. Zie [Cloud Service grootten](cloud-services-sizes-specs.md) voor meer informatie over grootte.

- U moet een wachtrij maken en deze koppelen aan een rol voordat u kunt een toepassing op basis van een bericht drempel schalen. Zie [het gebruik van de wachtrij Storage-Service](../storage/storage-dotnet-how-to-use-queues.md)voor meer informatie.

- U kunt resources die zijn gekoppeld aan uw cloudservice schalen. Zie voor meer informatie over het koppelen van resources, [hoe: een resource koppelen naar een cloudservice](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Als u wilt inschakelen betere beschikbaarheid van uw toepassing, moet u ervoor zorgen dat deze wordt gedistribueerd met twee of meer exemplaren van de rol. Zie [Niveau serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/)voor meer informatie.



## <a name="schedule-scaling"></a>Schaalbaarheid van de planning

Standaard alle rollen niet op een specifieke planning. Daarom toepassen gewijzigd instellingen op alle tijden en alle dagen hele jaar. Als u wilt, kunt u handmatig of automatisch schaalbaarheid voor kunt instellen:

- Weekdagen
- Weekends
- Week overnachtingen
- Week voor u uitvoeren
- Specifieke datums
- Specifiek datumbereik

Dit is conigured in de [klassieke Azure-portal](https://manage.windowsazure.com/) op de  
**Cloud Services** > **\[uw cloudservice\]** > **schaal** > **\[productie of tijdelijke\] ** pagina.

Klik op de knop **planning tijden instellen** voor elke functie die u wilt wijzigen.

![Cloud service Automatische schaling op basis van een planning][scale_schedules]



## <a name="manual-scale"></a>Handmatige schaal

Klik op de pagina **schaal** kunt u handmatig vergroot of verkleint het aantal exemplaren uitgevoerd in een cloudservice. Dit is geconfigureerd voor elke planning die u hebt gemaakt of naar alle tijd als u niet een planning hebt gemaakt.

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com/) **Cloudservices**op en klik vervolgens op de naam van de cloudservice om te openen van het dashboard.

    > [AZURE.TIP] Als u uw cloudservice niet ziet, moet u mogelijk wijzigen van **productie** in **tijdelijke** of vice versa.

2. Klik op **schaal**.

3. Selecteer de gewenste schaal opties wijzigen voor planning. Standaard *geen geplande tijden* als er geen planningen gedefinieerd.

4. Zoek de sectie **schaal door metrisch** en selecteer ****. Dit is de standaardinstelling voor alle rollen.

5. Elke rol in de cloudservice heeft een schuifregelaar voor het wijzigen van het aantal exemplaren gebruiken.

    ![Handmatig schalen dat een rol cloud-service][manual_scale]

    Als u meer exemplaren nodig hebt, moet u mogelijk de [cloud service VM grootte](cloud-services-sizes-specs.md)wijzigen.

6. Klik op **Opslaan**.  
Rol exemplaren wordt toegevoegd of verwijderd op basis van de gewenste opties.

>[AZURE.TIP] Wanneer u ziet ![][tip_icon] Verplaats uw muis naar deze en kunt u deze krijgen over wat een specifieke instelling bevat.


## <a name="automatic-scale---cpu"></a>Automatische schaal - processor

Hiermee wordt aangepast als het gemiddelde percentage CPU-gebruik boven of onder de opgegeven drempelwaarden gaat; exemplaren van de rol zijn gemaakt of verwijderd.

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com/) **Cloudservices**op en klik vervolgens op de naam van de cloudservice om te openen van het dashboard.

    > [AZURE.TIP] Als u uw cloudservice niet ziet, moet u mogelijk wijzigen van **productie** in **tijdelijke** of vice versa.

2. Klik op **schaal**.

3. Selecteer de gewenste schaal opties wijzigen voor planning. Standaard *geen geplande tijden* als er geen planningen gedefinieerd.

4. Zoek de sectie **schaal door metrisch** en selecteer **CPU**.

5. U kunt nu een minimum- en maximumwaarden bereik van rollen exemplaren, het doel CPU-gebruik (zo activeren dat een schaal omhoog) en hoeveel exemplaren wilt schalen omhoog en omlaag door te configureren.

![Een rol cloud-service met cpu-belasting wilt verkleinen][cpu_scale]

>[AZURE.TIP] Wanneer u ziet ![][tip_icon] Verplaats uw muis naar deze en kunt u deze krijgen over wat een specifieke instelling bevat.





## <a name="automatic-scale---queue"></a>Automatische schaal - wachtrij

Dit automatisch aangepast als het aantal berichten in een wachtrij boven of onder een bepaalde drempel gaat; exemplaren van de rol zijn gemaakt of verwijderd.

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com/) **Cloudservices**op en klik vervolgens op de naam van de cloudservice om te openen van het dashboard.

    > [AZURE.TIP] Als u uw cloudservice niet ziet, moet u mogelijk wijzigen van **productie** in **tijdelijke** of vice versa.

2. Klik op **schaal**.

3. Zoek de sectie **schaal door metrisch** en selecteer **CPU**.

4. U kunt nu een minimum- en maximumwaarden bereik van de rollen exemplaren, de wachtrij en het bedrag voor berichten moeten worden verwerkt voor elk exemplaar en hoeveel exemplaren wilt schalen omhoog en omlaag door te configureren.

![Een rol cloud-service met een berichtenwachtrij wilt verkleinen][queue_scale]

>[AZURE.TIP] Wanneer u ziet ![][tip_icon] Verplaats uw muis naar deze en kunt u deze krijgen over wat een specifieke instelling bevat.


## <a name="scale-linked-resources"></a>De schaal van gekoppelde resources aanpassen

Vaak wanneer u een rol wilt verkleinen, hoeft u nuttig om de schaal van de database waarin de toepassing wordt ook gebruikt. Als u de database aan de cloudservice koppelt, kunt u de schaal instellingen voor de desbetreffende resource openen door te klikken op de gewenste koppeling.

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com/) **Cloudservices**op en klik vervolgens op de naam van de cloudservice om te openen van het dashboard.

    > [AZURE.TIP] Als u uw cloudservice niet ziet, moet u mogelijk wijzigen van **productie** in **tijdelijke** of vice versa.

2. Klik op **schaal**.

3. Zoek het gedeelte **gekoppelde bronnen** en hebt geklikt op **schaal beheren voor deze database**.

    > [AZURE.NOTE] Als u een gedeelte **gekoppelde bronnen** niet ziet, u waarschijnlijk geen gekoppelde bronnen.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
