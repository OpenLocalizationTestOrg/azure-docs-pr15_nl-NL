<properties
   pageTitle="Inschakelen of uitschakelen van Azure VM bewaken"
   description="Wordt beschreven hoe u in- of uitschakelen van Azure VM bewaken"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>In- of uitschakelen van Azure VM bewaken

In deze sectie wordt beschreven hoe in- of uitschakelen op virtuele machines waarop Azure cmdlets voor controle. Cmdlets voor controle is standaard ingeschakeld op Azure virtuele machines als geïmplementeerd via de [portal van Azure](https://portal.azure.com) -en controle grafieken zijn opgegeven al dan niet standaard op een punt 1 minuten. U kunt inschakelen of uitschakelen via de portal of de Interface van Azure-opdrachtregel voor Mac, Linux en Windows (de Azure CLI) cmdlets voor controle. 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Inschakelen / uitschakelen via de Portal van Azure bewaken
 
Cmdlets voor controle van uw VM Azure, die gegevens over uw exemplaar in 1 minuten perioden bevat, kunt u inschakelen. (opslag wijzigingen van toepassing). Gedetailleerde naar diagnostische gegevens is beschikbaar voor de VM in de portal grafieken of via de API. Standaard Azure portal kunt controleren, maar u kunt deze uitschakelen zoals hieronder beschreven. Cmdlets voor controle terwijl de VM is gestart of in staat gestopt, kunt u inschakelen.

- Open de Azure portal bij ** [https://portal.azure.com](https://portal.azure.com)**

- Klik in het linker navigatiegedeelte op virtuele machines.

- Selecteer in de lijst virtuele machines, een exemplaar uitgevoerd of is gestopt. VM blad wordt geopend.

- Klik op "Alle instellingen".

- Klik op "Diagnose".

- Status wijzigen naar aan of uit. U kunt ook kiezen in deze blade het niveau van de details die u wilt inschakelen voor uw VM cmdlets voor controle.

[Azure.Note] De schakeloptie diagnostische gegevens op is de standaardwaarde wanneer u een nieuwe virtuele machine maken

![Inschakelen / uitschakelen via de Portal van Azure cmdlets voor controle.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Inschakelen / bewaken met Azure CLI uitschakelen
 
Om te schakelen voor een VM Azure cmdlets voor controle.

- Een bestand met de naam zoals PrivateConfig.json met de volgende inhoud maken.
        {"storageAccountName": "de opslagruimte account om gegevens te ontvangen", "storageAccountKey": "de sleutel van het account"}
- Voer de volgende opdracht uit Azure CLI.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] U kunt wijzigen van versie 2.0 naar een nieuwere versie indien beschikbaar. 

Voor meer informatie over het configureren van monitoring maatstaven en steekproeven, gaat u naar het document - **[Gebruiken Linux diagnostische-extensie voor Monitor Linux VM prestaties en diagnostische gegevens](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

