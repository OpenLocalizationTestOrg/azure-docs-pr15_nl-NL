<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Azure CLI gebruiken

De volgende stappen kunnen u Azure CLI eenvoudig werken met de meest recente versie en het juiste abonnement. Als u nodig hebt voor het installeren van Azure CLI en deze eerst verbinding met uw account, raadpleegt u de [Interface van Azure-opdrachtregel (Azure CLI)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Stap 1: Update Azure CLI versie

Als u wilt gebruiken Azure CLI voor dwingende opdrachten met de modus voor het beheer van service, moet u indien mogelijk een recente versie hebben. Typ om uw versie `azure --version`. Worden er ongeveer als volgt:

    $ azure --version
    0.8.17 (node: 0.10.25)

Als u bijwerken van uw versie van Azure CLI wilt, raadpleegt u [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Stap 2: De Azure-account en een abonnement instellen

Nadat u uw CLI Azure met het account dat u wilt gebruiken verbonden hebt, hebt u mogelijk meer dan één abonnement. Wanneer u doet, raadpleegt u de abonnementen beschikbaar voor uw account door te typen `azure account list`, en selecteer vervolgens het abonnement dat u wilt gebruiken door te typen `azure account set <subscription id or name> true` waar _abonnements-id of de naam_ is de abonnements-id of de naam van het abonnement dat u werken wilt met in de huidige sessie. Worden er ongeveer als volgt te werk:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Als u niet al een Azure-account hebt, maar u een abonnement op MSDN-abonnement hebt, kunt u verkrijgen gratis Azure tegoeden door het activeren van uw [MSDN abonnee voordelen hier](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -- of als u het gratis account kunnen gebruiken. Een werkt voor Azure toegang.
