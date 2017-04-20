<properties
    pageTitle="Een Azure RemoteApp voor maken op basis van een VM Azure | Microsoft Azure"
    description="Informatie over het maken van een afbeelding voor Azure RemoteApp door te beginnen met een Azure virtuele machines."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Maken van Azure RemoteApp op basis van een Azure virtuele machines

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

U kunt Azure RemoteApp-afbeeldingen (de knop houdt u de apps die u in uw siteverzameling delen) maken vanuit een Azure virtuele machines. U kunt ook een virtuele machine afbeelding die wordt toegevoegd aan de afbeeldingengalerie Azure VM die voldoet aan de vereisten voor de afbeelding van de Azure RemoteApp gebruiken: u kunt die VM-afbeelding gebruiken als uitgangspunt voor uw eigen VM, als u wilt. Zoek gaan naar de afbeelding 'Windows Server extern bureaublad' in de bibliotheek.

Er zijn twee stappen voor het maken van uw eigen afbeelding op basis van een VM Azure - maken van de afbeelding en klik vervolgens in de bibliotheek Azure VM uploaden naar Azure RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Een aangepaste afbeelding op basis van een VM Azure maken

Gebruik deze stappen om een afbeelding op basis van een VM Azure te maken.

1. Maak een Azure virtuele machines. U kunt de "Windows Server extern bureaublad' of de"Windows Server extern bureaublad sessie Host met Microsoft Office 365 ProPlus"afbeelding uit de afbeeldingengalerie Azure virtuele machines. In deze afbeelding alle Azure RemoteApp sjabloon afbeelding aan de eisen voldoet.

    Zie voor meer informatie [een VM maken waarop Windows wordt uitgevoerd](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Verbinding maken met de VM en installeren en configureren van de apps die u wilt delen via RemoteApp. Zorg ervoor dat eventuele aanvullende Windows-configuraties vereist door uw apps uitvoeren.

    Lees [hoe u aanmelden bij een virtuele Machine uitvoeren Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md)voor meer informatie.

3. Als u een van de afbeeldingen van Windows Server extern bureaublad gebruikt, is er een opgenomen gegevensvalidatie-script dat zorgt ervoor dat uw VM voldoet aan de RemoteApp pre-reqs. Als u wilt script uitvoeren, dubbelklikt u op **ValidateRemoteAppImage** op het bureaublad. Zorg ervoor dat alle fouten die door het script worden opgelost voordat u verder gaat met de volgende stap.

4. SYSPREP generalize en vastleggen van de afbeelding. Lees [hoe u het vastleggen van een virtuele Windows-computer om te gebruiken als een sjabloon](../virtual-machines/virtual-machines-windows-classic-capture-image.md) voor instructies.



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Het importeren van de afbeelding in de Azure RemoteApp-afbeeldingsbibliotheek is geopend

Gebruik deze stappen voor het importeren van de nieuwe afbeelding in Azure RemoteApp:

1. Klik in het tabblad **Afbeeldingen van sitesjablonen** :
    - Als u geen bestaande afbeeldingen hebt, klikt u op **uploaden of de afbeelding van een sjabloon importeren**.
    - Als u ten minste één afbeelding al hebt, klikt u op **+** een nieuwe afbeelding toevoegen.

2. Selecteer **een afbeelding van uw virtuele Machines importeren** bibliotheek en klik vervolgens op **volgende**.

3. Op de volgende pagina, selecteer uw aangepaste afbeelding in de lijst en Bevestig dat u de stappen weergegeven wanneer u uw afbeelding hebt gemaakt. Klik op **volgende**.
4. Voer een naam voor de nieuwe RemoteApp-afbeelding en kies de locatie en klik op het vinkje om het importproces te starten.

> [AZURE.NOTE] U kunt afbeeldingen importeren vanaf elke Azure locatie door Azure virtuele Machines naar de gewenste Azure locatie wordt ondersteund door Azure RemoteApp ondersteund. Afhankelijk van de locaties kan het importeren maximaal 25 minuten duren.

U bent nu klaar zijn de nieuwe verzameling, ofwel een [cloud](remoteapp-create-cloud-deployment.md) -siteverzameling of [hybride](remoteapp-create-hybrid-deployment.md), afhankelijk van uw behoeften maken.
