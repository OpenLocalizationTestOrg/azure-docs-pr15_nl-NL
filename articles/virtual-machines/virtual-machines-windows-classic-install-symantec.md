<properties
    pageTitle="Symantec Endpoint Protection installeren op een VM | Microsoft Azure"
    description="Informatie over het installeren en configureren van de extensie Symantec Endpoint Protection-beveiliging op een nieuw of bestaand Azure-VM die zijn gemaakt met het implementatiemodel klassieke."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Het installeren en configureren van Symantec Endpoint Protection op een Windows-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

In dit artikel leest u hoe installeren en configureren van de Symantec Endpoint Protection-client op een bestaande virtuele machine (VM) met Windows Server. Dit is de volledige client, waaronder services zoals virussen en spywarebeveiliging, firewall en indringers preventie. De client wordt geïnstalleerd als de extensie voor een waardepapier met behulp van de VM-Agent.

Als u een bestaand abonnement van Symantec voor een on-premises implementatie-oplossing hebt, kunt u het beveiligen van uw Azure virtuele machines. Als u niet een klant nog bent, kunt u zich kunt aanmelden voor een proefabonnement. Zie voor meer informatie over deze oplossing, [Symantec Endpoint Protection op van Microsoft Azure platform][Symantec]. Deze pagina, heeft ook koppelingen naar licentie-informatie en instructies voor het installeren van de client als u al een Symantec-klant bent.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Symantec Endpoint Protection installeren op een bestaande VM

Voordat u begint, moet u het volgende:

- De Azure PowerShell-module, versie 0.8.2 of hoger, op uw werkcomputer. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met de **Get-Module azure | tabel opmaken versie** opdracht. Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell][PS]. Meld u aan bij het gebruik van uw abonnement op Azure `Add-AzureAccount`.

- De VM-Agent die wordt uitgevoerd op de Azure virtuele Machine.

Controleer eerst of dat de VM-Agent al is geïnstalleerd op de virtuele machine. Geef de naam van de cloud-service en VM naam en voer de volgende opdrachten vanaf de beheerdersniveau Azure PowerShell-prompt. Vervang alles in de aanhalingstekens, inclusief de < en > tekens.

> [AZURE.TIP] Als u niet de cloudservice en VM namen weet, voert u **Get-AzureVM** als u wilt de namen voor alle virtuele machines in uw huidige abonnement.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Als de opdracht **schrijven-host** wordt weergegeven **waar**, wordt de VM-Agent is geïnstalleerd. Als deze wordt **Onwaar**weergegeven, raadpleegt u de instructies en een koppeling naar het downloaden van de Azure blog posten [VM Agent en uitbreidingen - deel 2][Agent].

Als de VM-Agent is geïnstalleerd, kunt u deze opdrachten voor het installeren van de Symantec Endpoint Protection-agent uitvoeren.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Controleren of de extensie Symantec-beveiliging is geïnstalleerd en is bijgewerkt:

1.  Meld u aan bij de virtuele machine. Zie voor instructies [hoe u aanmelden bij een virtuele Machine uitvoeren Windows Server][Logon].
2.  Voor Windows Server 2008 R2, klikt u op **starten > Symantec Endpoint Protection**. Voor Windows Server 2012- of Windows Server 2012 R2, van het startscherm en typ **Symantec**en klik vervolgens op **Symantec Endpoint Protection**.
3.  Het tabblad van de **Status** van het venster **Status Symantec Endpoint Protection** en updates toepassen of opnieuw starten indien nodig.

## <a name="additional-resources"></a>Aanvullende informatie

[Hoe meld u aan bij een virtuele Machine met Windows Server][Logon]

[Azure VM Extensions en functies][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493