<properties
   pageTitle="VM moet opnieuw worden gestart of de grootte van problemen | Microsoft Azure"
   description="Problemen oplossen klassieke implementatie met moet opnieuw worden gestart of de grootte van een bestaande Linux VM in Azure wordt aangegeven"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Problemen oplossen klassieke implementatie met moet opnieuw worden gestart of de grootte van een bestaande Linux VM in Azure wordt aangegeven

> [AZURE.SELECTOR]
- [Klassieke](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Resourcemanager](../articles/virtual-machines/virtual-machines-linux-restart-resize-error-troubleshooting.md)

Wanneer u probeert te starten een gestopt Azure Virtual Machine (VM) of de grootte van een bestaande Azure VM wijzigen, is de algemene fout dat er een fout bij het toewijzen. Deze fout treedt op wanneer het cluster of de regio geen bronnen die beschikbaar zijn of biedt geen ondersteuning voor de gevraagde VM grootte.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Logboeken verzamelen controle

Als u wilt oplossen, de controlelogboeken om te identificeren van de fout die is gekoppeld aan het probleem te verzamelen.

Klik op **Bladeren**in de portal Azure > **virtuele machines** > _uw VM Linux_ > **Instellingen** > **controlelogboeken bijhouden**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probleem: Fout bij het starten van een gestopt VM

U probeert te starten een gestopt VM maar met een toewijzing is mislukt.

### <a name="cause"></a>Oorzaak

Het verzoek om de gestopt VM te moet worden uitgevoerd op het oorspronkelijke cluster waarop de cloudservice. Het cluster heeft echter geen ruimte beschikbaar voor het uitvoeren van de aanvraag.

### <a name="resolution"></a>Resolutie

* Maak een nieuwe cloudservice en koppelen aan een een regio of een virtuele regio-netwerk, maar niet naar een groep affiniteit.

* Verwijder de gestopt VM.

* Maak de VM in de nieuwe cloudservice opnieuw met behulp van de schijven.

* Start het opnieuw gemaakt VM.

Als u een foutbericht krijgt wanneer u probeert te maken van een nieuwe cloudservice, probeer op een later tijdstip of wijzigen van het gebied voor de cloudservice.

> [AZURE.IMPORTANT] De nieuwe cloudservice heeft een nieuwe naam en VIP, dus moet u die gegevens voor alle afhankelijkheden die deze informatie voor de bestaande cloudservice gebruiken wijzigen.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probleem: Fout wanneer het formaat van een bestaande VM

U probeert te het formaat van een bestaande VM maar ophalen van een toewijzing is mislukt.

### <a name="cause"></a>Oorzaak

Het verzoek om het formaat van de VM te moet worden uitgevoerd op het oorspronkelijke cluster waarop de cloudservice. Het cluster ondersteunt echter niet de gevraagde VM grootte.

### <a name="resolution"></a>Resolutie

De gevraagde VM verkleinen en probeer het verzoek tabelgrootte wijzigen.

* Klik op **Bladeren alle** > **virtuele machines (klassieke)** > _uw VM_ > **Instellingen** > **grootte**. Zie [de grootte van de virtuele machine wijzigen](https://msdn.microsoft.com/library/dn168976.aspx)voor meer informatie.

Als het niet mogelijk om het te verkleinen VM is, voert u de volgende stappen uit:

  * Maak een nieuwe cloudservice, zorgen dat dit is niet gekoppeld aan een groep affiniteit en niet is gekoppeld aan een virtuele netwerk dat is gekoppeld aan een groep affiniteit.

  * Maak een nieuwe, groter formaat VM erin.

U kunt uw VMs in dezelfde cloudservice samenvoegen. Als uw bestaande cloudservice gekoppeld aan een virtuele regio-netwerk is, kunt u de nieuwe cloudservice koppelen aan het bestaande virtuele netwerk.

Als u de bestaande cloudservice is niet gekoppeld aan een virtuele regio-netwerk, hebt u de VMs in de bestaande cloudservice verwijderen en maak deze opnieuw in de nieuwe cloudservice van hun schijven. Het is echter belangrijk te onthouden zijn dat de nieuwe cloudservice wordt een nieuwe naam en VIP, dus moet u deze voor alle afhankelijkheden die momenteel deze informatie voor de bestaande cloudservice gebruiken bijwerken.

## <a name="next-steps"></a>Volgende stappen

Als er problemen optreden wanneer u een nieuwe Linux VM in Azure wordt aangegeven maakt, raadpleegt u [problemen oplossen met de implementatie van problemen met het maken van een nieuwe Linux virtuele machine in Azure wordt aangegeven](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
