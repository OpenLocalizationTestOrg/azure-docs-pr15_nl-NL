<properties
   pageTitle="Inschakelen van netwerk beveiligingsgroepen in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de aanbeveling Azure Beveiligingscentrum **Netwerk beveiligingsgroepen inschakelen**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-network-security-groups-in-azure-security-center"></a>Beveiligingsgroepen netwerk in Azure Beveiligingscentrum inschakelen

Azure Beveiligingscentrum wordt aangeraden een beveiligingsgroep netwerk (NSG) in te schakelen als een al niet is ingeschakeld. NSGs bevatten een lijst met regels voor Access (Toegangsbeheerlijst) die toestaan of weigeren netwerkverkeer tot uw VM-sessies in een virtueel netwerk. NSGs kan worden gekoppeld aan subnetten of afzonderlijke VM exemplaren in dat subnet. Wanneer een NSG gekoppeld aan een subnet is, wordt de ACL regels toepassen op alle VM exemplaren in dat subnet. Daarnaast verkeer naar een afzonderlijke VM kan worden beperkt door te koppelen van een NSG rechtstreeks naar die VM verdere. Voor meer informatie vindt u meer [Wat is een netwerk beveiliging groep (NSG)?](../virtual-network/virtual-networks-nsg.md)

Als u nog geen NSGs ingeschakeld, Beveiligingscentrum twee aanbevelingen voor u wordt presenteren: netwerk-beveiligingsgroepen inschakelen op subnetten en netwerk-beveiligingsgroepen inschakelen op virtuele machines. U kiezen welke niveau, subnet of VM, NSGs toepassen.


> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in het blad **aanbevelingen** **Netwerk beveiligingsgroepen inschakelen** op subnetten of op virtuele machines.
![Beveiligingsgroepen netwerk inschakelen][1]

2. Hiermee opent u het blad **Ontbreken netwerk beveiligingsgroepen configureren** voor subnetten of voor virtuele machines, afhankelijk van de aanbeveling die u hebt geselecteerd. Selecteer een subnet of een virtuele machine een NSG op configureren.

  ![NSG voor subnet configureren][2]

  ![NSG voor VM configureren][3]
3. Klik op het blad **kiezen netwerk-beveiligingsgroep** selecteert u een bestaande NSG of Selecteer een nieuwe NSG maken.

  ![Kies netwerk-beveiligingsgroep][4]

Als u een nieuwe NSG maakt, volgt u de stappen in [het beheren van NSGs met behulp van de Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) maken van een NSG en configureert beveiligingsregels.

## <a name="see-also"></a>Zie ook

In dit artikel u geleerd hoe u de aanbeveling alleen Beveiligingscentrum "Inschakelen netwerk beveiligingsgroepen" voor subnetten of virtuele machines implementeert. Zie de volgende onderwerpen voor meer informatie over het inschakelen van NSGs:

- [Wat is een netwerk beveiliging groep (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Het beheren van NSGs met behulp van de Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --het laatste nieuws van Azure beveiliging en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
