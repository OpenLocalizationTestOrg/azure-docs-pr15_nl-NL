<properties 
   pageTitle="Problemen met StorSimple implementatie oplossen | Microsoft Azure"
   description="Wordt beschreven hoe automatisch opsporen en oplossen van fouten die optreden wanneer u eerst StorSimple implementeren."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Problemen met de implementatie van de StorSimple apparaat oplossen

## <a name="overview"></a>Overzicht

In dit artikel bevat nuttige voor probleemoplossing voor uw Microsoft Azure StorSimple-implementatie. Veelvoorkomende problemen, mogelijke oorzaken en aanbevolen stappen waarmee u problemen kunt oplossen die optreden mogelijk bij het configureren van StorSimple beschrijving. Deze informatie geldt voor zowel het StorSimple on-premises implementatie fysiek en de StorSimple virtuele apparaat.

> [AZURE.NOTE] Apparaat-configuraties problemen die u mogelijk betrokken kunnen optreden wanneer u het apparaat voor de eerste keer implementeert, of ze zich later, voordoen kunnen wanneer het apparaat functioneert. In dit artikel bevat informatie over het oplossen van problemen met de eerste keer-implementatie. Om op te lossen een operationele-apparaat, gaat u naar [problemen operationele apparaat oplossen](storsimple-troubleshoot-operational-device.md).

In dit artikel worden de hulpmiddelen voor probleemoplossing StorSimple implementaties ook en vindt u een voorbeeld van stapsgewijze probleemoplossing.

## <a name="first-time-deployment-issues"></a>Problemen met de implementatie van de eerste keer

Als u een probleem ondervindt bij het implementeren van uw apparaat voor de eerste keer, het volgende overwegen:

