<properties
    pageTitle="Azure-toets kluis met Azure automatisering beheren | Microsoft Azure"
    description="Meer informatie over hoe de automatisering Azure-service kan worden gebruikt voor het beheren van Azure-toets kluis."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Azure-toets kluis met Azure automatisering beheren

Deze handleiding laat u kennismaken met de automatisering Azure-service en hoe deze kan worden gebruikt voor het beheer van uw toetsen en geheimen in Azure-toets kluis vereenvoudigen.

## <a name="what-is-azure-automation"></a>Wat Azure automatisering is?

[Azure automatisering](../automation/automation-intro.md) is een Azure-service voor het beheer van de cloud met procesautomatisering en configuratie van de gewenste status vereenvoudigen. Met Azure automatisering, handmatige, kunnen herhaald, langdurige en lastige taken worden geautomatiseerd om de betrouwbaarheid, efficiency en tijd aan de waarde voor uw organisatie te verbeteren.

Azure automatisering biedt een uiterst betrouwbare, uiterst beschikbaar werkstroom execution engine die wordt aangepast aan uw wensen. In Azure automatisering, processen volledig uitschakelen handmatig door 3e leveranciers of regelmatig zodat taken plaatsvinden precies wanneer nodig.

Operationele realiseren en vrijgemaakt IT en DevOps personeel kunt richten op het werk dat bedrijven waarde wordt toegevoegd door uw beheertaken voor de cloud worden automatisch wordt uitgevoerd door Azure automatisering te bewegen.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Hoe kunt Azure automatisering Azure-toets kluis beheren?

Toets kluis kunnen worden beheerd in Azure automatisering met behulp van de [AzureRM sleutel kluis cmdlets] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) en de [bijbehorende cmdlets Azure klassieke toets kluis](https://msdn.microsoft.com/library/azure/dn868052.aspx). De Azure-module voor het beheren van klassieke toets kluis vindt u automatisch in Azure automatisering en kunt u de [module AzureRM-KeyVault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) importeren in Azure automatisering, zodat u veel van uw sleutel kluis beheertaken binnen de service kunt uitvoeren. U kunt ook deze cmdlets in Azure automatisering met de cmdlets voor andere Azure-services, complexe om taken te automatiseren via Azure services en 3e partijen systemen koppelen.

Met de cmdlets Azure-toets kluis kunt u deze taken onder andere uitvoeren: 

- Maken en configureren van een belangrijke kluis
- Maken of importeren van een sleutel
- Maken of een geheim bijwerken
- Kenmerken van een sleutel bijwerken
- Een toets of geheim verkrijgen
- Een toets of geheim verwijderen

Hier volgen enkele voorbeelden van het gebruik van de PowerShell-toets kluis beheren:  

* [Azure belangrijke kluis - stap voor stap](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Instellen en configureren van een Azure belangrijke kluis](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Azure automatisering en hoe deze kan worden gebruikt voor het beheren van Azure-toets kluis hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure automatisering.

* Zie de Azure automatisering [aan de slag zelfstudie](../automation/automation-first-runbook-graphical.md).
* Zie de [Azure toets kluis PowerShell-scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).
