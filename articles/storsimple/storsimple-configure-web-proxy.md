<properties 
   pageTitle="Webproxy voor een StorSimple apparaat instellen | Microsoft Azure"
   description="Leer hoe u met Windows PowerShell voor StorSimple web proxy-instellingen voor uw apparaat StorSimple configureren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-web-proxy-for-your-storsimple-device"></a>Webproxy voor uw apparaat StorSimple configureren

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt beschreven hoe om Windows PowerShell voor StorSimple gebruiken om te configureren en web proxy-instellingen voor uw apparaat StorSimple weergeven. De web-proxy-instellingen worden gebruikt door het apparaat StorSimple bij communicatie met de cloud. Een web-proxyserver wordt gebruikt voor het toevoegen van een andere laag voor waardepapier, filterinhoud, cache bandbreedtevereisten ease of zelfs hulp bij analytics.

Webproxy is een optionele configuratie voor uw apparaat StorSimple. U kunt de webproxy alleen via Windows PowerShell voor StorSimple configureren. De configuratie is een proces als volgt:

1. U web proxy-instellingen via de wizard setup of Windows PowerShell voor StorSimple cmdlets voor het eerst configureren.

2. U vervolgens inschakelen de geconfigureerde web proxy-instellingen via Windows PowerShell voor StorSimple cmdlets.

Wanneer de configuratie van de web-proxy voltooid is, kunt u de geconfigureerde web proxy-instellingen weergeven in de service Microsoft Azure StorSimple Manager en de Windows PowerShell voor StorSimple. 

Na het lezen van deze zelfstudie, wordt u zichtbaar mogen zijn:

- Webproxy configureren met behulp van de wizard setup en de bijbehorende cmdlets
- Webproxy inschakelen met behulp van cmdlets
- Web proxy-instellingen in de Azure klassieke portal weergeven
- Fouten corrigeren tijdens het configureren van de web-proxy


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Webproxy via Windows PowerShell voor StorSimple configureren

U kunt een van de volgende web proxy-instellingen configureren:

- De installatiewizard om u te helpen u bij de volgende configuratiestappen uit.

- Cmdlets voor in Windows PowerShell voor StorSimple.

Elk van de volgende manieren wordt in de volgende secties besproken.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Webproxy via de wizard setup configureren

U kunt de wizard setup helpt u bij de stappen voor het web proxyconfiguratie. Voer de volgende stappen om webproxy te configureren op uw apparaat.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Webproxy via de wizard setup configureren

1. Kies optie 1, **aanmelden met de volledige toegang** en het **apparaat beheerderswachtwoord**opgeven in het menu seriële console. Typ de volgende opdracht uit een sessie van de wizard setup te starten:

    `Invoke-HcsSetupWizard`

2. Als dit de eerste keer dat u de installatiewizard voor apparaatregistratie hebt gebruikt, moet u voor het configureren van de vereiste netwerkinstellingen totdat u bij de configuratie van de web-proxy. Als uw apparaat al is geregistreerd, kunt u de geconfigureerde netwerkinstellingen accepteren totdat u bij de configuratie van de web-proxy. In de installatiewizard wanneer dit wordt gevraagd naar de web proxy-instellingen configureren, typt u **Ja**.

3. Geef het IP-adres of de volledig gekwalificeerde domeinnaam (FQDN) van uw web-proxyserver en het poortnummer in TCP die u uw apparaat dat wilt wilt gebruiken wanneer u communiceren met de cloud voor de **Proxy-Web-URL**. Gebruik de volgende indeling:

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    Standaard is TCP-poortnummer 8080 opgegeven.

