<properties
   pageTitle="Azure schijfversleuteling voor Windows en Linux IaaS VMs | Microsoft Azure"
   description="Het papier biedt een overzicht van Microsoft Azure schijf versleuteling voor Windows en Linux IaaS VMs."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="krkhan"/>


#<a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Azure schijfversleuteling voor Windows en Linux IaaS VMs

Microsoft Azure wordt ten zeerste erop gebrand om te zorgen dat uw gegevensprivacy, gegevens te garanderen en kunt u aan een besturingselement voor uw Azure gegevens via een bereik van gehost technologieën geavanceerde versleutelen, beheren en beheren van versleuteling toetsen, beheer en controle toegang van gegevens. Dit biedt Azure klanten de flexibiliteit om de oplossing die het beste aan hun zakelijke behoeften te selecteren. In dit artikel, we maakt u kennis met een nieuwe technologieoplossing "Azure schijfversleuteling voor Windows en Linux IaaS VM van" om u te helpen beveiligen en beschermen van uw gegevens om te voldoen aan uw organisatie-beveiliging en naleving verplichtingen. Het papier biedt uitgebreide instructies over het gebruik van de Azure schijf versleuteling functies, inclusief de ondersteunde scenario's en de gebruiker ervaringen.

**Opmerking**: bepaalde aanbevelingen hierin kunnen dit leiden tot hogere gegevens, netwerk of een berekeningscluster Resourcegebruik met resultaat extra licentie of abonnement kosten.

## <a name="overview"></a>Overzicht

