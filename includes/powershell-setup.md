<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="rasquill" />

## <a name="setting-up-powershell"></a>Bij het instellen van PowerShell

Voordat u Azure PowerShell gebruikt kunt, volgt u deze stappen.

### <a name="verify-powershell-versions"></a>Controleer of de PowerShell-versies

Voordat u Windows PowerShell gebruiken kunt, moet u Windows PowerShell, versie 3.0 of 4.0 hebben. Als u wilt de versie van Windows PowerShell hebt gevonden, typt u deze opdracht opdrachtprompt van Windows PowerShell.

    $PSVersionTable

U ziet ongeveer zo uitziet.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Controleer of de waarde van **PSVersion** 3.0 of 4.0. Een compatibele om versie te installeren, raadpleegt u [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

U moet ook Azure PowerShell versie 0.8.0 of hoger. U kunt de versie van Azure PowerShell die u hebt geïnstalleerd met deze opdracht bij de opdrachtprompt Azure PowerShell controleren.

    Get-Module azure | format-table version

U ziet ongeveer zo uitziet.

    Version
    -------
    0.8.16.1

Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell](powershell-install-configure.md).


### <a name="set-your-azure-account-and-subscription"></a>Stel uw Azure-account en een abonnement

Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

Open een opdrachtprompt Azure PowerShell en meld u aan bij Azure met deze opdracht.

    Add-AzureAccount

Als u meerdere Azure abonnementen hebt, kunt u uw Azure abonnementen met deze opdracht weergeven.

    Get-AzureSubscription

Ontvangt u het volgende type van gegevens:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

U kunt het huidige Azure abonnement instellen door deze opdrachten bij de opdrachtprompt Azure PowerShell uit te voeren. Vervang alles in de aanhalingstekens, inclusief de < en > tekens op, met de juiste naam.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Zie voor meer informatie over Azure abonnementen en -accounts, [hoe: verbinding maken met uw abonnement](powershell-install-configure.md#Connect).
