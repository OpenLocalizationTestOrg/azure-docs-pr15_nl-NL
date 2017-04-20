<properties 
    pageTitle="SSH toetsen gebruiken met Windows voor Linux VMs | Microsoft Azure" 
    description="Leer hoe u genereren en SSH toetsen op een Windows-computer om te communiceren met een Linux virtuele machine op Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Hoe gebruik SSH toetsen met Windows op Azure

> [AZURE.SELECTOR]
- [Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux/Mac](virtual-machines-linux-mac-create-ssh-keys.md)

Wanneer u verbinding met Linux virtuele machines (VMs) in Azure wordt aangegeven maakt, moet u [openbare sleutel cryptografische](https://wikipedia.org/wiki/Public-key_cryptography) op te geven van een veiligere manier aanmelden bij uw VM Linux. Hierbij moet een openbare en persoonlijke sleutels uitwisselen met de secure (SSH)-shellopdracht om te verifiëren uzelf in plaats van een gebruikersnaam en wachtwoord. Wachtwoorden zijn vatbaar voor gewelddadige aanvallen, met name op internetgerichte VMs zoals-endwebservers. Dit artikel bevat een overzicht van SSH toetsen en het genereren van de juiste toetsen op een Windows-computer.


## <a name="overview-of-ssh-and-keys"></a>Overzicht van SSH en sleutels

U kunt veilig Meld u aan bij uw VM Linux met behulp van openbare en persoonlijke sleutels:

- De **openbare sleutel** is op uw VM Linux of een andere service die u wilt gebruiken met openbare sleutel cryptografische geplaatst.
- De **persoonlijke sleutel** is wat u presenteren aan uw VM Linux wanneer u zich hebt aangemeld, uw identiteit verifiëren. Deze persoonlijke sleutel beschermen. Deze niet delen.

Deze gedeelde en persoonlijke sleutels kunnen worden gebruikt op meerdere VMs en -services. U hoeft niet een paar sleutels voor elke VM of de service die u wilt openen. Zie [openbare sleutel cryptografische](https://wikipedia.org/wiki/Public-key_cryptography)voor een gedetailleerd overzicht.

SSH is een versleutelde verbinding-protocol waarmee beveiligde aanmeldingen via onbeveiligde verbindingen. Dit is het protocol van de verbinding standaard voor Linux VMs gehost in Azure wordt aangegeven. Maar SSH zelf een versleutelde verbinding biedt, verlaat het gebruik van wachtwoorden met SSH verbindingen nog steeds de VM vatbaar voor gewelddadige aanvallen of te raden van wachtwoorden. Een veiligere, en de voorkeur, methode verbinding maakt met een VM via SSH is met behulp van deze gedeelde en persoonlijke sleutels, ook bekend als SSH toetsen.

Als u niet gebruiken SSH toetsen wilt, kunt u nog steeds aanmelden bij uw Linux VMs met een wachtwoord. Als uw VM wordt niet blootgesteld aan Internet, gebruik van wachtwoorden mogelijk niet voldoende. Echter nog steeds moet u uw wachtwoorden voor elke Linux VM beheren en onderhouden in orde wachtwoordbeleidsregels en procedures, zoals minimale wachtwoordlengte en deze regelmatig bij te werken. Het gebruik van SSH sleutels Hiermee reduceert u de complexiteit van afzonderlijke referenties Gebruikersbeheer in meerdere VMs.


## <a name="windows-packages-and-ssh-clients"></a>Windows-pakketten en SSH-clients

U verbinding maken met en beheren van Linux VMs in Azure wordt aangegeven met een **ssh** -client. Windows-computers hebben niet meestal een **ssh** client is geïnstalleerd. Algemene Windows SSH clients die kunt u installeren zijn opgenomen in de volgende pakketten:

- [Cijfer voor Windows](https://git-for-windows.github.io/)
- [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)

> [AZURE.NOTE] De meest recente Update van Windows 10 jubileum bevat Bash voor Windows. Deze functie kunt u het Windows-subsysteem voor Linux en toegang hulpprogramma's zoals een client SSH uitvoeren. Bash voor Windows nog steeds in ontwikkeling is en wordt beschouwd als een bètaversie. Zie [Bash op Ubuntu op Windows](https://msdn.microsoft.com/commandline/wsl/about)voor meer informatie over Bash voor Windows.


## <a name="which-key-files-do-you-need-to-create"></a>Welke belangrijke bestanden moet u maken?

Azure moet ten minste 2048-bits, **ssh-rsa** openbare en persoonlijke sleutels opmaken. Als u Azure resources met het model Klassiek implementatie beheert, moet u ook een PEM genereren (`.pem` bestand).

Dit zijn de implementatie-scenario's en de soorten bestanden die u in elk gebruiken:

1. **SSH-rsa** -sleutels zijn vereist voor elke implementatie met behulp van de [Azure-portal](https://portal.azure.com)en gebruik van de [Azure CLI](../xplat-cli-install.md)resourcemanager-implementaties.
    - Deze toetsen zijn meestal alle de meeste mensen nodig hebt.
2. `.pem`bestand is verplicht voor het maken van VMs met behulp van de [klassieke portal](https://manage.windowsazure.com). Deze toetsen worden ook ondersteund in de klassieke implementaties die gebruikmaken van de [Azure CLI](../xplat-cli-install.md).
    - Alleen moet u deze extra toetsen en certificaten maken als u resources die zijn gemaakt met behulp van het model Klassiek implementatie beheert.


## <a name="install-git-for-windows"></a>Cijfer voor Windows installeren

De voorgaande sectie vermeld meerdere pakketten waarin de `openssl` hulpmiddel voor Windows. Dit hulpmiddel is nodig om u te maken van openbare en persoonlijke sleutels. De volgende voorbeelden Detailstijlen installeren en gebruiken van **Cijfer voor Windows**, hoewel u ongeacht pakket dat u liever kunt kiezen. **Cijfer voor Windows** hebt u toegang tot enkele extra open source software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) hulpprogramma's die nuttig zijn kunnen terwijl u met Linux VMs werkt.

1. Download en installeer **Cijfer voor Windows** van de volgende locatie: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. De standaardopties accepteren tijdens het installatieproces stop te zetten, tenzij u precies wilt wijzigen.

3. **Cijfer we vaker doen** vanuit het **Menu Start**uitvoeren > **cijfer** > **Cijfer we vaker doen**. De console ziet er ongeveer als volgt:

    ![Cijfer voor Windows Bash shell](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Een persoonlijke sleutel maken

1. Gebruik in uw **We vaker doen cijfer** -venster, `openssl.exe` maken van een persoonlijke sleutel. Het volgende voorbeeld wordt een sleutel met de naam `myPrivateKey` en met de naam certificaat `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    De uitvoer ziet er ongeveer als volgt:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Beantwoord de aanwijzingen voor het landnaam, locatie, de organisatienaam van de, enzovoort.

3. Uw nieuwe persoonlijke sleutel en certificaat zijn gemaakt in uw huidige werkmap. Voor aanbevolen procedures voor beveiliging, moet u de machtigingen instellen op uw persoonlijke sleutel, zodat alleen u het kunt openen:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Als u ook klassieke resources beheren moet, converteert u het `myCert.pem` naar `myCert.cer` (met DER-codering X509 certificaat). Deze optionele stap alleen uitvoeren als u nodig hebt voor het beheren van specifiek oudere klassieke resources. 

    Converteert u het certificaat dat met de volgende opdracht:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Een persoonlijke sleutel voor stopverf maken

Stopverf is een algemene SSH-client voor Windows. U bent mag een SSH-client die u wilt gebruiken. Als u wilt gebruiken stopverf, moet u een extra type toets - een stopverf persoonlijke sleutel (PPK) maakt. Als u niet stopverf gebruiken wilt, moet u dit gedeelte overslaan.

Het volgende voorbeeld wordt deze aanvullende persoonlijke sleutel specifiek voor stopverf gebruiken:

1. Gebruik **Cijfer we vaker doen** en uw persoonlijke sleutel omzetten in een RSA persoonlijke sleutel die PuTTYgen kunnen begrijpen. Het volgende voorbeeld wordt een sleutel met de naam `myPrivateKey_rsa` vanuit de bestaande sleutel met de naam `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Voor aanbevolen procedures voor beveiliging, moet u de machtigingen instellen op uw persoonlijke sleutel, zodat alleen u het kunt openen:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Downloaden en uitvoeren van PuTTYgen van de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Klik op het menu: **bestand** > **laden een persoonlijke sleutel**

4. Zoek uw persoonlijke sleutel (`myPrivateKey_rsa` in het vorige voorbeeld). De standaardmap tijdens het starten van **Cijfer we vaker doen** is `C:\Users\%username%`. Wijzigen van het bestandsfilter om weer te geven **alle bestanden (\*.\*)**:

    ![De bestaande persoonlijke sleutel laden in PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Klik op **openen**. Een vraag wordt aangegeven dat de sleutel geïmporteerd is:

    ![Geïmporteerd toets om PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Klik op **OK** om te sluiten van de vraag.

7. De openbare sleutel wordt weergegeven boven aan het venster **PuTTYgen** . U kopiëren en plakken van deze openbare sleutel in de portal van Azure of de resourcemanager Azure sjabloon wanneer u een VM Linux maakt. U kunt ook klikken op **openbare sleutel opslaan** als u wilt een kopie opslaan op uw computer:

    ![Stopverf openbare sleutel bestand opslaan](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    Het volgende voorbeeld ziet u hoe u wilt kopiëren en plak deze openbare sleutel in de portal van Azure wanneer u een VM Linux maakt. De openbare sleutel vervolgens meestal wordt opgeslagen in `~/.ssh/authorized_keys` op uw nieuwe VM.

    ![Openbare sleutel gebruiken wanneer u een VM in de portal van Azure maakt](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Klik op in **PuTTYgen**, **persoonlijke sleutel opslaan**:

    ![Persoonlijke sleutel stopverf bestand opslaan](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Gevraagd als u doorgaan wilt zonder een wachtwoordzin invoeren voor uw sleutel. Een wachtwoordzin is vergelijkbaar met een wachtwoord dat is gekoppeld aan uw persoonlijke sleutel. Zelfs als iemand zijn voor uw persoonlijke sleutel, zou ze nog steeds niet kunnen om te verifiëren met alleen de toets. Ook moeten ze de wachtwoordzin. Zonder een wachtwoordzin, als iemand uw persoonlijke sleutel wordt opgehaald kunnen ze aanmelden bij een VM of de service die wordt gebruikt die toets. Het is raadzaam om dat u een wachtwoordzin maken. Als u de wachtwoordzin vergeet, is er echter geen manier om deze herstellen.

    Als u een wachtwoordzin invoeren wilt, klikt u op **Nee**, voert u een wachtwoordzin in het hoofdvenster van PuTTYgen en klik vervolgens nogmaals op **persoonlijke sleutel opslaan** . Anders, klikt u op **Ja** om door te gaan zonder de optioneel wachtwoordzin.

8. Voer een naam en locatie op uw PPK-bestand wilt opslaan.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Gebruik stopverf naar SSH naar een Linux-computer

Klik nogmaals is stopverf een algemene SSH-client voor Windows. U bent mag een SSH-client die u wilt gebruiken. De volgende stappen wordt beschreven hoe u uw persoonlijke sleutel gebruiken om te verifiëren met uw Azure VM via SSH. De stappen lijken in andere SSH key-clients in hoeft te laden van uw persoonlijke sleutel de SSH-verbinding te verifiëren.

1. Downloaden en uitvoeren stopverf van de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Vul de hostnaam of IP-adres van uw VM van de Azure-portal:

    ![Nieuwe stopverf verbinding openen](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Voordat u selecteert **openen**, klikt u op **verbinding** > **SSH** > **Auth** tabblad. Blader naar en selecteer uw persoonlijke sleutel:

    ![Selecteer uw stopverf persoonlijke sleutel voor verificatie](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Klik op **openen** om op te verbinden met uw VM
 

## <a name="next-steps"></a>Volgende stappen
U kunt ook genereren de gedeelde en persoonlijke sleutels [OS X- en Linux gebruiken](virtual-machines-linux-mac-create-ssh-keys.md).

Zie [Bash op Ubuntu op Windows](https://msdn.microsoft.com/commandline/wsl/about)voor meer informatie over Bash voor Windows en de voordelen van OSS hulpprogramma's binnen handbereik op uw Windows-computer.

Als u problemen ondervindt bij het via SSH verbinding maken met uw VMs Linux, raadpleegt u [problemen met SSH verbindingen met een Azure Linux VM](virtual-machines-linux-troubleshoot-ssh-connection.md).