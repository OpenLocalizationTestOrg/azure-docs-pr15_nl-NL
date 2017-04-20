<properties 
   pageTitle="Uw apparaat StorSimple (Update 2) implementeren | Microsoft Azure"
   description="Beschrijft de stappen en aanbevolen procedures voor het implementeren van de StorSimple Update 2 apparaat en de service."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="deploy-your-on-premises-storsimple-device-update-2"></a>Uw on-premises implementatie StorSimple apparaat (Update 2) implementeren

> [AZURE.SELECTOR]
- [Update 2 en hoger](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Update 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [GA Release](../articles/storsimple/storsimple-deployment-walkthrough.md)

## <a name="overview"></a>Overzicht

Welkom bij Microsoft Azure StorSimple apparaat implementatie. Deze zelfstudies implementatie van toepassing op StorSimple 8000 reeks Update 2. Deze reeks zelfstudies bevat een Configuratiecontrolelijst, configuratie vereisten en gedetailleerde configuratiestappen voor uw apparaat StorSimple.

De informatie in deze zelfstudies wordt ervan uitgegaan dat u hebt de voorzorgsmaatregelen beoordeeld en uitgepakt, racks en uw apparaat StorSimple bekabelde. Als u nog nodig hebt u deze taken uitvoert, begint u met het controleren van de [voorzorgsmaatregelen](storsimple-safety.md). Volg de specifieke instructies apparaat uitpakken, rek mountains en kabel van uw apparaat.

- [Uitpakken, rek mountains en uw 8100 kabel](storsimple-8100-hardware-installation.md)
- [Uitpakken, rek mountains en uw 8600 kabel](storsimple-8600-hardware-installation.md)

Moet u beheerdersrechten om de installatie en configuratie-proces te voltooien. Het is raadzaam dat u de Configuratiecontrolelijst bekijken voordat u begint. Het installatie en configuratie kan enige tijd duren.

> [AZURE.NOTE] De StorSimple implementatie-gegevens die zijn gepubliceerd op de website van Microsoft Azure is van toepassing op alleen StorSimple 8000 reeks-apparaten. Voor volledige informatie over de apparaten 7000 reeks, gaat u naar: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Zie de [StorSimple systeem aan de slag:](http://onlinehelp.storsimple.com/111_Appliance/)voor 7000 reeks implementatie informatie.

## <a name="deployment-steps"></a>Implementatiestappen

Deze vereiste stappen voor het configureren van uw apparaat StorSimple en verbindt u deze met uw service StorSimple Manager uitvoeren. Naast de vereiste stappen zijn optioneel stappen en procedures die u u tijdens de implementatie moet mogelijk. De implementatie van stapsgewijze instructies geven aan wanneer u moet elk van deze optionele stappen uitvoert.


| Stap                                                                                   | Beschrijving                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **VEREISTEN VOOR**                                                                      | Deze moeten worden voltooid in voorbereiden voor de komende implementatie.                                                                                        |
| [Controlelijst voor implementatie-configuratie](#deployment-configuration-checklist)                                                     | Gebruik deze controlelijst verzamelen en vastleggen van informatie voorafgaand aan en tijdens de implementatie.                                                                       |
| [Vereisten voor implementatie](#deployment-prerequisites)                                                               | Deze valideren van de omgeving gereed is voor implementatie.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **STAPSGEWIJZE IMPLEMENTATIE**                                                                   | Deze stappen zijn vereist om uw apparaat StorSimple in productie implementeren.                                                                                      |
| [Stap 1: Maak een nieuwe service](#step-1-create-a-new-service)                                                         | Cloud management en opslagruimte voor uw apparaat StorSimple instellen. *Deze stap hebt u een bestaande service voor andere apparaten StorSimple overslaan*.                |
| [Stap 2: Overleg de service registratie-toets](#step-2-get-the-service-registration-key)                                               | Gebruik deze toets om te registreren en het koppelen van een StorSimple-apparaat met de management-service.                                                                         |
| [Stap 3: Configureren en het apparaat via Windows PowerShell registreren voor StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)    | Het apparaat verbinden met uw netwerk en het registreren met Azure om de configuratie met de management-service te voltooien.                                            |
| [Stap 4: Voltooid minimale apparaat instellen](#step-4-complete-minimum-device-setupd)</br>[Optioneel: Uw apparaat StorSimple bijwerken](#scan-for-and-apply-updates)      | Gebruik de management-service voltooit de configuratie van het apparaat en deze te kunnen opslaan inschakelen.                                                                      |
| [Stap 5: Een container volume maken](#step-5-create-a-volume-container)                                                      | Een container inrichten volume maken. Een container volume heeft opslag-account, bandbreedte en versleutelingsinstellingen voor alle de hoeveelheden die dit.    |
| [Stap 6: Een volume maken](#step-6-create-a-volume)                                                                | Opslag volume (s) op het apparaat StorSimple voor uw servers inrichten.                                                                                        |
| [Stap 7: Koppelen, geïnitialiseerd en een volume opmaken](#step-7-mount-initialize-and-format-a-volume)</br>[Optioneel: MPIO configureren](storsimple-configure-mpio-windows-server.md)            | Uw servers verbinden met de iSCSI-opslag verstrekt door het apparaat. (Optioneel) MPIO om ervoor te zorgen dat uw servers koppeling-, netwerk- en interface mislukt mogelijk zonder configureren.                                                                                                                                                              |
| [Stap 8: Een back-up maken](#step-8-take-a-backup)                                                                  | Uw back-beleid instellen voor uw gegevens beschermen                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **ANDERE PROCEDURES**                                                                   | Mogelijk moet u deze procedures raadplegen wanneer u uw oplossing implementeert.                                                                                        |
| [Een nieuw account in de opslagruimte voor de service configureren](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [Met stopverf verbinding maken met de seriële console apparaat](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [De IQN van een Windows Server-host ophalen](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Een handmatige back-up maken](#create-a-manual-backup)                                                                 | 


## <a name="deployment-configuration-checklist"></a>Controlelijst voor implementatie-configuratie

Voordat u uw apparaat implementeert, moet u voor het verzamelen van informatie om te configureren van de software op uw apparaat StorSimple. Enkele van deze gegevens tijd vooraf voorbereiden helpt het proces van het implementeren van het apparaat StorSimple in uw omgeving stroomlijnen. Download en gebruik deze controlelijst om de configuratiegegevens noteren implementatie van uw apparaat.

- [Controlelijst voor de configuratie van de StorSimple implementatie downloaden](http://www.microsoft.com/download/details.aspx?id=49159)


## <a name="deployment-prerequisites"></a>Vereisten voor implementatie

De volgende secties wordt uitgelegd dat de configuratie vereisten voor uw StorSimple Manager-service en het apparaat StorSimple.

### <a name="for-the-storsimple-manager-service"></a>Voor de service StorSimple Manager

Controleer het volgende voordat u begint:

- U hebt uw Microsoft-account met access-referenties.

- U hebt uw account van Microsoft Azure opslagruimte met access-referenties.

- Uw abonnement op Microsoft Azure is ingeschakeld voor de StorSimple Manager-service. Uw abonnement moet worden aangeschaft via de [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).

- U hebt toegang tot terminalemulatiesoftware zoals stopverf.

### <a name="for-the-device-in-the-datacenter"></a>Voor het apparaat in het datacenter

Voordat u configureert het apparaat, zorg ervoor dat uw apparaat is volledig uitgepakt, gekoppeld in een rek en volledig bekabelde voor power-, netwerk- en seriële toegang zoals is beschreven in:

-  [Uitpakken, rek mountains en uw apparaat 8100 kabel](storsimple-8100-hardware-installation.md)
-  [Uitpakken, rek mountains en uw apparaat 8600 kabel](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter

Controleer het volgende voordat u begint:

- De poorten in de firewall datacenter worden geopend als u wilt toestaan voor iSCSI en cloud verkeer zoals is beschreven in de [vereisten voor uw apparaat StorSimple netwerkproblemen](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).


## <a name="step-by-step-deployment"></a>Stapsgewijze implementatie

Gebruik de volgende stapsgewijze instructies voor de implementatie van uw apparaat StorSimple in het datacenter.

## <a name="step-1-create-a-new-service"></a>Stap 1: Maak een nieuwe service

Een service StorSimple Manager kunt meerdere StorSimple apparaten beheren. De volgende stappen als u wilt maken van een nieuw exemplaar van de service StorSimple Manager uitvoeren.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Als u niet automatisch wordt gemaakt van een account opslagruimte met uw service heeft ingeschakeld, moet u ten minste één opslag-account maken nadat u een service hebt gemaakt. Dit account opslag wordt gebruikt wanneer u een container volume maakt. 
>
> * Als u een opslag-account niet automatisch gemaakt hebt, gaat u naar [een nieuw account in de opslagruimte voor de service configureren](#configure-a-new-storage-account-for-the-service) voor gedetailleerde instructies. 
> * Als u het automatisch maken van een opslag-account hebt ingeschakeld, gaat u naar [stap 2: de sleutel voor de registratie ophalen](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Stap 2: Overleg de service registratie-toets

Nadat de StorSimple Manager-service actief is, moet u om de service registratie-toets. Deze toets wordt gebruikt om te registreren en uw apparaat StorSimple verbinden met de service.

De volgende stappen uitvoeren in de beheerportal.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Stap 3: Configureren en het apparaat via Windows PowerShell registreren voor StorSimple

Windows PowerShell voor StorSimple voor het voltooien van de eerste configuratie van uw apparaat StorSimple, zoals wordt beschreven in de volgende procedure gebruiken. U moet terminalemulatiesoftware gebruiken om te voltooien van deze stap. Zie [Gebruik stopverf verbinding maken met de seriële console apparaat](#use-putty-to-connect-to-the-device-serial-console)voor meer informatie.

[AZURE.INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Stap 4: Voltooid minimale apparaat instellen

Voor de minimale apparaatconfiguratie van uw apparaat StorSimple zijn u moet: 

- Stel de secundaire DNS-server.
- ISCSI op ten minste één netwerkinterface inschakelen.
- Vaste IP-adressen beide de controller toewijzen.

De volgende stappen uitvoeren in de beheerportal de minimale apparaat-installatie te voltooien.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Stap 5: Een container volume maken

Een container volume heeft opslag-account, bandbreedte en versleutelingsinstellingen voor alle de hoeveelheden die dit. U moet een container volume maken voordat u kunt volumes op uw apparaat StorSimple inrichten. 

De volgende stappen uitvoeren in de beheerportal maken van een container volume.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Stap 6: Een volume maken

Nadat u een container volume hebt gemaakt, kunt u een volume opslagruimte op het apparaat StorSimple inrichten voor uw servers. De volgende stappen uitvoeren in de beheerportal een volume maken.

> [AZURE.IMPORTANT] StorSimple Manager kunt beide dunne en volledig ingerichte volumes maken. U kunt geen echter gedeeltelijk ingerichte volumes maken. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Stap 7: Koppelen, geïnitialiseerd en een volume opmaken

De volgende stappen worden uitgevoerd op uw Windows Server-host. 


> [AZURE.IMPORTANT]

> - De beschikbaarheid van uw oplossing StorSimple, is het raadzaam dat u MPIO op uw hostservers (optioneel configureren) voordat u iSCSI-configureren. MPIO-configuratie op hostservers zorgt ervoor dat dat de servers mogelijk zonder een koppeling, netwerk of interface is mislukt.

> - Ga naar [MPIO configureren voor uw apparaat StorSimple](storsimple-configure-mpio-windows-server.md)MPIO en iSCSI-installatie en configuratie voor instructies op Windows Server-host. Deze worden ook de stappen om te koppelen, geïnitialiseerd en formatteren StorSimple opgenomen.

> - Voor MPIO en iSCSI-installatie en configuratie instructies over een Linux-host, gaat u naar [MPIO configureren voor uw StorSimple Linux-host](storsimple-configure-mpio-on-linux.md)

Als u geen te MPIO configureren, moet u de volgende stappen om te koppelen, geïnitialiseerd en StorSimple begint op een Windows Server-host uitvoeren.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Stap 8: Een back-up maken

Back-ups point-in-time bescherming van hoeveelheden bieden en herstelmogelijkheden tegelijkertijd herstellen tijden te verbeteren. U kunt twee soorten back-up uitvoeren op uw apparaat StorSimple: lokale momentopnamen en cloud momentopnamen. Elk van deze back-typen kunnen zijn **gepland** of **handmatig**. 

De volgende stappen uitvoeren in de beheerportal een geplande back-up maken.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

U kunt een handmatig back-up uitvoeren op elk gewenst moment. Voor procedures uit, gaat u naar [een handmatige back-up maken](#create-a-manual-backup). 

## <a name="configure-a-new-storage-account-for-the-service"></a>Een nieuw account in de opslagruimte voor de service configureren

Dit is een optionele stap die u alleen uitvoeren moet als u niet automatisch wordt gemaakt van een account opslagruimte met uw service heeft ingeschakeld. Een Microsoft Azure opslag-account is vereist voor het maken van een StorSimple volume container.

Als u een Azure opslag-account maken in een andere regio wilt, raadpleegt u [Over Azure opslag Accounts](../storage/storage-create-storage-account.md) voor stapsgewijze instructies.

De volgende stappen uitvoeren in de beheerportal op de pagina **StorSimple Manager-service** .

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Met stopverf verbinding maken met de seriële console apparaat

Als u wilt verbinding maken met Windows PowerShell voor StorSimple, moet u terminalemulatiesoftware zoals stopverf gebruiken. U kunt stopverf gebruiken wanneer u het apparaat rechtstreeks via de seriële console of als u een telnetsessie opent vanuit een externe computer.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]


## <a name="scan-for-and-apply-updates"></a>Zoeken en updates toepassen

Bijwerken van uw apparaat kan het enkele uren duren. Voer de volgende stappen uit als u wilt zoeken en updates toepassen op uw apparaat.
<!--can take 1-4 hours--> 

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Bijwerken van uw apparaat

1.  Klik op de pagina apparaat **Snel starten** op **apparaten**. Selecteer het apparaat fysiek **onderhoud** op en klik vervolgens op **Updates scannen**.  

2.  Een taak voor het zoeken naar beschikbare updates wordt gemaakt. Als updates beschikbaar zijn, wordt de **Updates scannen** gewijzigd in **Updates installeren**. Klik op **Updates installeren**. 

3.  Een bijwerktaak wordt gemaakt. De status van de update controleren door te schuiven aan **projecten**.

    > [AZURE.NOTE] Wanneer de bijwerktaak wordt gestart, wordt onmiddellijk de status weergegeven als 50 procent. Het veld status wordt gewijzigd tot 100 procent alleen wanneer de bijwerktaak voltooid is. Er is geen realtime status voor het updateproces.

4.  Nadat het apparaat is bijgewerkt, schakelt u gegevens 2 en 3 van de gegevens netwerk-interfaces als deze zijn uitgeschakeld.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## <a name="get-the-iqn-of-a-windows-server-host"></a>De IQN van een Windows Server-host ophalen

Voer de volgende stappen uit als u de iSCSI Qualified Name (IQN) van een Windows-host met Windows Server® 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Een handmatige back-up maken

De volgende stappen uitvoeren in de beheerportal maken van een op aanvraag handmatig back-up voor één volume op uw apparaat StorSimple.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]


## <a name="next-steps"></a>Volgende stappen

- Een [virtueel apparaat](storsimple-virtual-device-u2.md)configureren.

- Gebruik de [StorSimple Manager-service](storsimple-manager-service-administration.md) voor het beheren van uw apparaat StorSimple.
 
