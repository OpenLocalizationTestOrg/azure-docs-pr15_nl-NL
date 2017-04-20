<properties
    pageTitle="Een VM implementeren met een certificaat dat met Azure stapel toets kluis | Microsoft Azure"
    description="Informatie over hoe een VM implementeren en een certificaat van Azure stapel toets kluis invoeren"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>Maken van VMs en certificaten opgehaald uit kluis sleutel toevoegen

VMs zijn geïmplementeerd via Azure resourcemanager Azure gestapelde, en u kunt nu certificaten opslaan in Azure stapel toets kluis. Vervolgens geeft Azure stapel (resource provider Microsoft.Compute specifiek) ze in uw VMs wanneer de VMs zijn geïmplementeerd. Certificaten kunnen worden gebruikt in veel scenario's, inclusief SSL-versleuteling en verificatie via clientcertificaat gebaseerd.

Met deze methode, kunt u het certificaat veilige behouden. Is het nu niet in de afbeelding VM of in van de toepassing configuratiebestanden of een andere onveilige locatie. Juiste-beleid voor de belangrijkste kluis instelt, kunt u ook bepalen wie er toegang tot uw certificaat. Een ander voordeel is dat u alle certificaten op één plaats in Azure stapel toets kluis kunt beheren.

Hier volgt een beknopt overzicht van het proces:

-   U moet een certificaat in de .pfx-indeling.

-   Maak een belangrijke kluis (met een sjabloon of het volgende voorbeeldscript).

-   Controleer of dat u de schakeloptie EnabledForDeployment hebt ingeschakeld.

-   Het certificaat als een geheim uploaden.

## <a name="deploying-vms"></a>VMs implementeren

Voorbeeldscript Hiermee maakt u een belangrijke kluis en vervolgens een certificaat dat is opgeslagen in het .pfx-bestand in een lokale map om de belangrijkste als een geheim wordt opgeslagen.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

Het eerste deel van het script leest de .pfx-bestand en klik vervolgens op winkels als een JSON object met het bestand inhoud base64 codering. De JSON-object wordt ook base64 codering.

Vervolgens Hiermee maakt u een nieuwe resourcegroep en maakt vervolgens een belangrijke kluis. Houd rekening met de laatste parameter aan de opdracht Nieuw AzureKeyVault '-EnabledForDeployment', die toegang verleent aan Azure (specifiek aan de Microsoft.Compute resource provider) te lezen geheimen uit de sleutel kluis voor implementaties.

De laatste opdracht gewoon het base64-gecodeerde JSON-object in de belangrijkste kluis opgeslagen als een geheim.

Hier volgt een voorbeelduitvoer van de voorgaande script:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

We kunnen nu klaar om te implementeren van een sjabloon VM. Opmerking de URI van het geheim uit de uitvoer (als dit is gemarkeerd in de voorgaande uitvoer groen).

U moet een sjabloon hier bevinden. De parameters van speciale rente (naast de gebruikelijke VM parameters) zijn de naam van de kluis, de resourcegroep kluis en de geheime URI. U kunt natuurlijk ook kunt downloaden uit GitHub en wijzig desgewenst.

Wanneer deze VM is geïmplementeerd, invoegt Azure het certificaat in de VM.
Certificaten in de .pfx-bestand worden toegevoegd in Windows, met de persoonlijke sleutel kan niet worden geëxporteerd. Het certificaat wordt toegevoegd aan de locatie van het certificaat LocalMachine, met de certificaat-store die de gebruiker opgegeven. Op Linux het certificaatbestand is geplaatst, onder de map /var/lib/waagent, klikt u met de bestandsnaam &lt;UppercaseThumbprint&gt;.crt voor de X509 certificaatbestand, en &lt;UppercaseThumbprint&gt;.prv voor de persoonlijke sleutel.
Beide bestanden zijn .pem opgemaakt.

De toepassing wordt meestal vindt u het certificaat met behulp van de vingerafdruk en wijziging niet nodig.

## <a name="retiring-certificates"></a>Certificaten intrekken


Klik in de voorgaande sectie we u laten zien hoe u een nieuw certificaat aan uw bestaande VMs push. Maar uw oude certificaat zich nog in de VM en kan niet worden verwijderd. Voor extra beveiliging, kunt u het kenmerk voor oude geheim wijzigen in 'Uitgeschakeld', zodat u zelfs als een sjabloon voor oude probeert te maken van een VM met deze oude versie van het certificaat, deze wordt. Hier ziet u hoe u een specifieke geheime versie worden uitgeschakeld instellen:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Sluiten


Met deze methode nieuwe kan het certificaat worden bewaard los van de afbeelding VM of de toepassing nettolading. We hebben dus één punt van belichting verwijderd.

Het certificaat kan ook worden vernieuwd en geüpload naar de toets kluis zonder dat u moet de VM-afbeelding of het implementatiepakket toepassing opnieuw te maken. De toepassing nog moet worden geleverd met de nieuwe URI voor deze nieuwe certificaatversie door.

Door te scheiden van het certificaat van de VM of de toepassing nettolading, hebben we nu het aantal medewerkers die direct toegang heeft tot het certificaat verminderd.

Een ander voordeel hebt u nu een handige plaats in de sleutel kluis om alle certificaten, inclusief de alle versies die zijn geïmplementeerd na verloop van tijd te beheren.

## <a name="next-steps"></a>Volgende stappen

[Een VM met een wachtwoord toets kluis implementeren](azure-stack-kv-deploy-vm-with-secret.md)

[Een toepassing voor toegang tot sleutel kluis toestaan](azure-stack-kv-sample-app.md)