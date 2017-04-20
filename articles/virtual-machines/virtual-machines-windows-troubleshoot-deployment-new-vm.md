<properties
   pageTitle="Problemen met Windows VM implementatie-RM | Microsoft Azure"
   description="Oplossen van problemen met de implementatie van resourcemanager wanneer u een nieuwe virtuele Windows-computer in Azure wordt aangegeven maakt"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Problemen met resourcemanager implementatie oplossen met het maken van een nieuwe virtuele Windows-computer in Azure wordt aangegeven

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Logboeken verzamelen controle

Als u wilt oplossen, de controlelogboeken om te identificeren van de fout die is gekoppeld aan het probleem te verzamelen. De volgende koppelingen bevatten gedetailleerde informatie over het proces te volgen.

[Problemen met resource groep implementaties met Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Bewerkingen met resourcemanager controleren](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Als het besturingssysteem Windows generalized, en deze is geüpload en/of vastgelegd met de algemene instellingen, klik er niet eventuele fouten. Op dezelfde manier als het besturingssysteem Windows gespecialiseerde, en deze is geüpload en/of vastgelegd met de instelling gespecialiseerde en er niet eventuele fouten.

**Upload fouten:**

**N<sup>1</sup>:** Als het besturingssysteem Windows generalized, en deze wordt geüpload als gespecialiseerde, krijgt u een time-out inrichten met de VM hangen aan het scherm OOBE.

**N<sup>2</sup>:** Als het besturingssysteem Windows gespecialiseerde en deze wordt geüpload zoals generalized is, krijgt u een inrichten fout is opgetreden met de VM hangen aan het scherm OOBE omdat de nieuwe VM wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord.

**Resolutie**

U lost beide deze fouten door [Toevoegen-AzureRmVhd voor het uploaden van de oorspronkelijke VHD](https://msdn.microsoft.com/library/mt603554.aspx), beschikbaar on-premises met dezelfde instelling als die voor de OS (generalized/gespecialiseerde) te gebruiken. Als u wilt uploaden generalized, moet u eerst sysprep uitvoeren.

**Vastleggen fouten:**

**N<sup>3</sup>:** Als het besturingssysteem Windows generalized, en deze is vastgelegd als gespecialiseerde, wordt er een time-out inrichten omdat de oorspronkelijke VM kan niet worden gebruikt als deze is gemarkeerd als generalized.

**N<sup>4</sup>:** Als het besturingssysteem Windows gespecialiseerde en deze is vastgelegd, zoals generalized is, krijgt u een inrichten is mislukt omdat de nieuwe VM wordt uitgevoerd met de naam van de oorspronkelijke computer, gebruikersnaam en wachtwoord. De oorspronkelijke VM is ook niet bruikbaar omdat deze is gemarkeerd als gespecialiseerde.

**Resolutie**

Om op te lossen beide deze fouten, door de huidige afbeelding uit de portal en [opnieuw vastleggen van de huidige VHD's](virtual-machines-windows-vhd-copy.md) met dezelfde instelling als die voor de OS (generalized/gespecialiseerde) te verwijderen.

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Probleem: Aangepaste/galerie/marketplace-afbeelding. Fout bij het toewijzen
Deze fout zich voordoet in situaties wanneer het nieuwe VM-verzoek wordt vastgemaakt aan een cluster biedt geen ondersteuning voor de grootte van de VM wordt aangevraagd of geen beschikbaar vrije ruimte voor het verzoek.

**Ertoe leiden dat 1:** Het cluster kan de gevraagde VM grootte niet ondersteunen.

**Oplossing 1:**

- Probeer het verzoek met een kleinere VM.
- Als u de grootte van de gevraagde VM kan niet worden gewijzigd:
  - Alle VMs in de set beschikbaarheid stoppen.
  Klik op **resourcegroepen** > *uw resourcegroep* > **Resources** > *uw beschikbaarheid instellen* > **virtuele Machines** > *uw VM* > **stoppen**.
  - Nadat alle VMs stoppen, maakt u de nieuwe VM in de gewenste grootte.
  - De nieuwe VM eerst starten en selecteer vervolgens elk van de gestopt VMs en klik op **Start**.

**Ertoe leiden dat 2:** Het cluster heeft geen gratis resources.

**Oplossing 2:**

- Probeer het verzoek op een later tijdstip.
- Als de nieuwe VM kunt deel uitmaakt van een reeks verschillende beschikbaarheid
  - Maak een nieuwe VM in een andere beschikbaarheid instellen (in dezelfde regio).
  - Voeg de nieuwe VM aan hetzelfde virtuele netwerk.

## <a name="next-steps"></a>Volgende stappen
Als er problemen optreden wanneer u een gestopt Windows VM starten of het formaat van een bestaande Windows VM in Azure wordt aangegeven, raadpleegt u [problemen met resourcemanager implementatieproblemen met moet opnieuw worden gestart of de grootte van een bestaande virtuele Windows-computer in Azure wordt aangegeven](virtual-machines-windows-restart-resize-error-troubleshooting.md).