Azure schijfversleuteling is een nieuwe mogelijkheid waarmee u uw Windows- en Linux IaaS VM schijven versleutelen. Azure schijfversleuteling maakt gebruik van de functie standaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) industrie van Windows en de functie [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) van Linux te leveren volume-versleuteling voor het besturingssysteem en de gegevensschijven. De oplossing is geïntegreerd met kluis [Azure-toets](https://azure.microsoft.com/documentation/services/key-vault/) om te bepalen en de schijf versleuteling toetsen en geheimen in uw belangrijkste kluis-abonnement en ervoor zorgen dat dat alle gegevens in de virtuele machine schijven worden gecodeerd in rust in Azure opslag beheren.

Azure schijfversleuteling voor Windows en Linux IaaS VMs is nu in de **Algemene beschikbaarheid** in alle Azure openbare regio's voor standaard VMs en VMs met premium opslagmedia.

### <a name="encryption-scenarios"></a>Scenario's voor versleuteling

De oplossing Azure schijfversleuteling ondersteunt de volgende klant-scenario's:

- Versleuteling voor nieuwe IaaS VMs gemaakt op basis van vooraf gecodeerde VHD en versleutelingssleutels inschakelen
- Inschakelen versleuteling voor nieuwe IaaS VMs gemaakt op basis van de galerie met Azure-afbeeldingen
- Versleuteling voor bestaande IaaS VMs uitgevoerd in Azure inschakelen
- Versleuteling voor Windows IaaS VMs uitschakelen
- Versleuteling voor gegevensstations voor Linux IaaS VMs uitschakelen

De oplossing ondersteunt de volgende handelingen uit voor IaaS VMs wanneer ingeschakeld in Microsoft Azure:

- Integratie met Azure belangrijke kluis
- Standaard laag VMs - [A, D, DS, G, GS enzovoort reeks IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)
- Versleuteling voor Windows en Linux IaaS VMs inschakelen
- Versleuteling voor OS en gegevens stations voor Windows IaaS VMs uitschakelen
- Versleuteling voor gegevensstations voor Linux IaaS VMs uitschakelen
- Versleuteling voor IaaS VMs met het Windows-Client-besturingssysteem inschakelen
- Codering voor mountains paden inschakelen
- Inschakelen versleuteling voor Linux VMs geconfigureerd met Software gebaseerde RAID-systeem 
- Versleuteling voor Windows VMs geconfigureerd met spaties opslag inschakelen
- Alle Azure openbare regio's worden ondersteund

De oplossing ondersteunt niet de volgende scenario's, functies en technologie in de versie:

- Eenvoudige laag IaaS VMs
- Versleuteling voor OS station voor Linux IaaS VMs uitschakelen
- IaaS VMs gemaakt met behulp van de methode voor klassieke VM maken
- Integratie met uw on-premises Key Management Service
- Windows Server 2016 Technical Preview wordt niet ondersteund in deze release
- Azure bestanden (Azure bestandsshare), netwerk NFS file system (), dynamische volumes, Windows VMs geconfigureerd met Software gebaseerde RAID-systemen


### <a name="encryption-features"></a>Functies voor versleuteling

Wanneer u inschakelen en implementeren van Azure schijfversleuteling voor Azure IaaS VMs, zijn de volgende mogelijkheden ingeschakeld, afhankelijk van de configuratie:

- Versleuteling van OS volume te beveiligen opstartvolume in rust in opslagruimte van de klant
- Codering van gegevens volume/s de gegevensvolumes in rust in opslagruimte van de klant beveiligen
- Versleuteling voor OS en gegevens stations voor Windows IaaS VMs uitschakelen
- Versleuteling voor gegevensstations voor Linux IaaS VMs uitschakelen
- De toetsen van de versleuteling en geheimen in klant Azure belangrijke kluis abonnement beveiligen
- Rapportage-versleuteling status van de versleutelde IaaS VM
- Verwijdering van schijf versleuteling configuratie-instellingen van de IaaS virtuele machine

De schijfversleuteling Azure voor IaaS VMS voor Windows en Linux-oplossing bevat de schijf versleuteling-extensie voor Windows, schijf versleuteling extensie voor Linux, schijf versleuteling PowerShell-cmdlets, schijf versleuteling CLI cmdlets en schijf versleuteling Azure resourcemanager sjablonen. De oplossing Azure schijf-versleuteling wordt ondersteund op IaaS VMs Windows-of Linux-besturingssysteem. Zie voor meer informatie over de ondersteunde besturingssystemen, vereisten sectie hieronder.

**Opmerking**: Er is zonder bijkomende kosten voor het coderen van VM schijven met Azure schijfversleuteling.

### <a name="value-proposition"></a>Toegevoegde waarde

De oplossing Azure schijf versleuteling Management kunt de volgende zakelijke behoeften in de cloud:

-   De IaaS VM zijn beveiligd in rust industriële standaard versleutelingstechnologie beveiliging van de adres-organisatie en nalevingsvereisten gebruiken.
-   De IaaS VM opstarten onder klant beheerd toetsen en beleid en ze hun gebruik in sleutel kluis kunnen controleren.


### <a name="encryption-workflow"></a>Versleuteling werkstroom

De vereiste om in te schakelen schijfversleuteling voor Windows en Linux VM van hoog niveau stappen volgen:

1. Klant kiest een versleutelingsscenario uit de bovenstaande versleuteling-scenario 's
2. Klant kiest in het dvd-codering via de Azure schijf versleuteling resourcemanager sjabloon of PS cmdlets of CLI opdracht inschakelen en de configuratie versleuteling

    - De klant versleuteld VHD scenario voor de klant uploads de versleutelde VHD op hun opslag-account en versleuteling belangrijke materiaal naar hun toets kluis en geef de configuratie versleuteling om in te schakelen versleuteling voor een nieuwe IaaS VM
    - Voor de nieuwe VM gemaakt op basis van de galerie met Azure en bestaande VM is gebeurd in Azure wordt aangegeven, klant bieden de configuratie versleuteling versleuteling voor de VM IaaS inschakelen

3. Klant verleent toegang tot Azure platform te lezen van de belangrijkste materiaal versleuteling (BitLocker versleuteling toetsen voor Windows-systemen en wachtwoordzin voor Linux) uit hun belangrijke kluis versleuteling voor de VM IaaS inschakelen
4. Klant bieden Azure AD toepassings-id te schrijven het materiaal dat versleuteling belangrijke hun belangrijke kluis versleuteling voor de IaaS VM voor scenario's die worden genoemd in #2 hierboven inschakelen
5.  Azure het model van de service VM bijwerkt met versleuteling en configuratie van de belangrijkste kluis en bepalingen versleuteld VM voor de klant

![Microsoft Antimalware in Azure wordt aangegeven](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Decoderen werkstroom

De vereiste uitschakelen schijfversleuteling voor IaaS VM van hoog niveau stappen volgen:

1. Klant kiest voor het uitschakelen van versleuteling (decoderen) op een actieve IaaS VM in Azure wordt aangegeven via de Azure schijf versleuteling resourcemanager sjabloon of PS cmdlets en de configuratie ontsleutelen.
2. De stap van de versleuteling uitschakelen worden versleuteling van het volume van het besturingssysteem of gegevens of beide van de actieve Windows IaaS VM uitgeschakeld. Echter uitschakelen OS schijf versleuteling voor Linux wordt niet ondersteund zoals in de bovenstaande documentatie is vermeld. De stap uitschakelen is alleen voor gegevensstations op Linux VMs toegestaan. 
4. Azure het model van de service VM bijgewerkt en de VM IaaS ontsleuteld is gemarkeerd. De inhoud van de VM zijn niet meer in rust versleuteld.
5. De klant belangrijke kluis en het materiaal dat versleuteling key, worden niet de uitschakelen versleuteling bewerking verwijderd-BitLocker versleuteling toetsen voor Windows of wachtwoordzin voor Linux.

## <a name="prerequisites"></a>Vereisten voor

De volgende zijn voorwaarden Azure schijfversleuteling voor Azure IaaS VMs inschakelen voor de ondersteunde scenario's in de sectie overview sectienummers

- Een gebruiker beschikken over een geldig actieve Azure abonnement resources maken in Azure wordt aangegeven in de regio's die worden ondersteund
- Azure schijfversleuteling wordt ondersteund op de volgende Windows server SKU's - Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2. Windows Server 2016 Technical Preview wordt niet ondersteund in deze release.
- Azure schijfversleuteling wordt ondersteund op de volgende Windows client SKU's - Client voor Windows 8 en Windows 10-Client.

**Opmerking**: voor Windows Server 2008 R2, .net framework 4.5 moet zijn geïnstalleerd voordat u inschakelt versleuteling in Azure wordt aangegeven. U kunt deze installeren via Windows update door het optionele update te installeren "Microsoft .NET Framework 4.5.2 voor Windows Server 2008 R2 x 64-systemen ([KB2901983](https://support.microsoft.com/kb/2901983))"

- Azure schijfversleuteling wordt ondersteund op de volgende Linux server SKU's - Ubuntu, CentOS, SUSE en SUSE Linux Enterprise Server (SLES) en rood rol Enterprise Linux.

**Opmerking**: Linux OS schijfversleuteling wordt momenteel ondersteund op de volgende Linux onderzoeken - RHEL 7.2, CentOS 7.2, Ubuntu 16.04

- Alle resources (Ex: toets kluis, opslag-account, VM, enzovoort:) moet behoren tot de dezelfde Azure regiocode en het abonnement.

**Opmerking**: Azure schijf codering is vereist dat de sleutel kluis en de VMs bevinden zich in dezelfde Azure regio. Deze configureren in afzonderlijke regio, treedt fout bij het inschakelen van Azure schijf versleutelingsfunctie.

- Als u wilt instellen en Azure-toets kluis configureren voor gebruik van de Azure schijf versleuteling, raadpleegt u sectie **-instelling en Azure toets kluis configureren voor gebruik van de Azure schijf versleuteling** in de sectie *voorwaarden* in dit artikel.
- Als u wilt instellen en configureren van Azure AD-toepassing in Azure Active directory voor gebruik van de Azure schijf versleuteling, raadpleegt u sectie- **configuratie van de Azure AD-toepassing in Azure Active Directory** in de sectie *voorwaarden* in dit artikel.
- Als u wilt instellen en configureren van toets kluis-beleid voor de Azure AD-toepassing, raadpleegt u sectie **instelling toets kluis-beleid voor de Azure AD-toepassing** in de sectie *voorwaarden* in dit artikel.
- Zie de sectie van het **voorbereiden van een vooraf gecodeerde Windows VHD** in de bijlage in dit artikel als u wilt een vooraf gecodeerde Windows VHD voorbereiden.
- Zie de sectie **voor het voorbereiden van een vooraf gecodeerde Linux VHD** in de bijlage in dit artikel als u wilt een vooraf gecodeerde Linux VHD voorbereiden.
- Azure platform vereisten voor toegang tot de toetsen versleuteling of geheimen in klant Azure-toets kluis deze beschikbaar is virtual machine opstarten en het volume VM OS ontsleutelen te maken. Machtigingen voor Azure platform voor toegang tot de klant-toets kluis wilt geven, moet **enabledForDiskEncryption** eigenschap worden ingesteld op de toets kluis voor deze vereiste. Raadpleeg de sectie **-instelling en Azure toets kluis configureren voor gebruik van de Azure schijf versleuteling** in de bijlage van dit artikel voor meer informatie.
- De toets kluis geheim en belangrijke sleutelversleuteling (KEK)-URL's moet versienummer. Azure zorgt ervoor dat deze beperking van versiebeheer. Zie hieronder voorbeelden voor geldige geheim en KEK URL:
    - Voorbeeld van een geldige geheime URL:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
    - Voorbeeld van een geldige KRK KEK:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Azure schijfversleuteling ondersteunt geen poortnummers wordt opgegeven als onderdeel van de sleutel kluis geheim en KEK URL's. Zie hieronder voorbeelden voor ondersteunde toets kluis URL:
    - Niet-geaccepteerde toets kluis URL   *https://contosovault.vault.azure.net:443/geheimen/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
    - Geaccepteerde belangrijke kluis URL:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Functie, de VMs IaaS moet voldoen aan de volgende netwerk eindpunt configuratievereisten om in te schakelen Azure schijfversleuteling: 
    - De IaaS VM moet kunnen verbinding maken met Azure Active Directory-eindpunt \[Login.windows.net\] om een token verbinding maken met Azure belangrijke kluis
    - De IaaS VM moet kunnen verbinding maken met Azure-toets kluis eindpunt te schrijven de toetsen coderingen belangrijke kluis van klant
    - De IaaS VM moet kunnen verbinding maken met Azure opslag-eindpunt waarin de Azure extensie opslagplaats en Azure opslag-mailaccount dat host fungeert voor de VHD-bestanden

**Notitie:** Als uw beveiligingsbeleid toegang via Azure VMs met Internet beperkt, kunt u de bovenstaande URI waarmee u nodig hebt connectivity en configureren van een bepaalde regel zodat uitgaande verbinding met het IP-adressen kunt oplossen.

- Gebruik de nieuwste versie van Azure PowerShell SDK versie Azure schijf codering configureren. Download de nieuwste versie van [Azure PowerShell vrijgeven](https://github.com/Azure/azure-powershell/releases)

**Notitie:** Azure schijfversleuteling wordt niet ondersteund op [Azure PowerShell SDK versie 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Als u een fout opgetreden met behulp van Azure PowerShell 1.1.0 ontvangt, raadpleegt u het artikel [Azure schijf versleuteling fout met betrekking tot Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

- Als u wilt uitvoeren op een van de Azure CLI-opdrachten en koppelen aan uw Azure-abonnement, moet u eerst Azure CLI versie te installeren:
    - Voor het installeren van Azure CLI en koppelen aan uw Azure-abonnement, raadpleegt u [het installeren en configureren van Azure CLI](../xplat-cli-install.md)
    - Via de CLI Azure voor Mac, Linux en Windows met Azure resourcemanager, leest u [hier](azure-cli-arm-commands.md)
- Azure schijf versleuteling oplossing gebruik BitLocker externe sleutel protector voor Windows IaaS VMs. Als uw VMs domein toegevoegd, geen push-Groepsbeleid dat TPM Protector afdwingen. Raadpleeg [in dit artikel](https://technet.microsoft.com/library/ee706521) voor meer informatie over het groepsbeleid voor 'Toestaan BitLocker zonder compatibele TPM'.
- Het Azure schijf versleuteling vereiste PowerShell-script maken van Azure AD-toepassing, nieuwe belangrijke kluis of setup bestaande belangrijke kluis maken en inschakelen versleuteling bevindt zich [hier](https://github.com/Azure/azure-powershell/blob/dev/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).

#### <a name="setup-the-azure-ad-application-in-azure-active-directory"></a>Configuratie van de Azure AD-toepassing in Azure Active Directory

Versleuteling moet worden ingeschakeld op een actieve VM in Azure wordt aangegeven, Azure schijfversleuteling wordt gegenereerd als de toetsen versleuteling schrijft naar uw sleutel kluis. Versleuteling toetsen in-toets kluis beheren, moet Azure AD-verificatie.

Dien moet een Azure AD-toepassing worden gemaakt. Gedetailleerde stappen voor het registreren van een toepassing vindt u hier in de sectie "Een identiteit voor de toepassing ophalen" sectie in dit [blog posten](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).  Dit bericht bevat ook een aantal nuttige voorbeelden over inrichting en configureren van uw sleutel kluis. Geheim client op gebaseerde verificatie of Azure AD certificaat gebaseerde clientverificatie kan worden gebruikt voor verificatiedoeleinden.

##### <a name="client-secret-based-authentication-for-azure-ad"></a>Geheim client op gebaseerde verificatie voor Azure AD

De volgende gedeelten hebben de benodigde stappen voor het configureren van een client geheime gebaseerde verificatie voor Azure AD.

##### <a name="create-a-new-azure-ad-app-using-azure-powershell"></a>Maak een nieuwe Azure AD-app via Azure PowerShell

Gebruik van de onderstaande PowerShell-cmdlet Maak een nieuwe Azure AD-app:

    $aadClientSecret = “yourSecret”
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

**Notitie:** $azureAdApplication.ApplicationId is de Azure AD-ClientID en $aadClientSecret is de client geheim dat u later gebruiken moet voor het inschakelen van ADE. U moet de geheim Azure AD-client correct beschermen.


##### <a name="provisioning-the-azure-ad-client-id-and-secret-from-the-azure-classic-deployment-model-portal"></a>Inrichting van de Azure AD-client-ID en geheim uit het model met Azure klassieke implementatie Portal

Azure AD-Client-ID en geheim kan ook worden ingericht behulp van de Azure klassieke implementatiemodel Portal in https://manage.windowsazure.com, voert u de onderstaande stappen voor het uitvoeren van deze taak:

1. Klik op het tabblad Active Directory, zoals wordt weergegeven in de volgende afbeelding:

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. Klik op toevoegen-toepassing en typt u de naam van de toepassing zoals hieronder wordt weergegeven:

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. Klik op de pijl en configureren van de app-eigenschappen, zoals hieronder wordt weergegeven:

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Klik op het vinkje in de linkerbenedenhoek om te voltooien. Pagina van de configuratie van de app wordt weergegeven. Zoals u ziet dat de Azure AD-Client-ID bevindt zich in de linkerbenedenhoek van de pagina, zoals wordt weergegeven in de onderstaande afbeelding.

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. de geheim Azure AD-client besparen en klikt u op in de knop opslaan. Klik in het opslaan naar de knop en houd rekening met het geheim van het tekstvak toetsen, dit is de geheim Azure AD-client. U moet de geheim Azure AD-client correct beschermen.

![Azure schijfversleuteling](./media/azure-security-disk-encryption/disk-encryption-fig7.png)


**Notitie:** deze stroom hierboven niet wordt ondersteund in de Portal.

##### <a name="use-an-existing-app"></a>Een bestaande app gebruiken

U moet de Azure AD PowerShell-module, die kan worden verkregen vanaf [hier](https://technet.microsoft.com/library/jj151815.aspx)de onderstaande opdrachten uitvoeren.

**Notitie:** de onderstaande opdrachten alleen worden uitgevoerd vanaf een nieuw PowerShell-venster. Gebruik geen Azure PowerShell of het venster Azure resourcemanager uitvoeren van deze opdrachten. De reden voor deze aanbeveling is dat deze cmdlets in de module MSOnline of Azure AD PowerShell zijn.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Op basis van certificaatverificatie voor Azure AD

> [AZURE.NOTE] AAD gebaseerd certificaatverificatie is momenteel niet ondersteund op Linux VMs.

De volgende gedeelten hebben de nodige maatregelen om een certificaat dat is gebaseerd verificatie configureren voor Azure AD.

##### <a name="create-a-new-azure-ad-app"></a>Maak een nieuwe Azure AD-app

De onderstaande Maak een nieuwe PowerShell-cmdlets uitvoeren Azure AD-app:

**Notitie:** Vervang `yourpassword` tekenreeks onder van uw wachtwoord beveiligde en het wachtwoord beveiligen.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Wanneer u klaar bent met deze stap, een .pfx-bestand uploaden naar toets kluis en schakelt u het access-beleid die nodig zijn voor het implementeren van dat certificaat voor een VM.

##### <a name="use-an-existing-azure-ad-app"></a>Gebruik een bestaande Azure AD-app
Als u op basis van certificaatverificatie voor een bestaande app configureert, gebruikt u de onderstaande PowerShell-cmdlets. Zorg ervoor dat ze uitvoeren vanuit een nieuwe PowerShell-venster.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Wanneer u klaar bent met deze stap, een .pfx-bestand uploaden naar toets kluis en schakelt u het access-beleid die nodig zijn voor het implementeren van dat certificaat voor een VM.

##### <a name="upload-a-pfx-file-to-key-vault"></a>Een PFX-bestand uploaden naar toets kluis
In dit [blog posten](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx) voor gedetailleerde uitleg over de werking van dit proces, kunt u lezen. De onderstaande PowerShell-cmdlets zijn echter alles wat die u nodig hebt voor deze taak. Controleer of ze vanuit Azure PowerShell-console uitvoeren:

**Notitie:** Vervang `yourpassword` tekenreeks onder van uw wachtwoord beveiligde en het wachtwoord beveiligen.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-key-vault-to-an-existing-vm"></a>Een certificaat in sleutel kluis implementeren naar een bestaande VM
Nadat de PFX uploaden is voltooid, gebruikt u de onderstaande stappen naar een certificaat in sleutel kluis implementeren naar een bestaande VM:

    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName


#### <a name="setting-key-vault-access-policy-for-the-azure-ad-application"></a>Toets kluis-beleid instellen voor de Azure AD-toepassing

Uw Azure AD-toepassing, moet de rechten voor toegang tot de toetsen of geheimen in de kluis. Gebruik de cmdlet [Set-AzureKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/dn903607.aspx) machtigen om de toepassing en het gebruik van de Client-Id (dat is gegenereerd wanneer de toepassing is geregistreerd) als de waarde van de parameter – ServicePrincipalName. U kunt [dit blogbericht](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) voor enkele voorbeelden op die lezen. Hieronder hebt u ook een voorbeeld van het uitvoeren van deze taak via PowerShell:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

**Opmerking**: Azure schijfversleuteling, moet u voor het configureren van de volgende clienttoegangsbeleid met de clienttoepassing van uw AAD - 'WrapKey' en 'Set-machtigingen

## <a name="terminology"></a>Terminologie

Gebruik de tabel terminologie als verwijzing voor meer informatie over enkele van de algemene voorwaarden die worden gebruikt door deze technologie:

| Terminologie           | Definitie                                                                                                                                                                                                                                   |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure AD                   | Azure AD is [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Azure AD-account is spelen voor verificatie, opslaan en geheimen ophalen uit de sleutel kluis.                                                                                                        |
| Azure belangrijke kluis [AKV] | Azure-toets kluis is een cryptografische key management-service op basis van FIPS gevalideerde Hardware beveiligingsmodules ter bescherming van uw cryptografische sleutels en vertrouwelijke geheimen veilig., raadpleegt u [Toets kluis](https://azure.microsoft.com/services/key-vault/) documentatie voor meer informatie.          |
| OP ARM                   | Azure resourcemanager                                                                                                                                                                                                                       |
| BitLocker             | [BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is dat een industriële herkend gebruikt om te schakelen schijfversleuteling voor Windows IaaS VMs volume-versleutelingstechnologie voor Windows                                                                                                                  |
| BEK                   | BitLocker versleutelingssleutels worden gebruikt voor het coderen van de OS opstartvolume en gegevensvolumes. De toetsen BitLocker zijn beschermen in van de klant Azure belangrijke kluis als geheimen.                                                                              |
| CLI                   | [Azure opdrachtregel-Interface](../xplat-cli-install.md)                                                                                                                                                                                                                 |
| DM-Crypt              | [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is het Linux gebaseerde transparante versleuteling schijfsubsysteem schijfversleuteling voor Linux IaaS VMs inschakelen                                                                                                                           |
| KEK                   | Sleutel versleutelingssleutel is de asymmetrische sleutel (2048 RSA) gebruikt om te beveiligen of laten teruglopen het geheim desgewenst. U kunt een HSM is beveiligd of software-beveiliging sleutel opgeeft. Voor meer informatie raadpleegt u [Azure toets kluis](https://azure.microsoft.com/services/key-vault/) documentatie voor meer informatie |
| PS cmdlets            | [Azure PowerShell-cmdlets](powershell-install-configure.md)                                                                                                                                                                                                                                           |

### <a name="setting-and-configuring-azure-key-vault-for-azure-disk-encryption-usage"></a>Instelling en configureren van Azure-toets kluis voor Azure schijf versleuteling gebruik

Azure schijfversleuteling beveiligt de schijf versleutelingssleutels en geheimen in uw Azure-toets kluis. Volg de stappen van elk niveau van de onderstaande secties Setup-toets kluis voor gebruik van de Azure schijf-codering.

#### <a name="create-a-new-key-vault"></a>Een nieuwe belangrijke kluis maken
Als u wilt een nieuwe sleutel kluis maken, gebruikt u een van de volgende opties:

- Gebruik de "101-maken-KeyVault" resourcemanager sjabloon zich [hier](https://github.com/Azure/azure-quickstart-templates/blob/master/101-create-key-vault/azuredeploy.json)
- Gebruik de Azure PowerShell- [cmdlets toets kluis](https://msdn.microsoft.com/library/dn868052.aspx).
- Gebruik de portal van de manager Azure resource.

**Notitie:** Als u al een toets kluis instellen voor uw abonnement hebt, gaat u naar volgende sectie.

![Azure belangrijke kluis](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="provisioning-a-key-encryption-key-optional"></a>Inrichten van een sleutel versleutelingssleutel (optioneel)

Als u laten teruglopen de BitLocker-versleutelingssleutels via een toets versleuteling sleutel (KEK) voor extra beveiliging wilt, moet u een KEK toevoegen aan uw sleutel kluis voor gebruik in het inrichten.  Gebruik de cmdlet [Toevoegen-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868048.aspx) om het maken van een nieuwe sleutel versleutelingssleutel in de sleutel kluis. U kunt ook KEK uit uw on-premises implementatie key management HSM importeren. Zie [toets kluis documentatie](https://azure.microsoft.com/documentation/services/key-vault/)voor meer informatie.

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

De KEK kan worden toegevoegd vanuit resourcemanager Azure-portal als u ook met Azure-toets kluis UX.

![Azure belangrijke kluis](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions-to-allow-the-azure-platform-access-to-the-keys-and-secrets"></a>Toets kluis-machtigingen toe te staan dat Azure platform toegang tot de toetsen en geheimen instellen

Het Azure platform vereisten voor toegang tot de toetsen versleuteling of geheimen in uw Azure-toets kluis deze beschikbaar te maken voor VM opstarten en de hoeveelheden ontsleutelen. Als u wilt machtigen om het Azure platform zodat deze toegang heeft tot de sleutel kluis, moet de eigenschap *enabledForDiskEncryption* worden ingesteld op de toets kluis. U kunt de eigenschap enabledForDiskEncryption instellen op uw belangrijkste kluis met de belangrijkste kluis PS cmdlet:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

U kunt ook de eigenschap *enabledForDiskEncryption* instellen via https://resources.azure.com. Zoals eerder, moet u de eigenschap *enabledForDiskEncryption* instellen op uw sleutel kluis. Anders de implementatie, mislukt.

U kunt clienttoegangsbeleid instellen voor uw toepassing AAD uit de sleutel kluis UX:

![Azure belangrijke kluis](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure belangrijke kluis](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Zorg ervoor dat toets kluis voor schijf-codering in 'Geavanceerde Clienttoegangsbeleid' is ingeschakeld:

![Azure belangrijke kluis](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Scenario's voor implementatie van schijf-versleuteling en gebruikerservaringen

Zijn er veel scenario's die u Schijfopruiming versleuteling inschakelen kunt en de stappen op basis van het scenario variëren kunnen. De volgende gedeelten wordt uitgelegd in meer details in deze scenario's.

### <a name="enable-encryption-on-new-iaas-vms-created-from-the-azure-gallery"></a>Inschakelen versleuteling voor nieuwe IaaS VM van gemaakt op basis van de Azure-galerie

Schijfversleuteling kan worden ingeschakeld op de nieuwe IaaS Windows VM van Azure galerie in Azure wordt aangegeven met de sjabloon resourcemanager gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image). Klik op "Implementeren naar Azure"-knop op de sjabloon Azure quickstart, de configuratie van de invoer versleuteling in het blad parameters en klik op OK. Selecteer het abonnement, de resourcegroep, de resource groep locatie, de juridische voorwaarden en de overeenkomst en klik op de knop maken om in te schakelen versleuteling voor een nieuwe IaaS VM.

**Notitie:** Deze sjabloon maakt een nieuwe versleutelde Windows VM gebruik van de afbeelding van de galerie met Windows Server 2012.

Schijfversleuteling kan worden ingeschakeld op een nieuwe IaaS RedHat Linux 7.2 VM met een 200 GB RAID-0-matrix met [deze](https://aka.ms/fde-rhel) sjabloon voor een resource manager. Nadat de sjabloon wordt geïmplementeerd, controleert u of de VM versleuteling status gebruiken de `Get-AzureRmVmDiskEncryptionStatus` cmdlet zoals is beschreven in de sectie "[coderen OS station op een actieve Linux VM](#encrypting-os-drive-on-a-running-linux-vm)". Wanneer de computer opgevraagd `VMRestartPending`, start u de VM opnieuw.

U kunt de details van resourcemanager sjabloon parameters zien voor nieuwe VM van Azure galerie scenario met Azure AD-Client-ID in de onderstaande tabel:

| Parameter                        | Beschrijving|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| adminUserName                 | Beheerder gebruikersnaam in te voeren voor de virtuele machine                                                                                                                           |
| Beheerderswachtwoord                 | Gebruiker beheerderswachtwoord voor de virtuele machine                                                                                                                       |
| newStorageAccountName         | Naam van het account opslag voor de opslag van OS en gegevens VHD 's                                                                                                             |
| vmSize                        | Grootte van de VM. Op dit moment worden alleen standaard A, D, G-reeks ondersteund                                                                                          |
| virtualNetworkName            | Naam van de VNet waarnaar de NIC VM moet behoort.                                                                                                            |
| subnetName                    | Naam van het subnet in de vNet waarnaar de NIC VM tot behoren moet                                                                                               |
| AADClientID                   | Cliënt-ID van de Azure AD-app met geheimen schrijfmachtigingen voor sleutel kluis                                                                                       |
| AADClientSecret               | Geheim van de client van de Azure AD-app met geheimen schrijfmachtigingen voor sleutel kluis                                                                                   |
| keyVaultURL                   | URL van de sleutel kluis naar welke BitLocker sleutel moet worden geüpload naar. U kunt werken met de cmdlet: (Get-AzureRmKeyVault - VaultName,-ResourceGroupName). VaultURI |
| keyEncryptionKeyURL           | URL van de Key versleuteling die wordt gebruikt voor het coderen van de gegenereerde BitLocker-sleutel. Dit is optioneel.                                                               |
| keyVaultResourceGroup         | Resourcegroep van de belangrijkste kluis                                                               |
| vmName                        | Naam van de VM over welke codering bewerking wordt uitgevoerd


**Notitie:** KeyEncryptionKeyURL is een optionele parameter. U kunt uw eigen KEK overbrengen naar verdere beschermen de gegevens versleutelingssleutel (wachtwoordzin geheim) in de sleutel kluis.

### <a name="enable-encryption-on-new-iaas-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Inschakelen versleuteling voor nieuwe IaaS VM van samengesteld op basis van de klant versleuteld VHD en versleuteling sleutels

In dit scenario kunt u inschakelen coderen met behulp van de resourcemanager sjabloon, de PowerShell-cmdlets of de CLI-opdrachten. De secties hierna wordt uitgelegd in meer details in de sjabloon resourcemanager en CLI-opdrachten.

Volg de instructies uit een van deze secties voor het voorbereiden van vooraf gecodeerde afbeeldingen die kunnen worden gebruikt in Azure wordt aangegeven. Nadat de afbeelding is gemaakt, kunnen de stappen in de volgende sectie voor het maken van een versleutelde Azure VM worden gebruikt.

- [Een vooraf gecodeerde Windows VHD voorbereiden](#preparing-a-pre-encrypted-windows-vhd)
- [Een vooraf gecodeerde Linux VHD voorbereiden](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-resource-manager-template"></a>Resourcemanager sjabloon gebruiken

Schijfversleuteling kan worden ingeschakeld op klant versleuteld met de sjabloon resourcemanager VHD gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm). Klik op "Implementeren naar Azure"-knop op de sjabloon Azure quickstart, de configuratie van de invoer versleuteling in het blad parameters en klik op OK. Selecteer het abonnement, de resourcegroep, de resource groep locatie, de juridische voorwaarden en de overeenkomst en klik op de knop maken om in te schakelen versleuteling voor nieuwe IaaS VM.

De details resourcemanager sjabloon parameters voor klant versleuteld VHD scenario worden beschreven in de onderstaande tabel:

| Parameter                        | Beschrijving|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| newStorageAccountName | Naam van het account opslag om op te slaan versleuteld OS vhd. Dit account opslag moet al zijn gemaakt in de resourcegroep voor dezelfde en dezelfde locatie als de VM                                                     |
| osVhdUri              | URI van OS vhd van opslag-account                                                                                                                                                                                      |
| waardereeksen                | OS producttype (Windows/Linux)                                                                                                                                                                                         |
| virtualNetworkName    | Naam van de VNet waarnaar de NIC VM moet behoort. Dit moet zijn al gemaakt in de resourcegroep voor dezelfde en dezelfde locatie als de VM                                                                     |
| subnetName            | Naam van het subnet in de vNet waarnaar de NIC VM tot behoren moet                                                                                                                                                     |
| vmSize                | Grootte van de VM. Op dit moment worden alleen standaard A, D, G-reeks ondersteund                                                                                                                                                |
| keyVaultResourceID    | De toets identificeren ResourceID kluis resource in ARM. U kunt werken met de PowerShell-cmdlet: (Get-AzureRmKeyVault - VaultName &lt;yourKeyVaultName&gt; - ResourceGroupName &lt;yourResourceGroupName&gt;). ResourceId |
| keyVaultSecretUrl     | URL van de schijf sleutel ingericht in belangrijke kluis                                                                                                                                                                |
| keyVaultKekUrl        | URL van de Key versleuteling die is voor het coderen van de sleutel wordt gegenereerd Schijfopruiming                                                                                                                                       |
| vmName               | Naam van de VM IaaS   |


#### <a name="using-powershell-cmdlets"></a>PowerShell-cmdlets gebruiken

Schijfversleuteling kan worden ingeschakeld op klant versleuteld VHD gebruik van de cmdlets PS gepubliceerde [hier](https://msdn.microsoft.com/library/azure/mt603746.aspx).  

#### <a name="using-cli-commands"></a>CLI-opdrachten gebruiken

Volg de onderstaande stappen om de schijf-codering in dit scenario met CLI opdrachten inschakelen:

1. Access-beleid instellen voor sleutel kluis:
    - De vlag 'EnabledForDiskEncryption' instellen:`azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
    - Machtigingen instellen bij Azure AD-app te schrijven geheimen KeyVault:`azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]`
2. Schakelt u versleuteling voor een bestaande/voorlopig VM:  *azure vm inschakelen-schijf-codering--resourcegroep <resourceGroupName> --naam <vmName> --aad-client-id <aadClientId> --aad-client-geheim <aadClientSecret> --schijf-versleuteling-toets-kluis-url <keyVaultURL> --schijf-versleuteling-toets-kluis-id <keyVaultResourceId> *
3. Versleuteling status ophalen: *"azure vm weergeven-schijf-versleuteling-status--resourcegroep <resourceGroupName> --naam <vmName> --json"*
4. Om in te schakelen versleuteling voor een nieuwe VM van klant versleuteld VHD, gebruikt u de onderstaande parameters met de opdracht 'azure vm maken':
    - schijf-versleuteling-toets-kluis-id < schijf-versleuteling-toets-kluis-id >
    - schijf-versleuteling-toets-url < schijf-versleuteling-toets-url >
    - toets-versleuteling-toets-kluis-id < toets-versleuteling-toets-kluis-id >
    - toets-versleuteling-toets-url < toets-versleuteling-toets-url >


### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Codering op bestaande of IaaS Windows VM uitgevoerd in Azure inschakelen

In dit scenario kunt u inschakelen coderen met behulp van de resourcemanager sjabloon, de PowerShell-cmdlets of de CLI-opdrachten. De secties hierna wordt uitgelegd in meer details in het inschakelen van deze sjabloon resourcemanager en CLI opdrachten gebruiken.

#### <a name="using-resource-manager-template"></a>Resourcemanager sjabloon gebruiken

Schijfversleuteling kan worden ingeschakeld op de bestaande/voorlopig IaaS Windows VM in Azure wordt aangegeven met de sjabloon resourcemanager gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm). Klik op "Implementeren naar Azure"-knop op de sjabloon Azure quickstart, de configuratie van de invoer versleuteling in het blad parameters en klik op OK. Selecteer het abonnement, de resourcegroep, de resource groep locatie, de juridische voorwaarden en de overeenkomst en klik op de knop maken om in te schakelen versleuteling voor bestaande/voorlopig IaaS VM.

De details resourcemanager sjabloon parameters voor bestaande/voorlopig VM scenario met Azure AD-Client-ID zijn beschikbaar in de onderstaande tabel:

| Parameter                 | Beschrijving|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AADClientID            | Cliënt-ID van de Azure AD-app met geheimen schrijfmachtigingen voor sleutel kluis                                                                                                                                              |
| AADClientSecret         | Geheim van de client van de Azure AD-app met geheimen schrijfmachtigingen voor sleutel kluis                                                                                                                                          |
| keyVaultName | Naam van de sleutel kluis naar welke BitLocker sleutel moet worden geüpload naar. U kunt werken met de cmdlet: (Get-AzureRmKeyVault - ResourceGroupName <yourResourceGroupName>). Vaultname   |
|  keyEncryptionKeyURL   | URL van de Key versleuteling die wordt gebruikt voor het coderen van de gegenereerde BitLocker-sleutel. Dit is optioneel als u selecteert `nokek` in de vervolgkeuzelijst UseExistingKek. Als u selecteert `kek` in de vervolgkeuzelijst UseExistingKek, moet u de waarde keyEncryptionKeyURL ingevoerd                                                                                                                        |
| volumeType             | Type van het volume over welke codering bewerking wordt uitgevoerd. Geldige waarden zijn "OS", "Gegevens", "Alle"                                                                                                                     |
| sequenceVersion         | De versie van de volgorde van de bewerking BitLocker. In dit versienummer verhoogd telkens wanneer een schijf versleuteling bewerking wordt uitgevoerd op de dezelfde VM                                                                             |
| vmName                 | Naam van de VM over welke codering bewerking wordt uitgevoerd


**Notitie:** KeyEncryptionKeyURL is een optionele parameter. U kunt uw eigen KEK overbrengen naar verdere beschermen de gegevens versleutelingssleutel (BitLocker versleuteling geheim) in de sleutel kluis.

#### <a name="using-powershell-cmdlets"></a>PowerShell-cmdlets gebruiken

Raadpleeg het **verkennen Azure schijfversleuteling met Azure PowerShell** -blog bericht [deel 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) en [deel 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) voor meer informatie over het inschakelen van versleuteling Azure schijf-versleuteling gebruikt met PS-cmdlets.

#### <a name="using-cli-commands"></a>CLI-opdrachten gebruiken

Volg de onderstaande stappen om de versleuteling inschakelen op IaaS Windows VM in Azure wordt aangegeven met CLI opdrachten bestaande/uitvoeren:

1. Access-beleid instellen voor sleutel kluis:
    - De vlag 'EnabledForDiskEncryption' instellen: "azure keyvault set-beleid--kluis-naam <keyVaultName> --ingeschakeld-voor-schijf-codering true"
    - Machtigingen instellen bij Azure AD-app te schrijven geheimen KeyVault: "azure keyvault set-beleid--kluis-naam <keyVaultName> --name <aadClientID> --perms-naar-toetsen [\"alle\"]--perms-naar-geheimen [\"alle\"]"
2. Schakelt u versleuteling voor een bestaande/voorlopig VM:  *azure vm inschakelen-schijf-codering--resourcegroep <resourceGroupName> --naam <vmName> --aad-client-id <aadClientId> --aad-client-geheim <aadClientSecret> --schijf-versleuteling-toets-kluis-url <keyVaultURL> --schijf-versleuteling-toets-kluis-id <keyVaultResourceId> *
3. Versleuteling status ophalen: *"azure vm weergeven-schijf-versleuteling-status--resourcegroep <resourceGroupName> --naam <vmName> --json"*
4. Om in te schakelen versleuteling voor een nieuwe VM van klant versleuteld VHD, gebruikt u de onderstaande parameters met de opdracht 'azure vm maken':
    - schijf-versleuteling-toets-kluis-id < schijf-versleuteling-toets-kluis-id >
    - schijf-versleuteling-toets-url < schijf-versleuteling-toets-url >
    - toets-versleuteling-toets-kluis-id < toets-versleuteling-toets-kluis-id >
    - toets-versleuteling-toets-url < toets-versleuteling-toets-url >


### <a name="enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure"></a>Codering op bestaande of IaaS Linux VM uitgevoerd in Azure inschakelen

Schijfversleuteling kan worden ingeschakeld op de bestaande/voorlopig IaaS Linux VM in Azure wordt aangegeven met de sjabloon resourcemanager gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm). Klik op "Implementeren naar Azure"-knop op de sjabloon Azure quickstart, de configuratie van de invoer versleuteling in het blad parameters en klik op OK. Selecteer het abonnement, de resourcegroep, de resource groep locatie, de juridische voorwaarden en de overeenkomst en klik op de knop maken om in te schakelen versleuteling voor bestaande/voorlopig IaaS VM.

De details resourcemanager sjabloon parameters voor bestaande/voorlopig VM scenario met Azure AD-Client-ID worden in de onderstaande tabel beschreven:

| Parameter                 | Beschrijving|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AADClientID            | Cliënt-ID van de Azure AD-app met geheimen schrijfmachtigingen voor sleutel kluis                                                                                                                                              |
| AADClientSecret         | Geheim van de client van de Azure AD-app met geheimen schrijfmachtigingen voor sleutel kluis                                                                                                                                          |
| keyVaultName | Naam van de sleutel kluis naar welke BitLocker sleutel moet worden geüpload naar. U kunt werken met de cmdlet: (Get-AzureRmKeyVault - ResourceGroupName <yourResourceGroupName>). Vaultname   |
|  keyEncryptionKeyURL   | URL van de Key versleuteling die wordt gebruikt voor het coderen van de gegenereerde BitLocker-sleutel. Dit is optioneel als u 'nokek' in de vervolgkeuzelijst UseExistingKek selecteert. Als u 'kek' in de vervolgkeuzelijst UseExistingKek selecteert, moet u de waarde keyEncryptionKeyURL ingevoerd                                                                                                                        |
| volumeType             | Type van het volume over welke codering bewerking wordt uitgevoerd. Geldige ondersteunde waarden zijn "OS" / "Alle" (voor RHEL 7.2, CentOS 7.2 en Ubuntu 16.04) en "Gegevens" voor alle andere distros.                                                                                                                      |
| sequenceVersion         | De versie van de volgorde van de bewerking BitLocker. In dit versienummer verhoogd telkens wanneer een schijf versleuteling bewerking wordt uitgevoerd op de dezelfde VM                                                                             |
| vmName                 | Naam van de VM over welke codering bewerking wordt uitgevoerd
| Wachtwoordzin              | Typ een sterke wachtwoordzin als de sleutel data-versleuteling                                                                                                                                                                       |                                                                                                                                                                                                                                                      

**Notitie:** KeyEncryptionKeyURL is een optionele parameter. U kunt uw eigen KEK overbrengen naar verdere beschermen de gegevens versleutelingssleutel (wachtwoordzin geheim) in de sleutel kluis.

#### <a name="cli-commands"></a>CLI-opdrachten

Schijfversleuteling kan worden ingeschakeld op klant versleuteld VHD met de opdracht CLI is geïnstalleerd vanaf [hier](../xplat-cli-install.md). Volg de onderstaande stappen om de versleuteling inschakelen op IaaS Linux VM in Azure wordt aangegeven met CLI opdrachten bestaande/uitvoeren:

1. Access-beleid instellen voor sleutel kluis:
    - De vlag 'EnabledForDiskEncryption' instellen: "azure keyvault set-beleid--kluis-naam <keyVaultName> --ingeschakeld-voor-schijf-codering true"
    - Machtigingen instellen bij Azure AD-app te schrijven geheimen KeyVault: "azure keyvault set-beleid--kluis-naam <keyVaultName> --name <aadClientID> --perms-naar-toetsen [\"alle\"]--perms-naar-geheimen [\"alle\"]"
2. Schakelt u versleuteling voor een bestaande/voorlopig VM:  *azure vm inschakelen-schijf-codering--resourcegroep <resourceGroupName> --naam <vmName> --aad-client-id <aadClientId> --aad-client-geheim <aadClientSecret> --schijf-versleuteling-toets-kluis-url <keyVaultURL> --schijf-versleuteling-toets-kluis-id <keyVaultResourceId> *
3. Versleuteling status ophalen: "azure vm weergeven-schijf-versleuteling-status--resourcegroep <resourceGroupName> --naam <vmName> --json"
4. Om in te schakelen versleuteling voor een nieuwe VM van klant versleuteld VHD, gebruikt u de onderstaande parameters met de opdracht 'azure vm maken'.
    - *schijf-versleuteling-toets-kluis-id < schijf-versleuteling-toets-kluis-id >*
    - *schijf-versleuteling-toets-url < schijf-versleuteling-toets-url >*
    - *toets-versleuteling-toets-kluis-id < toets-versleuteling-toets-kluis-id >*
    - *toets-versleuteling-toets-url < toets-versleuteling-toets-url >*

### <a name="get-encryption-status-of-an-encrypted-iaas-vm"></a>Versleuteling status van een versleutelde IaaS VM ophalen

U gaat versleuteling status met Azure resourcemanager-portal, [PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt622700.aspx) of CLI opdrachten. De secties hierna wordt uitgelegd hoe de Azure-portal en CLI-opdrachten gebruiken om de status versleuteling.

#### <a name="get-encryption-status-of-an-encrypted-windows-vm-using-azure-resource-manager-portal"></a>Versleuteling status van een versleutelde Windows VM met behulp van Azure resourcemanager portal ophalen

U kunt de status versleuteling van IaaS VM krijgen van Azure resourcemanager-portal. Aanmelden bij Azure-portal op https://portal.azure.com/, klik op de koppeling virtuele machines in het linkermenu om te zien overzichtsweergave van de virtuele machines in uw abonnement. U kunt de weergave virtuele machines filteren op de naam van het abonnement selecteren in de vervolgkeuzelijst abonnement. Klik op kolommen aan de bovenkant van het menu van de pagina virtuele machines bevinden. Selecteer Schijfopruiming versleuteling kolom in het blad van de kolom kiezen en klik op bijwerken. Hier ziet u de schijf versleuteling kolom met de status van het versleuteling "Ingeschakeld" of "Niet ingeschakeld" voor elke VM zoals wordt weergegeven in de onderstaande afbeelding.

![Microsoft Antimalware in Azure wordt aangegeven](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-encryption-status-of-an-encrypted-windowslinux-iaas-vm-using-disk-encryption-ps-cmdlet"></a>Versleuteling status van een versleutelde (Windows/Linux) IaaS VM schijf-versleuteling gebruikt PS Cmdlet ophalen
U kunt de status versleuteling van IaaS VM krijgen van de schijf versleuteling PS cmdlet "Get-AzureRmVMDiskEncryptionStatus". Als u de versleutelingsinstellingen voor uw VM, typt u in uw Azure PowerShell-sessie:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

De uitvoer van Get-AzureRmVMDiskEncryptionStatus kan worden gecontroleerd op versleuteling belangrijke URL's.
    
    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMNam
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey
    
    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

De waarde van de instellingen OSVolumeEncrypted en DataVolumesEncrypted zijn ingesteld op "Versleutelde" met dat zowel de hoeveelheden worden gecodeerd Azure schijf-versleuteling gebruikt. Raadpleeg het **verkennen Azure schijfversleuteling met Azure PowerShell** -blog bericht [deel 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) en [deel 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) voor meer informatie over het inschakelen van versleuteling Azure schijf-versleuteling gebruikt met PS-cmdlets.

**Opmerking**: klik op de Linux-VMs de `Get-AzureRmVMDiskEncryptionStatus` cmdlet duurt 3 en 4 minuten om de status versleuteling te rapporteren.

#### <a name="get-encryption-status-of-the-iaas-vm-from-disk-encryption-cli-command"></a>Versleuteling status van de VM IaaS ophalen uit schijfversleuteling CLI opdracht

U kunt de status versleuteling van IaaS VM krijgen van de schijfversleuteling CLI opdracht *azure vm weergeven-schijf-versleuteling-status*. Als u de versleutelingsinstellingen voor uw VM, typt u in uw Azure CLI-sessie:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Versleuteling op Windows IaaS VM uitgevoerd uitschakelen

U kunt versleuteling op een actieve Windows of Linux IaaS VM via de Azure schijf versleuteling resourcemanager sjabloon of PS cmdlets uitschakelen en de configuratie ontsleutelen.


##### <a name="windows-vm"></a>Windows VM

De stap van de versleuteling uitschakelen worden versleuteling van het volume van het besturingssysteem of gegevens of beide van de actieve Windows IaaS VM uitgeschakeld. U kunt niet het volume van het besturingssysteem uitschakelen en laat u het gegevensvolume is versleuteld. Wanneer de uitschakelen versleuteling stap wordt uitgevoerd, Azure klassieke implementatiemodel het model van de service VM bijgewerkt en de Windows IaaS VM ontsleuteld is gemarkeerd. De inhoud van de VM zijn niet meer in rust versleuteld. De belangrijkste kluis klant en de versleuteling belangrijke materiaal, dat wil BitLocker versleuteling toetsen voor Windows en wachtwoordzin voor Linux zeggen worden niet door de versleuteling uitschakelen worden verwijderd. 

##### <a name="linux-vm"></a>Linux VM

De stap van de versleuteling uitschakelen versleuteling van het gegevensvolume op de actieve Linux IaaS VM wordt uitgeschakeld

**Opmerking**: codering op OS schijf uitschakelen is niet toegestaan voor Linux VMs.

##### <a name="disable-encryption-on-existingrunning-iaas-vm-in-azure-using-resource-manager-template"></a>Versleuteling voor bestaande/voorlopig IaaS VM in Azure wordt aangegeven met resourcemanager sjabloon uitschakelen

Schijfversleuteling kan worden uitgeschakeld voor het uitvoeren van Windows IaaS VM met de sjabloon resourcemanager gepubliceerde [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm). Klik op "Implementeren naar Azure"-knop op de sjabloon Azure quickstart, de configuratie van de invoer decoderen in het blad parameters en klik op OK. Selecteer het abonnement, de resourcegroep, de resource groep locatie, de juridische voorwaarden en de overeenkomst en klik op de knop maken om in te schakelen versleuteling voor een nieuwe IaaS VM.

Voor Linux VM, kan [deze](https://aka.ms/decrypt-linuxvm) sjabloon worden gebruikt voor het uitschakelen van versleuteling.

Resourcemanager sjabloon parameters details voor het uitschakelen van versleuteling voor IaaS VM uitgevoerd:

| vmName         | Naam van de VM over welke codering bewerking wordt uitgevoerd                                                                                                                                                                       |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| volumeType     | Type van het volume op welke decoderen bewerking wordt uitgevoerd. Geldige waarden zijn "OS", "Gegevens", "Alle". **Notitie:** U kunt versleuteling voor het uitvoeren van Windows IaaS VM OS/opstarten volume zonder codering op "Gegevens" volume uitschakelen niet uitschakelen. **Opmerking**: codering op OS schijf uitschakelen is niet toegestaan voor Linux VMs. |
| sequenceVersion | De versie van de volgorde van de bewerking BitLocker. In dit versienummer verhoogd telkens wanneer een schijf decoderingsbewerking wordt uitgevoerd op de dezelfde VM                                                                                          |

##### <a name="disable-encryption-on-existingrunning-iaas-vm-in-azure-using-ps-cmdlet"></a>Versleuteling voor bestaande/voorlopig IaaS VM in Azure wordt aangegeven met de cmdlet PS uitschakelen

Als u wilt uitschakelen met de cmdlet PS, worden [Uitschakelen-AzureRmVMDiskEncryption](https://msdn.microsoft.com/library/azure/mt715776.aspx) cmdlet versleuteling voor een infrastructuur uitgeschakeld als een VM service (IaaS). Deze cmdlet ondersteunt zowel Windows en Linux VMs. Deze cmdlet installeert een uitbreiding op de virtuele machine versleuteling uitschakelen. Als de parameter naam niet wordt opgegeven, wordt een uitbreiding met de standaardnaam "AzureDiskEncryption voor Windows VMs" gemaakt. 

Klik op Linux VMs, wordt de extensie "AzureDiskEncryptionForLinux" gebruikt.

**Opmerking**: deze cmdlet opnieuw is opgestart de virtuele machine. 

## <a name="appendix"></a>Bijlage

### <a name="connect-to-your-subscription"></a>Verbinding maken met uw abonnement

Zorg ervoor dat de sectie *voorwaarden* in dit document voordat u verder gaat bekijken. Als u zeker bent dat alle voorwaarden hebt voldaan, volgt u de onderstaande stappen om te verbinden met uw abonnement:

1. een Azure PowerShell-sessie starten en meld u aan bij uw Azure-account met de volgende opdracht uit:

    Login-AzureRmAccount

2. Als u meerdere abonnementen hebt en geeft u aan een specifieke een gebruiken, typt u de volgende handelingen uit als u wilt zien van de abonnementen voor uw account:

    Get-AzureRmSubscription

3. wilt opgeven van het abonnement dat u wilt gebruiken, typt u:

    Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>

4. om te controleren of het abonnement dat is geconfigureerd klopt, typ:

    Get-AzureRmSubscription

5. om te bevestigen dat de cmdlets Azure schijfversleuteling zijn geïnstalleerd, typ:

    Get-command *diskencryption*

6. u ziet het onder de acceptatie van Azure schijf versleuteling PowerShell installatie uitvoer:

    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     

### <a name="preparing-a-pre-encrypted-windows-vhd"></a>Een vooraf gecodeerde Windows VHD voorbereiden
De volgende gedeelten zijn nodig bij het voorbereiden van een vooraf gecodeerde Windows VHD voor implementatie als een versleutelde VHD in Azure IaaS. De stappen worden gebruikt voor het voorbereiden en een vers windows VM (vhd) op Hyper-V of Azure opstarten.

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Update groepsbeleid toe te staan dat niet-TPM voor OS-beveiliging
U moet het groepsbeleid BitLocker in te stellen BitLocker station codering met genoemd, bevindt zich onder beleid voor lokale Computer \Computer Configuration\Administrative Templates\Windows onderdelen configureren. Deze instelling te wijzigen: *Besturingssysteem stations - extra verificatie bij het opstarten - toestaan BitLocker zonder compatibele TPM vereisen* zoals wordt weergegeven in de onderstaande afbeelding:

![Microsoft Antimalware in Azure wordt aangegeven](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>Onderdelen van de functie BitLocker installeren
Voor Windows Server 2012 en gebruik de onderstaande opdracht:

    dism /online /Enable-Feature /all /FeatureName:Bitlocker /quiet /norestart

Voor Windows Server 2008 R2 gebruik het onder de opdracht:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-os-volume-for-bitlocker-using-bdehdcfg"></a>Het volume van de OS voorbereiden voor het gebruik van BitLocker`bdehdcfg`

Voer de onderstaande voor compressie van het besturingssysteem partition en de computer voorbereiden voor BitLocker opdracht.

    bdehdcfg -target c: shrink -quiet

#### <a name="using-bitlocker-to-protect-the-os-volume"></a>Met BitLocker is het volume van het besturingssysteem beveiligen
Gebruik de [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) opdracht inschakelen versleuteling voor het gebruik van een externe sleutel protector en zet de externe sleutel (.bek-bestand) op de externe schijf of het volume. Versleuteling worden ingeschakeld voor het systeem/na opnieuw opstarten.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

**Notitie:** De VM moet worden voorbereid met een vhd afzonderlijk gegevens/resource voor het ophalen van de externe sleutel met BitLocker.

#### <a name="encrypting-os-drive-on-a-running-linux-vm"></a>OS station op een actieve Linux VM versleutelt

Codering van OS station op een actieve Linux VM wordt ondersteund op de volgende distros:

- RHEL 7.2
- CentOS 7.2
- Ubuntu 16.04

Vereisten voor OS schijfversleuteling:

- VM moet worden gemaakt van Azure-galerij in resourcemanager Azure-portal.
- Azure VM met ten minste 4 GB RAM (aanbevolen grootte is 7 GB).
- (Voor RHEL en CentOS) SELinux moet zijn [uitgeschakeld](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) op de VM. De VM opnieuw moet worden gestart ten minste eenmaal nadat SELinux is uitgeschakeld.


##### <a name="steps"></a>Stappen

1. maken een VM met een van de bovenstaande distros.

Voor CentOS 7.2, OS schijf codering via een speciale afbeelding. Als u wilt gebruiken in deze afbeelding, Geef "7.2n" Als de Sku bij het maken van de VM:

    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"

2. configureren de VM op basis van uw behoeften voldoet. Als u versleutelen van alle wilt (OS + gegevens) stations de gegevens stations hoeven te worden opgegeven en mogelijk uit /etc/fstab.

> [AZURE.NOTE] Moet u UUID =... om op te geven gegevens stations in/enzovoort/fstab in plaats van de precisie van blok naam van het apparaat, bijvoorbeeld /dev/sdb1. De volgorde van stations wordt tijdens versleuteling wijzigen op de VM. Als uw VM afhankelijk van een specifieke volgorde van blokapparaten is zijn deze niet worden ze na versleuteling te koppelen.

3. afmelden SSH-sessies.

4. voor het coderen van het besturingssysteem, geef volumeType "In alle" of "OS" wanneer [codering inschakelen](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

> [AZURE.NOTE] Alle gebruiker-ruimte-processen die niet worden uitgevoerd als `systemd` services moeten worden beëindigd met een `SIGKILL`. De VM moet opnieuw worden gestart. U plannen op downtime van VM wanneer inschakelen OS schijf versleuteling voor een actieve VM.

5. regelmatig de voortgang van versleuteling volgens de instructies in de [volgende sectie](#monitoring-os-encryption-progress)bewaken.

6. als Get-AzureRmVmDiskEncryptionStatus "VMRestartPending" ziet, start u uw VM opnieuw door een van beide logboekregistratie kunnen deze of via PowerShell-Portal/CLI.

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, please reboot the VM

Het wordt aanbevolen om op te slaan [opstarten diagnostische gegevens](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) van de VM *voordat* opgestart.

#### <a name="monitoring-os-encryption-progress"></a>OS versleuteling voortgang controleren

Er zijn drie manieren om de voortgang van OS versleuteling te houden.

1. Gebruik de cmdlet Get-AzureRmVmDiskEncryptionStatus en het veld ProgressMessage controleren: 
 
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started

Zodra de VM bereikt "OS schijf versleuteling gestart" het duurt ongeveer 40 tot 50 minuten back VM op een Premium-opslag.

Aangezien het [probleem #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` en `DataVolumesEncrypted` weergegeven als `Unknown` in sommige distros. Met WALinuxAgent versie 2.1.5 en hoger dit wordt automatisch opgelost. In het geval er `Unknown` in de uitvoer, kunt u Schijfopruiming versleuteling status controleren met Azure Resource Viewer.

Ga naar [Azure Resource Viewer](https://resources.azure.com/)en vervolgens deze hiërarchie in het deelvenster selectie op links uitvouwen:

~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

Schuif omlaag in de InstanceView, om de versleuteling status van uw stations te bekijken.

![VM exemplaar weergeven](./media/azure-security-disk-encryption/vm-instanceview.png)

2. kijkt u naar [opstarten diagnostische gegevens](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/). Berichten van ADE extensie moeten worden voorafgegaan door `[AzureDiskEncryption]`.

3. aanmelding bij de VM via SSH en aan het logboek extensie vanaf

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

Het wordt niet aanbevolen aan te melden bij de VM terwijl OS versleuteling uitgevoerd wordt. De logboeken kunnen daarom moeten worden gekopieerd alleen wanneer de andere twee methoden is mislukt.

#### <a name="preparing-a-pre-encrypted-linux-vhd"></a>Een vooraf gecodeerde Linux VHD voorbereiden

##### <a name="ubuntu-16"></a>Ubuntu 16

###### <a name="configure-encryption-during-distro-install"></a>Codering tijdens de installatie van distro configureren

1. Selecteer "Configureren gecodeerde schijfstations" Wanneer partitioneren schijven.

![Ubuntu 16.04-instelling](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. een afzonderlijke opstartstation die niet moet worden gecodeerd maken. Versleutel station van de hoofdmap.

![Ubuntu 16.04-instelling](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. bieden een wachtwoordzin. Dit is de wachtwoordzin die u naar KeyVault uploadt.

![Ubuntu 16.04-instelling](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. einddatum partitioneren.

![Ubuntu 16.04-instelling](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. bij het opstarten de VM, wordt u gevraagd voor een wachtwoordzin. Gebruik de wachtwoordzin die u in stap 3 hebt opgegeven.

![Ubuntu 16.04-instelling](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. VM voorbereiden voor het uploaden naar Azure [deze](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/)instructies. Worden niet uitgevoerd voor de laatste stap (deprovisioning de VM) nog.

###### <a name="configure-encryption-to-work-with-azure"></a>Codering voor gebruik met Azure configureren

1. een bestand onder /usr/local/sbin/azure_crypt_key.sh, maken met de inhoud in het onderstaande script. Let op de KeyFileName, omdat deze de wachtwoordzin bestandsnaam plaatsen door Azure.

    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do
        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi


2. wijzigen de zoekconfiguratie crypt in */etc/crypttab*. Deze ziet er als volgt:

    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh

3. Als u de *azure_crypt_key.sh* in Windows bewerkt en gekopieerd naar Linux, vergeet niet om uit te voeren *dos2unix /usr/local/sbin/azure_crypt_key.sh*.

4. uitvoerbare machtigingen toevoegen aan het script:

    chmod +x /usr/local/sbin/azure_crypt_key.sh

4. bewerken */etc/initramfs-tools/modules* door regels toe te voegen:

    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1

5. uitvoeren `update-initramfs -u -k all` bij de initramfs zodat de `keyscript` pas van kracht.
6. nu kunt u de VM deprovision.

![Ubuntu 16.04-instelling](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

7. gaat u verder met de volgende stap en [upload uw VHD](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure.

##### <a name="opensuse-132"></a>openSUSE 13.2

###### <a name="configure-encryption-during-distro-install"></a>Codering tijdens de installatie van distro configureren

1. Selecteer "Versleutelen Volume groep" tijdens het partitioneren schijven. Geef een wachtwoordzin. Dit is de wachtwoordzin die u naar KeyVault uploadt.

![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Start de VM met uw wachtzin.

![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. VM voorbereiden voor het uploaden naar Azure [deze](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131)instructies. Worden niet uitgevoerd voor de laatste stap (deprovisioning de VM) nog.

###### <a name="configure-encryption-to-work-with-azure"></a>Codering voor gebruik met Azure configureren

1. Bewerk de /etc/dracut.conf en voeg de volgende regel toe:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2. Opmerking om deze regels aan het einde van het bestand "/ usr/lib/dracut/modules.d/90crypt/module-setup.sh ':

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3. toevoegen de volgende regel aan het begin van het bestand "/ usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"

    DRACUT_SYSTEMD=0

en alle exemplaren van wijzigen

    if [ -z "$DRACUT_SYSTEMD" ]; then

Aan

    if [ 1 ]; then

4. /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh bewerken en dit na het '# openen LUKS apparaat' toevoegen

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5. uitvoeren de "/ usr/sbin/dracut - f - v ' bij het initrd.

6. u kunt nu de VM en [upload uw VHD](#upload-encrypted-vhd-to-an-azure-storage-account) deprovision in Azure.

##### <a name="centos-7"></a>CentOS 7

###### <a name="configure-encryption-during-distro-install"></a>Codering tijdens de installatie van distro configureren

1. Selecteer "versleutelen mijn gegevens" tijdens het partitioneren schijven.

![CentOS 7-instelling](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Zorg 'Versleutelen' is ingeschakeld voor de hoofdmap partition.

![CentOS 7-instelling](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. bieden een wachtwoordzin. Dit is de wachtwoordzin die u naar KeyVault uploadt.

![CentOS 7-instelling](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. bij het opstarten de VM, wordt u gevraagd voor een wachtwoordzin. Gebruik de wachtwoordzin die u in stap 3 hebt opgegeven.

![CentOS 7-instelling](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. VM voorbereiden voor het uploaden naar Azure [deze](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70)instructies. Worden niet uitgevoerd voor de laatste stap (deprovisioning de VM) nog.

6. u kunt nu de VM en [upload uw VHD](#upload-encrypted-vhd-to-an-azure-storage-account) deprovision in Azure.

###### <a name="configure-encryption-to-work-with-azure"></a>Codering voor gebruik met Azure configureren

1. Bewerk de /etc/dracut.conf en voeg de volgende regel toe:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2. Opmerking om deze regels aan het einde van het bestand "/ usr/lib/dracut/modules.d/90crypt/module-setup.sh ':

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3. toevoegen de volgende regel aan het begin van het bestand "/ usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"

    DRACUT_SYSTEMD=0

en alle exemplaren van wijzigen

    if [ -z "$DRACUT_SYSTEMD" ]; then

Aan

    if [ 1 ]; then

4. /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh bewerken en dit na het '# openen LUKS apparaat' toevoegen

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5. uitvoeren de "/ usr/sbin/dracut - f - v ' bij het initrd.

![CentOS 7-instelling](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Versleutelde VHD uploaden naar een Azure opslag-account
Als BitLocker versleuteling prijs DM-Crypt versleuteling is ingeschakeld, moet de lokale versleutelde VHD bij uw account opslag worden geüpload.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-disk-encryption-secret-for-the-pre-encrypted-vm-to-key-vault"></a>Schijf versleuteling geheim voor vooraf gecodeerde VM naar toets kluis uploaden
De schijf versleuteling geheim verkregen moet eerder als een geheim in sleutel kluis worden geüpload. De toets kluis machtigingen nodig hebben ingeschakeld voor uw AAD-client en de schijfversleuteling.


    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $KeyVault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Schijf versleuteling geheim niet versleuteld met een KEK
[Set-AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) gebruiken voor het inrichten van het geheim in belangrijke kluis. Voor het geval een virtuele Windows-computer, is het bestand bek gecodeerd als een tekenreeks base64 en vervolgens naar belangrijke kluis met de cmdlet Set-AzureKeyVaultSecret geüpload. Voor Linux, is de wachtwoordzin gecodeerd als een tekenreeks base64 en vervolgens naar de toets kluis geüpload. Bovendien Zorg ervoor dat de volgende codes zijn ingesteld bij het maken van het geheim in belangrijke kluis.

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

De `$secretUrl` mag worden gebruikt in de volgende stap voor het [koppelen van de schijf OS zonder KEK](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Schijf versleuteling geheim versleuteld met een KEK

Het geheim kunt (optioneel) worden versleuteld met een toets versleutelingssleutel voordat u uploadt naar toets kluis. Gebruik de tekstterugloop [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) eerst het gebruik van de Key versleuteling geheim versleutelen. De uitvoer van deze bewerking tekstterugloop is een base64 codering URL-tekenreeks die vervolgens als een geheim met de cmdlet [Set-AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) wordt geüpload.

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

De `$KeyEncryptionKey` en `$secretUrl` mag worden gebruikt in de volgende stap voor het [koppelen van de OS schijf KEK gebruiken](#using-a-kek).

### <a name="specify-secret-url-when-attaching-os-disk"></a>Geheim URL opgeeft wanneer OS schijf koppelen

#### <a name="without-using-a-kek"></a>Zonder een KEK

Tijdens het toevoegen van de schijf OS `$secretUrl` moeten worden doorgegeven. De URL is in de sectie ["schijf versleuteling geheim niet gecodeerd, met een KEK"](#disk-encryption-secret-not-encrypted-with-a-kek)gegenereerd.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Een KEK gebruiken

Tijdens het toevoegen van de schijf OS `$KeyEncryptionKey` en `$secretUrl` moeten worden doorgegeven. De URL is in de sectie ["schijf versleuteling geheim versleuteld met een KEK"](#disk-encryption-secret-encrypted-with-a-kek)gegenereerd.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a>Deze handleiding downloaden
U kunt deze handleiding downloaden vanuit de [Galerie met TechNet](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).


## <a name="for-more-information"></a>Voor meer informatie
[Azure schijfversleuteling met Azure PowerShell verkennen](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)

[Kennismaken met Azure schijfversleuteling met Azure PowerShell - deel 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
