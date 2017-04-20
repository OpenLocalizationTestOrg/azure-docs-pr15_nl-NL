<properties
    pageTitle="WinRM toegang instellen voor virtuele Machines in Azure resourcemanager | Microsoft Azure"
    description="WinRM toegang voor gebruik met een resourcemanager Azure virtuele machine instellen"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>WinRM toegang voor virtuele Machines in Azure resourcemanager in te stellen

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM in beheer van Azure-Service tegenover resourcemanager van Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel

* Zie voor een overzicht van de Azure Resource Manager, in dit [artikel](../azure-resource-manager/resource-group-overview.md)
* Voor de verschillen tussen het beheer van Azure-Service en Azure resourcemanager, raadpleegt u in dit [artikel](../resource-manager-deployment-model.md)

Het belangrijkste verschil bij het instellen van WinRM configuratie tussen de twee stapels is hoe het certificaat op de VM wordt geïnstalleerd. De certificaten worden in de stapel Azure resourcemanager gebaseerd als resources die worden beheerd door de sleutel kluis Resource-Provider. Daarom moet de gebruiker om te leveren van hun eigen certificaat en uploadt dit naar een toets kluis voordat u deze in een VM.

Dit zijn de stappen die u uitvoeren moet voor het instellen van een VM met WinRM-connectiviteit

1. Een belangrijke kluis maken
2. Een zelfondertekend certificaat maken
3. Uw zelfondertekend certificaat uploaden naar toets kluis
4. De URL voor uw zelfondertekend certificaat in de sleutel kluis ophalen
5. Overzicht van de URL van uw zelfondertekende certificaten tijdens het maken van een VM

## <a name="step-1-create-a-key-vault"></a>Stap 1: Maak een belangrijke kluis

U kunt het onder de opdracht om te maken van de kluis-toets

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Stap 2: Een zelfondertekend certificaat maken
U kunt een zelfondertekend certificaat met deze PowerShell-script maken

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Stap 3: Uw zelfondertekend certificaat uploaden naar de kluis-toets

Voordat u uploadt het certificaat om de toets in stap 1 hebt gemaakt, moet deze geconverteerd naar een de provider van de resource Microsoft.Compute duidelijke. De onderstaande PowerShell script, kunt u dit doen

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Stap 4: Zorgen dat de URL voor uw zelfondertekend certificaat in de kluis-toets

De provider van de resource Microsoft.Compute moet een URL naar het geheim binnen de sleutel kluis terwijl de inrichting van de VM. Hiermee worden de provider van de resource Microsoft.Compute downloaden van het geheim en de overeenkomstige certificaat op de VM maken.

>[AZURE.NOTE]De URL van het geheim moet bevatten als u ook de versie. Een voorbeeld-URL eruit onder https://contosovault.vault.azure.net:443/geheimen/contososecret/01h9db0df2cd4300a20ence585a6s7ve


#### <a name="templates"></a>Sjablonen

U krijgt de koppeling naar de URL in het sjabloon met de onderstaande code

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell

U krijgt deze URL nu de onder PowerShell-opdracht

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Stap 5: Overzicht van de URL van uw zelfondertekende certificaten tijdens het maken van een VM

#### <a name="azure-resource-manager-templates"></a>Azure resourcemanager-sjablonen

Tijdens het maken van een VM tot en met sjablonen, wordt het certificaat verwezen in de sectie geheimen en de sectie winRM zoals hieronder:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Een voorbeeldsjabloon voor de bovenstaande vindt u hier op [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Broncode voor deze sjabloon kunt u vinden op [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Stap 6: Verbinding maken met de VM
Voordat u verbinding kunt maken met de VM moet u zorgen is de computer is geconfigureerd voor WinRM extern beheer. PowerShell als beheerder starten en uitvoeren het onder de opdracht om te controleren of u bent instellen.

    Enable-PSRemoting -Force

>[AZURE.NOTE] U moet mogelijk Controleer of dat de WinRM-service wordt uitgevoerd als de bovenstaande niet werkt. U kunt uitvoeren die met`Get-Service WinRM`

Nadat de setup is voltooid, kunt u verbinding maken met het VM het onder de opdracht

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate