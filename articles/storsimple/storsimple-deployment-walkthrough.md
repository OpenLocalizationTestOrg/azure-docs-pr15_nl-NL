<properties
   pageTitle="Implementatie van een on-premises implementatie StorSimple-apparaat | Microsoft Azure"
   description="Beschrijft de stappen en aanbevolen procedures voor het implementeren van de StorSimple apparaat en de service. (Van toepassing op Microsoft Azure StorSimple versie.3 of eerder.)"
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="deploy-your-on-premises-storsimple-device"></a>Uw on-premises implementatie StorSimple apparaat implementeren

> [AZURE.SELECTOR]
- [Update 2](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Update 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [GA Release](../articles/storsimple/storsimple-deployment-walkthrough.md)

## <a name="overview"></a>Overzicht

Welkom bij Microsoft Azure StorSimple apparaat implementatie. Deze zelfstudies implementatie van toepassing op StorSimple 8000 reeks releaseversie, StorSimple 8000 reeks Update 0,1, StorSimple 8000 reeks Update 0,2 en StorSimple 8000 reeks Update 0,3. Deze reeks zelfstudies wordt beschreven hoe u uw apparaat StorSimple en bevat een Configuratiecontrolelijst, configuratie vereisten en configuratie van gedetailleerde stappen.


De informatie in deze zelfstudies wordt ervan uitgegaan dat u hebt de voorzorgsmaatregelen beoordeeld en uitgepakt, racks en uw apparaat StorSimple bekabelde. Als u nog nodig hebt u deze taken uitvoert, begint u met het controleren van de [voorzorgsmaatregelen](storsimple-safety.md). Afhankelijk van uw Apparaatmodel u vervolgens kunt uitpakken, rek koppelen en kabel volgens de instructies in:

- [Uitpakken, rek mountains en uw 8100 kabel](storsimple-8100-hardware-installation.md)
- [Uitpakken, rek mountains en uw 8600 kabel](storsimple-8600-hardware-installation.md)

Moet u beheerdersrechten om de installatie en configuratie-proces te voltooien. Het is raadzaam dat u de Configuratiecontrolelijst bekijken voordat u begint. Het installatie en configuratie kan enige tijd duren.

> [AZURE.NOTE] De StorSimple implementatie-gegevens die zijn gepubliceerd op de website van Microsoft Azure is van toepassing op alleen StorSimple 8000 reeks-apparaten. Voor volledige informatie over de reeks 5000 en 7000 apparaten, gaat u naar: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Zie de [StorSimple systeem aan de slag:](http://onlinehelp.storsimple.com/111_Appliance/)voor 5000 en 7000 reeks implementatie informatie.

## <a name="deployment-steps"></a>Implementatiestappen

Deze vereiste stappen voor het configureren van uw apparaat StorSimple en verbindt u deze met uw service StorSimple Manager uitvoeren. Naast de vereiste stappen zijn optioneel stappen en procedures die u u tijdens de implementatie moet mogelijk. De implementatie van stapsgewijze instructies geven aan wanneer u moet elk van deze optionele stappen uitvoert.


| Stap                                                                                   | Beschrijving                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **VEREISTEN VOOR**                                                                      | Deze moeten worden voltooid in voorbereiden voor de komende implementatie.                                                                                        |
| Controlelijst voor implementatie-configuratie.                                                     | Gebruik deze controlelijst verzamelen en vastleggen van informatie voorafgaand aan en tijdens de implementatie.                                                                       |
| Vereisten voor implementatie.                                                               | Deze valideren van de omgeving gereed is voor implementatie.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **STAPSGEWIJZE IMPLEMENTATIE**                                                                   | Deze stappen zijn vereist om uw apparaat StorSimple in productie implementeren.                                                                                      |
| Stap 1: Maak een nieuwe service.                                                         | Cloud management en opslagruimte voor uw apparaat StorSimple instellen. Als u een bestaande service voor andere apparaten StorSimple hebt, kunt u deze stap overslaan.                |
| Stap 2: Overleg de sleutel voor de registratie.                                               | Gebruik deze toets om te registreren en het koppelen van een StorSimple-apparaat met de management-service.                                                                         |
| Stap 3: Configureren en het apparaat via Windows PowerShell registreren voor StorSimple.    | Het apparaat verbinden met uw netwerk en het registreren met Azure om de configuratie met de management-service te voltooien.                                            |
| Stap 4: Voltooid minimale apparaat instellen</br>Optioneel: Werk uw apparaat StorSimple.      | Gebruik de management-service voltooit de configuratie van het apparaat en deze te kunnen opslaan inschakelen.                                                                      |
| Stap 5: Een container volume maken.                                                      | Een container inrichten volume maken. Een container volume heeft opslag-account, bandbreedte en versleutelingsinstellingen voor alle de hoeveelheden die dit.    |
| Stap 6: Een volume maken.                                                                | Opslag volume (s) op het apparaat StorSimple voor uw servers inrichten.                                                                                        |
| Stap 7: Koppelen, geïnitialiseerd en maak een volume.</br>Optioneel: Configureren MPIO.            | Uw servers verbinden met de iSCSI-opslag verstrekt door het apparaat. (Optioneel) configureren MPIO om ervoor te zorgen dat uw servers mogelijk zonder koppeling-, netwerk- en interface is mislukt.                                                                                                                                                              |
| Stap 8: Een back-up uitvoeren.                                                                  | Uw back-beleid instellen voor uw gegevens beschermen                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **ANDERE PROCEDURES**                                                                   | Mogelijk moet u deze procedures raadplegen wanneer u uw oplossing implementeert.                                                                                        |
| Een nieuw account in de opslagruimte voor de service configureren.                                      |                                                                                                                                                               |
| Met stopverf verbinding maken met de seriële apparaat-console.                                    |                                                                                                                                                               |
| Krijgen de IQN van een Windows Server-host.                                                   |                                                                                                                                                               |
| Een handmatige back-up maken.                                                                 |


## <a name="deployment-configuration-checklist"></a>Controlelijst voor implementatie-configuratie

De volgende controlelijst voor implementatie-configuratie beschrijft de informatie die u nodig hebt voor het verzamelen en voordat u de software op uw apparaat StorSimple configureren. Enkele van deze gegevens tijd vooraf voorbereiden helpt het proces van het implementeren van het apparaat StorSimple in uw omgeving stroomlijnen. Gebruik deze controlelijst om de configuratiedetails van de ook noteren implementatie van uw apparaat.

| Fase                                  | Parameter                                         | Meer informatie                                                                                                                                                                | Waarden |
|----------------------------------------|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| **Kabel aan op uw apparaat**                      | Seriële toegang                                     | Apparaatconfiguratie van de eerste                                                                  | Ja/Nee |
|   |   |  |  |
| **Configureren en apparaat registreren**          | Gegevens 0 netwerkinstellingen                           | Gegevens 0 IP-adres:</br>Subnetmasker:</br>Gateway:</br>Primaire DNS-server:</br>Primaire NTP-server:</br>Web-proxyserver IP-/ FQDN (optioneel):</br>Web proxypoort:|        |
|                                        | Beheerderswachtwoord apparaat                     | Wachtwoord moet tussen 8 en 15 tekens met kleine letters, hoofdletters, numerieke en speciale tekens. |        |
|                                        | StorSimple momentopname Manager-wachtwoord              | Wachtwoord moet 14 of 15 tekens met kleine letters, hoofdletters, numerieke en speciale tekens.|        |
|                                        | Service registratiegegevens sleutel                          | Deze toets wordt afgeleid van de Azure klassieke portal.    |        |
|                                        | Sleutel service Data-versleuteling                       | Deze toets wordt gemaakt wanneer het apparaat dat is geregistreerd bij de management-service via de Windows PowerShell voor StorSimple. Kopieer deze toets en sla deze op een veilige locatie.|  |
|   |   |  |  |
| **Voltooid minimale apparaat instellen**          | Beschrijvende naam voor uw apparaat                     | Dit is een beschrijvende naam voor het apparaat. |        |
|                                        | Tijdzone                                          | Uw apparaat, worden deze tijdzone gebruiken voor alle geplande bewerkingen.  |        |
|                                        | Secundaire DNS-server                              | Dit is een vereiste configuratie.                                  |        |
|                                        | Netwerkinterface: gegevens 0 controller vaste IP-adressen                                     | De volgende IP moet geschikt zijn met Internet.</br>Controller 0 vaste IP address:</br>Controller 1 vaste IP address:|
|   |   |  |  |
| **Gebruikersinterface van extra netwerkinstellingen**  | Netwerkinterface: gegevens 1</br>Configureer de Gateway niet als iSCSI is ingeschakeld.      | Doel: Cloud/iSCSI/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|                                        | Netwerkinterface: gegevens 2</br>Configureer de Gateway niet als iSCSI is ingeschakeld.      | Doel: Cloud/iSCSI/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|                                        | Netwerkinterface: gegevens 3</br>Configureer de Gateway niet als iSCSI is ingeschakeld.      | Doel: Cloud/iSCSI/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|                                        | Netwerkinterface: gegevens 4</br>Configureer de Gateway niet als iSCSI is ingeschakeld.      | Doel: Cloud/iSCSI/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|                                        | Netwerkinterface: gegevens 5</br>Configureer de Gateway niet als iSCSI is ingeschakeld.      | Doel: Cloud/iSCSI/niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway:|
|   |   |  |  |
| **Een container volume maken**                      | Naam van de container volume:                            | Naam voor de container                                                                                                                                                 |        |
|                                        | Azure opslag-account:                            | Opslag account naam & voor de sneltoets om te koppelen aan deze container volume                                                                                              |        |
|                                        | Cloud opslag versleutelingssleutel:                     | Versleutelingssleutel voor opslagruimte in elke container                                                                                                                           |        |
|   |   |  |  |
| **Een volume maken**                        | Details voor elk volume                           | De naam van de volume:                                                                                                                                                           |        |
|                                        |                                                   | Grootte:                                                                                                                                                                  |        |
|                                        |                                                   | Gebruikstype:                                                                                                                                                            |        |
|                                        |                                                   | De naam van de ACR:                                                                                                                                                              |        |
|                                        |                                                   | Standaardbeleid voor back-up:                                                                                                                                                 |        |
|   |   |  |  |
| **Koppelen, geïnitialiseerd en een volume opmaken** | Gegevens voor elke host-server verbinding maakt met de opslag | De naam van de Windows Server:                                                                                                                                                   |        |
|                                        |                                                   | Windows Server IQN:                                                                                                                                                    |        |
|                                        |                                                   | Het volume van de naam van Windows Server:                                                                                                                                                   |        |
|                                        |                                                   | NTFS koppelen punt/station letter:                                                                                                                                      |        |

## <a name="deployment-prerequisites"></a>Vereisten voor implementatie

De volgende secties wordt uitgelegd dat de configuratie vereisten voor uw StorSimple Manager-service, uw apparaat StorSimple en het netwerk in uw datacenter.

### <a name="for-the-storsimple-manager-service"></a>Voor de service StorSimple Manager

Controleer het volgende voordat u begint:

- U hebt uw Microsoft-account met access-referenties.

- U hebt uw account van Microsoft Azure opslagruimte met access-referenties.

- Uw abonnement op Microsoft Azure is ingeschakeld voor de StorSimple Manager-service. Uw abonnement moet worden aangeschaft via de [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).

- U hebt toegang tot terminalemulatiesoftware zoals stopverf.

### <a name="for-the-device-in-the-datacenter"></a>Voor het apparaat in het datacenter

Voordat u configureert het apparaat, zorg ervoor dat:

- Uw apparaat is volledig uitgepakt, gekoppeld in een rek en volledig bekabelde voor power-, netwerk- en seriële toegang zoals is beschreven in:

    -  [Uitpakken, rek mountains en uw apparaat 8100 kabel](storsimple-8100-hardware-installation.md)
    -  [Uitpakken, rek mountains en uw apparaat 8600 kabel](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter

Controleer het volgende voordat u begint:

- De poorten in de firewall datacenter worden geopend als u wilt toestaan voor iSCSI en cloud verkeer zoals is beschreven in de [vereisten voor uw apparaat StorSimple netwerkproblemen](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
- Het apparaat in uw datacenter kunt verbinding maken met externe netwerk. Voer de volgende [Windows PowerShell 4.0](http://www.microsoft.com/download/details.aspx?id=40855) -cmdlets (tabelindeling hieronder) voor het valideren van de verbinding met het externe netwerk. Deze validatie uitvoeren op een computer (in datacenter netwerk) waarop connectiviteit voor Azure en waar u uw apparaat StorSimple gaat implementeren.  

| Voor deze parameter...       | Controleer de geldigheid...                                                                                                                                                                                | Voer deze opdrachten/cmdlets.                                                                                                                                                             |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **IP**</br>**Subnet**</br>**Gateway** | Is dit een geldig IPv4 of IPv6-adres?</br>Is dit een geldige subnet?</br>Is dit een geldige gateway?</br>Is dit een dubbele IP-adres in een netwerk?                                                                          | `ping ip`</br>`arp -a`</br>De `ping` en `arp` opdrachten moeten mislukt die aangeeft dat er geen apparaat in het netwerk datacenter waarin deze IP wordt gebruikt.
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **DNS**                       | Is dit een geldige DNS en Azure-URL's kunt oplossen?                                                                                                                    | `Resolve-DnsName -Name www.bing.com -Server <DNS server IP address>` </br>Er is een alternatief opdracht die kan worden gebruikt:</br>`nslookup --dns-ip=<DNS server IP address> www.bing.com`      |
|                           | Controleer of poort 53 geopend is. Dit is alleen van toepassing als u van een externe DNS voor uw apparaat gebruikmaakt. Interne DNS moet de externe URL's automatisch opgelost.  | `Test-Port -comp dc1 -port 53 -udp -UDPtimeout 10000`  </br>[Meer informatie over deze cmdlet](http://learn-powershell.net/2011/02/21/querying-udp-ports-with-powershell/)|
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **NTP**                       | We activeren een tijdsynchronisatie zodra NTP-server worden ingevoerd. Controleer UDP-poorten 123 is geopend wanneer u input `time.windows.com` of openbare tijdservers). | [Downloaden en gebruiken van dit script](https://gallery.technet.microsoft.com/scriptcenter/Get-Network-NTP-Time-with-07b216ca).                                                                                                                                                           |
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **Proxy (optioneel)**          | Is dit een geldige proxy URI en poorten? </br> De verificatiemodus correct is?                                                                                                                                | <code>wget http://bing.com &#124; % {$_.StatusCode}</code></br>Deze opdracht moet worden uitgevoerd direct na de webproxy configureren. Als een statuscode van 200 wordt geretourneerd, geeft aan dat de verbinding geslaagd is.                                                                                                                                 |
|                           | Verkeer via proxy geschikt is?                                                                                                                                 | De DNS-validatie, NTP HTTP in en één keer na proxy configureren op uw apparaat uitgevoerd. Hiermee krijgt een duidelijke afbeelding als verkeer ophalen geblokkeerd op proxy of ergens anders.                                                                                                                      |
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **Registratie**              | Controleer als uitgaande TCP-poorten 443, 80, 9354 geopend zijn.                                                                                                                |  `Test-NetConnection -Port   443 -InformationLevel Detailed`</br>[Meer informatie voor Test-NetConnection cmdlet](https://technet.microsoft.com/library/dn372891.aspx)                                                                           |

## <a name="step-by-step-deployment"></a>Stapsgewijze implementatie

Gebruik de volgende stapsgewijze instructies voor de implementatie van uw apparaat StorSimple in het datacenter.

## <a name="step-1-create-a-new-service"></a>Stap 1: Maak een nieuwe service

Een service StorSimple Manager kunt meerdere StorSimple apparaten beheren. Voor de implementatie van uw eerste StorSimple-apparaat moet u een nieuwe StorSimple Manager-service maken.

> [AZURE.IMPORTANT] Deze stap overslaan als u een bestaande StorSimple Manager-service hebt en u wilt implementeren van uw apparaat StorSimple met deze service.

De volgende stappen als u wilt maken van een nieuw exemplaar van de service StorSimple Manager uitvoeren.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Als u niet automatisch wordt gemaakt van een account opslagruimte met uw service heeft ingeschakeld, moet u ten minste één opslag-account maken nadat u een service hebt gemaakt. Dit account opslag wordt gebruikt wanneer u een container volume maakt.
>
> Als u een opslag-account niet automatisch gemaakt hebt, gaat u naar [een nieuw account in de opslagruimte voor de service configureren](#configure-a-new-storage-account-for-the-service) voor gedetailleerde instructies.
> Als u het automatisch maken van een opslag-account hebt ingeschakeld, gaat u naar [stap 2: de sleutel voor de registratie ophalen](#step-2:-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Stap 2: Overleg de service registratie-toets

Nadat de StorSimple Manager-service actief is, moet u om de service registratie-toets. Deze toets wordt gebruikt om te registreren en uw apparaat StorSimple verbinden met de service.

De volgende stappen uitvoeren in de portal van Azure klassieke.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Stap 3: Configureren en het apparaat via Windows PowerShell registreren voor StorSimple

> [AZURE.IMPORTANT] Verwijder alle netwerkinterfaces dan gegevens 0 op zowel (actief en passieve) de controllers voordat het uitvoeren van deze configuratie.

Windows PowerShell voor StorSimple voor het voltooien van de eerste configuratie van uw apparaat StorSimple, zoals wordt beschreven in de volgende procedure gebruiken. U moet terminalemulatiesoftware gebruiken om te voltooien van deze stap. Zie [Gebruik stopverf verbinding maken met de seriële console apparaat](#use-putty-to-connect-to-the-device-serial-console)voor meer informatie.

[AZURE.INCLUDE [storsimple-configure-and-register-device](../../includes/storsimple-configure-and-register-device.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Stap 4: Voltooid minimale apparaat instellen

Voor de minimale apparaatconfiguratie van uw apparaat StorSimple zijn u moet:

- Stel de secundaire DNS-server.
- ISCSI op ten minste één netwerkinterface inschakelen.
- Vaste IP-adressen beide de controller toewijzen.

De volgende stappen uitvoeren in de portal van Azure klassieke de minimale apparaat-installatie te voltooien.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup.md)]

Nadat de apparaatconfiguratie voltooid is, moet u zoeken naar updates en indien beschikbaar, updates installeren. De updates kunnen enkele uren duren. Volg de instructies in [Zoeken en updates toepassen](#scan-for-and-apply-updates).


## <a name="step-5-create-a-volume-container"></a>Stap 5: Een container volume maken

Een container volume heeft opslag-account, bandbreedte en versleutelingsinstellingen voor alle de hoeveelheden die dit. U moet een container volume maken voordat u kunt volumes op uw apparaat StorSimple inrichten.

De volgende stappen uitvoeren in de portal van Azure klassieke een container volume maken.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Stap 6: Een volume maken

Nadat u een container volume hebt gemaakt, kunt u een volume opslagruimte op het apparaat StorSimple inrichten voor uw servers. De volgende stappen uitvoeren in de portal van Azure klassieke een volume maken.

> [AZURE.IMPORTANT] StorSimple Manager kunt alleen dun ingerichte volumes maken.  U kunt geen geheel of gedeeltelijk ingerichte volumes maken.

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Stap 7: Koppelen, geïnitialiseerd en een volume opmaken

> [AZURE.IMPORTANT]

> - De beschikbaarheid van uw oplossing StorSimple, is het raadzaam dat u MPIO op uw Windows Server-host (optioneel configureren) voordat u iSCSI-configureren op uw Windows Server-host. MPIO-configuratie op hostservers zorgt ervoor dat dat de servers mogelijk zonder een koppeling, netwerk of interface is mislukt.

> - Ga naar [MPIO configureren voor uw apparaat StorSimple](storsimple-configure-mpio-windows-server.md)MPIO en iSCSI-installatie en configuratie voor instructies. Deze worden ook de stappen om te koppelen, geïnitialiseerd en formatteren StorSimple opgenomen.

Als u geen te MPIO configureren, moet u de volgende stappen om te koppelen, geïnitialiseerd en begint StorSimple uitvoeren.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Stap 8: Een back-up maken

Back-ups point-in-time bescherming van hoeveelheden bieden en herstelmogelijkheden tegelijkertijd herstellen tijden te verbeteren. U kunt twee soorten back-up uitvoeren op uw apparaat StorSimple: lokale momentopnamen en cloud momentopnamen. Elk van deze back-typen kunnen zijn **gepland** of **handmatig**.

De volgende stappen uitvoeren in de portal van Azure klassieke een geplande back-up maken.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

U kunt een handmatig back-up uitvoeren op elk gewenst moment. Voor procedures uit, gaat u naar [een handmatige back-up maken](#Create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-the-service"></a>Een nieuw account in de opslagruimte voor de service configureren

Dit is een optionele stap die u alleen uitvoeren moet als u niet automatisch wordt gemaakt van een account opslagruimte met uw service heeft ingeschakeld. Een Microsoft Azure opslag-account is vereist voor het maken van een StorSimple volume container.

Als u een Azure opslag-account maken in een andere regio wilt, raadpleegt u [Over Azure opslag Accounts](../storage/storage-create-storage-account.md) voor stapsgewijze instructies.

De volgende stappen uitvoeren in de klassieke Azure portal, klik op de pagina **StorSimple Manager-service** .

[AZURE.INCLUDE [storsimple-configure-new-storage-account](../../includes/storsimple-configure-new-storage-account.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Met stopverf verbinding maken met de seriële console apparaat

Als u wilt verbinding maken met Windows PowerShell voor StorSimple, moet u terminalemulatiesoftware zoals stopverf gebruiken. U kunt stopverf gebruiken wanneer u het apparaat rechtstreeks via de seriële console of als u een telnetsessie opent vanuit een externe computer.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Zoeken en updates toepassen

Bijwerken van uw apparaat kan duren 1-4 uur. Voer de volgende stappen uit als u wilt zoeken en updates toepassen op uw apparaat.

> [AZURE.NOTE] Als er een gateway op een netwerkinterface dan gegevens 0 geconfigureerd, moet u voor het uitschakelen van gegevens 2 en 3 van de gegevens via interfaces zoals netwerk voordat u de update installeert. Ga naar **apparaten > configureren** en uitschakelen via interfaces zoals gegevens 2 en 3 van de gegevens. U moet deze interfaces weer inschakelen nadat het apparaat wordt bijgewerkt.

#### <a name="to-update-your-device"></a>Bijwerken van uw apparaat
1.  Klik op de pagina apparaat **Snel starten** op **apparaten**. Selecteer het apparaat fysiek **onderhoud** op en klik vervolgens op **Updates scannen**.  
2.  Een taak voor het zoeken naar beschikbare updates wordt gemaakt. Als updates beschikbaar zijn, wordt de **Updates scannen** gewijzigd in **Updates installeren**. Klik op **Updates installeren**. U wordt mogelijk gevraagd om uit te schakelen gegevens 2 en 3 van de gegevens voordat u de updates worden geïnstalleerd. Moet u deze netwerkinterfaces uitschakelen of de updates kunnen mislukken.
3.  Een bijwerktaak wordt gemaakt. De status van de update controleren door te schuiven aan **projecten**.

    > [AZURE.NOTE] Wanneer de bijwerktaak wordt gestart, wordt onmiddellijk de status weergegeven als 50 procent. Het veld status wordt vervolgens tot 100 procent gewijzigd nadat de bijwerktaak voltooid is. Er is geen realtime status voor het proces updates.

4.  Nadat het apparaat is bijgewerkt, schakelt u gegevens 2 en 3 van de gegevens netwerk-interfaces als deze zijn uitgeschakeld.



## <a name="get-the-iqn-of-a-windows-server-host"></a>De IQN van een Windows Server-host ophalen

Voer de volgende stappen uit als u de iSCSI Qualified Name (IQN) van een Windows-host met Windows Server 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]


## <a name="create-a-manual-backup"></a>Een handmatige back-up maken

De volgende stappen uitvoeren in de portal van Azure klassieke maken van een op aanvraag handmatig back-up voor één volume op uw apparaat StorSimple.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]


## <a name="next-steps"></a>Volgende stappen

- Een [virtueel apparaat](storsimple-virtual-device-u2.md)configureren.

- Gebruik de [StorSimple Manager-service](https://msdn.microsoft.com/library/azure/dn772396.aspx) voor het beheren van uw apparaat StorSimple.
