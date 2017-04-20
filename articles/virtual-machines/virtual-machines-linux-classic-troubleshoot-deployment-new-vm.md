<properties
   pageTitle="Problemen met Linux VM implementatie-klassiek | Microsoft Azure"
   description="Klassieke implementatie van problemen, wanneer u een nieuwe Linux virtuele machine in Azure wordt aangegeven maakt"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Klassieke implementatie problemen met het maken van een nieuwe Linux virtuele machine in Azure wordt aangegeven

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Logboeken verzamelen controle

Als u wilt oplossen, de controlelogboeken om te identificeren van de fout die is gekoppeld aan het probleem te verzamelen.

Klik op **Bladeren**in de portal Azure > **virtuele machines** > *virtuele uw Windows-computer* > **Instellingen** > **controlelogboeken bijhouden**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Als het besturingssysteem Linux generalized, en deze is geüpload en/of vastgelegd met de algemene instellingen, klik er niet eventuele fouten. Op dezelfde manier als het besturingssysteem is Linux gespecialiseerde, en deze wordt geüpload en/of vastgelegd met de instelling gespecialiseerde en er niet eventuele fouten.

**Upload fouten:**

**N<sup>1</sup>:** Als het besturingssysteem Linux generalized, en deze wordt geüpload als gespecialiseerde, wordt er een time-out inrichten omdat de VM in het inrichten stadium hangen blijft.

**N<sup>2</sup>:** Als het besturingssysteem is Linux gespecialiseerde en deze wordt geüpload zoals generalized, krijgt u een inrichten is mislukt omdat de nieuwe VM wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord.

**Oplossing:**

Uploaden om op te lossen beide deze fouten, de oorspronkelijke VHD, beschikbaar on-premises, met dezelfde instelling als die voor de OS (generalized/gespecialiseerde). Als u wilt uploaden generalized, onthouden om uit te voeren - eerst deprovision. Zie [maken en uploaden van een virtuele hardeschijf met het besturingssysteem Linux](virtual-machines-linux-classic-create-upload-vhd.md) voor meer informatie.

**Vastleggen fouten:**

**N<sup>3</sup>:** Als het besturingssysteem Linux generalized, en deze is vastgelegd als gespecialiseerde, wordt er een time-out inrichten omdat de oorspronkelijke VM kan niet worden gebruikt als deze is gemarkeerd als generalized.

**N<sup>4</sup>:** Als het besturingssysteem is Linux gespecialiseerde en deze is vastgelegd, zoals generalized, krijgt u een inrichten is mislukt omdat de nieuwe VM wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord. De oorspronkelijke VM is ook niet bruikbaar omdat deze is gemarkeerd als gespecialiseerde.

**Oplossing:**

Om op te lossen beide deze fouten, door de huidige afbeelding uit de portal en [opnieuw vastleggen van de huidige VHD's](virtual-machines-linux-classic-capture-image.md) met dezelfde instelling als die voor de OS (generalized/gespecialiseerde) te verwijderen.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probleem: Aangepaste / galerie / marketplace-afbeelding. Fout bij het toewijzen
Deze fout zich voordoet in situaties wanneer het nieuwe VM-verzoek wordt verzonden naar een cluster die geen beschikbaar vrije ruimte voor het verzoek of de grootte van de VM wordt aangevraagd kan niet worden ondersteund. Deze is niet mogelijk is om te mengt u andere reeks VMs in de dezelfde cloudservice. Het verzoek berekeningscluster, dus mislukt als u maken van een nieuwe VM van een andere grootte wilt dan wat uw cloudservice ondersteunt.

Afhankelijk van de beperkingen van de cloudservice die u kunt de nieuwe VM maken, kunt u een fout veroorzaakt door een van twee gevallen tegenkomen.

**Ertoe leiden dat 1:** De cloudservice wordt vastgemaakt aan een specifiek cluster of deze is gekoppeld aan een groep affiniteit en daarom zijn vastgemaakt aan een specifiek cluster inherent aan het ontwerp. Dus nieuwe berekeningscluster resource aanvraagt in dat affiniteit groep zijn geprobeerd in hetzelfde cluster waar de bestaande resources worden gehost. Echter hetzelfde cluster mogelijk geen ondersteuning voor de gevraagde VM grootte of er onvoldoende ruimte, waardoor een fout toegewezen hebt. Dit is waar of de nieuwe resources worden gemaakt via een nieuwe cloudservice of via een bestaande cloudservice.

**Oplossing 1:**

- Maak een nieuwe cloudservice en koppelen aan een regio of een virtuele regio-netwerk.
- Maak een nieuwe VM in de nieuwe cloudservice.
  Als u een foutbericht krijgt wanneer u probeert te maken van een nieuwe cloudservice, probeer op een later tijdstip of wijzigen van het gebied voor de cloudservice.

> [AZURE.IMPORTANT] Als u probeerde te maken van een nieuwe VM in een bestaande cloudservice, maar kan niet en hebt u er een nieuwe cloudservice voor uw nieuwe VM maken, kunt u kiezen om samen te voegen uw VMs in de dezelfde cloudservice. Hiervoor het VMs in de bestaande cloudservice verwijderen en opnieuw ze vanaf hun schijven in de nieuwe cloudservice vastleggen. Het is echter belangrijk te onthouden zijn dat de nieuwe cloudservice wordt een nieuwe naam en VIP, dus moet u deze voor alle afhankelijkheden die momenteel deze informatie voor de bestaande cloudservice gebruiken bijwerken.

**Ertoe leiden dat 2:** De cloudservice is gekoppeld aan een virtueel netwerk dat is gekoppeld aan een groep affiniteit, zodat deze wordt vastgemaakt aan een specifiek cluster inherent aan het ontwerp. Alle nieuwe berekeningscluster resource aanvraagt in dat affiniteit groep zijn er daarom heeft geprobeerd in hetzelfde cluster waar de bestaande resources worden gehost. Echter hetzelfde cluster mogelijk geen ondersteuning voor de gevraagde VM grootte of er onvoldoende ruimte, waardoor een fout toegewezen hebt. Dit is waar of de nieuwe resources worden gemaakt via een nieuwe cloudservice of via een bestaande cloudservice.

**Oplossing 2:**

- Maak een nieuwe regionale virtueel netwerk.
- Maak de nieuwe VM in het nieuwe virtuele netwerk.
- [Verbinding maken met uw bestaande virtuele netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) met het nieuwe virtuele netwerk. Zie meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). U kunt ook [uw affiniteit-groep - virtuele netwerk met een regionale virtuele netwerk migreren](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), en vervolgens de nieuwe VM te maken.

## <a name="next-steps"></a>Volgende stappen
Als er problemen optreden wanneer u een gestopt Linux VM starten of het formaat van een bestaande Linux VM in Azure wordt aangegeven, raadpleegt u [problemen klassieke implementatie oplossen met moet opnieuw worden gestart of de grootte van een bestaande Linux VM in Azure wordt aangegeven](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md).
