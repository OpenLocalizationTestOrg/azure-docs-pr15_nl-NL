<properties
   pageTitle="StorSimple virtuele matrix 1: voorbereiding van de Portal implementeren"
   description="Eerste zelfstudie om te implementeren StorSimple is virtual matrix heeft betrekking op het voorbereiden van de portal"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/24/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>Implementeren StorSimple virtuele matrix - de-portal voorbereiden

![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>Overzicht

In dit artikel is van toepassing op Microsoft Azure StorSimple virtuele matrix (ook wel bekend als de StorSimple on-premises implementatie virtueel apparaat of een virtueel apparaat StorSimple) actieve maart 2016 beschikbaarheid (GA) van de algemene release. Dit is het eerste artikel in de reeks van implementatie zelfstudies vereist om uw virtuele matrix volledig te implementeren als een bestandsserver of een iSCSI-server. In dit artikel worden de vereiste maken en configureren van uw service StorSimple Manager vóór de inrichting van een virtuele matrix voorbereiding. In dit artikel ook wordt gekoppeld af aan een controlelijst voor implementatie-configuratie, evenals de configuratie vereisten.

Moet u beheerdersrechten om de installatie en configuratie-proces te voltooien. Het is raadzaam dat u de implementatie, Configuratiecontrolelijst bekijken voordat u begint. De voorbereiding van de portal wordt minder dan 10 minuten duren.

De gegevens die zijn gepubliceerd in dit artikel is van toepassing op de implementatie van StorSimple virtuele matrices in Azure klassieke portal, evenals Microsoft Azure overheid Cloud.

### <a name="get-started"></a>Aan de slag

De implementatie-werkstroom bestaat uit de portal voorbereiden, inrichting van een virtuele matrix in uw gevirtualiseerde omgeving en de setup is voltooid. Als u wilt beginnen met de implementatie StorSimple virtuele matrix als een bestandsserver of een iSCSI-server, moet u om te verwijzen naar de volgende gebruikmaking bronnen (artikelen en video's).

#### <a name="deployment-articles"></a>Implementatie artikelen

Raadpleeg de volgende artikelen in de vooraf aangegeven volgorde om te implementeren van uw StorSimple virtuele matrix.

| **#** | **In deze stap**                          | **Doet u dit...**                                                         | **Gebruik deze documenten.**|
|------|-------------------------------------------|--------------------------------------------------------------------------------|------------------------|
|1.   | **Instellen van de Azure klassieke portal**       | Maken en configureren van uw service StorSimple Manager vóór de inrichting van een virtueel StorSimple-apparaat.  |[De portal voorbereiden](storsimple-ova-deploy1-portal-prep.md)|
|2.   | **Inrichten van de virtuele matrix**           | Voor Hyper-V, inrichten en verbinding maken met een virtueel apparaat StorSimple op een hostsysteem Hyper-V op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2. <br></br> <br></br> Voor VMware, inrichten en verbinding maken met een StorSimple on-premises implementatie virtueel apparaat op een hostsysteem VMware ESXi 5.5 en hoger.<br></br>| [Inrichten van een virtuele matrix in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [Een virtuele matrix in VMware inrichten](storsimple-ova-deploy2-provision-vmware.md)|
|3.    | **De virtuele matrix instellen**              | Voor uw bestandsserver, eerste configuratie uitvoeren, uw bestandsserver StorSimple registreren en voltooit de configuratie van het apparaat. U kunt vervolgens SMB aandelen inrichten. <br></br> <br></br> Eerste configuratie uitvoeren voor de server iSCSI, uw StorSimple iSCSI-server registreren en voltooit de configuratie van het apparaat. U kunt vervolgens iSCSI-volumes inrichten.| [Virtuele matrix als bestandsserver instellen](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[Virtuele matrix instellen als iSCSI-server](storsimple-ova-deploy3-iscsi-setup.md)|

#### <a name="deployment-videos"></a>Video's voor implementatie

| **Voer deze stap...** |  **Bekijk deze video.**|
|----------------|-------------|
| Stapsgewijze instructies voor het aan de slag met de virtuele StorSimple-matrix. | [Aan de slag met de virtuele StorSimple-matrix](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/)|
| Stapsgewijze instructies voor het inrichten van een virtuele matrix StorSimple in Hyper-V.|[Maak een virtuele StorSimple-matrix](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
|Stapsgewijze instructies voor het configureren en registreren van een virtuele StorSimple-matrix|[Een virtuele matrix StorSimple configureren](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/)|
|Stapsgewijze instructies voor het maken van de waarden voor aandelen, back-up van waarden voor aandelen en herstellen van gegevens op een StorSimple virtuele matrix geconfigureerd als een bestandsserver|[Gebruik de virtuele StorSimple-matrix](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/)|
|Stapsgewijze instructies voor failover en noodgevallen herstel van een virtuele StorSimple-matrix|[Herstel van StorSimple virtuele matrix](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/)

U kunt nu beginnen met het instellen van de Azure klassieke portal.

## <a name="configuration-checklist"></a>Van Configuratiecontrolelijst

De Configuratiecontrolelijst beschrijft de informatie die u nodig hebt voor het verzamelen van voordat u de software op uw apparaat StorSimple configureren. Deze informatie tijd vooraf voorbereiden helpt het proces van het implementeren van het apparaat StorSimple in uw omgeving stroomlijnen. Afhankelijk van of uw StorSimple virtuele apparaat wordt geïmplementeerd als een bestandsserver of een iSCSI-server, moet u een van de volgende controlelijsten.

-   Download de [StorSimple virtuele matrix bestand Server Configuratiecontrolelijst](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).

-   Download de [StorSimple virtuele matrix iSCSI Server Configuratiecontrolelijst](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Vereisten voor

Hier vindt u de vereisten voor de configuratie voor uw StorSimple Manager-service, uw StorSimple virtuele apparaat en het netwerk van het datacenter.

### <a name="for-the-storsimple-manager-service"></a>Voor de service StorSimple Manager

Controleer het volgende voordat u begint:

-   U hebt uw Microsoft-account met access-referenties.

-   U hebt uw account van Microsoft Azure opslagruimte met access-referenties.

-   Uw abonnement op Microsoft Azure moet worden ingeschakeld voor StorSimple Manager-service.

### <a name="for-the-storsimple-virtual-device"></a>Voor het virtuele StorSimple-apparaat

Controleer het volgende voordat u een virtueel apparaat implementeert:

-   U hebt toegang tot een hostsysteem met Hyper-V of hoger voor Windows Server 2008 R2 of VMware (ESXi 5,5 of later) dat kan worden gebruikt voor een voorziening een apparaat.

-   Het hostsysteem kan opslagquota van de volgende bronnen voor het inrichten van uw virtuele apparaat:

    -   Minimaal 4 cores.

    -   Ten minste 8 GB RAM.

    -   Gebruikersinterface van één netwerk.

    -   Een 500 GB virtuele schijf voor systeemgegevens.

### <a name="for-the-datacenter-network"></a>Voor het netwerk van het datacenter

Controleer het volgende voordat u begint:

-   Het netwerk in uw datacenter is aan de hand van de netwerken vereisten voor uw apparaat StorSimple geconfigureerd. Zie de [StorSimple virtuele matrix systeemvereisten](storsimple-ova-system-requirements.md)voor meer informatie.

-   Het virtuele apparaat StorSimple heeft een speciale 5 Mbps internetbandbreedte (of meer) beschikbaar te allen tijde. Deze bandbreedte mag niet worden gedeeld met andere toepassingen.

## <a name="step-by-step-preparation"></a>Voorbereiding van stapsgewijze

Gebruik de volgende stapsgewijze instructies voor het voorbereiden van uw portal voor de service StorSimple Manager.

## <a name="step-1-create-a-new-service"></a>Stap 1: Maak een nieuwe service

Een enkel exemplaar van de service StorSimple Manager kunt meerdere StorSimple 1200 apparaten beheren. De volgende stappen als u wilt maken van een nieuw exemplaar van de service StorSimple Manager uitvoeren. Als u een bestaande StorSimple Manager-service voor het beheren van uw apparaten 1200 hebt, deze stap overslaan en Ga naar [stap 2: de sleutel voor de registratie ophalen](#step-2-get-the-service-registration-key).

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [AZURE.IMPORTANT]
>
> Als u niet automatisch wordt gemaakt van een account opslagruimte met uw service heeft ingeschakeld, moet u ten minste één opslag-account maken nadat u een service hebt gemaakt.
>

> - Als u een opslag-account niet automatisch gemaakt hebt, gaat u naar [een nieuw account in de opslagruimte voor de service configureren](#optional-step-configure-a-new-storage-account-for-the-service) voor gedetailleerde instructies.
>

> - Als u het automatisch maken van een opslag-account hebt ingeschakeld, gaat u naar [stap 2: de sleutel voor de registratie ophalen](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Stap 2: Overleg de service registratie-toets


Nadat de StorSimple Manager-service actief is, moet u om de service registratie-toets. Deze toets wordt gebruikt om te registreren en uw apparaat StorSimple verbinden met de service.

De volgende stappen uitvoeren in de [klassieke Azure-portal](https://manage.windowsazure.com/).


[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [AZURE.NOTE]
>
> De registersleutel voor de service wordt gebruikt voor alle StorSimple Manager apparaten die hoeft te registreren bij uw Manager StorSimple-service registreren.

## <a name="step-3-download-the-virtual-device-image"></a>Stap 3: De afbeelding virtueel apparaat downloaden

Nadat u de sleutel voor de registratie hebt, moet u de juiste virtueel apparaat afbeelding als u wilt een virtueel apparaat op uw hostsysteem inrichten te downloaden. De afbeeldingen virtueel apparaat zijn specifiek besturingssysteem en kunnen worden gedownload van de pagina snel aan de slag in de portal van Azure klassieke.

> [AZURE.IMPORTANT] De software uitvoeren op de StorSimple virtuele matrix kan alleen worden gebruikt in combinatie met de Storsimple Manager-service.


De volgende stappen uitvoeren in de [klassieke Azure-portal](https://manage.windowsazure.com/).

#### <a name="to-get-the-virtual-device-image"></a>Om de afbeelding virtueel apparaat

1.  Klik op de pagina **service StorSimple Manager** op de service die u hebt gemaakt. Hiermee gaat u naar de pagina **Snel starten** . (U kunt klikken op het pictogram aan de slag ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) voor toegang tot de pagina **Snel starten** op elk gewenst moment.)

1.  Klik op de koppeling die overeenkomt met de afbeelding die u wilt downloaden van het Microsoft Download Center. De afbeeldingsbestanden zijn ongeveer 4,8 GB.

    -   VHDX voor Hyper-V op Windows Server 2012 en hoger

    -   VHD voor Hyper-V in Windows Server 2008 R2 en hoger

    -   VMDK voor VMWare ESXi 5.5 en hoger

2.  Download en pak het bestand op een lokale schijf, waardoor een notitie van waarin het uitgepakt bestand zich bevindt.

![videopictogram](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **Video beschikbaar**

Bekijk de video Stapsgewijze instructies voor het aan de slag met de virtuele StorSimple-matrix.

> [AZURE.VIDEO get-started-with-the-storsimple-virtual-array]



## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Optionele stap: een nieuw account in de opslagruimte voor de service configureren

Dit is een optionele stap dat moet worden uitgevoerd alleen als u niet automatisch wordt gemaakt van een account opslagruimte met uw service heeft ingeschakeld.

Als u een Azure opslag-account maken in een andere regio wilt, raadpleegt u [hoe u een opslag-account maken](storage-create-storage-account.md#create-a-storage-account) voor stapsgewijze instructies.

De volgende stappen uitvoeren in de [klassieke Azure-portal](https://manage.windowsazure.com/) op de pagina van de service StorSimple Manager naar een bestaand Microsoft Azure opslag-account toevoegen.

#### <a name="to-add-a-storage-account"></a>Een opslag-account toevoegen

1.  Op de pagina lossen StorSimple Manager-service, selecteer uw service en dubbelklik erop. Hiermee gaat u naar de pagina **Snel starten** . Selecteer de pagina **configureren** .

2.  Klik op **Add/edit opslag-account**. Ga als volgt te werk in het dialoogvenster **Add/Edit opslag Account** :

    1.  Klik op **Nieuwe toevoegen**.

    1.  Geef een naam voor uw account opslag.

    1.  De primaire **Sleutel van Access** voor uw account van Microsoft Azure opslag opgeven.

    1.  Selecteer **SSL inschakelen modus** voor het maken van een beveiligd kanaal voor communicatie tussen uw apparaat en de cloud. Schakel het selectievakje **SSL inschakelen modus** alleen als u in een persoonlijke cloud werkt.

    1.  Klik op het pictogram van het selectievakje ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). U krijgt nadat de opslag-account is gemaakt.

        ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)

1.  Het gemaakte opslag-account wordt weergegeven op de pagina **configureren** onder **opslag-accounts**. Klik op **Opslaan** om op te slaan het zojuist gemaakte opslag-account. Klik op **OK** wanneer u wordt gevraagd om bevestiging.


## <a name="next-step"></a>Volgende stap

De volgende stap is voor het inrichten van een virtuele machine voor uw virtuele StorSimple-apparaat. Afhankelijk van uw hostbesturingssysteem, raadpleegt u de gedetailleerde instructies in:

-   [Inrichten van een virtuele matrix StorSimple in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md)

-   [Een virtuele matrix StorSimple in VMware inrichten](storsimple-ova-deploy2-provision-vmware.md)
