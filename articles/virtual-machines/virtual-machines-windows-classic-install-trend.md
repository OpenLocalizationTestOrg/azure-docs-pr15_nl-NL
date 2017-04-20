<properties
    pageTitle="Trend Micro uitgebreide Security installeren op een VM | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe installeren en configureren van Trend Micro beveiliging op een VM die zijn gemaakt met het implementatiemodel klassieke in Azure wordt aangegeven."
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


# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Het installeren en configureren van Trend Micro uitgebreide Security als een Service op een Windows-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

In dit artikel leest u hoe installeren en configureren van Trend Micro uitgebreide Security als een Service die u op een nieuw of bestaand virtuele machine (VM) met Windows Server. Uitgebreide beveiliging als een Service bevat beveiliging tegen malware, een firewall, een indringers preventie systeem en integriteit bewaken.

De client is als extensie beveiliging via de VM-Agent geïnstalleerd. Op een nieuwe virtuele machine installeert u de VM-Agent samen met de uitgebreide beveiligings-Agent. Op een bestaande virtuele machine die geen de VM-Agent, moet u dit downloaden en installeren eerst. In dit artikel heeft betrekking op beide gevallen.

Als u bestaande abonnement van Trend Micro voor een on-premises implementatie-oplossing hebt, kunt u deze gebruiken om te helpen beveiligen uw Azure virtuele machines. Als u niet een klant nog bent, kunt u zich kunt aanmelden voor een proefabonnement. Zie voor meer informatie over deze oplossing, [Microsoft Azure VM Agent extensie voor uitgebreide beveiliging](http://go.microsoft.com/fwlink/p/?LinkId=403945)voor de Trend Micro blogbericht.

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>De uitgebreide beveiligings-Agent installeren op een nieuwe VM

De [portal van Azure klassieke](http://manage.windowsazure.com) kunt u de VM-Agent en de Trend Micro beveiliging extensie installeren wanneer u de optie **Uit galerie** gebruikt voor het maken van de virtuele machine. Als u een één virtuele machine maakt, is met behulp van de portal een eenvoudige manier om de beveiliging van Trend Micro toevoegen.

Deze optie **Uit galerie** opent een wizard waarmee u de virtuele machine instellen. De laatste pagina van de wizard kunt u de VM-Agent en Trend Micro beveiliging extensie installeren. Zie [een virtuele machine maken waarop Windows in de portal van Azure klassieke wordt uitgevoerd](virtual-machines-windows-classic-tutorial.md)voor algemene instructies. Als u naar de laatste pagina van de wizard, als volgt:

1.  Schakel onder **VM Agent**, **VM Agent installeren**.

2.  Schakel onder **Beveiligingsextensies** **Trend Micro uitgebreide Security Agent**.

    ![Installeer de VM-Agent en de uitgebreide beveiligings-Agent](./media/virtual-machines-windows-classic-install-trend/InstallVMAgentandTrend.png)

3.  Klik op het vinkje om te maken van de virtuele machine.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>De uitgebreide beveiligings-Agent installeren op een bestaande VM

Als u wilt de agent installeren op een bestaande VM, hebt u het volgende nodig:

- De Azure PowerShell-module, versie 0.8.2 of nieuwere, is geïnstalleerd op uw lokale computer. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met behulp van de **Get-Module azure | tabel opmaken versie** opdracht. Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Meld u aan bij het gebruik van uw abonnement op Azure `Add-AzureAccount`.

- De VM-Agent is geïnstalleerd op de doelsite virtuele machine.

Controleer eerst of dat de VM-Agent al is geïnstalleerd. Geef de naam van de cloud-service en VM naam en voer de volgende opdrachten vanaf de beheerdersniveau Azure PowerShell-prompt. Vervang alles in de aanhalingstekens, inclusief de < en > tekens.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Als u niet de cloudservice en de naam van de virtuele machine weet, voert u **Get-AzureVM** informatie wilt weergeven die voor alle virtuele machines in uw huidige abonnement.

Als de opdracht **schrijven-host** **True**retourneert, wordt de VM-Agent is geïnstalleerd. Als het resultaat **Onwaar**, raadpleegt u de instructies en een koppeling naar het downloaden van de Azure blog posten [VM Agent en uitbreidingen - deel 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Als de VM-Agent is geïnstalleerd, kunt u deze opdrachten uitvoeren.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

Het duurt een paar minuten voordat de agent om te beginnen worden uitgevoerd wanneer dit is geïnstalleerd. Daarna moet u uitgebreide beveiliging op de virtuele machine activeren zodat deze kan worden beheerd door een uitgebreide beveiliging Manager. Zie de volgende onderwerpen voor aanvullende instructies:

- Trend van artikel over deze oplossing [Instant-On Cloud Security voor Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
- Een [voorbeeld van Windows PowerShell-script](http://go.microsoft.com/fwlink/?LinkId=404100) de virtuele machine configureren
- [Instructies](http://go.microsoft.com/fwlink/?LinkId=404099) voor de steekproef

## <a name="additional-resources"></a>Aanvullende informatie

[Aanmelden met een virtuele machine met Windows Server]

[Azure VM Extensions en functies]


<!--Link references-->
[Aanmelden met een virtuele machine met Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Azure VM Extensions en functies]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
