<properties
    pageTitle="SSH toetsen maken op Linux en Mac | Microsoft Azure"
    description="Genereren en gebruiken van SSH toetsen op Linux en Mac voor de resourcemanager en klassieke implementatiemodellen op Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>SSH toetsen op Linux en Mac maken voor Linux VMs in Azure wordt aangegeven

Met een SSH-sleutelpaar kunt u virtuele Machines maken op Azure die standaard over het gebruik van SSH toetsen voor verificatie, hoeft u de wachtwoorden aan te melden.  Wachtwoorden kunnen worden raden en open uw VMs maximaal aanhoudend felle pogingen te raden uw wachtwoord. VMs die zijn gemaakt met Azure sjablonen of de `azure-cli` uw openbare sleutel SSH kunt opnemen als onderdeel van de implementatie, een post-implementatie-configuratie verwijderen.  Als u verbinding met een VM Linux vanuit Windows maakt, raadpleegt u [Dit document.](virtual-machines-linux-ssh-from-windows.md)

In het artikel moet:

- een Azure-account (het[ophalen van een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)).

- de [Azure CLI](../xplat-cli-install.md) bent aangemeld`azure login`

- de Azure CLI _, moeten zich in_ resourcemanager Azure-modus`azure config mode arm`

## <a name="quick-commands"></a>Snelle opdrachten

Klik in de volgende opdrachten, kunt u de voorbeelden vervangen door uw eigen keuzen.

SSH sleutels zijn al dan niet standaard bewaard de `.ssh` directory.  

```bash
cd ~/.ssh/
```

Als u nog geen een `~/.ssh` directory de `ssh-keygen` opdracht wordt deze automatisch gemaakt met de juiste machtigingen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Voer de naam van het bestand dat is opgeslagen in de `~/.ssh/` directory:

```bash
id_rsa
```

Wachtwoordzin voor id_rsa invoeren:

```bash
correct horse battery staple
```

Er is nu een `id_rsa` en `id_rsa.pub` SSH belangrijke paar gegevenspunten in de `~/.ssh` directory.

```bash
ls -al ~/.ssh
```

De zojuist gemaakte sleutel aan toevoegen `ssh-agent` op Linux en Mac (ook toegevoegd aan OSX sleutelhanger):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Kopieer de SSH openbare sleutel toe aan uw Linux-Server.

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Test de Meld u aan met de toetsen in plaats van een wachtwoord:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

