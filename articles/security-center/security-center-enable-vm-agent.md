<properties
   pageTitle="VM Agent in Azure Beveiligingscentrum inschakelen | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de aanbeveling Azure Beveiligingscentrum **VM-Agent inschakelen**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="enable-vm-agent-in-azure-security-center"></a>VM Agent in Azure Beveiligingscentrum inschakelen

De VM-Agent moeten worden geïnstalleerd op virtuele machines (VMs) in volgorde naar [verzamelen inschakelen](security-center-enable-data-collection.md).  Azure Beveiligingscentrum kunt u zien welke VMs vereisen de VM-Agent en wordt u aangeraden de VM-Agent op deze VMs in te schakelen.

De VM-Agent is al dan niet standaard geïnstalleerd voor VMs die zijn geïmplementeerd van Azure Marketplace. Het artikel [VM Agent en uitbreidingen – deel 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) vindt u informatie over het installeren van de VM-Agent.


> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie. Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in de **aanbevelingen blade**, **VM-Agent inschakelen**.
![VM Agent inschakelen][1]

2. Hiermee opent u het blad **VM Agent ontbreken of niet reageert**. Deze blade bevat de VMs waarvoor de VM-Agent. Volg de instructies op het blad de VM-agent installeren.
![VM Agent ontbreekt][2]

## <a name="see-also"></a>Zie ook

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md)--meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging in Azure Beveiligingscentrum beheren](security-center-recommendations.md)--Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging servicestatus bewaken in Azure Beveiligingscentrum](security-center-monitoring.md)--informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingsmeldingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md)--meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md)--zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/)--het laatste nieuws van Azure beveiliging en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