4. Kies het verificatietype als **NTLM**, **eenvoudige**of **geen**. Eenvoudige is de minst veilige verificatie voor de configuratie van de proxyserver. NT LAN Manager (NTLM) is een zeer beveiligde en complexe verificatieprotocol dat een drie manieren-berichtensysteem (soms vier als extra integriteit vereist is) gebruikt om een gebruiker te verifiëren. De standaardverificatie is NTLM. Zie [eenvoudige](http://hc.apache.org/httpclient-3.x/authentication.html) en [NTLM-verificatie](http://hc.apache.org/httpclient-3.x/authentication.html)voor meer informatie. 

    > [AZURE.IMPORTANT] **In de service StorSimple Manager, het apparaat controleren grafieken werken niet als eenvoudige of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. Voor de controle grafieken om te werken, moet u om ervoor te zorgen dat de verificatie is ingesteld op geen.**

5. Als u verificatie gebruikt, een **Web Proxy-gebruikersnaam** en een **Web Proxywachtwoord**opgeven. U moet ook het wachtwoord te bevestigen.

    ![Webproxy configureren op StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Als u uw apparaat voor de eerste keer registreert, gaat u verder met de registratie. Als uw apparaat al is geregistreerd, wordt de wizard wordt afgesloten. De geconfigureerde instellingen worden, opgeslagen.

Webproxy worden ook nu ingeschakeld. U kunt het [inschakelen van webproxy](#enable-web-proxy) overslaan en ga direct naar de [weergave web proxy-instellingen in de portal van Azure klassieke](#view-web-proxy-settings-in-the-azure-classic-portal).


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Webproxy via Windows PowerShell voor StorSimple cmdlets configureren

Er is een andere methode voor het web proxy-instellingen configureren via de Windows PowerShell voor StorSimple cmdlets. Voer de volgende stappen uit om webproxy te configureren.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Webproxy via cmdlets configureren

1. Kies optie 1, **aanmelden met de volledige toegang**in het menu seriële console. Wanneer u wordt gevraagd, kunt u het **apparaat beheerderswachtwoord**opgeven. Het standaardwachtwoord is `Password1`.

2. Typ bij de opdrachtprompt:

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    Verstrekt en Bevestig het wachtwoord wanneer u wordt gevraagd, zoals hieronder wordt weergegeven.

    ![Webproxy configureren op StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

De webproxy is nu geconfigureerd en moet worden ingeschakeld.

## <a name="enable-web-proxy"></a>Webproxy inschakelen

Webproxy is standaard uitgeschakeld. Nadat u de web-proxy-instellingen op uw apparaat StorSimple configureren, moet u de Windows PowerShell voor StorSimple gebruiken om te schakelen van de web-proxy-instellingen.

> [AZURE.NOTE] **Deze stap worden niet moeten uitgevoerd als u de wizard setup hebt gebruikt voor het configureren van webproxy. Webproxy is automatisch ingeschakeld al dan niet standaard na een sessie van de wizard setup.**

De volgende stappen uitvoeren in Windows PowerShell voor StorSimple inschakelen van webproxy op uw apparaat:

#### <a name="to-enable-web-proxy"></a>Inschakelen van webproxy

1. Kies optie 1, **aanmelden met de volledige toegang**in het menu seriële console. Wanneer u wordt gevraagd, kunt u het **apparaat beheerderswachtwoord**opgeven. Het standaardwachtwoord is `Password1`.

2. Typ bij de opdrachtprompt:

    `Enable-HcsWebProxy`

    U hebt nu de configuratie van de web-proxy op uw apparaat StorSimple ingeschakeld.

    ![Webproxy configureren op StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Web proxy-instellingen in de Azure klassieke portal weergeven

De web-proxy-instellingen zijn geconfigureerd via de Windows PowerShell-interface en kunnen niet worden gewijzigd in de klassieke portal. U kunt echter deze geconfigureerde instellingen weergeven in de klassieke portal. De volgende stappen om weer te geven van webproxy uitvoeren.

#### <a name="to-view-web-proxy-settings"></a>Web proxy-instellingen weergeven
1. Navigeer naar **StorSimple Manager-service > apparaten**. Selecteer en klik op een apparaat en ga vervolgens naar **configureren**.
1. Schuif omlaag op de pagina **configureren** naar de sectie **Web proxy-instellingen** . U kunt de geconfigureerde web proxy-instellingen weergeven op uw apparaat StorSimple zoals hieronder wordt weergegeven.

    ![Weergave webproxy in Management Portal](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## <a name="errors-during-web-proxy-configuration"></a>Fouten tijdens het configureren van de web-proxy

Als de web-proxy-instellingen zijn onjuist geconfigureerd, wordt foutberichten weergegeven aan de gebruiker in Windows PowerShell voor StorSimple. De volgende tabel worden enkele van deze foutberichten worden weergegeven, de mogelijke oorzaken en aanbevolen acties.

|Seriële Nee.|HRESULT-foutcode|Mogelijke oorzaak|Aanbevolen actie|
|:---|:---|:---|:---|
|1.|0x80070001|Opdracht vanaf de passieve controller wordt uitgevoerd en is niet kunt communiceren met de actieve controller.|De opdracht uitvoeren op de actieve controller uit. Als u wilt de opdracht uitvoeren vanaf de passieve controller, moet u de connectiviteit van passieve met actieve controller oplossen. U moet verrichten van Microsoft Support als deze verbinding verbroken is.|
|2.|0x800710dd - de bewerkings-id is niet geldig|Proxy-instellingen worden niet ondersteund op StorSimple virtueel apparaat.|Proxy-instellingen worden niet ondersteund op StorSimple virtueel apparaat. Dit kunnen alleen worden geconfigureerd op een fysieke StorSimple-apparaat.|
|3.|0x80070057 - ongeldige parameter|Een van de parameters opgegeven voor de proxy-instellingen is niet geldig.|De URI is niet beschikbaar in de juiste indeling. Gebruik de volgende indeling:`http://<IP address or FQDN of the web proxy server>:<TCP port number>`|
|4.|0x800706ba - RPC-server niet beschikbaar|De onderliggende oorzaak is een van de volgende opties:</br></br>Cluster is niet omhoog.</br></br>DataPath service wordt niet uitgevoerd.</br></br>De opdracht vanaf passieve controller wordt uitgevoerd en is niet kunt communiceren met de actieve controller.|Neem deelnemen Microsoft Support om ervoor te zorgen dat het cluster omhoog en datapath service wordt uitgevoerd.</br></br>De opdracht uitvoeren van de actieve controller. Als u de opdracht uitvoeren vanaf de passieve controller wilt, moet u zorgen dat de passieve controller kan communiceren met de actieve controller. U moet verrichten van Microsoft Support als deze verbinding verbroken is.|
|5.|0x800706be - RPC-aanroep is mislukt|Cluster is niet beschikbaar.|Neem deelnemen Microsoft Support om ervoor te zorgen dat het cluster omhoog.|
|6.|0x8007138f - clusterbron is niet gevonden|De bron van het cluster voor platform-service is niet gevonden. Dit kan gebeuren wanneer de installatie niet BEGINLETTERS is.|Mogelijk moet u uitvoeren van een fabriek opnieuw instellen op uw apparaat. Mogelijk moet u een resource platform maakt. Neem contact op met Microsoft Support voor de volgende stappen.|
|7.|0x8007138c - cluster resource niet online|Platform of datapath cluster resources zijn niet online.|Neem contact op met Microsoft Support om ervoor te zorgen dat de service datapath en platform resource online zijn.|

> [AZURE.NOTE] 
> 
> -  De bovenstaande lijst met foutberichten is niet volledig. 
> - Fouten met betrekking tot web proxy-instellingen worden niet weergegeven in de portal voor Azure klassieke in uw StorSimple Manager-service. Als er een probleem met webproxy wanneer de configuratie is voltooid, wordt de apparaatstatus wijzigen naar **Offline** in de klassieke portal. |

## <a name="next-steps"></a>Volgende stappen

- Als u problemen ondervindt bij het implementeren van uw apparaat of web proxy-instellingen configureren, raadpleegt u [problemen oplossen met de implementatie van StorSimple apparaat](storsimple-troubleshoot-deployment.md).

- Meer informatie over het gebruik van de service StorSimple Manager, gaat u naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md).
