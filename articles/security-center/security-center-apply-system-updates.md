<properties
   pageTitle="Systeemupdates in Azure Beveiligingscentrum toepassen | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de Azure Beveiligingscentrum aanbevelingen **systeemupdates toepassen** en **Start opnieuw op na het systeemupdates**."
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

# <a name="apply-system-updates-in-azure-security-center"></a>Systeemupdates in Azure Beveiligingscentrum toepassen

Azure Beveiligingscentrum bewaakt dagelijks Windows en Linux virtuele machines (VMs) voor het ontbrekende besturingssysteem updates. Beveiligingscentrum haalt een lijst met beschikbare beveiligingsupdates en essentiële updates vanuit Windows Update of Windows Server Update Services (WSUS), afhankelijk van welke service is geconfigureerd op een Windows-VM.  Beveiligingscentrum ook Hiermee wordt gecontroleerd op de meest recente updates Linux-systemen bij. Als u een systeemupdate ontbreekt in uw VM, wordt Beveiligingscentrum aanbevelen u systeemupdates

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in het blad **aanbevelingen** **toepassen systeemupdates**.
![Systeemupdates toepassen][1]

2. Het blad **toepassen systeemupdates** wordt geopend met een lijst met VMs ontbrekende systeemupdates. Selecteer een VM.
![Selecteer een VM][2]

3. Een blade wordt geopend met een lijst met ontbrekende updates voor die VM. Selecteer het bijwerken van een systeem. Selecteer in dit voorbeeld laten we KB3156016.
![Ontbrekende beveiligingsupdates][3]

4. Volg de stappen in het blad **Beveiligingsupdate** de ontbrekende update toepassen.
![Beveiligingsupdate][4]

## <a name="reboot-after-system-updates"></a>Start opnieuw op na het systeemupdates

5. Ga terug naar het blad **aanbevelingen** . Een nieuwe vermelding is gegenereerd nadat u de systeemupdates genoemd **starten na systeemupdates**hebt toegepast. Dit item kunt u weet dat u start opnieuw op de VM om het proces moet van het toepassen van de systeemupdates te voltooien.
![Start opnieuw op na het systeemupdates][5]

6. Selecteer **opnieuw opstarten na systeemupdates**. Hiermee opent u **dat een herstart in behandeling systeemupdates voltooid is** blade weergeven van een lijst met VMs die u nodig hebt om opnieuw om het te voltooien toepassen systeem updates te starten.
![Start opnieuw in behandeling][6]

Start opnieuw op de VM van Azure om het proces te voltooien.

## <a name="see-also"></a>Zie ook

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --zoeken blog over Azure beveiliging en naleving in berichten.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
