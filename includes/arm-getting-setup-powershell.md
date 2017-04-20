## <a name="setting-up-powershell-for-resource-manager-templates"></a>PowerShell in te stellen voor resourcemanager sjablonen

Voordat u Azure PowerShell met Resource Manager gebruiken kunt, moet u het rechts Windows PowerShell en Azure PowerShell-versies.

### <a name="verify-powershell-versions"></a>Controleer of de PowerShell-versies

Controleer of dat u Windows PowerShell versie 3.0 of 4.0 hebt. Als u wilt de versie van Windows PowerShell hebt gevonden, typt u deze opdracht opdrachtprompt van Windows PowerShell.

    $PSVersionTable

Ontvangt u het volgende type van gegevens:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Controleer of de waarde van **PSVersion** 3.0 of 4.0. Als dat niet zo is, raadpleegt u [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Stel uw Azure-account en een abonnement

Als u nog geen een Azure-abonnement hebt, kunt u uw [MSDN abonnee voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of Registreer omhoog activeren voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

Open een opdrachtprompt Azure PowerShell en meld u aan bij Azure met deze opdracht.

    Login-AzureRmAccount

Als u meerdere Azure abonnementen hebt, kunt u uw Azure abonnementen met deze opdracht weergeven.

    Get-AzureRmSubscription

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

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Zie voor meer informatie over Azure abonnementen en -accounts, [hoe: verbinding maken met uw abonnement](powershell-install-configure.md#Connect).
