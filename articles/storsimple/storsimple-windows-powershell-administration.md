<properties 
   pageTitle="PowerShell voor het beheer van de apparaten StorSimple | Microsoft Azure"
   description="Leer hoe u Windows PowerShell voor StorSimple voor het beheren van uw apparaat StorSimple gebruiken."
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
   ms.author="alkohli@microsoft.com" />

# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Windows PowerShell voor StorSimple voor het beheren van uw apparaat gebruiken

## <a name="overview"></a>Overzicht

Windows PowerShell voor StorSimple biedt een opdrachtregel-interface die u gebruiken kunt voor het beheren van uw Microsoft Azure StorSimple-apparaat. De naam geeft al, is een op op basis van Windows PowerShell, opdrachtregel-interface die is ingebouwd in een beperkte runspace. Vanuit het perspectief van de gebruiker op de opdrachtregel, wordt een beperkte runspace weergegeven als een beperkte versie van Windows PowerShell. Behoud van enkele van de eenvoudige mogelijkheden van Windows PowerShell, heeft dit interface aanvullende speciale cmdlets die zijn gericht op het beheren van uw Microsoft Azure StorSimple-apparaat. 

Dit artikel worden de Windows PowerShell voor StorSimple-functies, inclusief hoe u verbinding kunt maken met deze interface, en bevat koppelingen naar stapsgewijze procedures of werkstromen die u kunt uitvoeren met deze interface. De werkstromen zijn hoe uw apparaat registreren, het netwerkinterface configureren op uw apparaat, installeer updates waarvoor het apparaat dat zich in de onderhoudsmodus, de status van het apparaat wijzigen en eventuele problemen die kunnen optreden.

Lees dit artikel en kunnen u:

- Verbinding maken met uw StorSimple-apparaat met Windows PowerShell voor StorSimple.

- Uw StorSimple-apparaat met Windows PowerShell voor StorSimple beheren.

- Hulp krijgen in Windows PowerShell voor StorSimple.

>[AZURE.NOTE]   

