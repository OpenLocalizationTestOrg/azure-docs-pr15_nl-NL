<properties 
   pageTitle="Extern verbinding maken met uw StorSimple-apparaat | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u uw apparaat gebruiken voor extern beheer configureert en hoe u verbinding maken met Windows PowerShell voor StorSimple via HTTP of HTTPS."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="alkohli" />

# <a name="connect-remotely-to-your-storsimple-device"></a>Extern verbinding maken met uw apparaat StorSimple

## <a name="overview"></a>Overzicht

U kunt Windows PowerShell externe aansluiten op uw apparaat StorSimple. Als u verbinding maakt met deze methode, ziet u een menu. (U een menu alleen zien als u de seriële console op het apparaat gebruiken om verbinding te maken.) Met Windows PowerShell externe, moet u een specifieke runspace verbinden. U kunt ook de weergavetaal opgeven. 

Ga naar de [Windows PowerShell gebruiken voor StorSimple kunt u uw apparaat StorSimple beheren](storsimple-windows-powershell-administration.md)voor meer informatie over het gebruik van externe Windows PowerShell voor het beheren van uw apparaat.

Deze zelfstudie wordt uitgelegd hoe voor het configureren van uw apparaat gebruiken voor extern beheer en klik vervolgens op hoe u verbinding maken met Windows PowerShell voor StorSimple. U kunt HTTP of HTTPS-verbinding via Windows PowerShell externe communicatie. Echter wanneer u hoe u verbinding maken met Windows PowerShell voor StorSimple zijn bepalen, het volgende overwegen: 

- Rechtstreeks verbinden met de seriële console apparaat is beveiligd, maar niet via netwerk schakelopties verbinding maken met de seriële console is. Wees voorzichtig met van het beveiligingsrisico het verbinding maken met de seriële console apparaat via netwerk schakelopties. 

- Een verbinding via een HTTP-sessie, is het mogelijk bieden meer beveiliging dan verbinding via de seriële console via het netwerk. Hoewel dit niet de meest veilige methode, is het aanvaardbaar op vertrouwde netwerken. 

- Een verbinding via een HTTPS-sessie met een zelfondertekend certificaat is het veiligst en de aanbevolen optie.

U kunt extern koppelen aan de Windows PowerShell-interface. Externe toegang tot uw apparaat StorSimple via de Windows PowerShell-interface is echter niet al dan niet standaard ingeschakeld. U moet eerst extern beheer op het apparaat inschakelen en klik vervolgens op de client die is gebruikt voor toegang tot uw apparaat.

De stappen in dit artikel beschreven uitgevoerd op een Windows Server 2012 R2-hostsysteem.

## <a name="connect-through-http"></a>Maak verbinding via HTTP

Verbinding maken met Windows PowerShell voor StorSimple via een HTTP-sessie biedt betere beveiliging dan verbinding via de seriële console van uw apparaat StorSimple. Hoewel dit niet de meest veilige methode, is het aanvaardbaar op vertrouwde netwerken.

U kunt de portal van Azure klassieke of de seriële console extern beheer configureren. Selecteer in de volgende procedures uit:

