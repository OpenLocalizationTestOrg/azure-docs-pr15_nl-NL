<properties
   pageTitle="Coderen van de schijf in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de aanbeveling Azure Beveiligingscentrum **toepassen schijfversleuteling**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="apply-disk-encryption-in-azure-security-center"></a>Schijfversleuteling in Azure Beveiligingscentrum toepassen

Azure Beveiligingscentrum wordt aangeraden dat u Schijfopruiming versleuteling toepassen als u Windows of Linux VM schijven die niet zijn versleuteld met Azure schijf codering hebt. Schijfversleuteling kunt u uw Windows- en Linux IaaS VM schijven versleutelen.  Versleuteling geschikt is voor de OS en de gegevens volumes op uw VM.


Schijfversleuteling maakt gebruik van de functie standaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) industrie van Windows en de functie [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) van Linux te leveren OS en gegevens versleuteling om u te helpen beveiligen en uw gegevens beschermen en voldoen aan uw organisatie-beveiliging en naleving verplichtingen. Schijfversleuteling is geïntegreerd met [Azure toets kluis](https://azure.microsoft.com/documentation/services/key-vault/) om te bepalen en de schijf versleuteling toetsen en geheimen in uw sleutel kluis-abonnement en ervoor zorgen dat dat alle gegevens in de schijven VM worden gecodeerd in rust in [Azure opslag](https://azure.microsoft.com/documentation/services/storage/)beheren.

> [AZURE.NOTE] Azure schijfversleuteling wordt ondersteund op de volgende Windows-besturingssystemen voor servers - Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2. Schijfversleuteling wordt ondersteund op de volgende Linux-besturingssystemen voor servers - Ubuntu, CentOS, SUSE en SUSE Linux Enterprise Server (SLES).

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in het blad **aanbevelingen** **toepassen schijfversleuteling**.
2. In het blad **toepassen schijfversleuteling** ziet u een lijst met VMs waarvoor schijfversleuteling wordt aanbevolen.
3. Volg de instructies voor het toepassen van versleuteling op deze VMs.

![][1]

Als u wilt versleutelen Azure virtuele Machines die door Beveiligingscentrum zijn geïdentificeerd als versleuteling, raden we aan de volgende stappen uit:

- Installeren en configureren van Azure PowerShell. Hiermee kunt u de PowerShell-opdrachten die is vereist voor het instellen van de vereisten die zijn vereist voor het coderen van Azure virtuele Machines uitvoeren.
- Aanvragen en het Azure schijf versleuteling vereisten Azure PowerShell-script uitvoeren.
- Uw virtuele machines versleutelen.

[Versleutelen een Azure virtuele machines](security-center-disk-encryption.md) u deze stappen doorlopen.  In dit onderwerp wordt ervan uitgegaan dat u Windows 10 gebruikt als de clientcomputer waaruit u Schijfopruiming-versleuteling configureren.

Zijn er veel methoden die kunnen worden gebruikt voor het instellen van de vereisten en codering voor Azure virtuele Machines configureren. Als u al goed versed in Azure PowerShell of Azure CLI bent, klikt u mogelijk liever alternatieve manieren. Andere methoden Zie voor meer informatie over deze [Azure schijfversleuteling](../security/azure-security-disk-encryption.md).



## <a name="see-also"></a>Zie ook

In dit document hebt u geleerd hoe u het implementeren van de aanbeveling Beveiligingscentrum "Toepassen schijfversleuteling." Zie de volgende onderwerpen voor meer informatie over Schijfopruiming versleuteling:

- [Versleuteling en key management met Azure toets kluis](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video: 36 min 39 sec): informatie over het gebruik van Schijfopruiming versleuteling management voor IaaS VMs en Azure-toets kluis om te helpen beveiligen en uw gegevens beschermen.
- [Een Azure virtuele machines versleutelen](security-center-disk-encryption.md) (document)--leren Azure virtuele Machines versleutelen.
- [Azure schijfversleuteling](../security/azure-security-disk-encryption.md) (document)--informatie over het inschakelen van schijfversleuteling voor Windows en Linux VMs.

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --zoeken blog over Azure beveiliging en naleving in berichten.



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
