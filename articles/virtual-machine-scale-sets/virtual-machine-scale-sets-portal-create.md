<properties
    pageTitle="Maak een virtuele Machine schaal instellen met behulp van de Azure portal | Microsoft Azure"
    description="Schaal sets met behulp van Azure portal implementeren."
    keywords="VM schaal sets" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Maak een virtuele Machine schaal instellen met behulp van de Azure portal

Deze zelfstudie wordt getoond hoe makkelijk het is een virtuele Machine schaal instellen maken in een paar minuten, met behulp van de Azure-portal. Als u geen een Azure-abonnement hebt, een [gratis account](https://azure.microsoft.com/free/) maken voordat u begint.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Kies de afbeelding VM van marketplace

U kunt eenvoudig een schaal instellen met CentOS, CoreOS, Debian, Open Suse, rood rol Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server of Windows Server afbeeldingen implementeren in de portal.

Ga eerst naar de [Azure-portal](https://portal.azure.com) in een webbrowser. Klik op `New`, zoekt `scale set`, en selecteer vervolgens de `Virtual machine scale set` invoer:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>De Linux virtuele machine maken

U kunt nu de standaardinstellingen en de virtuele machine snel te maken.

* Klik op de `Basics` blade, typ een naam voor de set schaal. Deze naam wordt het grondtal voor de FQDN-naam van de verdeling van de belasting vóór de set schaal, dus controleer of dat de naam is uniek voor alle Azure.

* Selecteer de gewenste OS typt, voer de gebruikersnaam van de gewenste en selecteer welke verificatiemethode typt u liever. Als u een wachtwoord hebt gekozen, moet ten minste 12 tekens lang en drie van de vier volgende complexiteitsvereisten voldoet aan: één kleine letter, één hoofdletter, één waarde en één speciaal teken. Zie meer informatie over [de vereisten voor de gebruikersnaam en wachtwoord](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Als u ervoor kiest `SSH public key`, worden alleen moet plakken in uw openbare sleutel, niet op uw persoonlijke sleutel:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Voer uw naam gewenste resourcegroep en de locatie en klik vervolgens op `OK`.

* Klik op de `Virtual machine scale set service settings` blade: Voer het gewenste domein naam etiket (het grondtal voor de FQDN-naam voor de verdeling van de belasting vóór de set schaal). Dit label moet uniek zijn voor alle Azure.

* Kies de gewenste besturingssysteem schijf afbeeldings-, exemplaar tellen en grootte van de computer.

* In- of uitschakelen van automatisch schalen en configureren indien ingeschakeld:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Klik op de `Summary` blade, wanneer validatie is voltooid, klikt u op `OK`.

* Ten slotte op de `Purchase` blade, klikt u op `Purchase` instellen om te beginnen de schaal implementatie.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Verbinding maken met een VM in de set schaal

Nadat een set schaal wordt geïmplementeerd, navigeer naar de `Inbound NAT Rules` tabblad van de verdeling van de belasting voor de set schaal:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

U kunt verbinding maken met elk VM in de schaal instellen met behulp van deze regels NAT. Bijvoorbeeld om een schaalfactor van Windows, als er een NAT-regel op inkomende poort 50000 u kan verbinding te maken met deze computer via RDP `<load-balancer-ip-address>:50000`. Om een schaal Linux, zou u verbinding met de opdracht `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Volgende stappen

Zie [deze documentatie](./virtual-machine-scale-sets-cli-quick-create.md)voor documentatie over het implementeren van sets van de schaal van de CLI.

Zie [deze documentatie](./virtual-machine-scale-sets-windows-create.md)voor documentatie over het implementeren van schaal sets van PowerShell.

Zie [deze documentatie](./virtual-machine-scale-sets-vs-create.md)voor de documentatie over het implementeren van schaal sets van Visual Studio.

Voor algemene documentatie, raadpleegt u de [overzichtspagina documentatie voor schaal ingesteld](./virtual-machine-scale-sets-overview.md).

Voor algemene informatie, raadpleegt u de [belangrijkste openingspagina voor schaal sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

