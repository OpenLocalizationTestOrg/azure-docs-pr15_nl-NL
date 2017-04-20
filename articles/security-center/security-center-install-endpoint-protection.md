<properties
   pageTitle="Endpoint Protection installeren in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de aanbeveling Azure Beveiligingscentrum **Endpoint Protection installeren**."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Endpoint Protection in Azure Beveiligingscentrum installeren

Azure Beveiligingscentrum wordt aangeraden dat u een programma antimalware aan uw Azure virtuele machines (VMs), inrichten als antimalware niet al is ingeschakeld. Deze aanbeveling alleen van toepassing op Windows VMs.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in het blad **aanbevelingen** **Endpoint Protection installeren**.
![Selecteer installeren Endpoint Protection][1]

2. Het blad **Installeren Endpoint Protection** wordt geopend met een lijst met VMs zonder antimalware ingeschakeld. Selecteer in de lijst de VMs die u wilt installeren antimalware op en klik op **installeren op VMs**.
![Selecteer VMs antimalware om op te installeren][2]

3. Het blad **Selecteren Endpoint Protection** wordt geopend dat u kunt het selecteren van de antimalware-oplossing die u wilt gebruiken. Selecteer in dit voorbeeld laten we **Microsoft Antimalware**.
![Endpoint Protection selecteren][3]

4. Meer informatie over de oplossing antimalware wordt weergegeven. Selecteer **maken**.
![Antimalware-oplossing maken][4]

5. Voer de vereiste configuratie-instellingen op het blad **Extensie toevoegen** en selecteer vervolgens **OK**. Meer informatie over de configuratie-instellingen, raadpleegt u [standaard en aangepaste Antimalware configuratie](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../azure-security-antimalware.md) is nu actief op de geselecteerde VMs.

## <a name="see-also"></a>Zie ook

In dit artikel beschreven wat u het implementeren van de aanbeveling Beveiligingscentrum "Installatiefout Endpoint Protection." Zie de volgende onderwerpen voor meer informatie over het inschakelen van een programma antimalware in Azure wordt aangegeven:

- [Microsoft Antimalware voor Cloudservices en virtuele Machines](../azure-security-antimalware.md) --informatie over het implementeren van Microsoft antimalware.

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --zoeken blog over Azure beveiliging en naleving in berichten.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