SSH openbare en persoonlijke sleutels gebruiken is de eenvoudigste manier om Meld u aan bij uw Linux-servers. [Openbare sleutel cryptografische](https://en.wikipedia.org/wiki/Public-key_cryptography) kunt u veel veiliger Meld u aan bij uw Linux of BSD VM in Azure wordt aangegeven dan wachtwoorden, die u eenvoudig kunnen brute wordt gedwongen veel meer worden kunnen. Uw openbare sleutel mogen worden gedeeld met iedereen; maar alleen u (of de infrastructuur van uw lokale beveiliging) beschikken over uw persoonlijke sleutel.  De persoonlijke sleutel SSH moet hebben een [zeer beveiligd-wachtwoordverificatie](https://www.xkcd.com/936/) (bron:[xkcd.com](https://xkcd.com)) om te beschermen.  Dit wachtwoord is alleen voor toegang tot de persoonlijke SSH-sleutel en **is niet** het wachtwoord voor een gebruikersaccount.  Wanneer u een wachtwoord aan uw sleutel SSH toevoegen, worden de persoonlijke sleutel versleuteld zodat de persoonlijke sleutel geen nut zonder het wachtwoord heeft te ontgrendelen.  Als onbevoegden uw persoonlijke sleutel stelen en die toets geen een wachtwoord heeft, is het zou dat ze gebruikmaken van deze persoonlijke sleutel voor aanmelding bij servers waarop de bijbehorende openbare sleutel.  Als u een persoonlijke sleutel is deze beveiligd met een wachtwoord kan niet worden gebruikt door deze aanvaller, waarin u een extra beveiliging voor de infrastructuur van uw op Azure.

In dit artikel wordt gemaakt *ssh-rsa* opgemaakt key-bestanden die worden aanbevolen voor implementaties van de Resource Manager.  *SSH-rsa* toetsen nodig op de [portal](https://portal.azure.com) voor klassieke zowel resourcemanager implementaties.

## <a name="create-the-ssh-keys"></a>De toetsen SSH maken

Azure moet ten minste 2048-bits, ssh-rsa openbare en persoonlijke sleutels opmaken. Maken van het gebruik van de toetsen `ssh-keygen`, waarin wordt gevraagd om een reeks vragen en schrijft u een persoonlijke sleutel en een overeenkomende openbare sleutel. Wanneer een VM Azure is gemaakt, de openbare sleutel wordt gekopieerd naar `~/.ssh/authorized_keys`.  SSH toetsen in `~/.ssh/authorized_keys` worden gebruikt voor de client zodat deze overeenkomt met de bijbehorende persoonlijke sleutel van een verbinding met SSH login uitdaging.

## <a name="using-ssh-keygen"></a>Ssh-keygen gebruiken

Deze opdracht maakt u een wachtwoord beveiligd (versleuteld) SSH-sleutelpaar met 2048-bits RSA en deze gemakkelijk identificeren deze is toegelicht.  

Begin met het wijzigen van mappen, zodat alle uw ssh sleutels worden gemaakt in die map.

```bash
cd ~/.ssh
```

Als u nog geen een `~/.ssh` directory de `ssh-keygen` opdracht wordt deze automatisch gemaakt met de juiste machtigingen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Uitleg over de opdracht_

`ssh-keygen`= het programma waarmee de toetsen maken

`-t rsa`= type sleutel maken dat de [RSA opmaak](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bits van de sleutel

`-C "myusername@myserver"`= een opmerking toegevoegd aan het einde van de openbare sleutel bestand naar deze eenvoudig te definiëren.  Normaal gesproken een e-mailbericht wordt gebruikt als de opmerking maar u kunt gebruiken wat het meest geschikt is voor de infrastructuur van uw.

### <a name="using-pem-keys"></a>PEM te gebruiken

Als u de klassieke model implementeert (klassieke Azure-beheerportal of de CLI Azure-Service Management `asm`), moet u mogelijk gebruiken PEM SSH toetsen voor toegang tot uw VMs Linux opgemaakt.  Hier wordt uitgelegd hoe u een sleutel PEM maken van een bestaande SSH openbare sleutel en een bestaande x509 certificaat.

Als u wilt maken van een PEM opgemaakt sleutel uit een bestaande SSH openbare sleutel:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Voorbeeld van ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Belangrijke bestanden opgeslagen:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

De naam van een combinatie van sleutel voor in dit artikel.  Met de combinatie van een sleutel **id_rsa** met de naam is de standaardwaarde en enkele technieken de naam van het bestand met persoonlijke sleutel **id_rsa** zou verwachten, dus met een een goed idee is. De map `~/.ssh/` is de standaardlocatie voor belangrijke paren SSH en het configuratiebestand SSH.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Een overzicht van de `~/.ssh` directory. `ssh-keygen`Hiermee maakt u de `~/.ssh` directory als dit niet aanwezig is en stelt ook de opties voor de juiste eigendom en de bestandsnaam.

Wachtwoord:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`verwijst naar een wachtwoord als "een wachtwoordzin."  Het wordt *ten zeerste* aanbevolen wachtwoorden toe te voegen aan uw paren sleutels. Zonder een wachtwoord beveiligen van de combinatie van de sleutel, kunt iedereen met het bestand met persoonlijke sleutel Hiermee aanmelden bij een andere server die de bijbehorende openbare sleutel heeft. Het toevoegen van een wachtwoord biedt meer beveiliging wanneer iemand toegang tot uw bestand met persoonlijke sleutel, krijgt u gegeven is tijd om te wijzigen van de pijltoetsen gebruikt om te verifiëren u.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Ssh-agent gebruiken voor de opslag van uw wachtwoord voor persoonlijke sleutel

Als u wilt voorkomen dat uw wachtwoord voor bestand met persoonlijke sleutel met elke SSH-aanmelding te typen, kunt u `ssh-agent` om uw wachtwoord voor bestand met persoonlijke sleutel in cache. Als u een Mac gebruikt, de sleutelhanger OSX de persoonlijke sleutels, wachtwoorden veilig opgeslagen wanneer u oproepen `ssh-agent`.

Controleer eerst `ssh-agent` wordt uitgevoerd

```bash
eval "$(ssh-agent -s)"
```

Deze worden toegevoegd aan de persoonlijke sleutel `ssh-agent` met de opdracht `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Het wachtwoord voor persoonlijke sleutel wordt nu opgeslagen in `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Maken en configureren van een SSH config-bestand

Is het aanbevolen aanbevolen maken en configureren een `~/.ssh/config` bestand versnellen ins aanmelden en voor het optimaliseren van het gedrag van uw SSH-client.

Het volgende voorbeeld ziet u een standaard configuratie.

### <a name="create-the-file"></a>Het bestand maken

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Bewerk het bestand als u wilt toevoegen van de nieuwe SSH-configuratie:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Voorbeeld `~/.ssh/config` bestand:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Deze config SSH kunt u secties voor elke server voor het inschakelen van elk van de combinatie van een eigen speciale sleutel. De standaardinstellingen (`Host *`) zijn voor hosts die niet overeenkomen met een van de specifieke hosts hoger staan in het configuratiebestand.


### <a name="config-file-explained"></a>Uitleg over configuratiebestand

`Host`de naam van de host wordt aangeroepen op de terminal =.  `ssh fedora22`Hiermee wordt aan `SSH` gebruik van de waarden in de blokkering van de instellingen voor het label `Host fedora22` Opmerking: dit is een etiket dat is logisch zijn voor uw gebruik en geeft de werkelijke hostname van een server geen.

`Hostname 102.160.203.241`= het IP-adres of de DNS-naam voor de server die wordt geopend.

`User myusername`= het externe gebruikersaccount gebruiken bij het aanmelden bij de server.

`PubKeyAuthentication yes`= SSH meldt dat u wilt gebruiken van een sleutel SSH aan te melden.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= de SSH persoonlijke sleutel en de bijbehorende openbare sleutel wilt gebruiken voor verificatie.


## <a name="ssh-into-linux-without-a-password"></a>SSH in Linux zonder een wachtwoord

Nu u een combinatie van SSH-sleutel en een geconfigureerde SSH config-bestand hebt, bent u mogen Meld u aan bij uw VM Linux snel en veilig. De eerste keer dat u aanmelden bij een server met een sleutel SSH de opdracht aanwijzingen u voor de wachtwoordzin voor dat belangrijke bestand.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Uitleg over de opdracht

Wanneer `ssh fedora22` wordt uitgevoerd SSH eerst zoekt en laadt u de instellingen van de `Host fedora22` blokkeren en geladen alle resterende instellingen van de laatste tekstblok `Host *`.

## <a name="next-steps"></a>Volgende stappen

Volgende omhoog is het opzetten van Azure Linux VMs met de nieuwe SSH openbare sleutel.  Azure VMs die zijn gemaakt met een openbare sleutel SSH als de aanmelding zijn beter beveiligd dan VMs die zijn gemaakt met de aanmelding standaardmethode wachtwoorden.  Azure VMs die zijn gemaakt met behulp van SSH sleutels zijn al dan niet standaard geconfigureerd met wachtwoorden uitgeschakeld, raden pogingen brute wordt gedwongen voorkomen.

- [Maken van een beveiligde Linux VM met een sjabloon van Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Een secure Linux VM met behulp van de Azure portal maken](virtual-machines-linux-quick-create-portal.md)
- [Een secure Linux VM gebruik van de Azure CLI maken](virtual-machines-linux-quick-create-cli.md)