>- Windows PowerShell voor StorSimple cmdlets kunt u uw apparaat StorSimple beheren vanaf een seriële console of extern via Windows PowerShell externe communicatie. Voor meer informatie over elk van de afzonderlijke cmdlets die kunnen worden gebruikt in deze interface, gaat u naar [cmdlet voor Windows PowerShell voor StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

>- De StorSimple van Azure PowerShell-cmdlets zijn in een andere verzameling cmdlets waarmee u kunt automatiseren StorSimple service level en migratietaken vanaf de opdrachtregel. Ga naar de [verwijzing naar de cmdlets van Azure StorSimple](https://msdn.microsoft.com/library/azure/dn920427.aspx)voor meer informatie over het Azure PowerShell-cmdlets voor StorSimple.

U kunt de Windows PowerShell voor StorSimple een van de volgende manieren openen:

- [Verbinding maken met StorSimple apparaat seriële console](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Extern verbinding maken met StorSimple via Windows PowerShell](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Verbinding maken met Windows PowerShell voor StorSimple via de seriële console apparaat

U kunt [downloaden stopverf](http://www.putty.org/) of soortgelijke terminal emulatiesoftware verbinding maken met Windows PowerShell voor StorSimple. U moet stopverf specifiek voor toegang tot het apparaat dat Microsoft Azure StorSimple configureren. De volgende onderwerpen bevatten meer informatie over het configureren van stopverf en verbinding maken met het apparaat. Verschillende opties in het menu in de seriële console worden ook beschreven.

### <a name="putty-settings"></a>Stopverf instellingen

Zorg ervoor dat u de volgende stopverf instellingen gebruiken om te verbinding maken met de Windows PowerShell-interface van de seriële console.

#### <a name="to-configure-putty"></a>Stopverf configureren

1. Selecteer in het dialoogvenster stopverf **opnieuw configureren** in het deelvenster **categorie** **toetsenbord**.

2. Zorg dat de volgende opties zijn ingeschakeld (dit zijn de standaardinstellingen wanneer u een nieuwe sessie starten). 

  	|Toetsenbord-item|Selecteer|
  	|---|---|
  	|Toets BACKSPACE te drukken|Besturingselement-? (127)|
  	|Start- en eindtijd toetsen|Standaard|
  	|Functietoetsen en toetsenblok|ESC [n ~|
  	|Eerste weergave van de cursortoetsen|Normaal|
  	|Eerste weergave van het numerieke toetsenblok|Normaal|
  	|Extra toetsenbordfuncties inschakelen|CTRL + ALT + verschilt van AltGr|

    ![Ondersteunde stopverf instellingen](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Klik op **toepassen**.

4. Selecteer in het deelvenster **categorie** **vertaling**.

5. Selecteer in het vak lijst van **externe tekenset** **UTF-8**.

6. Selecteer onder voor de **verwerking van lijnen tekenen tekens**, **Gebruik Unicode lijnen tekenen code wordt verwezen**. De volgende afbeelding ziet u de juiste stopverf selecties.

    ![UTF-stopverf instellingen](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Klik op **toepassen**.


U kunt nu stopverf verbinding maken met de seriële console apparaat door de volgende stappen uit te voeren.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### <a name="about-the-serial-console"></a>Over de seriële console

Als u de Windows PowerShell-interface van uw apparaat StorSimple via de seriële console, een bannerbericht wordt weergegeven, gevolgd door opties in het menu. 

Het bannerbericht bevat StorSimple apparaat basisgegevens zoals het model, naam, geïnstalleerde versie en status van de controller die u wilt openen. De volgende afbeelding ziet u een voorbeeld van een bannerbericht.

![Seriële bannerbericht](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] U kunt het bannerbericht gebruiken om te zien of de controller die er verbinding is met actief of passieve.

De volgende afbeelding ziet u de verschillende runspace opties die beschikbaar in het menu seriële console zijn.

![Uw apparaat 2 registreren](./media/storsimple-windows-powershell-administration/IC740906.png)

U kunt kiezen uit de volgende instellingen:

1. **Aanmelden met de volledige toegang** Deze optie kunt u verbinding maken (met de juiste referenties) naar de runspace **SSAdminConsole** op de lokale controller uit. (De lokale controller is de controller uit die u momenteel via de seriële console van uw apparaat StorSimple opent.) Deze optie kan ook worden gebruikt toe te staan dat Microsoft Support voor toegang tot onbeperkte runspace (een ondersteuningssessie) om op te lossen eventuele problemen mogelijke apparaten. Nadat u de optie 1 aan te melden hebt gebruikt, kunt u de Microsoft Support-engineeren voor toegang tot onbeperkte runspace door een specifieke cmdlet uit te voeren. Voor meer informatie raadpleegt u een ondersteuningssessie te [starten](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).

2. **Meld u aan bij peer-controller met volledige toegang** Deze optie is hetzelfde als de optie 1, behalve dat u verbinding (met de juiste referenties) naar de runspace **SSAdminConsole** van de peer-besturing maken kunt. Omdat het apparaat StorSimple een apparaat beschikbaarheid met twee controllers in een actieve-passieve-configuratie is, wordt peer verwijst naar de andere controller in het apparaat dat u via de seriële console opent).
Net zoals bij optie 1, deze optie kan ook worden gebruikt toe te staan dat Microsoft Support voor toegang tot onbeperkte runspace op een peer-controller.

3. **Verbinding maken met beperkte toegang** Deze optie wordt gebruikt voor toegang tot Windows PowerShell-interface in beperkte modus. U wordt niet gevraagd om toegang tot referenties. Deze optie maakt verbinding met een meer beperkte runspace vergeleken met opties voor 1 en 2.  Enkele van de taken die beschikbaar via de optie 1 zijn die **kan niet* worden uitgevoerd in deze runspace zijn:

    - Terugzetten naar de fabrieksinstellingen
    - Het wachtwoord wijzigen
    - In- of uitschakelen van ondersteuning voor access
    - Updates toepassen
    - Hotfixes installeren 
                                                

    >[AZURE.NOTE] **Dit is de beste optie als u het wachtwoord van het apparaat-beheerder vergeten bent en kan geen verbinding met behulp van de optie 1 of 2 maken.**

4. **Taal wijzigen** Deze optie kunt u de weergavetaal op de Windows PowerShell-interface wijzigen. De talen die worden ondersteund zijn Engels, Japans, Russisch, Frans, Zuid Koreaans, Spaans, Italiaans, Duits, Chinees en Portugees (Brazilië).


## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Extern verbinding maken met Windows PowerShell gebruiken voor StorSimple StorSimple

U kunt Windows PowerShell externe aansluiten op uw apparaat StorSimple. Als u verbinding maakt met deze methode, ziet u een menu. (U een menu alleen zien als u de seriële console op het apparaat om verbinding te gebruiken. Via een externe computer gaat u rechtstreeks naar het equivalent van "optie 1 – volledige toegang' op de seriële console.) Met Windows PowerShell externe, moet u een specifieke runspace verbinden. U kunt ook de weergavetaal opgeven. 

De weergavetaal staat los van de taal die u met behulp van de optie **Taal wijzigen** in het menu seriële console hebt ingesteld. Externe PowerShell gewoon automatisch door de landinstelling van het apparaat dat die u de verbinding maakt als geen is opgegeven.

>[AZURE.NOTE] Als u met Microsoft Azure virtual hosts en StorSimple virtuele apparaten werkt, kunt u Windows PowerShell voor externe communicatie en de virtuele host verbinding maken met het virtuele apparaat. Als u een gedeelde locatie op de host waarop u wilt opslaan van gegevens uit de Windows PowerShell-sessie hebt ingesteld, moet u zich bewust zijn dat de hoofdsom iedereen alleen geverifieerde gebruikers bevat. Als u de optie voor delen hebt ingesteld om toegang te krijgen door iedereen en u verbinding maakt zonder op te geven van referenties, de niet-geverifieerde anonieme hoofdsom worden gebruikt en ziet u een fout. Wilt kunt dit probleem oplossen, hosten op de optie voor delen u moet de account Gast inschakelen en geef vervolgens de Gast volledige toegang tot de optie voor delen of moet u geldige referenties samen met de Windows PowerShell-cmdlet.

U kunt HTTP of HTTPS-verbinding via Windows PowerShell externe communicatie. Volg de instructies in de volgende zelfstudies:

- [Verbinding maken met behulp van extern HTTP](storsimple-remote-connect.md#connect-through-http)
- [Verbinding maken met extern HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Beveiligingsoverwegingen voor verbinding

U hoe u verbinding maken met Windows PowerShell voor StorSimple zijn Houd rekening met het volgende:

- Rechtstreeks verbinden met de seriële console apparaat is beveiligd, maar niet via netwerk schakelopties verbinding maken met de seriële console is. Wees voorzichtig met van het beveiligingsrisico het verbinding maken met apparaat seriële via netwerk schakelopties.

- Een verbinding via een HTTP-sessie, is het mogelijk bieden meer beveiliging dan verbinding via de seriële console via netwerk. Hoewel dit niet de meest veilige methode, is het aanvaardbaar op vertrouwde netwerken.

- Een verbinding via een HTTPS-sessie is het veiligst en de aanbevolen optie.


## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Uw StorSimple-apparaat met Windows PowerShell voor StorSimple beheren
De volgende tabel bevat een overzicht van alle veelvoorkomende beheertaken en complexe werkstromen die kunnen worden uitgevoerd binnen de Windows PowerShell-interface van uw apparaat StorSimple. Voor meer informatie over elke werkstroom, klikt u op de gewenste vermelding in de tabel.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShell voor StorSimple werkstromen

|Als u dit wilt doen...|Gebruik deze procedure.|
|---|---|
|Uw apparaat registreren|[Configureren en het apparaat met Windows PowerShell voor StorSimple registreren](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Webproxy configureren</br>Weergave web proxy-instellingen|[Webproxy voor uw apparaat StorSimple configureren](storsimple-configure-web-proxy.md)|
|Instellingen voor de interface van gegevens 0 netwerk op uw apparaat wijzigen|[GEGEVENS 0 netwerkinterface voor uw apparaat StorSimple wijzigen](storsimple-modify-data-0.md)|
|Een controller stoppen </br> Opnieuw opstarten of een controller afsluiten </br> Een apparaat afsluiten</br>Het apparaat opnieuw standaardinstellingen|[Apparaatcontrollers beheren](storsimple-manage-device-controller.md)|
|Installeer onderhoud modus updates en hotfixes|[Bijwerken van uw apparaat](storsimple-update-device.md)|
|Onderhoud gezet </br>Modus voor het onderhoud van afsluiten|[StorSimple apparaat modi](storsimple-device-modes.md)|
|Een ondersteuningspakket maken</br>Ontsleutelen en bewerken van een ondersteuningspakket|[Maken en beheren van een ondersteuningspakket](storsimple-create-manage-support-package.md)|
|Een ondersteuningssessie met starten</br>|[Een ondersteuningssessie voor in Windows PowerShell voor StorSimple starten](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Hulp krijgen in Windows PowerShell voor StorSimple

In Windows PowerShell voor StorSimple is de cmdlet hulp beschikbaar. Een actuele versie van deze Help-informatie is ook beschikbaar, waarmee u kunt de Help op de computer bijwerken.

Help opvragen in deze interface is vergelijkbaar met die in Windows PowerShell en de Help-gerelateerde cmdlets de meeste werkt. U kunt Help voor Windows PowerShell online vinden in de TechNet-bibliotheek: [uitvoeren van scripts met Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=108518).

Hier volgt een korte beschrijving van de soorten Help-informatie voor deze Windows PowerShell-interface, waaronder over het bijwerken van de Help.

#### <a name="to-get-help-for-a-cmdlet"></a>Hulp krijgen voor een cmdlet

- Als u Help-cmdlet of functie, gebruikt u de volgende opdracht uit:`Get-Help <cmdlet-name>`

- Als u online-Help voor elke cmdlet, gebruikt u de vorige cmdlet met de `-Online` parameter:`Get-Help <cmdlet-name> -Online`

- Volledige hulp nodig hebt, kunt u de `–Full` parameter, en voor voorbeelden, gebruikt u de `–Examples` parameter.

#### <a name="to-update-help"></a>Help bijwerken

U kunt gemakkelijk de Help in de Windows PowerShell-interface bijwerken. De volgende stappen als u wilt bijwerken van de Help op uw systeem uitvoeren.

#### <a name="to-update-cmdlet-help"></a>Cmdlet Help bijwerken

1. Windows PowerShell wordt gestart met de optie **Als administrator uitvoeren** .

1. Typ bij de opdrachtprompt: `Update-Help`

1. De bijgewerkte Help-bestanden wordt geïnstalleerd.

1. Nadat de Help-bestanden zijn geïnstalleerd, typ: `Get-Help Get-Command`. Hiermee wordt een lijst met cmdlets waarvoor Help beschikbaar is weergegeven.


>[AZURE.NOTE] Als u een lijst met alle beschikbare cmdlets in een runspace, meld u aan bij de corresponderende menuoptie en uitvoeren de `Get-Command` cmdlet.

## <a name="next-steps"></a>Volgende stappen
Als u problemen met uw apparaat StorSimple optreden bij het uitvoeren van een van de bovenstaande werkstromen, raadpleegt u [hulpprogramma's voor probleemoplossing StorSimple implementaties](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

