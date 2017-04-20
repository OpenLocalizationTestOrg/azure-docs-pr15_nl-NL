<properties 
   pageTitle="StorSimple virtueel apparaat Update 2 | Microsoft Azure"
   description="Informatie over het maken en beheren van een virtueel apparaat StorSimple in een netwerk met een Microsoft Azure virtual implementeren. (Geldt voor StorSimple Update 2)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Implementeren en beheren van een virtueel apparaat StorSimple in Azure wordt aangegeven


##<a name="overview"></a>Overzicht
Het StorSimple 8000 reeks virtuele apparaat is een extra functie die wordt geleverd met uw Microsoft Azure StorSimple-oplossing. Het virtuele StorSimple-apparaat wordt uitgevoerd op een virtuele machine in een netwerk met een Microsoft Azure virtual en u kunt deze back-up maken en gegevens van uw hosts klonen. Deze zelfstudie wordt beschreven hoe u implementeren en beheren van een virtueel apparaat in Azure wordt aangegeven en is van toepassing op de virtuele apparaten met softwareversie Update 2- en ondergrens.


#### <a name="virtual-device-model-comparison"></a>Vergelijking van gegevensmodellen virtueel apparaat

Het virtuele apparaat StorSimple is beschikbaar in twee modellen, een standaard 8010 (voorheen bekend als de 1100) en een premium 8020 (geïntroduceerd in Update 2). Een vergelijking van de twee modellen tabelindeling hieronder.


