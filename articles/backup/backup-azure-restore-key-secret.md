<properties
    pageTitle="Toets kluis sleutel en geheim herstellen voor versleutelde VMs met back-up van Azure | Microsoft Azure"
    description="Informatie over het herstellen van toets kluis sleutel en geheim in Azure back-up maken via PowerShell"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Toets kluis sleutel en geheim herstellen voor versleutelde VMs met back-up van Azure
In dit artikel moment spreekt over het gebruik van Azure VM back-up terugzetten van versleutelde Azure VMs, als uw sleutel en geheim niet aanwezig zijn in de belangrijkste kluis. Deze stappen kunnen ook worden gebruikt als u wilt een afzonderlijke kopie van de toets (toets versleuteling) en geheim (BitLocker versleuteling toets) voor de herstelde VM onderhouden.

## <a name="pre-requisites"></a>Minimumvereisten

1. **Back-up versleuteld VMs** - versleuteld Azure VMs reservekopie is met Azure back-up. Raadpleeg het artikel [back-up en herstellen van Azure VMs via PowerShell beheren](backup-azure-vms-automation.md) voor meer informatie over hoe u de back-up maken versleutelde Azure VMs.

2. **Configureren Azure toets kluis** : Zorg ervoor dat belangrijke kluis waaraan toetsen en geheimen moeten worden teruggezet is al aanwezig. Raadpleeg het artikel [Aan de slag met Azure toets kluis](../key-vault/key-vault-get-started.md) voor meer informatie over het beheer van de belangrijkste kluis.

## <a name="setup-recovery-services-vault"></a>Herstel services kluis instellen 
Gebruik van de volgende stappen om Meld u aan bij PowerShell en herstel services kluis context te zetten

### <a name="log-in-to-azure-powershell"></a>Meld u aan bij Azure PowerShell 

Meld u aan bij gebruik van de volgende cmdlet Azure-account

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Set herstel services kluis context

Nadat u bent aangemeld, gebruikt u de volgende cmdlet om de lijst met uw beschikbaar abonnementen

```
PS C:\> Get-AzureRmSubscription
```

Selecteer het abonnement waarin resources beschikbaar zijn

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

De kluis context, waarbij herstel Services kluis waar back-up is ingeschakeld voor versleutelde VMs instellen

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Herstelpunt ophalen 

Selecteer de container in de kluis die versleutelde Azure virtuele machines aangeeft

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Met deze container, terugkeren item voor de bijbehorende virtuele machine omhoog

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Een matrix van herstel punten voor het geselecteerde back-item in de variabele rp ophalen

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Versleutelde VM herstellen
Gebruik de volgende stappen uit om terug te zetten versleutelde VM, de sleutel en geheim.

### <a name="restore-key"></a>Toets herstellen

De matrix $rp bovenstaande, is in omgekeerde volgorde gesorteerd tijd met de meest recente herstel komma op index 0. Bijvoorbeeld: $rp [0] Hiermee selecteert u de meest recente herstel komma.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Nadat deze cmdlet voltooid wordt, wordt een blob-bestand wordt gegenereerd in de opgegeven map op de computer waarop deze wordt uitgevoerd. In dit bestand blob vertegenwoordigt sleutel versleuteld sleutel gecodeerd.

Herstel de belangrijkste kluis met de volgende cmdlet belangrijke terug. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Geheim herstellen

Geheime gegevens herstellen uit herstelpunt verkregen boven

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
De tekst voordat vault.azure.net oorspronkelijke naam van de belangrijkste kluis vertegenwoordigt. De tekst na geheimen / geheime naam vertegenwoordigt. 

Lees het geheime naam en de waarde uit de resultaten van de cmdlet uitvoeren bovenstaande geval u wilt dezelfde geheime naam gebruiken. In andere gevallen moeten $secretname hieronder worden bijgewerkt als de naam van de nieuwe geheime wilt gebruiken. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Stel labels voor het geheim geval VM moet ook worden hersteld. Voor de tag DiskEncryptionKeyFileName, wordt de naam van het geheim die u wilt gebruiken in waarde moet bevatten. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
Waarde voor DiskEncryptionKeyFileName komt overeen met geheime naam verkregen hierboven. Waarde voor DiskEncryptionKeyEncryptionKeyURL kan worden verkregen uit belangrijke kluis na de toetsen weer terugzetten en het gebruik van de cmdlet [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx)   

De geheime terug naar de belangrijkste kluis instellen

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>VM herstellen
De bovenstaande PowerShell-cmdlets kunt herstellen sleutel en geheime terug naar de belangrijkste kluis, als u een back-up versleutelde VM met back-up van Azure VM hebt gemaakt. Na deze terug te zetten, raadpleegt u het artikel [back-up en herstellen van Azure VMs via PowerShell beheren](backup-azure-vms-automation.md) versleutelde VMs herstellen.
