<properties
    pageTitle="Opnieuw instellen van access op Azure Linux VMs met de extensie VMAccess | Microsoft Azure"
    description="Opnieuw instellen van access op Azure Linux VMs met de extensie VMAccess."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Beheren van gebruikers, SSH en controleren of schijven op Azure Linux VMs met de extensie VMAccess herstellen

In dit artikel leest u het gebruik van de Azure VMAcesss extensie om te controleren of een schijf herstellen, opnieuw instellen van de toegang van gebruikers, gebruikersaccounts beheren of opnieuw instellen van de configuratie SSHD op Linux. In het artikel moet:

- een Azure-account (het[ophalen van een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)).

- de [Azure CLI](../xplat-cli-install.md) aangemeld `azure login`.

- de modus Azure CLI _, moeten zich in_ Azure resourcemanager `azure config mode arm`.

## <a name="quick-commands"></a>Snelle opdrachten

Er zijn twee manieren VMAccess gebruiken op uw VMs Linux:

- Gebruik van de Azure CLI en de vereiste parameters.
- Gebruiken onbewerkte JSON-bestanden die VMAccess verwerkt en klik vervolgens op te volgen.

Voor de sectie snelle opdracht gaan we gebruiken de CLI Azure `azure vm reset-access` methode. Vervang de waarden met "voorbeeld" met de waarden uit uw eigen omgeving in de volgende voorbeelden van de opdracht.

## <a name="create-a-resource-group-and-linux-vm"></a>Een resourcegroep en Linux VM maken

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Een Debian VM maken

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Hoofdmap wachtwoord opnieuw instellen

De hoofdmap wachtwoord opnieuw instellen:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH belangrijke opnieuw instellen

Opnieuw instellen van de SSH-sleutel van een gebruiker niet-hoofdsite:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Een gebruiker maken

Een gebruiker maken:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Een gebruiker verwijderen

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>Beginwaarden SSHD

Opnieuw instellen van de configuratie SSHD:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

### <a name="vmaccess-defined"></a>VMAccess gedefinieerd:

De schijf op uw VM Linux wordt fouten weergegeven. U op het hoofdniveau-wachtwoord opnieuw instellen voor uw VM Linux of uw persoonlijke sleutel SSH per ongeluk wordt verwijderd. Als dat is gebeurd terug in de dagen van het datacenter, moet u er station en open vervolgens het KVM om op de server-console. Als het ware de extensie Azure VMAccess die KVM switch waarmee u toegang tot de console als u wilt herstellen van access naar Linux of schijf niveau onderhoud uitvoeren.

We gaan de lange VMAccess, onbewerkte JSON-bestanden waarbij gebruik wordt gebruikt voor de gedetailleerd overzicht.  Deze VMAccess JSON-bestanden kunnen ook worden aangeroepen vanuit Azure sjablonen.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>VMAccess gebruiken om te controleren of de schijf van een Linux VM herstellen

Met VMAccess kunt u een fsck doen uitvoeren op de schijf onder uw VM Linux.  U kunt ook een schijf controleren en een schijf herstellen met een VMAccess doen.

De schijf gebruiken om te controleren en klik vervolgens repareren dit VMAccess-script:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Het script VMAccess met uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Gebruik van VMAccess gebruikerstoegang naar Linux opnieuw in te stellen

Als u toegang in hoofdmap op uw VM Linux kwijt, kunt u een VMAccess-script om de hoofdmap wachtwoord opnieuw te starten.

Als u wilt de hoofdmap wachtwoord opnieuw in, gebruik deze VMAccess-script:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Het script VMAccess met uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Als u wilt de SSH-sleutel van een gebruiker niet-hoofdsite opnieuw hebt ingesteld, kunt u dit VMAccess-script gebruiken:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Het script VMAccess met uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>VMAccess gebruiken voor het beheren van gebruikersaccounts op Linux

VMAccess is een Python-script die kan worden gebruikt voor het beheren van gebruikers in uw Linux VM zonder logboekregistratie in en sudo of het account van de hoofdmap.

Als u wilt maken van een gebruiker, moet u dit VMAccess-script gebruiken:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Het script VMAccess met uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Als u wilt verwijderen van een gebruiker, moet u dit VMAccess-script gebruiken:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Het script VMAccess met uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>Gebruik van VMAccess de configuratie SSHD opnieuw in te stellen

Als u wijzigingen in de configuratie Linux VMs SSHD aanbrengen en de verbinding SSH sluit voordat de wijzigingen te verifiëren, u mogelijk geen SSH'ing weer aan.  VMAccess kan worden gebruikt de configuratie SSHD terug naar een bekende goede configuratie zonder te worden geregistreerd in via SSH opnieuw in te stellen.

Als u opnieuw wilt gebruiken de configuratie SSHD dit VMAccess-script:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Het script VMAccess met uitvoeren:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Volgende stappen

Linux bijwerken, is het gebruik van Azure VMAccess extensies is een methode wijzigingen aanbrengen in een actieve Linux VM.  U kunt ook hulpprogramma's zoals cloud initialisatie en Azure sjablonen gebruiken voor het wijzigen van uw VM Linux bij het opstarten.

[Over VM extensies en functies](virtual-machines-linux-extensions-features.md)

[Azure resourcemanager sjablonen met Linux VM extensies ontwerpen](virtual-machines-linux-extensions-authoring-templates.md)

[Gebruik van cloud initialisatie voor het aanpassen van een Linux VM tijdens het maken van](virtual-machines-linux-using-cloud-init.md)