- [Gebruik van de Azure klassieke portal wilt extern beheer inschakelen via HTTP](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [De seriële console extern beheer inschakelen via HTTP gebruiken](#use-the-serial-console-to-enable-remote-management-over-http)

Nadat u extern beheer inschakelt, gebruikt u de volgende procedure voor het voorbereiden van de client voor een verbinding met extern.

- [De client voor verbinding met extern voorbereiden](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>Gebruik van de Azure klassieke portal wilt extern beheer inschakelen via HTTP 

De volgende stappen uitvoeren in de portal van Azure klassieke extern beheer inschakelen via HTTP.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Extern beheer via de portal van Azure klassieke inschakelen

1. Toegang tot **apparaten** > **configureren** voor uw apparaat.

2. Schuif omlaag naar de sectie **Extern beheer** .

3. Stel **extern beheer inschakelen** op **Ja**.

4. U kunt nu verbinding maken met behulp van HTTP. (De standaardinstelling is via HTTPS verbinding maken.) Zorg ervoor dat HTTP is geselecteerd.

    >[AZURE.NOTE] Verbinding maken via HTTP is acceptabel alleen op vertrouwde netwerken.

6. Klik op **Opslaan** onder aan de pagina.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>De seriële console extern beheer inschakelen via HTTP gebruiken

De volgende stappen uitvoeren op het apparaat seriële console extern beheer inschakelen.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Extern beheer via de seriële console apparaat inschakelen

1. Selecteer de optie 1 in het menu seriële console. Ga naar de [verbinding maken met Windows PowerShell voor StorSimple via de seriële console apparaat](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console)voor meer informatie over het gebruik van de seriële console op het apparaat.

2. Typ bij de prompt:`Enable-HcsRemoteManagement –AllowHttp`

3. U wordt geïnformeerd over de problemen in de beveiliging van het gebruik van HTTP verbinding maken met het apparaat. Wanneer u wordt gevraagd, bevestig door te typen **Y**.

4. Controleer of HTTP is ingeschakeld door te typen:`Get-HcsSystem`

5. Controleer of het veld **RemoteManagementMode** wordt **HttpsAndHttpEnabled**. De volgende afbeelding ziet deze instellingen in stopverf.

     ![Seriële HTTPS en HTTP ingeschakeld](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>De client voor verbinding met extern voorbereiden

De volgende stappen uitvoeren op de client extern beheer inschakelen.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Voor het voorbereiden van de client voor verbinding met extern

1. Een Windows PowerShell-sessie starten als beheerder.

2. Typ de volgende opdracht uit het IP-adres van het apparaat StorSimple toevoegen aan lijst met vertrouwde hosts van de klant: 

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

     <*Device_ip*> vervangen door het IP-adres van uw apparaat; bijvoorbeeld: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Typ de volgende opdracht uit de referenties apparaat opslaan in een variabele: 

     *$cred = get-referenties*

4. In het dialoogvenster dat wordt weergegeven:

    1. Typ de gebruikersnaam in deze indeling: *device_ip\SSAdmin*.
    2. Typ het wachtwoord van het apparaat-beheerder dat is ingesteld wanneer het apparaat is geconfigureerd met de wizard setup. Het standaardwachtwoord is *Wachtwoord1*.

7. Een Windows PowerShell-sessie starten op het apparaat door deze opdracht te typen:

     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`

     >[AZURE.NOTE] Toevoegen als u wilt maken voor gebruik in een Windows PowerShell-sessie met het virtuele StorSimple-apparaat, de `–Port` parameter en geef de openbare poort die u in de externe toegang voor StorSimple virtuele toestel hebt geconfigureerd.

     U kunt een actieve externe Windows PowerShell-sessie bij het apparaat nu nodig hebt.

    ![PowerShell externe communicatie met behulp van HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Maak verbinding via HTTPS

Verbinding maken met Windows PowerShell voor StorSimple tot en met een HTTPS-sessie is de meest beveiligde en aanbevolen methode externe verbinding maken met uw Microsoft Azure StorSimple-apparaat. De volgende procedures wordt uitgelegd hoe de seriële console en client-computers zo instellen dat u kunt HTTPS verbinding maken met Windows PowerShell voor StorSimple.

U kunt de portal van Azure klassieke of de seriële console extern beheer configureren. Selecteer in de volgende procedures uit:

- [Gebruik van de Azure klassieke portal wilt extern beheer inschakelen via HTTPS](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [De seriële console gebruiken om te schakelen van extern beheer via HTTPS](#use-the-serial-console-to-enable-remote-management-over-https)

Nadat u extern beheer inschakelt, gebruikt u de volgende procedures voor het voorbereiden van de host voor een extern beheer en verbinding maken met het apparaat van de externe host.

- [De host voorbereiden voor extern beheer](#prepare-the-host-for-remote-management)

- [Verbinding maken met het apparaat van de externe host](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>Gebruik van de Azure klassieke portal wilt extern beheer inschakelen via HTTPS

De volgende stappen uitvoeren in de portal van Azure klassieke extern beheer inschakelen via HTTPS.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Extern beheer inschakelen via HTTPS van de Azure klassieke portal

1. Toegang tot **apparaten** > **configureren** voor uw apparaat.

2. Schuif omlaag naar de sectie **Extern beheer** .

3. Stel **extern beheer inschakelen** op **Ja**.

4. U kunt nu verbinding maken met HTTPS. (De standaardinstelling is via HTTPS verbinding maken.) Zorg ervoor dat HTTPS is geselecteerd. 

5. Klik op **extern beheer certificaat downloaden**. Geef een locatie om op te slaan dit bestand. U moet dit certificaat installeren op de client of host computer waarmee u kunt verbinding maken met het apparaat.

6. Klik op **Opslaan** onder aan de pagina.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>De seriële console gebruiken om te schakelen van extern beheer via HTTPS

De volgende stappen uitvoeren op het apparaat seriële console extern beheer inschakelen.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Extern beheer via de seriële console apparaat inschakelen

1. Selecteer de optie 1 in het menu seriële console. Ga naar de [verbinding maken met Windows PowerShell voor StorSimple via de seriële console apparaat](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console)voor meer informatie over het gebruik van de seriële console op het apparaat.

2. Typ bij de prompt: 

     `Enable-HcsRemoteManagement`

    Dit moet HTTPS inschakelen op uw apparaat.

3. Controleer of dat HTTPS is ingeschakeld door te typen: 

     `Get-HcsSystem`

    Zorg ervoor dat het veld **RemoteManagementMode** **HttpsEnabled**ziet. De volgende afbeelding ziet deze instellingen in stopverf.

     ![Seriële HTTPS ingeschakeld](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. Uit de resultaten van `Get-HcsSystem`, het seriële getal van het apparaat te kopiëren en deze opslaan voor toekomstig gebruik.

    >[AZURE.NOTE] Het seriële getal wordt toegewezen aan de naam van de CN in het certificaat.

5. Een extern beheer-certificaat verkrijgen door te typen: 
 
     `Get-HcsRemoteManagementCert`

    Een certificaat dat de volgende strekking weergegeven.

    ![Extern beheer certificaat ophalen](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. Kopieer de informatie in het certificaat in **---BEGIN certificaat---** **---** beëindigen certificaat---in een teksteditor zoals Kladblok en sla deze op te geven als een .cer-bestand. (Kopieert u dit bestand aan de host van uw externe wanneer u de host voorbereidt.)

    >[AZURE.NOTE] Als u wilt een nieuw certificaat genereren, gebruikt u de `Set-HcsRemoteManagementCert` cmdlet.

### <a name="prepare-the-host-for-remote-management"></a>De host voorbereiden voor extern beheer

Als u wilt de hostcomputer voorbereiden op een externe verbinding met een HTTPS-sessie, voert u de volgende procedures uit:

- [Importeren de .cer-bestand in het archief van de hoofdsite van de client of externe host](#to-import-the-certificate-on-the-remote-host).

- [Het apparaat seriële getallen naar het hostsbestand op uw externe host toevoegen](#to-add-device-serial-numbers-to-the-remote-host).

Elk van deze procedures wordt hieronder beschreven.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Het certificaat op de externe host importeren

1. Met de rechtermuisknop op de .cer-bestand en selecteer **certificaat installeren**. Hiermee start u de Wizard Certificaat importeren.

    ![Wizard 1 van de certificaat importeren](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. Voor **Archieflocatie**, selecteert u **Lokale computer**en klik vervolgens op **volgende**.

3. Selecteer **alle certificaten in de volgende store**en klik vervolgens op **Bladeren**. Ga naar het archief van de hoofdsite van uw externe host, en klik vervolgens op **volgende**.

    ![Wizard 2 van de certificaat importeren](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. Klik op **Voltooien**. Een bericht dat u dat het importeren voltooid is wordt weergegeven.

    ![Wizard 3 van certificaat importeren](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Apparaat seriële getallen optellen met de externe host

1. Start Kladblok als een beheerder en open het hostsbestand op \Windows\System32\Drivers\etc.

2. De volgende drie vermeldingen toevoegen aan het Hostsbestand: **gegevens 0 IP-adres**, **Controller 0 vast IP-adres**en **Controller 1 vast IP-adres**.

3. Voer in het seriële getal van apparaat dat u eerder hebt opgeslagen. Zoals in de volgende afbeelding, moet u dit toewijzen aan het IP-adres. Toevoegen voor de Controller 0 en 1 Controller, **Controller0** en **Controller1** aan het einde van het seriële getal (CN naam).

    ![CN naam toe te voegen aan hosts-bestand](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. Sla het Hostsbestand.

### <a name="connect-to-the-device-from-the-remote-host"></a>Verbinding maken met het apparaat van de externe host

Windows PowerShell en SSL gebruiken om in te voeren van een sessie SSAdmin op uw apparaat uit een externe host of de client. De sessie SSAdmin kaarten met optie 1 in het menu [seriële console](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) van uw apparaat.

Voer de volgende procedure uit op de computer van waaruit u wilt maken van verbinding met extern Windows PowerShell.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Een sessie SSAdmin invoeren op het apparaat met Windows PowerShell en SSL

1. Een Windows PowerShell-sessie starten als beheerder.

2. Het apparaat IP-adres toevoegen aan de vertrouwde hosts van de klant door te typen:

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

    Waar <*device_ip*> is het IP-adres van uw apparaat; bijvoorbeeld: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Maak een nieuwe referentie door te typen: 

     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`

    Waar <*IP van apparaat*> is het IP-adres van de gegevens 0 voor uw apparaat; bijvoorbeeld **10.126.173.90** zoals wordt weergegeven in de voorgaande afbeelding van het Hostsbestand. De beheerderswachtwoord voor uw apparaat ook opgeven.

4. Een sessie maken door te typen:

     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`

    Voor de parameter - computernaam in de cmdlet het <*seriële getal van apparaat*> te verstrekken. Dit serienummer is toegewezen aan het IP-adres van de gegevens 0 in het hostsbestand op uw externe host; bijvoorbeeld **SHX0991003G44MT** zoals wordt weergegeven in de volgende afbeelding.

5. Type: 

     `Enter-PSSession $session`

6. U moet wacht enkele minuten en vervolgens wordt u verbonden aan uw apparaat via HTTPS via SSL. Hier ziet u een bericht waarin dat u bent verbonden met uw apparaat.

    ![PowerShell externe HTTPS en SSL gebruiken](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Windows PowerShell voor het beheren van uw apparaat StorSimple](storsimple-windows-powershell-administration.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
