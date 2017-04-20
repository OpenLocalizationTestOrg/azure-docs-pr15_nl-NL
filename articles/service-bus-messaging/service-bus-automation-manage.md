<properties
    pageTitle="Azure-Service Bus met Azure automatisering beheren | Microsoft Azure"
    description="Informatie over het gebruik van de Azure automatisering-service voor het beheren van Azure Service Bus."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Azure-Service Bus met Azure automatisering beheren

Deze handleiding laat u kennismaken met de automatisering Azure-service en hoe deze kan worden gebruikt om beheer van Azure Service Bus te vereenvoudigen.

## <a name="what-is-azure-automation"></a>Wat Azure automatisering is?

[Azure automatisering](../automation/automation-intro.md) is een Azure-service voor het beheer van de cloud met procesautomatisering en configuratie van de gewenste status vereenvoudigen. Met Azure automatisering, handmatige, kunnen herhaald, langdurige en lastige taken worden geautomatiseerd om de betrouwbaarheid, efficiency en tijd aan de waarde voor uw organisatie te verbeteren.

Azure automatisering biedt een uiterst betrouwbare, grote beschikbaarheid werkstroom execution engine die wordt aangepast aan uw wensen. In Azure automatisering, processen volledig uitschakelen handmatig door 3e leveranciers of regelmatig zodat taken plaatsvinden precies wanneer nodig.

Operationele realiseren en vrijgemaakt IT en DevOps personeel kunt richten op het werk dat bedrijven waarde wordt toegevoegd door uw beheertaken voor de cloud worden automatisch wordt uitgevoerd door Azure automatisering te bewegen.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Hoe kunt Azure automatisering Azure Service Bus beheren?

U kunt Service Bus met Azure automatisering beheren met behulp van de [Service Bus REST API](https://msdn.microsoft.com/library/azure/mt639375.aspx). U kunt binnen Azure automatisering PowerShell-scripts om uit te voeren veel van uw Service Bus taken met de REST API uitvoeren. U kunt ook deze REST API-oproepen in Azure automatisering met de cmdlets voor andere Azure-services, complexe om taken te automatiseren via Azure services en 3e partijen systemen koppelen.

Hier volgen enkele voorbeelden van PowerShell gebruiken voor het beheren van Azure Service Bus:

* [Aangepaste PowerShell-cmdlets voor het beheren van Azure Service Bus wachtrijen](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Het maken van Service Bus wachtrijen, onderwerpen en abonnementen met een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Maken van Azure Service Bus naamruimten via PowerShell](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

De PowerShell-module voor het werken met Azure service bus in automatisering runbooks kan worden gedownload van de [PowerShell-galerie](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0).


## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Azure automatisering en hoe deze kan worden gebruikt voor het beheren van Azure Service Bus hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure automatisering.

* Zie de Azure automatisering [aan de slag zelfstudie](../automation/automation-first-runbook-graphical.md)
* Zie het [beheren van Service Bus met PowerShell](service-bus-powershell-how-to-provision.md)