- Als u een fysiek apparaat oplossen wilt, zorgen dat de hardware is geïnstalleerd en geconfigureerd zoals is beschreven in de [installatie van uw apparaat StorSimple 8100](storsimple-8100-hardware-installation.md) of [uw apparaat StorSimple 8600](storsimple-8600-hardware-installation.md).
- Controleer de vereisten voor implementatie. Zorg ervoor dat u alle gegevens die worden beschreven in de [implementatie, Configuratiecontrolelijst](storsimple-deployment-walkthrough.md#deployment-configuration-checklist)hebben.
- Bekijk de releaseopmerkingen StorSimple om te zien als het probleem wordt beschreven. De releaseopmerkingen zoals tijdelijke oplossingen voor bekende installatieproblemen. 

Tijdens de implementatie van apparaat problemen de meest voorkomende die gebruikers kunnen nominale optreden wanneer ze de wizard setup uitvoert en wanneer ze het apparaat via Windows PowerShell registreren voor StorSimple. (U kunt Windows PowerShell voor StorSimple registreren en configureren van uw apparaat StorSimple. Zie voor meer informatie over apparaatregistratie, [stap 3: configureren en registreren van uw apparaat via Windows PowerShell voor StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

De volgende secties kunt u oplossen die optreden wanneer u het apparaat StorSimple voor de eerste keer configureren.

## <a name="first-time-setup-wizard-process"></a>Eerst installatieprocedure voor wizard

Het proces van de wizard setup samenvatting maken van de volgende stappen uit. Zie voor gedetailleerde informatie, [Deploy uw on-premises implementatie StorSimple-apparaat](storsimple-deployment-walkthrough.md).

1. De cmdlet [Roep HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) Start de wizard setup die u bij de overige stappen begeleidt uitvoeren. 
2. Het netwerk configureren: de wizard setup kunt u netwerkinstellingen voor de gegevens 0 network interface configureren op uw apparaat StorSimple. Deze instellingen zijn:
  - Virtuele IP-(VIP), subnet invoermasker en gateway-de cmdlet [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) op de achtergrond wordt uitgevoerd. Deze configureert het IP-adres, subnet invoermasker, en gateway voor de interface van gegevens 0 netwerk op uw apparaat StorSimple.
  - Primaire DNS-server – de cmdlet [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) wordt uitgevoerd op de achtergrond. De DNS-instellingen voor uw oplossing StorSimple configureren
  - NTP-server – de cmdlet [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) wordt uitgevoerd op de achtergrond. De serverinstellingen NTP voor uw oplossing StorSimple configureren
  - Optionele webproxy – de cmdlet [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) wordt uitgevoerd op de achtergrond. Dit wordt ingesteld en de configuratie van de web-proxy voor uw oplossing StorSimple ingeschakeld.
3. De wachtwoorden instellen: de volgende stap is voor het instellen van apparaatbeheerder en StorSimple momentopname Manager wachtwoorden opnieuw instellen. Als u een Update 1 uitvoert, zijn vervolgens niet moet u voor het instellen van het StorSimple momentopname Manager-wachtwoord.
  - Het apparaat beheerderswachtwoord wordt gebruikt voor het aanmelden bij uw apparaat. Het wachtwoord van het apparaat is **Wachtwoord1**.
  - Het wachtwoord StorSimple momentopname Manager is vereist als u een apparaat met StorSimple momentopname Manager configureren. U moet eerst het wachtwoord instellen in de installatiewizard en vervolgens kunt u instellen en wijzig deze van de StorSimple Manager-service. Dit wachtwoord wordt geverifieerd door het apparaat met StorSimple momentopname Manager.
 
    > [AZURE.IMPORTANT] Wachtwoorden zijn verzameld vóór registratie, maar alleen nadat u het apparaat dat is geregistreerd toegepast. Als er een fout is met een wachtwoord wilt toepassen, wordt u gevraagd het wachtwoord nogmaals leveren totdat de vereiste wachtwoorden (die voldoen aan de complexiteitsvereisten) zijn verzameld.

4. Het apparaat hebt geregistreerd: de laatste stap is het apparaat registreren bij de service StorSimple Manager wordt uitgevoerd in Microsoft Azure. De registratie, moet u om [de sleutel voor de registratie](storsimple-manage-service.md#get-the-service-registration-key) van de Azure klassieke portal en deze in de installatiewizard te bieden. Nadat het apparaat dat is geregistreerd, is een service gegevens versleutelingssleutel voor u beschikbaar. Zorg ervoor dat deze toets versleuteling houden op een veilige locatie, omdat deze uitgevoerd moeten worden op alle volgende apparaten met de service registreren.

## <a name="common-errors-during-device-deployment"></a>Veelvoorkomende fouten tijdens de implementatie van apparaat

De volgende tabellen bevatten de veelvoorkomende fouten die wanneer optreden kunnen u:

- De vereiste netwerkinstellingen configureren.
- De optioneel web proxy-instellingen configureren.
- De apparaatbeheerder van het en StorSimple momentopname Manager wachtwoorden instellen. 
- Het apparaat registreren. 

## <a name="errors-during-the-required-network-settings"></a>Fouten tijdens de vereiste netwerkinstellingen

| Nee.| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
| ---| ------------- | --------------- | ------------------ |
| 1  | Roepen-HcsSetupWizard: Deze opdracht kan alleen worden uitgevoerd op de actieve controller uit. | Configuratie is die wordt uitgevoerd op de controller uit passieve.| Deze opdracht uitvoeren van de actieve controller. Zie [identificeren een actieve controller op uw apparaat](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device)voor meer informatie.|
| 2 | Roepen-HcsSetupWizard: Apparaat niet gereed. | Er zijn problemen met de netwerkverbinding op gegevens 0.| Controleer de fysieke netwerkverbinding op gegevens 0.|
| 3 | Roepen-HcsSetupWizard: Er is een IP-adresconflict met een ander systeem in het netwerk (uitzondering van HRESULT: 0x80070263). | De opgegeven voor gegevens 0 IP wordt al gebruikt door een ander systeem. | Geef een nieuwe IP niet wordt gebruikt.|
| 4 | Roepen-HcsSetupWizard: Een resource cluster is mislukt. (Uitzondering van HRESULT: 0x800713AE). | Dubbele VIP. De opgegeven IP is al in gebruik.| Geef een nieuwe IP niet wordt gebruikt.|
| 5 | Roepen-HcsSetupWizard: Ongeldige IPv4-adres. | Het IP-adres is opgegeven in een onjuiste indeling.| Controleer de indeling en uw IP-adres opnieuw leveren. Zie voor meer informatie [IPv4-adressering][1]. |
| 6 | Roepen-HcsSetupWizard: Ongeldige IPv6-adres. | Het IP-adres is opgegeven in een onjuiste indeling.| Controleer de indeling en uw IP-adres opnieuw leveren. Zie voor meer informatie, [IPv6-adressering][2].|
| 7 | Roepen-HcsSetupWizard: Er zijn geen eindpunten meer van de eindpunttoewijzing. (Uitzondering van HRESULT: 0x800706D9) | De functionaliteit voor cluster werkt niet. | [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor de volgende stappen.

## <a name="errors-during-the-optional-web-proxy-settings"></a>Fouten tijdens de optioneel web proxy-instellingen

| Nee.| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
| ---| ------------- | --------------- | ------------------ |
| 1  | Roepen-HcsSetupWizard: Ongeldige parameter (uitzondering van HRESULT: 0x80070057) | Een van de parameters opgegeven voor de proxy-instellingen is niet geldig.| De URI is niet beschikbaar in de juiste indeling. Gebruik de volgende indeling: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 | Roepen-HcsSetupWizard: RPC-server niet beschikbaar (uitzondering van HRESULT: 0x800706ba) | De onderliggende oorzaak is een van de volgende opties:<ol><li>Het cluster is afgerond.</li><li>De passieve controller niet kunt communiceren met de actieve controller en de opdracht uit passieve controller wordt uitgevoerd.</li></ol> | Afhankelijk van de onderliggende oorzaak:<ol><li>[Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om ervoor te zorgen dat het cluster omhoog is.</li><li>De opdracht uitvoeren van de actieve controller. Als u de opdracht uitvoeren vanaf de passieve controller wilt, moet u om ervoor te zorgen dat de passieve controller met de actieve controller communiceren kan. U moet [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) als deze verbinding verbroken is.</li></ol> |
| 3 | Roepen-HcsSetupWizard: RPC-aanroep is mislukt (uitzondering van HRESULT: 0x800706be) | Cluster is niet beschikbaar. | [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om ervoor te zorgen dat het cluster omhoog is.|
| 4 | Roepen-HcsSetupWizard: Cluster bron is niet gevonden (uitzondering van HRESULT: 0x8007138f) | De resource cluster is niet gevonden. Dit kan gebeuren wanneer de installatie niet juist is. | Mogelijk moet u het apparaat opnieuw naar de standaardinstellingen. [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor het maken van een resource cluster.|
| 5 | Roepen-HcsSetupWizard: Cluster resource niet online (uitzondering van HRESULT: 0x8007138c)| Cluster resources zijn niet online. | [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor de volgende stappen.|

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Fouten met betrekking tot apparaatbeheerder en StorSimple momentopname Manager wachtwoorden opnieuw instellen

Het beheerderswachtwoord voor standaard-apparaat is **Wachtwoord1**. Dit wachtwoord verloopt na de eerste keer aanmelden; u moet dus, gebruik de installatiewizard om deze te wijzigen. Wanneer u het apparaat hebt geregistreerd voor de eerste keer, moet u een nieuwe apparaat beheerderswachtwoord opgeven. 

Als u de StorSimple momentopname Manager-software die is uitgevoerd op de Windows Server-host gebruikt voor het beheren van het apparaat, moet u ook een wachtwoord StorSimple momentopname Manager tijdens de registratie van de eerste keer opgeven. 

Zorg ervoor dat uw wachtwoorden voldoet aan de volgende vereisten:

- Uw beheerderswachtwoord apparaat moet liggen tussen 8 en 15 tekens bevatten.
- Uw wachtwoord StorSimple momentopname Manager moet 14 of 15 tekens lang zijn.
- Wachtwoorden 3 van de volgende 4 tekentypen moeten bevatten: kleine letters, hoofdletters, numerieke en speciale. 
- Uw wachtwoord mogen niet hetzelfde als de laatste 24 wachtwoorden.

Daarnaast Houd er rekening mee dat wachtwoorden verlopen van elk jaar en kunnen worden gewijzigd nadat u het apparaat dat is geregistreerd. Als de registratie om welke reden dan ook is mislukt, wordt de wachtwoorden niet worden gewijzigd. Voor meer informatie over apparaatbeheerder en StorSimple momentopname Manager wachtwoorden opnieuw instellen, gaat u naar [gebruiken de StorSimple Manager-service StorSimple wachtwoorden wijzigen](storsimple-change-passwords.md).

U kunt een of meer van de volgende fouten kan optreden bij het instellen van de apparaatbeheerder van het en StorSimple momentopname Manager wachtwoorden opnieuw instellen.

| Nee.| Foutbericht | Aanbevolen actie |
| ---| ------------- | ------------------ | 
| 1  | Het wachtwoord overschrijdt de maximale lengte. |Typ een wachtwoord dat aan deze vereisten voldoet:<ul><li>Uw beheerderswachtwoord apparaat moet liggen tussen 8 en 15 tekens bevatten.</li><li>Uw wachtwoord StorSimple momentopname Manager moet 14 of 15 tekens lang zijn.</li></ul> | 
| 2 | Het wachtwoord voldoet niet aan de vereiste lengte. | Typ een wachtwoord dat aan deze vereisten voldoet:<ul><li>Uw beheerderswachtwoord apparaat moet liggen tussen 8 en 15 tekens bevatten.</li><li>Uw wachtwoord StorSimple momentopname Manager moet 14 of 15 tekens lang zijn.</lu></ul> |
| 3 | Het wachtwoord moet kleine letters tekens bevatten. | Wachtwoorden 3 van de volgende 4 tekentypen moeten bevatten: kleine letters, hoofdletters, numerieke en speciale. Zorg ervoor dat uw wachtwoord aan deze vereisten voldoet. |
| 4 | Het wachtwoord moet numerieke tekens bevatten. | Wachtwoorden 3 van de volgende 4 tekentypen moeten bevatten: kleine letters, hoofdletters, numerieke en speciale. Zorg ervoor dat uw wachtwoord aan deze vereisten voldoet. |
| 5 | Het wachtwoord moet speciale tekens bevatten. | Wachtwoorden 3 van de volgende 4 tekentypen moeten bevatten: kleine letters, hoofdletters, numerieke en speciale. Zorg ervoor dat uw wachtwoord aan deze vereisten voldoet. |
| 6 | Het wachtwoord moet 3 van de volgende tekentypen met 4 bevatten: hoofdletters, kleine letters, cijfers en speciale. | Uw wachtwoord bevat geen de vereiste soorten tekens. Zorg ervoor dat uw wachtwoord aan deze vereisten voldoet. |
| 7 | Parameter niet overeenkomen met bevestiging. | Zorg ervoor dat uw wachtwoord aan alle vereisten voldoet en u deze juist ingevoerd. |
| 8 | Uw wachtwoord kan niet overeenkomen met de standaardwaarde. | Het standaardwachtwoord is *Wachtwoord1*. U moet dit wachtwoord wijzigen nadat u zich voor de eerste keer hebt aangemeld. |
| 9 | Het wachtwoord die u hebt ingevoerd, komt niet overeen met het wachtwoord van het apparaat. Typ het wachtwoord in. | Het wachtwoord en typ het nogmaals. |

Wachtwoorden zijn verzameld voordat het apparaat dat is geregistreerd, maar alleen na registratie voltooid worden toegepast. De werkstroom voor het herstellen van wachtwoord is vereist voor het apparaat te registreren. 

> [AZURE.IMPORTANT] In het algemeen, als een poging tot het toepassen van een wachtwoord is mislukt, probeert klikt u vervolgens de software herhaaldelijk voor het verzamelen van het wachtwoord totdat deze gelukt is. In uitzonderlijke gevallen kan niet het wachtwoord worden toegepast. In dit geval kunt u het apparaat hebt geregistreerd en gaan echter de wachtwoorden wordt niet gewijzigd. U ontvangt geen aanwijzingen welke wachtwoord is niet gewijzigd, het wachtwoord van het apparaat-beheerder of het wachtwoord StorSimple momentopname Manager. Als deze situatie kan voorkomen, kunt u beide wachtwoorden wijzigen.

U kunt de wachtwoorden in de portal van Azure klassieke via de StorSimple Manager-service opnieuw instellen. Ga voor meer informatie naar: 

- [Het apparaat beheerder-wachtwoord wijzigen](storsimple-change-passwords.md#change-the-device-administrator-password).
- [Het StorSimple momentopname Manager-wachtwoord wijzigen](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Fouten tijdens de apparaatregistratie

De service StorSimple Manager wordt uitgevoerd in Microsoft Azure kunt u het apparaat te registreren. Er kan een of meer van de volgende problemen optreden tijdens de apparaatregistratie.

| Nee.| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
| ---| ------------- | --------------- | ------------------ |
| 1  | Fout 350027: Kan niet het apparaat met de StorSimple Manager registreren. |  | Wacht enkele minuten en probeer het opnieuw. Als het probleem zich blijft voordoen, [neemt u contact Microsoft ondersteuning](storsimple-contact-microsoft-support.md).|
| 2  | Fout 350013: Er is een fout opgetreden bij het registreren van het apparaat. Dit kan zijn vanwege onjuiste service registratiegegevens sleutel. | | Registreer het apparaat opnieuw met de juiste service registratie-toets. Zie voor meer informatie [krijgen van de sleutel voor de registratie.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 | Fout 350063: Verificatie StorSimple Manager service doorgegeven maar registratie is mislukt. Probeer de bewerking na het enige tijd. | Deze fout wordt aangegeven dat verificatie met ACS is verstreken, maar de register-oproep aangebracht in de service is mislukt. Dit kan als resultaat een enkel geval netwerk storing zijn. | Als het probleem zich blijft voordoen, neemt u [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md). |
| 4 | Fout 350049: De service kan niet worden bereikt tijdens de registratie. | Wanneer de oproep wordt uitgevoerd naar de service, ontvangen een Webuitzondering. In sommige gevallen, kan dit krijgen opgelost met het opnieuw later. | Controleer uw IP-adres en de naam van de DNS- en probeer de bewerking. Als het probleem zich blijft voordoen, [contact op met Microsoft Support.](storsimple-contact-microsoft-support.md) | 
| 5 | Fout 350031: Het apparaat is al geregistreerd. | | Geen actie nodig. |
| 6 | Fout 350016: Apparaatregistratie is mislukt. | |Zorg ervoor dat de registratie-sleutel correct is. |
| 7 | Roepen-HcsSetupWizard: Een fout opgetreden tijdens het registreren van uw apparaat; Dit kan zijn vanwege onjuist IP-adres of DNS-naam. Controleer uw netwerkinstellingen en probeer het opnieuw. Als het probleem zich blijft voordoen, [neemt u contact Microsoft ondersteuning](storsimple-contact-microsoft-support.md). (Fout 350050) | Zorg ervoor dat uw apparaat het externe netwerk kunt ping. Als u geen verbinding met extern netwerk, wordt de registratie kan mislukken met deze fout. Deze fout mogelijk een combinatie van een of meer van de volgende opties:<ul><li>Onjuiste IP</li><li>Onjuiste subnet</li><li>Onjuiste gateway</li><li>Onjuiste DNS-instellingen</li></ul> | Zie de stappen in het [voorbeeld van stapsgewijze probleemoplossing](#step-by-step-storsimple-troubleshooting-example). |
| 8 | Roepen-HcsSetupWizard: De huidige bewerking is mislukt vanwege een interne servicefout [0x1FBE2]. Probeer de bewerking later opnieuw. Als het probleem zich blijft voordoen, neemt u contact op met Microsoft Support. | Dit is een algemene fout gegenereerd voor alle gebruikers onzichtbare fouten in agent of service. De meest voorkomende reden is mogelijk dat de ACS-verificatie is mislukt. Een mogelijke oorzaak voor de fout is dat er problemen met de configuratie van de NTP-server zijn en tijd op het apparaat niet juist is ingesteld. | Corrigeer de tijd (als er problemen zijn) en probeer de registratie-bewerking. Als u de opdracht Set-HcsSystem - Tijdzoneverschil gebruikt om aan te passen de tijdzone, beginhoofdletter in de tijdzone (bijvoorbeeld "Pacific Standard Time").  Als dit probleem zich blijft voordoen, de [contactpersoon van Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor volgende stappen. |
| 9 | Waarschuwing: Kan het apparaat niet activeren. De apparaatbeheerder van uw en StorSimple momentopname Manager wachtwoorden zijn niet gewijzigd. | Als de registratie mislukt, worden de apparaatbeheerder van het en StorSimple momentopname Manager wachtwoorden niet gewijzigd. |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Hulpprogramma's voor probleemoplossing StorSimple-implementaties

StorSimple bevat verschillende hulpprogramma's die u gebruiken kunt om uw oplossing StorSimple. Hierbij:

- Apparaat-logboeken en ondersteuningspakketten 
- Cmdlets voor die speciaal zijn ontworpen voor probleemoplossing 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Ondersteuning voor pakketten en apparaat logboeken beschikbaar voor probleemoplossing

Een ondersteuningspakket bevat alle relevante logboeken die het team van Microsoft Support bij het oplossen van problemen met apparaat kunnen helpen. Windows PowerShell voor StorSimple kunt u een versleutelde ondersteuningspakket dat u vervolgens met ondersteuningspersoneel delen kunt genereren.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>De logboeken of de inhoud van het ondersteuningspakket weergeven

1. Windows PowerShell voor StorSimple gebruiken om u te genereren van een ondersteuningspakket, zoals beschreven in [maken en beheren van een ondersteuningspakket](storsimple-create-manage-support-package.md).

2. Download het [decoderen script](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokaal op de clientcomputer.

3. Gebruik deze [Stapsgewijze procedure](storsimple-create-manage-support-package.md#edit-a-support-package) om te openen en ontsleutelen van het ondersteuningspakket.

4. De logboeken ontsleuteld ondersteuning-pakket hebben etw/etvx-indeling. U kunt de volgende stappen uit als u wilt deze bestanden weergeven in Windows-Logboeken uitvoeren:
  1. De opdracht **eventvwr** uitvoeren op uw Windows-client. Hiermee start u de logboeken.
  2. Klik in het deelvenster **Acties** op **Opgeslagen logboek openen** en wijst u de logboekbestanden in etvx/etw-indeling (het ondersteuningspakket). U kunt nu het bestand bekijken. Nadat u het bestand hebt geopend, kunt u met de rechtermuisknop op en sla het bestand als tekst.
   
    > [AZURE.IMPORTANT] U kunt ook de cmdlet **Get-WinEvent** deze bestanden openen in Windows PowerShell gebruiken. Zie [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) in de Windows PowerShell-cmdlet verwijzing documentatie voor meer informatie.

5. Wanneer de logboeken in Logboeken opent, zoekt u de volgende logboeken die met betrekking tot de configuratie van het apparaat bevatten:

  - hcs_pfconfig/operationele Log
  - hcs_pfconfig/Config

6. De logboekbestanden, zoek naar tekenreeksen met betrekking tot de cmdlets aangeroepen door de installatiewizard. Zie [eerst wizard installatieprocedure](#first-time-setup-wizard-process) voor een overzicht van deze cmdlets. 

7. Als u niet kunt om te bepalen de oorzaak van het probleem, kunt u [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor de volgende stappen. Voer de stappen in [een verzoek voor ondersteuning maken](storsimple-contact-microsoft-support.md#create-a-support-request) wanneer u contact opnemen met Microsoft Support voor hulp.

## <a name="cmdlets-available-for-troubleshooting"></a>Cmdlets voor die beschikbaar zijn voor probleemoplossing

Gebruik de volgende Windows PowerShell-cmdlets om te bepalen connectivity fouten.

- `Get-NetAdapter`: Gebruik deze cmdlet om te bepalen de status van netwerkinterfaces. 

- `Test-Connection`: Gebruik deze cmdlet om te controleren van de netwerkverbinding binnen en buiten het netwerk.

- `Test-HcsmConnection`: Gebruik deze cmdlet om te controleren van de verbinding van een geregistreerd apparaat.

Als u Update 1 op uw apparaat StorSimple worden uitgevoerd, zijn er ook de volgende cmdlets uit diagnostische gegevens beschikbaar.

- `Sync-HcsTime`: Gebruik deze cmdlet voor het weergeven van apparaat en een tijdsynchronisatie met de server NTP afdwingen.

- `Enable-HcsPing`en `Disable-HcsPing`: gebruik van deze cmdlets toe te staan dat de hosts naar de netwerkinterfaces op uw apparaat StorSimple ping. Standaard reageert de StorSimple netwerk-interfaces niet op ping-verzoeken.

- `Trace-HcsRoute`: Gebruik deze cmdlet als een hulpmiddel voor het traceren van route. Deze pakketten verzendt naar elke router voor de manier waarop naar een uiteindelijke bestemming gedurende een periode en vervolgens expressie resultaten op basis van de pakketten geretourneerd uit elke hop wordt berekend. Aangezien `Trace-HcsRoute` ziet u de mate van pakketverlies op elke router en de koppeling, kunt u welke routers pinpoint of koppelingen veroorzaakt problemen met het netwerk. 

- `Get-HcsRoutingTable`: Gebruik deze cmdlet om weer te geven van de lokale IP-mailroutering tabel.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Problemen met de cmdlet Get-NetAdapter

Als u netwerkinterfaces voor de implementatie van een apparaat van de eerste keer configureert, is de hardwarestatus niet beschikbaar in de StorSimple Manager-service UI omdat het apparaat dat is nog niet geregistreerd met de service. Bovendien de Hardware statuspagina mogelijk niet altijd overeenkomen met de status van het apparaat, vooral als er zijn problemen die betrekking hebben op service-synchronisatie. In deze situaties, kunt u de `Get-NetAdapter` cmdlet om te bepalen de gezondheid en de status van uw netwerkinterfaces.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Voor een overzicht van alle netwerkadapters op uw apparaat

1. Windows PowerShell voor StorSimple starten en typ vervolgens `Get-NetAdapter`. 

2. De uitvoer van gebruiken de `Get-NetAdapter` cmdlet en de volgende richtlijnen voor meer informatie over de status van uw netwerkinterface.
  - Als de interface orde en is ingeschakeld, wordt de **ifIndex** -status wordt weergegeven als **Omhoog**.
  - Als de interface in orde is, maar niet (via een netwerkkabel verbonden is), wordt de **ifIndex** weergegeven als **uitgeschakeld**.
  - Als de interface correct, maar niet ingeschakeld, wordt de **ifIndex** -status wordt weergegeven als **NotPresent**.
  - Als de interface niet bestaat, wordt deze niet weergegeven in deze lijst. Deze interface wordt nog steeds door de service StorSimple Manager UI defect worden weergegeven.

Ga naar [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) in een verwijzing naar de Windows PowerShell-cmdlets voor meer informatie over het gebruik van deze cmdlet. 

De volgende secties worden voorbeelden van uitvoer van de `Get-NetAdapter` cmdlet. 

 In deze voorbeelden, controller 0 is de passieve controller en is geconfigureerd als volgt:

- GEGEVENS 0, DATA 1, 2 van de gegevens en gegevens 3 netwerk interfaces bestaan op het apparaat.
- GEGEVENS 4 en 5 gegevens netwerk interface kaarten zijn niet aanwezig is; Deze worden daarom niet weergegeven in de uitvoer.
- GEGEVENS 0 is ingeschakeld.

Controller 1 is de actieve controller en is geconfigureerd als volgt:

- GEGEVENS 0, DATA 1, 2 van de gegevens, gegevens 3, 4 van de gegevens en gegevens-5-netwerk interfaces bestaan op het apparaat.
- GEGEVENS 0 is ingeschakeld.

**Voorbeeld uitvoer – controller 0**

Hieronder volgt de uitvoer van controller 0 (de passieve controller). GEGEVENS 1, gegevens 2 en 3 van de gegevens hebt geen verbinding. GEGEVENS 4 en 5 van de gegevens worden niet weergegeven omdat deze niet op het apparaat aanwezig zijn. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Voorbeeld uitvoer – controller 1**

Hieronder volgt de uitvoer van controller 1 (de actieve controller). Alleen de gegevens 0 netwerkinterface op het apparaat is geconfigureerd en een werken.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent

 
## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Problemen met de cmdlet verbinding testen

U kunt de `Test-Connection` cmdlet om te bepalen of uw apparaat StorSimple verbinding met het externe netwerk maken kunt. Als alle netwerken parameters, met inbegrip van de DNS-records, correct zijn geconfigureerd in de installatiewizard, kunt u de `Test-Connection` te102826339-cmdlet ping een bekend adres buiten het netwerk, zoals outlook.com. 

Ping connectivity problemen oplossen met deze cmdlet als ping is uitgeschakeld, moet u ook weer inschakelen.

Zie de volgende voorbeelden van uitvoer van de `Test-Connection` cmdlet. 

> [AZURE.NOTE] In de eerste steekproef, is het apparaat geconfigureerd met een onjuiste DNS-server. In het tweede voorbeeld heeft is de DNS-records correct.
 
**Voorbeeld uitvoer – onjuiste DNS**

In het onderstaande voorbeeld is er geen uitvoer voor het IPV4 en IPV6-adressen, waarmee wordt aangegeven dat de DNS-records niet is opgelost. Dit betekent dat er geen verbinding met het externe netwerk is en een juiste DNS moet worden opgegeven. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Voorbeeld van de uitvoer – DNS corrigeren**

In het volgende voorbeeld wordt de DNS-records geeft als resultaat de IPv4-adres, dat aangeeft dat de DNS-records correct is geconfigureerd. Dit bevestigt dat er verbinding met het externe netwerk is. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Problemen met de cmdlet Test-HcsmConnection

Gebruik de `Test-HcsmConnection` cmdlet voor apparaten die al is verbonden met en geregistreerd bij uw Manager StorSimple-service. Deze cmdlet kunt u controleren of de connectiviteit tussen een geregistreerde apparaat en de bijbehorende StorSimple Manager-service. U kunt deze opdracht uitvoeren op Windows PowerShell voor StorSimple. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>De Test-HcsmConnection-cmdlet uitvoeren

1. Zorg ervoor dat het apparaat dat is geregistreerd.

2. Controleer de apparaatstatus. Als het apparaat is gedeactiveerd, in de onderhoudsmodus voor, of offline zetten, ziet u mogelijk de volgende fouten: 

   - ErrorCode.CiSDeviceDecommissioned – Hiermee wordt aangeduid dat het apparaat is gedeactiveerd.
   - ErrorCode.DeviceNotReady – Hiermee wordt aangeduid dat het apparaat in de onderhoudsmodus voor.
   - ErrorCode.DeviceNotReady – Hiermee wordt aangeduid dat het apparaat niet online.

3. Controleer of de StorSimple Manager-service is uitgevoerd (Gebruik de cmdlet [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) ). Als de service niet actief is, ziet u mogelijk de volgende fouten:

   - ErrorCode.CiSApplianceAgentNotOnline
   - ErrorCode.CisPowershellScriptHcsError – Hiermee wordt aangegeven dat er een uitzondering opgetreden is bij het uitvoeren van Get-ClusterResource.

4. Controleer het token Access Control Service (ACS). Als deze een Webuitzondering genereert, is het mogelijk het resultaat van een gateway-probleem, een ontbrekende proxyverificatie, een onjuiste DNS-server of een verificatie mislukt. Mogelijk ziet u de volgende fouten:

   - ErrorCode.CiSApplianceGateway – Hiermee wordt een uitzondering HttpStatusCode.BadGateway aangeduid: de naam resolvercache-service kan niet is verholpen nadat de hostnaam. 
   - ErrorCode.CiSApplianceProxy – Hiermee wordt een uitzondering HttpStatusCode.ProxyAuthenticationRequired (HTTP-statuscode 407) aangeduid: de client kan niet worden geverifieerd door de proxyserver. 
   - ErrorCode.CiSApplianceDNSError – Hiermee wordt een uitzondering WebExceptionStatus.NameResolutionFailure aangeduid: de naam resolvercache-service kan niet is verholpen nadat de hostnaam.
   - ErrorCode.CiSApplianceACSError – Hiermee wordt aangeduid dat de service heeft een verificatiefout geretourneerd, maar er verbinding is.
   
    Als dit een Webuitzondering niet genereren is, schakelt u voor ErrorCode.CiSApplianceFailure. Hiermee wordt aangeduid dat het toestel is mislukt.

5. Controleer de verbinding van de cloud-service. Als de service een Webuitzondering genereert, ziet u mogelijk de volgende fouten:

  - ErrorCode.CiSApplianceGateway – Hiermee wordt een uitzondering HttpStatusCode.BadGateway aangeduid: een tussenliggende proxyserver een ongeldige aanvraag van een andere proxy of van de oorspronkelijke server ontvangen.
  - ErrorCode.CiSApplianceProxy – Hiermee wordt een uitzondering HttpStatusCode.ProxyAuthenticationRequired (HTTP-statuscode 407) aangeduid: de client kan niet worden geverifieerd door de proxyserver. 
  - ErrorCode.CiSApplianceDNSError – Hiermee wordt een uitzondering WebExceptionStatus.NameResolutionFailure aangeduid: de naam resolvercache-service kan niet is verholpen nadat de hostnaam.
  - ErrorCode.CiSApplianceACSError – Hiermee wordt aangeduid dat de service heeft een verificatiefout geretourneerd, maar er verbinding is.
  
    Als dit een Webuitzondering niet genereren is, schakelt u voor ErrorCode.CiSApplianceSaasServiceError. Hiermee wordt aangeduid met een probleem met de StorSimple Manager-service.
 
6. Controleer de verbinding van Azure Service Bus. ErrorCode.CiSApplianceServiceBusError geeft aan dat het apparaat geen verbinding met de Service-Bus maken.
 
De logboekbestanden CiSCommandletLog0Curr.errlog en CiSAgentsvc0Curr.errlog wordt bevatten meer informatie, zoals uitzonderingsdetails. 

Verwijzen naar documentatie voor meer informatie over het gebruik van de cmdlet, gaat u naar [Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) in de Windows PowerShell.

> [AZURE.IMPORTANT] U kunt deze cmdlet voor zowel het actieve en de passieve controller uitvoeren. 
 
Zie de volgende voorbeelden van uitvoer van de `Test-HcsmConnection` cmdlet. 

**Voorbeeld van de uitvoer – geregistreerd apparaat uitgevoerd StorSimple Release (juli 2014)**

De eerste steekproef afkomstig is van een apparaat dat is geregistreerd met de service StorSimple Manager en heeft geen verbindingsproblemen. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Voorbeeld van uitvoer – geregistreerd apparaat met StorSimple Update 1**

Als u Update 1 op uw apparaat StorSimple uitvoert, moet u niet met de uitgebreide schakeloptie worden uitgevoerd.

      Controller1>Test-HcsmConnection
       
      Checking device registration state  ... Success
      Device registered successfully
       
      Checking primary NTP server [time.windows.com] ... Success
       
      Checking web proxy  ... NOT SET
       
      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET
       
      Checking device online  ... Success
 
      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success
       
      Checking connectivity from device to service  ... This will take a few minutes.
       
      Checking connectivity from device to service  ... Success
       
      Checking connectivity from service to device  ... Success
       
      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Voorbeeld van de uitvoer – offline apparaat uitgevoerd StorSimple Release (juli 2014)**

In dit voorbeeld is vanaf een apparaat met de status van **Offline** in de portal van Azure klassieke.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Het apparaat kan geen verbinding maken met de configuratie van de huidige web-proxy. Mogelijk een probleem met de configuratie van de web-proxy of een probleem met de netwerkverbinding. In dit geval moet u ervoor dat uw web-proxy-instellingen juist zijn en de web-proxyservers online en bereikbaar zijn. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Problemen met de synchronisatie-HcsTime-cmdlet

Gebruik deze cmdlet de tijd apparaat wilt weergeven. Als de tijd van het apparaat een verschoven met de NTP-server heeft, klikt u vervolgens kunt u deze cmdlet de tijd met een dwingen synchroniseren met uw NTP-server. Als u de afstand tussen het apparaat en NTP-server groter is dan 5 minuten, ziet u een waarschuwing. Als de verschuiving groter is dan 15 minuten, wordt het apparaat offline gaat. U kunt nog steeds deze cmdlet waarmee een tijdsynchronisatie wordt afgedwongen. Echter als de verschuiving groter is dan 15 uur, is klikt u vervolgens niet mogelijk om te dwingen-synchronisatie van de tijd en een foutbericht wordt weergegeven.

**Voorbeeld van uitvoer – afgedwongen tijd synchroniseren met de synchronisatie-HcsTime**
 
     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.
 
     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Problemen met de cmdlets inschakelen-HcsPing en uitschakelen-HcsPing

Gebruik deze cmdlets om ervoor te zorgen dat de netwerkinterfaces op uw apparaat op ICMP ping aanvragen reageren. Al dan niet standaard reageert de StorSimple netwerk-interfaces niet op ping-verzoeken. Gebruik deze cmdlet is de eenvoudigste manier om te weten als uw apparaat online en bereikbaar is.  

**Voorbeeld van uitvoer – inschakelen-HcsPing en uitschakelen-HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Problemen met de cmdlet doelcellen-HcsRoute

Gebruik deze cmdlet als een hulpmiddel voor het traceren van route. Deze pakketten verzendt naar elke router voor de manier waarop naar een uiteindelijke bestemming gedurende een periode en vervolgens expressie resultaten op basis van de pakketten geretourneerd uit elke hop wordt berekend. Omdat de cmdlet ziet u de mate van pakketverlies op elke gewenste router of koppeling, kunt u welke routers pinpoint of koppelingen veroorzaakt problemen met het netwerk.

**Voorbeeld van uitvoer laat zien hoe u de route van een pakket met doelcellen HcsRoute traceren**

     Controller0>Trace-HcsRoute -Target 10.126.174.25
     
     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]
      
     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]
      
     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Problemen met de cmdlet Get-HcsRoutingTable

Gebruik deze cmdlet om weer te geven van de routering tabel voor uw apparaat StorSimple. Een routeren tabel is een set regels om te bepalen waar gegevenspakketten reizen via een netwerk IP (Internet Protocol) doorgestuurd. 

De routering tabel staan de interfaces en de gateway waarmee de gegevens met de opgegeven netwerken. Dit kunt ook de routering meetwaarde dat wil de maker beschikking voor het pad zeggen naar een bepaalde bestemming. De laagste de routering meetwaarde, hoe hoger de voorkeur. 

Bijvoorbeeld, hebt u 2 netwerkinterfaces, gegevens 2 en 3 voor gegevens, verbonden met Internet. Als de doelstellingen van de routering voor gegevens 2 en 3 van de gegevens respectievelijk 15 en 261 zijn, is 2 van de gegevens met de onderste routeren meetwaarde de voorkeur interface gebruikt om te bereiken van Internet.

Als u Update 1 op uw apparaat StorSimple worden uitgevoerd, heeft uw gegevens 0 netwerk-interface de hoogste voorkeur in de cloud-verkeer is toegestaan. Dit betekent dat zelfs als er andere interfaces cloud is ingeschakeld, de cloud-verkeer zou worden doorgestuurd door gegevens 0. 

Als u de `Get-HcsRoutingTable` cmdlet zonder op te geven parameters (als het volgende voorbeeld wordt getoond), de cmdlet IPv4 en IPv6 routeren tabellen wordt uitgevoerd. U kunt ook opgeven `Get-HcsRoutingTable -IPv4` of `Get-HcsRoutingTable -IPv6` om een relevante routeren tabel.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================
       
      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================
       
      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None
       
      Controller0>
 
## <a name="step-by-step-storsimple-troubleshooting-example"></a>Voorbeeld van stapsgewijze StorSimple probleemoplossing

Het volgende voorbeeld ziet stapsgewijze probleemoplossing van een StorSimple-implementatie. Klik in het voorbeeldscenario mislukt apparaatregistratie met een foutbericht met de mededeling dat de netwerkinstellingen of de naam van de DNS-onjuist is.

Het foutbericht dat wordt geretourneerd luidt als volgt:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

De fout kan worden veroorzaakt door een van de volgende opties:

- Onjuiste hardware installatie
- Defect netwerk interfaces
- Onjuiste IP-adres, subnetmasker, gateway, primaire DNS-server of webproxy
- Onjuiste registratie-toets
- Onjuiste firewallinstellingen

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Om te zoeken en het apparaat registratie probleem oplossen

1. Controleer de apparaatconfiguratie van uw: uitvoeren op de actieve controller, `Invoke-HcsSetupWizard`.

     > [AZURE.NOTE] De wizard setup moet uitvoeren op de actieve controller uit. Om te bevestigen dat u met de actieve controller verbonden bent, kijkt u naar de banner gepresenteerd in de seriële console. De banner geeft aan of u verbonden bent met controller 0 of controller 1, en of de controller actieve of passieve. Ga naar [identificeren een actieve controller op uw apparaat](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device)voor meer informatie.
 
2. Zorg ervoor dat het apparaat juist is bekabelde: Controleer de netwerkkabels op het apparaat weer luchtvervoer. De kabel is specifiek voor het model. Ga naar de [installatie van uw apparaat StorSimple 8100](storsimple-8100-hardware-installation.md) of [uw apparaat StorSimple 8600](storsimple-8600-hardware-installation.md)voor meer informatie.

     > [AZURE.NOTE] Als u 10 GbE netwerkpoorten gebruikt, moet u de meegeleverde QSFP SFP adapters en SFP kabels gebruiken. Raadpleeg de [lijst met kabels, schakelopties, en plaatsen door de OEM-leverancier voor Mellanox poorten aanbevolen](http://www.mellanox.com/page/cables?mtag=cable_overview)voor meer informatie.
 
3. Controleer de status van het netwerk:

   - Gebruik de cmdlet Get-NetAdapter om te bepalen de status van het netwerkinterfaces voor gegevens 0. 
   - Als de koppeling is niet werkt, kan de status **ifindex** wordt aangegeven dat de interface is niet beschikbaar. Vervolgens moet u de netwerkverbinding van de poort op het toestel en met de schakeloptie controleren. U moet ook sprake is van slechte kabels. 
   - Als u vermoedt dat de gegevens 0 poort op de actieve controller is mislukt, u dit controleren kunt door het verbinding maken met de gegevens 0 poort op controller 1. U kunt dit controleren de netwerkkabel verbreken door de achterzijde van het apparaat van controller 0, sluit u de kabel aan op de controller 1 en voer de cmdlet Get-NetAdapter opnieuw. 
   Als de gegevens 0 poort op mislukt een controller [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor de volgende stappen. Mogelijk moet u de controller op uw systeem vervangen.
 
4. Controleer of de verbinding met de schakeloptie:
   - Zorg ervoor dat gegevens 0 netwerkinterfaces op controller 0 en controller 1 in uw primaire insluiting in hetzelfde subnet. 
   - Controleer de hub of router. Meestal, moet u beide controllers verbinding maken met dezelfde hub of router. 
   - Zorg ervoor dat de schakelopties die u voor de verbinding gebruikt hebben met gegevens 0 voor besturing voor beide in het dezelfde vLAN.
   
5. Eventuele gebruikersfouten verwijderen:

  - Voer de installatiewizard opnieuw (uitvoeren **Roep-HcsSetupWizard**) en voer de waarden opnieuw om ervoor te zorgen dat er geen fouten zijn. 
  - Controleer of de registratie sleutel die wordt gebruikt. Dezelfde registratie sleutel kan worden gebruikt om verbinding maken met meerdere apparaten met een StorSimple Manager-service. Gebruik de procedure in het [ophalen van de service registratie-toets](storsimple-manage-service.md#get-the-service-registration-key) om ervoor te zorgen dat u de juiste registratiegegevens sleutel worden gebruikt.

    > [AZURE.IMPORTANT] Als er meerdere services worden uitgevoerd, moet u om ervoor te zorgen dat de registratie-toets voor de desbetreffende service wordt gebruikt voor het registreren van het apparaat. Als u een apparaat hebt geregistreerd met de verkeerde StorSimple Manager-service, moet u [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor de volgende stappen op te nemen. Mogelijk moet u uitvoeren van een fabriek opnieuw instellen van het apparaat (dit kan leiden tot gegevensverlies) aan en maak verbinding met de beoogde-service.

6. Gebruik de cmdlet verbinding testen om te bevestigen dat u verbinding met het externe netwerk hebt. Ga naar [problemen met de cmdlet verbinding testen](#troubleshoot-with-the-test-connection-cmdlet)voor meer informatie.

7. Controleren op firewall interferentie. Als u hebt gecontroleerd, die de virtuele IP-(VIP), subnet, gateway en DNS-instellingen alle juist zijn, zijn er nog steeds verbindingsproblemen en is het mogelijk dat communicatie tussen uw apparaat en het externe netwerk wordt geblokkeerd door de firewall. Moet u ervoor zorgen dat poort 80 en 443 beschikbaar zijn op uw apparaat StorSimple voor uitgaande communicatie. Zie [vereisten voor uw apparaat StorSimple toegang](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)voor meer informatie.

8. Bekijk de logboeken. Ga naar [ondersteuning-pakketten en apparaat logboeken beschikbaar voor probleemoplossing](#support-packages-and-device-logs-available-for-troubleshooting).

9. Als de voorgaande stappen het probleem, [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor hulp niet is opgelost.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het oplossen van een operationele-apparaat](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