| Apparaatmodel          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Maximale capaciteit**      | 30 TB                                                                     | 64 TB                                                                                                                                |
| **Azure VM**              | Standard_A3 (4 cores, 7 GB geheugen vereist)                                                                      | Standard_DS3 (4 cores, 14 GB geheugen vereist)                                                                                                                          |
| **Versiecompatibiliteit** | Versies bijwerken vooraf 2 of hoger                                             | Update uitvoert, 2 of latere versies                                                                                                  |
| **Beschikbaarheid van de regio**   | Alle Azure regio 's                                                         | Azure regio's die ondersteuning bieden voor Premium-opslag<br></br>Zie voor een lijst met regio's, [regio's voor 8020 die worden ondersteund](#supported-regions-for-8020) |
| **Opslagtype**          | Azure standaardopslag voor lokale schijven wordt gebruikt<br></br> Informatie over het [maken van een standaard-opslag-account]() | Azure Premium opslag voor lokale schijven<sup>2</sup> wordt gebruikt <br></br>Leer hoe u [een Premium-opslag-account maken](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)                                                               |
| **Werkbelasting richtlijnen**     | Item niveau ophalen van bestanden uit de back-ups                                              | Cloud ontwikkelaar en testen van scenario's, lage latentie, hogere prestaties werkbelasting <br></br>Secundaire apparaat voor herstel                                                                                            |
 
<sup>1</sup> *voorheen bekend als de 1100*.

<sup>2</sup> *zowel de 8010 als 8020 gebruik Azure standaardopslag voor de laag cloud. Het verschil is alleen beschikbaar in de lokale laag binnen het apparaat*.

#### <a name="supported-regions-for-8020"></a>Ondersteunde regio's voor 8020

De Premium-opslag regio's die momenteel worden ondersteund voor 8020 zijn onder de tabelindeling. Deze lijst worden continu worden bijgewerkt zodra Premium-opslag in meer regio's beschikbaar. 

| S. geen.                                                  | Momenteel ondersteund in de regio 's |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | Central US                     |
| 2                                                       |  Oost-VS                       |
| 3                                                       |  Oost-Amerikaanse 2                     |
| 4                                                       | West VS                        |
| 5                                                       | Noord-Europa                   |
| 6                                                       | West Europa                    |
| 7                                                       | Zuidoost-Azië                 |
| 8                                                       | Japan Oost                     |
| 9                                                       | Japan West                     |
| 10                                                      | Australië Oost                 |
| 11                                                      | Australië Zuidoost *           |
| 12                                                      | Oost-Azië *                     |
| 13                                                      | Zuid-centraal Amerikaans *              |

* Premium opslag is onlangs in deze geos gestart.

In dit artikel worden de stapsgewijze proces van het implementeren van een virtueel apparaat StorSimple in Azure wordt aangegeven. Lees dit artikel en zal u het volgende doen:

- Begrijpen hoe het virtuele apparaat verschilt van het apparaat.

- Mogelijk te maken en configureren van het virtuele apparaat.

- Verbinding maken met het virtuele apparaat.

- Informatie over het werken met het virtuele apparaat.

Deze zelfstudie geldt voor alle StorSimple virtuele apparaten met Update 2 en hoger. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Hoe het virtuele apparaat verschilt van het apparaat fysiek

Het virtuele apparaat StorSimple is een alleen-software versie van StorSimple die wordt uitgevoerd op één knooppunt in een Microsoft Azure Virtual Machine. Het virtuele apparaat ondersteunt noodgevallen herstel scenario's waarin uw apparaat is niet beschikbaar en geschikt is voor gebruik in itemniveau voor het ophalen van back-ups, herstel van on-premises implementatie en ontwikkelaar en test cloud-scenario's.

#### <a name="differences-from-the-physical-device"></a>Verschillen van het apparaat fysiek

De volgende tabel ziet u enkele belangrijke verschillen tussen de StorSimple virtuele en de StorSimple fysiek apparaat.

|                             | Fysiek apparaat                                          | Virtueel apparaat                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Locatie**                    | Bevindt zich in het datacenter.                               | Wordt uitgevoerd in Azure wordt aangegeven.                                                                            |
| **Netwerkinterfaces**          | Zes netwerkinterfaces heeft: gegevens 0 tot en met 5 van de gegevens.                  | Heeft slechts één netwerkinterface: gegevens 0.                                        |
| **Registratie**                | Tijdens de configuratiestap geregistreerd.                | Registratie is een afzonderlijke taak.                                                          |
| **Sleutel service data-versleuteling** | Klik op het apparaat fysiek genereren en werkt u vervolgens het virtuele apparaat met de nieuwe sleutel.           | Kunnen niet opnieuw genereren van het virtuele apparaat. |


## <a name="prerequisites-for-the-virtual-device"></a>Vereisten voor het virtuele apparaat

De volgende secties wordt uitgelegd dat de configuratie vereisten voor uw StorSimple virtuele apparaat. Controleer voordat u een virtueel apparaat wordt geïmplementeerd, de [Beveiligingsoverwegingen voor het gebruik van een virtueel apparaat](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Azure vereisten

Voordat u het virtuele apparaat inrichten, moet u de volgende voorbereidingen treffen in uw Azure-omgeving:

- Voor het virtuele apparaat, [een virtueel netwerk op Azure configureren](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Als de opslagruimte Premium gebruikt, moet u een virtueel netwerk maken in een Azure gebied die ondersteuning biedt voor Premium opslag. Meer informatie over [regio's die momenteel worden ondersteund voor 8020](#supported-regions-for-8020).
- Het is raadzaam gebruik van de standaard DNS-server van Azure in plaats van de naam van uw eigen DNS-server opgeven. Als de naam van uw DNS-server ongeldig is of als de DNS-server is niet juist IP-adressen oplossen, mislukt het maken van het virtuele apparaat.
- Punt-naar-site en site-naar-site zijn optioneel, maar niet vereist. Als u wilt, kunt u deze opties voor geavanceerdere scenario's kunt configureren. 
- U kunt [Azure virtuele Machines](../virtual-machines/virtual-machines-linux-about.md) (hostservers) in het virtuele netwerk waarmee de hoeveelheden die worden aangeboden door het virtuele apparaat kunt maken. Deze servers moeten voldoen aan de volgende vereisten:                            
    - Windows- of Linux VMs met iSCSI-initiatorsoftware geïnstalleerd zijn.
    - Wordt uitgevoerd in hetzelfde virtuele netwerk als het virtuele apparaat.
    - Mogelijk verbinding maken met het iSCSI-doel van het virtuele apparaat via de interne IP-adres van het virtuele apparaat.

- Zorg ervoor dat u de ondersteuning voor iSCSI en cloud-verkeer is toegestaan op hetzelfde virtuele netwerk hebt ingesteld.


#### <a name="storsimple-requirements"></a>StorSimple vereisten

Controleer de volgende updates met uw service Azure StorSimple voordat u een virtueel apparaat maakt:


- [Access besturingselement records](storsimple-manage-acrs.md) toevoegen voor de VMs die in aanmerking voor host-servers voor uw virtuele apparaat.

- Een [account van opslag](storsimple-manage-storage-accounts.md#add-a-storage-account) in hetzelfde gebied, als het virtuele apparaat gebruiken. Opslag-accounts in verschillende regio's kunnen dit leiden tot slechte prestaties. Met het virtuele apparaat kunt u een standaard- of opslag van de Premium-account. Meer informatie over het maken van een [standaard-opslag-account]() of een [Premium-opslag-account](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)

- Gebruik een andere opslag-account om virtueel apparaat te maken van het account waarmee voor uw gegevens. Met dezelfde opslag account, kan dit leiden tot slechte prestaties.

Controleer of u hebt de volgende informatie voordat u begint:

- Uw Azure klassieke portal account met access-referenties.

- Een kopie van de service gegevens versleutelingssleutel uit uw apparaat.


## <a name="create-and-configure-the-virtual-device"></a>Maken en configureren van het virtuele apparaat

Voordat u deze procedures, zorg dat u de [vereisten voor het virtuele apparaat](#prerequisites-for-the-virtual-device)hebt voldaan. 

Nadat u hebt gemaakt van een virtueel netwerk, een StorSimple Manager-service hebt geconfigureerd en uw fysieke StorSimple apparaat geregistreerd met de service, kunt u de volgende stappen uit om te maken en configureren van een virtueel StorSimple-apparaat. 

### <a name="step-1-create-a-virtual-device"></a>Stap 1: Een virtueel apparaat maken

De volgende stappen als u wilt maken van het virtuele apparaat StorSimple uitvoeren.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Als het maken van het virtuele apparaat in deze stap is mislukt, kan er geen verbinding met Internet. Ga voor meer informatie, problemen met [Internet](#troubleshoot-internet-connectivity-errors) wanneer creatig een virtueel apparaat.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Stap 2: Configureren en het virtuele apparaat registreren

Voordat u deze procedure begint, zorg dat er een kopie van de sleutel gegevens versleuteling. De service gegevenssleutel is gemaakt wanneer u uw eerste StorSimple apparaat hebt geconfigureerd en u wilt opslaan op een veilige locatie is gevraagd. Als u een kopie van de sleutel gegevens versleuteling niet hebt, moet u contact opnemen met Microsoft Support voor hulp.

De volgende stappen uitvoeren om te configureren en uw StorSimple virtuele apparaat registreren.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Stap 3: (Optioneel) wijzigen de configuratie-instellingen van het apparaat

De volgende sectie worden de configuratie-instellingen van het apparaat nodig voor de StorSimple virtueel apparaat als u wilt gebruiken CHAP, StorSimple momentopname Manager of het Apparaatbeheerder-wachtwoord wijzigen.

#### <a name="configure-the-chap-initiator"></a>Het beginpunt CHAP configureren

Deze parameter bevat de referenties die uw virtuele apparaat (doel) van de initiators (servers) die u probeert verwacht voor toegang tot de hoeveelheden. De initiators, vindt u een CHAP-gebruikersnaam en wachtwoord hebben CHAP identificeren zelf aan uw apparaat tijdens deze verificatie. Ga naar het [CHAP configureren voor uw apparaat](storsimple-configure-chap.md#unidirectional-or-one-way-authentication)voor meer informatie.

#### <a name="configure-the-chap-target"></a>Het doel CHAP configureren

Deze parameter bevat de referenties die uw virtuele apparaat wordt gebruikt wanneer een initiator CHAP ingeschakelde onderlinge of bidirectionele verificatie vraagt. Het virtuele apparaat wordt een omgekeerde CHAP gebruikersnaam in te voeren en het wachtwoord omgekeerde CHAP gebruikt om zichzelf te identificeren naar het beginpunt tijdens dit verificatieproces. Houd er rekening mee dat CHAP target instellingen algemene instellingen zijn. Wanneer deze worden toegepast, wordt de hoeveelheden die zijn verbonden met het virtuele opslagapparaat CHAP-verificatie gebruikt. Ga naar het [CHAP configureren voor uw apparaat](storsimple-configure-chap.md#bidirectional-or-mutual-authentication)voor meer informatie.

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Het wachtwoord StorSimple momentopname Manager configureren

StorSimple momentopname Manager-software bevindt zich op uw Windows-host en kunnen beheerders voor het beheren van back-ups van uw apparaat StorSimple in de vorm van lokale en cloud momentopnamen.

>[AZURE.NOTE] Voor het virtuele apparaat is uw Windows-host een Azure virtuele machines.

Tijdens het configureren van een apparaat in de StorSimple momentopname Manager, wordt u gevraagd de StorSimple apparaat IP-adres en wachtwoord om te verifiëren van uw apparaat opslagruimte op te geven. Ga naar [configureren StorSimple momentopname Manager-wachtwoord](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)voor meer informatie.

#### <a name="change-the-device-administrator-password"></a>Het wachtwoord van de beheerder van apparaat wijzigen 

Wanneer u de Windows PowerShell-interface voor toegang tot het virtuele apparaat gebruikt, moet u een wachtwoord van de beheerder apparaat in te voeren. Voor de beveiliging van uw gegevens zijn de verplicht voor het wijzigen van dit wachtwoord voordat het virtuele apparaat kan worden gebruikt. Ga naar [configureren apparaat beheerderswachtwoord](storsimple-change-passwords.md#change-the-device-administrator-password)voor gedetailleerde stappen.

## <a name="connect-remotely-to-the-virtual-device"></a>Extern verbinding maken met het virtuele apparaat
Externe toegang tot uw virtuele apparaat via de Windows PowerShell-interface wordt niet standaard ingeschakeld. U moet eerst extern beheer op het virtuele apparaat inschakelen en vervolgens op de client die wordt gebruikt voor toegang tot uw virtuele apparaat in te schakelen.

Het proces om extern verbinding te wordt hieronder beschreven.

### <a name="step-1-configure-remote-management"></a>Stap 1: Extern beheer configureren

De volgende stappen uitvoeren om te extern beheer voor uw StorSimple virtuele apparaat configureren.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Stap 2: Externe toegang tot het virtuele apparaat

Nadat u extern beheer op de configuratiepagina van StorSimple apparaat hebt ingeschakeld, kunt u Windows PowerShell externe verbinding maken met het virtuele apparaat van een andere VM binnen hetzelfde virtuele netwerk; u kunt bijvoorbeeld uit de host VM die u geconfigureerd en gebruikt om verbinding iSCSI te koppelen. In de meeste implementaties, wordt u al hebt geopend een openbare eindpunt voor toegang tot uw host VM die u gebruiken kunt voor toegang tot het virtuele apparaat.

>[AZURE.WARNING] **Voor een betere beveiliging wordt aangeraden HTTPS gebruiken wanneer u verbinding maakt met de eindpunten en verwijder vervolgens de eindpunten als u klaar bent met uw externe PowerShell-sessie.**

Volg de procedures in [verbinding maken met uw apparaat StorSimple](storsimple-remote-connect.md) voor het instellen van externe toegang voor uw virtuele apparaat.

## <a name="connect-directly-to-the-virtual-device"></a>Sluit rechtstreeks aan op het virtuele apparaat

U kunt ook rechtstreeks met het virtuele apparaat verbinden. Als u verbinding rechtstreeks naar het virtuele apparaat uit een andere computer buiten het virtuele netwerk of buiten de Microsoft Azure-omgeving wilt instellen, moet u aanvullende eindpunten maken, zoals wordt beschreven in de volgende procedure. 

De volgende stappen om te maken van een openbare eindpunt in het virtuele apparaat uitvoeren.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Het is raadzaam om verbinding te maken vanaf een andere virtuele machine binnen hetzelfde virtuele netwerk omdat deze procedure wordt het aantal openbare eindpunten op uw virtuele netwerk beperkt. Wanneer u deze methode gebruikt, kunt u eenvoudig verbinding maken met de virtuele machine via een extern bureaublad-sessie en configureert die virtuele machine voor gebruik zoals u zou doen met een andere Windows-client op een lokale netwerk. U hoeft niet het poortnummer in openbare toevoegen omdat de poort al bekend zijn.

## <a name="work-with-the-storsimple-virtual-device"></a>Werken met het virtuele StorSimple-apparaat

Nu u hebt gemaakt en het virtuele apparaat StorSimple geconfigureerd, bent u klaar om te beginnen met het gebruik ervan. U kunt werken met volume containers, volumes en back-beleid op een virtueel apparaat net zoals u zou op een fysieke StorSimple apparaat doen; de enige verschil is dat u nodig hebt om ervoor te zorgen dat u het virtuele apparaat in de lijst van uw apparaat selecteert. Ga naar [de StorSimple Manager-service voor het beheren van een virtueel apparaat gebruiken](storsimple-manager-service-administration.md) voor stapsgewijze procedures van de verschillende beheertaken voor het virtuele apparaat.

De volgende secties worden enkele van de verschillen die u ondervinden zullen bij het werken met het virtuele apparaat.

### <a name="maintain-a-storsimple-virtual-device"></a>Voor het behoud van een virtueel StorSimple-apparaat

Omdat het een alleen-software-apparaat, worden onderhouden voor het virtuele apparaat minimale in vergelijking met onderhoud voor het apparaat. Hebt u de volgende opties:

- **Software-updates** : U kunt weergeven van de datum waarop de software voor het laatst werd bijgewerkt, samen met een bijwerken statusberichten. De knop **Scan updates** kunt u onder aan de pagina scannen handmatig als u wilt controleren op nieuwe updates. Als updates beschikbaar zijn, klikt u op **Updates installeren** om te installeren. Omdat er slechts één interface op het virtuele apparaat, betekent dit dat er een lichte serviceonderbreking wanneer updates worden toegepast. Het virtuele apparaat afsluiten en opnieuw (indien nodig) eventuele updates die zijn vrijgegeven toepassen. Voor een stapsgewijze procedure, gaat u naar het [bijwerken van uw apparaat](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- **Ondersteuning voor pakket** – u kunt maken en een ondersteuningspakket om Microsoft Support problemen oplossen met het virtuele apparaat te uploaden. Ga naar het [maken en beheren van een ondersteuningspakket met](storsimple-create-manage-support-package.md)voor een stapsgewijze procedure.

### <a name="storage-accounts-for-a-virtual-device"></a>Opslag-accounts voor een virtueel apparaat

Opslag accounts worden gemaakt voor gebruik door de service StorSimple Manager, door het virtuele apparaat en door het apparaat fysiek. Wanneer u uw accounts opslag maakt, is het raadzaam dat u een regio-id in de beschrijvende naam om ervoor te zorgen dat de regio alle systeemonderdelen consistent is gebruikt. Voor een virtueel apparaat is het belangrijk dat alle onderdelen in hetzelfde gebied om te voorkomen dat prestatieproblemen.

Voor een stapsgewijze procedure, gaat u naar [een opslag-account toevoegen](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Een virtueel apparaat StorSimple deactiveren

Het deactiveren van een virtueel apparaat Hiermee verwijdert u de VM en de resources die zijn gemaakt wanneer deze is ingericht. Nadat het virtuele apparaat is gedeactiveerd, kan deze niet worden hersteld naar de vorige status. Voordat u het virtuele apparaat hebt gedeactiveerd, moet u om te stoppen of clients en hosts die afhankelijk van deze zijn verwijderen.

Het deactiveren van een virtueel apparaat resulteert in de volgende bewerkingen uit:

- Het virtuele apparaat wordt verwijderd.

- De OS schijf en gegevensschijven gemaakt voor het virtuele apparaat worden verwijderd.

- De gehoste service en virtuele netwerken die zijn gemaakt tijdens het inrichten worden bewaard. Als u deze niet gebruikt, moet u deze handmatig verwijderen.

- Cloud momentopnamen gemaakt voor het virtuele apparaat worden bewaard.

Voor een stapsgewijze procedure, gaat u naar [deactiveren en verwijderen van uw apparaat StorSimple](storsimple-deactivate-and-delete-device.md).

Zodra het virtuele apparaat wordt weergegeven als op de pagina van de service StorSimple Manager gedeactiveerd, kunt u het virtuele apparaat verwijderen uit lijst met apparaten op de pagina **apparaten** .


### <a name="start-stop-and-restart-a-virtual-device"></a>Starten, stoppen en opnieuw starten van een virtueel apparaat
In tegenstelling tot de fysieke StorSimple-apparaat is het geen power aan of uit om de push op een virtueel apparaat StorSimple power. Maar er zijn mogelijk gelegenheden waar u wilt stoppen en opnieuw starten van het virtuele apparaat. Sommige updates moet bijvoorbeeld dat de VM opnieuw worden gestart om te voltooien het updateproces. De eenvoudigste manier om te starten, stoppen en opnieuw starten een virtueel apparaat is het gebruik van de beheerconsole virtuele Machines.

Wanneer u de beheerconsole bekijkt, is de apparaatstatus is virtual **uitgevoerd** omdat deze al dan niet standaard is gestart nadat deze is gemaakt. U kunt beginnen, stoppen en opnieuw starten van een virtuele machine op elk gewenst moment.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Fabrieksinstellingen herstellen

Als u besluit dat u alleen wilt beginnen met uw virtuele apparaat, te deactiveren en verwijder deze en maakt u een nieuwe record. Net als wanneer uw fysieke apparaat opnieuw wordt ingesteld, hebben uw nieuwe virtuele apparaat geen updates geïnstalleerd. Zorg ervoor dat u controleren op updates voordat u deze gebruikt.


## <a name="fail-over-to-the-virtual-device"></a>Naar het virtuele apparaat mislukken

Herstel (DR) is een van de belangrijke scenario's die voor het virtuele apparaat StorSimple is ontworpen. In dit scenario de fysieke StorSimple apparaat of de hele datacenter mogelijk niet beschikbaar. Gelukkig kunt u een virtueel apparaat herstellen bewerkingen in een andere locatie. Tijdens DR, het volume containers van het bronapparaat eigendom wijzigen en worden overgebracht naar het virtuele apparaat. De vereisten voor DR zijn dat het virtuele apparaat is gemaakt en geconfigureerd, de hoeveelheden binnen de container volume offline is geopend en de container volume een gekoppeld heeft cloud momentopname.

>[AZURE.NOTE] 
>
> - Wanneer u een virtueel apparaat als het secundaire apparaat voor DR, houd er rekening mee dat het 8010 30 TB opslagruimte op standaard kwijt heeft en 8020 64 TB opslagruimte op Premium kwijt heeft. Het hogere capaciteit 8020 virtuele apparaat zijn mogelijk meer geschikt is voor een DR-scenario.
> - U kunt niet failover of klonen vanaf een apparaat met-Update 2 en een apparaat met oude Update 1-software. U kunt echter niet over een apparaat met-Update 2 en een apparaat met Update 1 (1.1 of 1.2)

Voor een stapsgewijze procedure, gaat u naar [een virtueel apparaat](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Afgesloten of het virtuele apparaat verwijderen

Als u eerder hebt geconfigureerd en een virtueel apparaat maar nu wil stoppen met het samenvoegen van berekeningscluster kosten voor het gebruik StorSimple gebruikt, kunt u het virtuele apparaat afsluiten. Het virtuele apparaat afgesloten, houdt dat niet het besturingssysteem of gegevensschijven in opslag verwijdert. Deze kosten die voor uw abonnement op stopt, maar opslag kosten voor de schijven OS en gegevens blijven.

Als u verwijderen of het virtuele apparaat uitschakelt, wordt deze weergegeven als **Offline** op de pagina apparaten van de StorSimple Manager-service. U kunt kiezen om te deactiveren of verwijderen van het apparaat als u ook wilt verwijderen van de back-ups gemaakt door het virtuele apparaat. Zie [deactiveren en een apparaat StorSimple verwijderen](storsimple-deactivate-and-delete-device.md)voor meer informatie.

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Internet connectivity oplossen 

Tijdens het maken van een virtueel apparaat, als er geen verbinding met Internet, is de stap maken, mislukt. Om op te lossen als de fout vanwege internetconnectiviteit is, voert u de volgende stappen uit in de klassieke Azure-portal:

1. Maak een Windows server 2012 virtuele machine in Azure wordt aangegeven. Deze virtuele machine moet hetzelfde opslag-account, VNet en subnet zoals gebruikt door uw virtuele apparaat gebruiken. Als u al een bestaande Windows Server-host in Azure wordt aangegeven met de dezelfde opslag-account, Vnet en subnet, kunt u deze ook gebruiken om op te lossen de internetconnectiviteit.
2. Extern log in de virtuele machine in de vorige stap hebt gemaakt. 
3. Open een opdrachtvenster in de virtuele machine (Win + R en begin te typen `cmd`).
4. Voer de volgende cmd bij de prompt.

    `nslookup windows.net`

5. Als `nslookup` mislukt, en vervolgens Internet connectivity mislukt verhindert het virtuele apparaat registreren bij de StorSimple Manager-service. 
6. Breng de gewenste wijzigingen aan uw virtuele netwerk om ervoor te zorgen dat het virtuele apparaat Azure sites zoals "windows.net" toegang tot is.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van een virtueel apparaat](storsimple-manager-service-administration.md).
 
- Meer informatie over hoe u [een volume StorSimple uit een back-up set herstelt](storsimple-restore-from-backup-set.md). 

