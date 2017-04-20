<properties
   pageTitle="Maken en uploaden van een afbeelding FreeBSD VM | Microsoft Azure"
   description="Leer hoe u maken en uploaden van een virtuele harde schijf (VHD) waarop het besturingssysteem FreeBSD als u wilt maken van een Azure virtuele machines"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Maken en uploaden van een VHD FreeBSD naar Azure

In dit artikel leest u hoe maken en uploaden van een virtuele harde schijf (VHD) waarop het besturingssysteem FreeBSD. Nadat u bestanden uploadt, kunt u deze als uw eigen afbeelding maken van een virtuele machine (VM) in Azure wordt aangegeven.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u de volgende items hebt:

- **Een Azure abonnement**--als u geen account hebt, kunt u er in een paar minuten kunt maken. Als u een MSDN-abonnement hebt, raadpleegt u [Azure maandelijkse creditnota voor abonnees met Visual Studio.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Anders informatie over het [maken van een gratis proefabonnement-account](https://azure.microsoft.com/pricing/free-trial/).  

- **Hulpmiddelen voor azure PowerShell**--het Azure PowerShell module moet worden geïnstalleerd en geconfigureerd voor gebruik van uw Azure-abonnement. Als u wilt downloaden van de module, Zie [Azure-downloads](https://azure.microsoft.com/downloads/). Een zelfstudie waarmee wordt beschreven hoe installeren en configureren van de module is hier beschikbaar. Gebruik de cmdlet [Azure Downloads](https://azure.microsoft.com/downloads/) voor het uploaden van de VHD.

- **FreeBSD-besturingssysteem in een bestand VHD**--een ondersteund FreeBSD besturingssysteem moet zijn geïnstalleerd op een virtuele harde schijf. Er bestaan meerdere hulpprogramma's om VHD-bestanden te maken. U kunt bijvoorbeeld een oplossing virtualization zoals Hyper-V het VHD-bestand maken en installeren van het besturingssysteem. Zie voor instructies over het installeren en gebruiken van Hyper-V [Hyper-V installeren en maak een virtuele machine](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] De nieuwere VHDX-indeling wordt niet ondersteund in Azure wordt aangegeven. U kunt de schijf converteren naar VHD opmaken met behulp van Hyper-V Manager of de cmdlet [converteren vhd](https://technet.microsoft.com/library/hh848454.aspx). Bovendien nog een [zelfstudie op MSDN over het gebruik van FreeBSD met Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

Deze taak bevat de volgende vijf stappen.

## <a name="step-1-prepare-the-image-for-upload"></a>Stap 1: Voorbereiden op de afbeelding uploaden

Op de virtuele machine waarop u het besturingssysteem FreeBSD hebt geïnstalleerd, voert u de volgende procedures uit:

1. DHCP inschakelen.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. SSH inschakelen.

    SSH is standaard ingeschakeld na de installatie vanaf schijf. Als deze voor andere reden niet is ingeschakeld of als u rechtstreeks FreeBSD VHD gebruiken, typt u het volgende:

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Het instellen van een seriële console.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Installeer sudo.

    De hoofdmap-account is uitgeschakeld in Azure wordt aangegeven. Dit betekent dat u moet gebruikmaken van sudo uit een onbevoegde gebruiker opdrachten uitvoeren met verhoogde bevoegdheden.

        # pkg install sudo
;
5. Vereisten voor Azure-Agent.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Azure-Agent installeren.

    De meest recente versie van de Azure-Agent kan altijd worden gevonden op [github](https://github.com/Azure/WALinuxAgent/releases). De versie 2.0.10 + officieel ondersteunt FreeBSD 10 en 10,1 en de versie 2.1.4 officieel ondersteunt FreeBSD 10.2 en toekomstige versies.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    Gebruik voor 2.0, laten we 2.0.16 als voorbeeld:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Gebruik voor 2.1, laten we 2.1.4 als voorbeeld:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Nadat u Azure-Agent hebt geïnstalleerd, is het verstandig om te bevestigen dat deze wordt uitgevoerd:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. Deprovision het systeem.

    Deprovision het systeem schoon en deze geschikt is voor het inrichten van opnieuw. De volgende opdracht worden ook de laatste ingerichte gebruikersaccount en de bijbehorende gegevens verwijderd:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Nu kunt u uw VM afsluiten.

## <a name="step-2-create-a-storage-account-in-azure"></a>Stap 2: Maak een account opslagruimte in Azure wordt aangegeven ##

Moet u een account opslagruimte in Azure VHD-bestand uploaden zodat deze kan worden gebruikt om te maken van een virtuele machine. U kunt de Azure klassieke portal gebruiken om een opslag-account te maken.

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).

2. Selecteer **Nieuw**op de balk met opdrachten.

3. Selecteer **gegevensservices** > **opslag** > **snel tabellen maken**.

    ![Snel een opslag-account maken](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Vul de velden als volgt in:

    - Typ de subdomeinnaam van een gebruiken in de URL van de opslag-account in het veld **URL** . De invoer kan van de getallen en kleine letters 3-24 bevatten. Deze naam wordt de hostnaam van de in de URL die wordt gebruikt om Azure-blobopslag, Azure wachtrij opslag of Azure tabel opslag resources voor het abonnement.

    - In de **Groep locatie/affiniteit** vervolgmenu, kies de **locatie of affiniteit groep** voor de opslag-account. Een groep affiniteit kunt u uw cloudservices en opslag in de dezelfde Datacenter hebt opgeslagen.

    - Beslissen of **Geografische-redundante** herhaling gebruiken voor de opslag-account in het veld **herhaling** . Geografische-replicatie is standaard ingeschakeld. Deze optie wordt overgenomen door uw gegevens een secundaire locatie, zonder bijkomende kosten voor u, zodat uw opslag wordt overgenomen naar die locatie als een storing doet zich voor op de primaire locatie. De secundaire locatie wordt automatisch toegewezen en kan niet worden gewijzigd. Als u meer controle over de locatie van uw cloudgebaseerde opslag vanwege een juridische vereisten of organisatie-beleid nodig hebt, kunt u geografische herhaling uitschakelen. Let echter dat als u later op geografische herhaling inschakelt, de kosten van een eenmalige gegevens doorverbinden brengt repliceren van uw bestaande gegevens naar de secundaire locatie. Opslagservices zonder geografische herhaling worden aangeboden met een korting. Meer informatie over het beheren van geografische-herhaling opslag accounts vindt u hier: [maken, beheren, of een account opslagruimte verwijderen](../storage-create-storage-account/#replication-options).

    ![Opslag-accountgegevens invoeren](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Selecteer **opslag-Account maken**. Het account wordt nu weergegeven onder **opslag**.

    ![Opslag-account gemaakt](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Maak vervolgens een container voor uw geüploade VHD-bestanden. Selecteer de naam van het opslag-account en selecteer **Containers**.

    ![Opslag account detail](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Selecteer **een Container maken**.

    ![Opslag account detail](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. Typ een naam voor de container in het veld **naam** . De **toegang** vervolgmenu, selecteer in welk type-beleid die u wilt.

    ![De containernaam van de](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Standaard is de container privé is en kan alleen worden gebruikt door de eigenaar van het account. Als u wilt toestaan dat openbare leestoegang aan de BLOB's in de container, maar niet aan de containereigenschappen en metagegevens, gebruikt u de optie **Openbare Blob** . Als volledige openbare leestoegang voor de container en BLOB's, gebruikt u de optie **Openbare Container** .

## <a name="step-3-prepare-the-connection-to-azure"></a>Stap 3: De verbinding met Azure voorbereiden

Voordat u een VHD-bestand uploaden kunt, moet u een beveiligde verbinding tussen uw computer en uw Azure-abonnement. U kunt de methode Azure Active Directory (Azure AD) of de certificaat u dit wilt doen.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Gebruik van de Azure AD-methode voor het uploaden van een VHD-bestand

1. Open de Azure PowerShell-console.

2. Typ de volgende opdracht uit:  
    `Add-AzureAccount`

    Deze opdracht opent een aanmeldingsproblemen-venster waar kunt u zich aan met uw werk- of schoolaccount.

    ![PowerShell-venster](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure verifieert en de referentiegegevens worden opgeslagen. Vervolgens wordt het venster gesloten.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Gebruik de methode certificaat VHD-bestand uploaden

1. Open de Azure PowerShell-console.

2. Type:  `Get-AzurePublishSettingsFile`.

3. Een browservenster wordt geopend en krijgt u een bestand .publishsettings te downloaden. Dit bestand bevat informatie en een certificaat voor uw Azure-abonnement.

    ![Downloadpagina van de browser](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Sla het bestand .publishsettings.

4. Type:  `Import-AzurePublishSettingsFile <PathToFile>`, waarbij `<PathToFile>` het volledige pad naar het bestand .publishsettings.

   Zie [aan de slag met Azure cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx)voor meer informatie.

   Zie voor meer informatie over het installeren en configureren van PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Stap 4: Het VHD-bestand uploaden

Als u het bestand VHD uploadt, kunt u deze overal plaatsen binnen uw blobopslag. Hier volgen enkele termen die u gebruiken wilt wanneer u het bestand uploaden:
-  **BlobStorageURL** is de URL voor de opslag-account dat u hebt gemaakt in stap 2.
-  **YourImagesFolder** is de container binnen blobopslag waar u uw afbeeldingen op te slaan.
- **VHDName** is het label dat wordt weergegeven in de portal van Azure klassieke om aan te geven van de virtuele harde schijf.
- **PathToVHDFile** is het volledige pad en de naam van het VHD-bestand.


Typ in het venster Azure PowerShell dat u in de vorige stap hebt gebruikt:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Stap 5: Een VM maken met de geüploade VHD-bestand
Nadat u het bestand VHD uploadt, kunt u deze als een afbeelding toevoegen aan de lijst met aangepaste afbeeldingen die zijn gekoppeld aan uw abonnement en een virtuele machine maken met deze aangepaste afbeelding.

1. Typ in het venster Azure PowerShell dat u in de vorige stap hebt gebruikt:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Gebruik Linux als het type OS. De huidige versie van Azure PowerShell accepteert alleen 'Linux' of 'Windows' als een parameter.

2. Nadat u de vorige stappen hebt voltooid, wordt de nieuwe afbeelding wordt weergegeven wanneer u het tabblad **afbeeldingen** op de Azure klassieke portal kiest.  

    ![Een afbeelding kiezen](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Maak een virtuele machine vanuit de galerie. Deze nieuwe afbeelding is nu beschikbaar wordt weergegeven onder **Mijn afbeeldingen**.
4. Selecteer de nieuwe afbeelding. Ga vervolgens tot en met de aanwijzingen voor het instellen van een hostnaam, wachtwoord, SSH sleutel, enzovoort.

    ![Aangepaste afbeelding](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Nadat u de inrichting van hebt voltooid, ziet u uw FreeBSD VM uitgevoerd in Azure wordt aangegeven.

    ![Afbeelding van de FreeBSD in Azure wordt aangegeven](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)
