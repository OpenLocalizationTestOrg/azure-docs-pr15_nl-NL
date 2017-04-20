<properties
   pageTitle="VM moet opnieuw worden gestart of de grootte van problemen | Microsoft Azure"
   description="Problemen oplossen resourcemanager implementatie met moet opnieuw worden gestart of de grootte van een bestaande Linux VM in Azure wordt aangegeven"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Problemen oplossen resourcemanager implementatie met moet opnieuw worden gestart of de grootte van een bestaande Linux VM in Azure wordt aangegeven

Wanneer u probeert te starten een gestopt Azure Virtual Machine (VM) of de grootte van een bestaande Azure VM wijzigen, is de algemene fout dat er een fout bij het toewijzen. Deze fout treedt op wanneer het cluster of de regio geen bronnen die beschikbaar zijn of biedt geen ondersteuning voor de gevraagde VM grootte.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Logboeken verzamelen controle

Als u wilt oplossen, de controlelogboeken om te identificeren van de fout die is gekoppeld aan het probleem te verzamelen. De volgende koppelingen bevatten gedetailleerde informatie over het proces:

[Probleemoplossing resource groep implementaties met Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Bewerkingen met resourcemanager controleren](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probleem: Fout bij het starten van een gestopt VM

U probeert te starten een gestopt VM maar met een toewijzing is mislukt.

### <a name="cause"></a>Oorzaak

Het verzoek om de gestopt VM te moet worden uitgevoerd op het oorspronkelijke cluster waarop de cloudservice. Het cluster heeft echter geen ruimte beschikbaar voor het uitvoeren van de aanvraag.

### <a name="resolution"></a>Resolutie

*   Stoppen met het VMs in de set beschikbaarheid en elke VM start opnieuw.

  1. Klik op **resourcegroepen** > _uw resourcegroep_ > **Resources** > _uw beschikbaarheid instellen_ > **virtuele Machines** > _uw VM_ > **stoppen**.

  2. Nadat alle VMs stoppen, selecteert u elk van de gestopt VMs en klik op Start.

*   Probeer het verzoek opnieuw starten op een later tijdstip.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probleem: Fout wanneer het formaat van een bestaande VM

U probeert te het formaat van een bestaande VM maar ophalen van een toewijzing is mislukt.

### <a name="cause"></a>Oorzaak

Het verzoek om het formaat van de VM te moet worden uitgevoerd op het oorspronkelijke cluster waarop de cloudservice. Het cluster ondersteunt echter niet de gevraagde VM grootte.

### <a name="resolution"></a>Resolutie

* Probeer het verzoek met een kleinere VM.

* Als u de grootte van de gevraagde VM kan niet worden gewijzigd:

  1. Alle VMs in de set beschikbaarheid stoppen.

    * Klik op **resourcegroepen** > _uw resourcegroep_ > **Resources** > _uw beschikbaarheid instellen_ > **virtuele Machines** > _uw VM_ > **stoppen**.

  2. Wanneer alle VMs stoppen, het formaat wijzigen de gewenste VM naar groter.
  3. Start elk van de gestopt VMs selecteert u de gewijzigde VM en klik op **Start**.

## <a name="next-steps"></a>Volgende stappen

Als er problemen optreden wanneer u een nieuwe Linux VM in Azure wordt aangegeven maakt, raadpleegt u [problemen oplossen met de implementatie van problemen met het maken van een nieuwe Linux virtuele machine in Azure wordt aangegeven](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
