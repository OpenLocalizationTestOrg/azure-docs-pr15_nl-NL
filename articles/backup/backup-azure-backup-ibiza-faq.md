<properties
   pageTitle="Herstel Services kluis Veelgestelde vragen over | Microsoft Azure"
   description="Deze versie van de veelgestelde vragen over ondersteunt de openbare Preview-versie van de back-up van Azure-service. Antwoorden op veelgestelde vragen over de back-agent, back-up- en bewaarbeleid, herstel, beveiliging en andere veelgestelde vragen over de Azure back-oplossing."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="back-oplossing; back-service"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Herstel Services kluis - Veelgestelde vragen


In dit artikel vindt u informatie die specifiek zijn voor herstel Services kluis en dit is een aanvulling op de [Back-up-Veelgestelde vragen over Azure](backup-azure-backup-faq.md). De veelgestelde vragen over back-up van Azure biedt de volledige reeks vragen en antwoorden over de back-up van Azure-service.  

U kunt vragen over Azure back-up in het gedeelte Disqus van dit artikel of een gerelateerde artikel. U kunt ook vragen over de back-up van Azure-service plaatsen in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Herstel Services kluizen zijn resourcemanager die zijn gebaseerd. Worden back-up kluizen (klassieke modus) nog steeds ondersteund? <br/>
Ja, back-up kluizen nog steeds worden ondersteund. Back-up kluizen in de [klassieke portal](https://manage.windowsazure.com)maken. Herstel Services kluizen in de [portal van Azure](https://portal.azure.com)maken. Wij raden u herstel services kluis maken als alle toekomstige verbeteringen wordt wel alleen beschikbaar in herstel Services kluis.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Kan ik een back-up-kluis migreren naar een kluis herstel Services? <br/>
Helaas Nee, niet kunt op dit moment u migreren de inhoud van een back-up-kluis naar een kluis herstel Services. We werken aan deze functionaliteit toevoegen, maar het is niet beschikbaar als onderdeel van de Public Preview.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Ondersteunen herstel Services kluizen klassieke VMs of resourcemanager op basis van VMs? <br/>
Herstel Services kluizen ondersteuning voor beide modellen.  U kunt een back-up een VM gemaakt in de klassieke-portal (dit zijn de klassieke modus VMs) of een VM naar een kluis herstel Services gemaakt in de Azure-portal (die zijn gebaseerd resourcemanager).

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Ik heb back-up gemaakt mijn klassieke VMs in back-kluis. Nu wil ik mijn VMs migreren van klassieke modus resourcemanager modus.  Hoe kan ik back-up ze in herstel services kluis?
Back-ups van klassieke VMs in back-kluis won't automatisch migreren naar herstel services kluis wanneer u de VMs van klassieke modus resourcemanager migreren. Volg de onderstaande stappen voor de migratie van back-ups van VM:

1. In back-kluis, gaat u naar het tabblad **Beveiligde Items** en selecteer de VM. Klik op [Beveiliging stoppen](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Laat *verwijderen gekoppeld back-upgegevens* optie **uitgeschakeld**.
2. De virtuele machine migreren van klassieke modus resourcemanager modus. Zorg ervoor dat opslagruimte en netwerk overeenkomt met VM ook worden gemigreerd naar resourcemanager-modus.
3. Een herstel services kluis maken en configureren van back-up op de gemigreerde virtuele machine met de **back-up** actie boven aan de kluis dashboard. Meer informatie over het [inschakelen van back-up in herstel services kluis](backup-azure-vms-first-look-arm.md)
