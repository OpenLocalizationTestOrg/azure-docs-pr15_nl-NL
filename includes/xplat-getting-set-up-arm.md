<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Azure CLI met Azure Resource Manager gebruiken (ARM)

Voordat u de CLI Azure met resourcemanager opdrachten en sjablonen gebruiken kunt om te implementeren Azure resources en werkbelasting resourcegroepen gebruiken, moet u een account met Azure (natuurlijk). Als u niet een account hebt, kunt u een [gratis evaluatieversie van Azure hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.

> [AZURE.NOTE] Als u niet al een Azure-account hebt, maar u een abonnement op MSDN-abonnement hebt, kunt u verkrijgen gratis Azure tegoeden door het activeren van uw [MSDN abonnee voordelen hier](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -- of als u het gratis account kunnen gebruiken. Een werkt voor Azure toegang.

### <a name="step-1-verify-the-azure-cli-version"></a>Stap 1: De versie van de Azure CLI controleren

Als u wilt Azure CLI voor dwingende opdrachten en ARM sjablonen gebruiken, moet u beschikken over minstens versie 0.8.17. Typ om uw versie `azure --version`. Worden er ongeveer als volgt:

    $ azure --version
    0.8.17 (node: 0.10.25)

Als u bijwerken van uw versie van Azure CLI wilt, raadpleegt u [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Stap 2: Controleren of u een werk of school identiteit met Azure

U kunt de modus van de opdracht ARM alleen gebruiken als u een [Azure Active Directory-tenant](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) of een [Service Principal Name](https://msdn.microsoft.com/library/azure/dn132633.aspx)gebruikt. (Deze zijn ook *organisatie-id's*genoemd.)

Als u wilt zien als u een hebt, moet u zich aanmelden door te typen `azure login` en met uw werk of school-gebruikersnaam en wachtwoord in als u hierom wordt gevraagd. Als u een hebt, worden er ongeveer als volgt te werk:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Als u dit niet ziet, moet u een nieuwe tenant (of service principal) met uw Microsoft-account-id maken. (Dit is vaak het geval met persoonlijke MSDN-abonnementen of gratis proefabonnementen.) Als u wilt maken van een werk of school-id van uw Azure-account gemaakt met een Microsoft-id, raadpleegt u [een Azure AD-Directory met een nieuw Azure-abonnement koppelen](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). Als u denkt dat u al een organisatie-id nodig hebt dat, moet u mogelijk praten met de persoon die het account voor u gemaakt.

### <a name="step-3-choose-your-azure-subscription"></a>Stap 3: Uw Azure abonnement kiezen

Als u slechts één abonnement in uw Azure-account hebt, koppelt Azure CLI zichzelf aan die abonnement al dan niet standaard. Als u meer dan één abonnement hebt, moet u Selecteer het abonnement dat u wilt gebruiken door te typen `azure account set <subscription id or name> true` waar _abonnements-id of de naam_ is de abonnements-id of de naam van het abonnement dat u werken wilt met in de huidige sessie.

Worden er ongeveer als volgt te werk:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Stap 4: Uw CLI Azure plaats in de modus ARM

Als u wilt de modus Azure Resource Management (ARM) gebruiken met de CLI Azure `azure config mode arm`. Worden er ongeveer als volgt te werk:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] U kunt overschakelen terug naar de opdrachten voor het beheer van Azure-service gebruiken door te typen `azure config mode asm`.
