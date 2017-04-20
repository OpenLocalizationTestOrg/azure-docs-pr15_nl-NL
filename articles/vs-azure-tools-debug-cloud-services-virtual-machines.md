<properties 
    pageTitle="Voor foutopsporing in een cloudservice Azure of virtuele machine in Visual Studio | Microsoft Azure"
    description="Voor foutopsporing in een Cloudservice binnenkomt of virtuele Machine in Visual Studio"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />
<tags 
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/15/2016"
    ms.author="tarcher" />

# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Voor foutopsporing in een cloudservice Azure of virtuele machine in Visual Studio

Visual Studio kunt u verschillende opties voor foutopsporing in cloudservices van Azure virtuele machines.



## <a name="debug-your-cloud-service-on-your-local-computer"></a>Fouten opsporen in de cloudservice op uw lokale computer

U kunt tijd besparen en geld met behulp van de Azure berekenen emulator voor foutopsporing van uw cloudservice op een lokale computer. U kunt door voor foutopsporing in een service lokaal voordat u deze implementeert, betrouwbaarheid en prestaties verbeteren zonder te betalen voor berekeningscluster tijd. Sommige fouten kunnen echter optreden alleen wanneer u een cloudservice uitvoert in Azure zelf. Als u foutopsporing op afstand inschakelen wanneer u uw service publiceren en voeg vervolgens de foutopsporing op een exemplaar van de rol, kunt u deze fouten fouten opsporen.

De emulator simuleert het berekenen van Azure-service en wordt uitgevoerd in uw lokale omgeving, zodat u kunt testen en fouten opsporen in uw cloudservice voordat u deze implementeert. De emulator omgaat met de levenscyclus van de exemplaren die uw rol en toegang geeft tot gesimuleerd bronnen, zoals lokale opslag. Wanneer u fouten opsporen in of het uitvoeren van uw service uit Visual Studio, wordt de emulator automatisch gestart als een toepassing voor de achtergrond en vervolgens uw service implementeren in de emulator. U kunt de emulator gebruiken om weer te geven van uw service wanneer deze wordt uitgevoerd in de lokale omgeving. U kunt de volledige versie of de express-versie van de emulator uitvoeren. (Beginnend met Azure 2.3, de express-versie van de emulator is de standaardwaarde.) Zie [Gebruik Emulator Express uit te voeren en fouten opsporen in een Cloudservice lokaal](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Voor foutopsporing uw cloudservice op uw lokale computer

1. Kies **Foutopsporing**, **Starten voor foutopsporing in** uw project Azure cloud-service uitvoeren op de menubalk. Als alternatief, kunt u op F5 drukt. Hier ziet u een bericht dat de Emulator berekenen wordt gestart. Wanneer de emulator wordt gestart, wordt deze door het pictogram op de taakbalk bevestigd.

    ![Azure emulator in het systeemvak](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. De gebruikersinterface weergeven voor de emulator berekeningscluster via het snelmenu voor het Azure-pictogram in het systeemvak en selecteer **Berekenen Emulator-gebruikersinterface weergeven**.

    Het deelvenster links van de gebruikersinterface bevat de services die momenteel zijn geïmplementeerd in de emulator berekenen en de rol-exemplaren die elke service wordt uitgevoerd. U kunt de service of rollen aan levenscyclus, logboekregistratie en diagnostische gegevens weergeven in het rechterdeelvenster. Als u de focus verplaatsen naar de bovenmarge van een venster opgenomen, wordt deze uitgebreid om te vullen het rechterdeelvenster.

1. De toepassing stapsgewijs door te selecteren van opdrachten in het menu **Foutopsporing** en onderbrekingspunten instellen in uw code. Als u de toepassing in Foutopsporing stapsgewijs, wordt de deelvensters worden bijgewerkt met de huidige status van de toepassing. Als u foutopsporing stopt, wordt de toepassingsimplementatie wordt verwijderd. Als uw toepassing een Webrol bevat en u de eigenschap opstarten actie starten van de webbrowser hebt ingesteld, wordt uw webtoepassing in Visual Studio gestart in de browser. Als u het aantal exemplaren van een rol in de configuratie van service wijzigt, moet u uw cloudservice stoppen en vervolgens opnieuw starten voor foutopsporing in zodat u kunt deze nieuwe exemplaren van de rol fouten.

    **Notitie:** Als u stopt met of voor foutopsporing in uw service, worden de lokale berekeningscluster emulator en opslag emulator niet worden gestopt. U moet ze expliciet stoppen in het systeemvak.


## <a name="debug-a-cloud-service-in-azure"></a>Fouten opsporen in een cloudservice in Azure wordt aangegeven

Als u wilt opsporen in een cloudservice uit een externe computer, moet u inschakelen die functionaliteit expliciet wanneer u uw cloudservice implementeert zodat vereist services (bijvoorbeeld msvsmon.exe) zijn geïnstalleerd op de virtuele machines die uw rol-processen worden uitgevoerd. Als u geen foutopsporing op afstand inschakelen wanneer u de service gepubliceerd, hebt u de service publiceren met externe foutopsporing is ingeschakeld.

Als u foutopsporing op afstand voor een cloudservice inschakelt, kunt deze niet verminderde prestaties vertoont of hogere kosten. U kunt externe foutopsporing op een productieservice, niet mag gebruiken omdat clients die gebruikmaken van de service mogelijk nadelig worden beïnvloed.

>[AZURE.NOTE] Wanneer u een cloudservice van Visual Studio publiceert, kunt u **IntelliTrace** inschakelen voor alle rollen in die service die zijn gericht op de .NET Framework-4 of de .NET Framework 4.5. U kunt met behulp van **IntelliTrace**, gebeurtenissen die zijn aangebracht in een exemplaar van de rol in het verleden onderzoeken en de context van dat moment reproduceren. Zie [voor foutopsporing in een gepubliceerde cloudservice met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) en [IntelliTrace gebruiken](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Naar foutopsporing op afstand voor een cloudservice inschakelen

1. Open het snelmenu voor het Azure project en selecteer vervolgens **publiceren**.

1. Selecteer de **tijdelijke** -omgeving en de configuratie **fouten opsporen in** .

    Dit is slechts een richtlijn. U kunt kiezen om uw testomgeving worden uitgevoerd in een productieomgeving. U kan echter gebruikers nadelig beïnvloeden als u foutopsporing op afstand op de productieomgeving inschakelen. U kunt de Release-configuratie, maar de configuratie foutopsporing maakt foutopsporing gemakkelijker.

    ![Kies de configuratie foutopsporing](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. De gebruikelijke stappen, maar het selectievakje **Externe foutopsporing inschakelen voor alle rollen** op het tabblad **Geavanceerde instellingen** .

    ![Fouten opsporen in configuratie](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Foutopsporing bijvoegen bij een cloudservice in Azure wordt aangegeven

1. Vouw het knooppunt voor uw cloudservice in Server Explorer.

1. Open het snelmenu voor de rol of rol instantie waarnaar u wilt bijvoegen en selecteert u vervolgens **Foutopsporing bijvoegen**.

    Als u een rol opsporen, wordt de foutopsporing Visual Studio gekoppeld aan elk exemplaar van die rol. Foutopsporing worden op een onderbrekingspunt voor het eerste exemplaar van de rol die wordt uitgevoerd die regel met code en voldoet aan de voorwaarden van deze onderbrekingspunt afgebroken. Als u een exemplaar, de aangesloten foutopsporing op alleen die instantie en -einden in een onderbrekingspunt alleen wanneer dit specifieke exemplaar wordt uitgevoerd die regel met code en voldoet aan de voorwaarden van het onderbrekingspunt fouten.

    ![Foutopsporing bijvoegen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. Nadat de foutopsporing aan een exemplaar koppelt, foutopsporing zoals u gewend bent. Foutopsporing worden automatisch gekoppeld aan het juiste hostproces voor uw rol. Afhankelijk van wat de rol is, wordt de foutopsporing koppelt aan w3wp.exe, WaWorkerHost.exe of WaIISHost.exe. Als u wilt controleren of het proces waaraan de foutopsporing is gekoppeld, vouw exemplaar in Server Explorer. Zie [Azure rol architectuur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) voor meer informatie over Azure processen.

    ![Het dialoogvenster type code selecteren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Als u wilt identificeren de processen waaraan foutopsporing is gekoppeld, open het dialoogvenster processen door op de menubalk, foutopsporing, Windows, processen te kiezen. (Toetsenbord: Ctrl + Alt + Z) Loskoppelen van een specifieke proces, opent u het snelmenu en klikt u vervolgens **Loskoppelen proces**hebt geselecteerd. Of, zoek het knooppunt exemplaar in Server Explorer, het proces hebt gevonden, opent u het snelmenu en vervolgens **Loskoppelen proces**hebt geselecteerd.

    ![Fouten opsporen in processen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Lange kleurovergangsbeëindigingen op onderbrekingspunten wanneer externe vermijden voor foutopsporing in. Azure behandelt een proces die langer dan een paar minuten reageert wordt gestopt en stopt verkeer naar die instantie verzendt. Als u te lang stopt, ontkoppelen msvsmon.exe van het proces

Als u wilt loskoppelen foutopsporing vanaf alle processen in uw exemplaar of de rol, opent u het snelmenu voor de rol of het exemplaar dat u foutopsporing en selecteert u vervolgens **Foutopsporing loskoppelen**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Beperkingen van externe foutopsporing in Azure wordt aangegeven

Van Azure SDK 2.3 heeft foutopsporing op afstand de volgende beperkingen.

- Met de externe foutopsporing ingeschakeld, kunt u een cloudservice waarin een rol meer dan 25 exemplaren heeft niet publiceren.

- Foutopsporing poorten 30400 naar 30424, 31400 naar 31424 en 32400 naar 32424 gebruikt. Als u probeert te gebruiken op een van deze poorten, niet mogelijk om te publiceren van uw service en een van de volgende foutberichten wordt weergegeven in het gebeurtenissenlogboek voor Azure: 

    - Fout bij valideren van het bestand .cscfg ten opzichte van het bestand .csdef. 
    De gereserveerde poort bereik 'range' voor eindpunt Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector van rol 'rol' overlapt met een al gedefinieerde poort of een bereik.
    - Toewijzing is mislukt. Probeer het later opnieuw, Verminder het VM formaat of het aantal exemplaren van de rol of probeer implementeren naar een ander gebied.


## <a name="debugging-azure-virtual-machines"></a>Voor foutopsporing in Azure virtuele machines

U kunt fouten opsporen in programma's die worden uitgevoerd op Azure virtuele machines met behulp van de Server Explorer in Visual Studio. Wanneer u foutopsporing op afstand op een Azure virtuele machine inschakelen, installeert u de externe foutopsporing extensie met Azure op de virtuele machine. U kunt vervolgens als bijlage toevoegen aan processen op de virtuele machine en fouten opsporen in zoals u gewend bent.

>[AZURE.NOTE] Virtuele machines gemaakt door de stapel van de manager Azure resource's extern inschakelen via Cloud Explorer in Visual Studio-2015. Zie [Azure Resources beheren met Cloud Explorer](http://go.microsoft.com/fwlink/?LinkId=623031)voor meer informatie.

### <a name="to-debug-an-azure-virtual-machine"></a>Voor foutopsporing van een Azure virtuele machines

1. Vouw het knooppunt virtuele Machines in Server Explorer en selecteer het knooppunt van de virtuele machine die u wilt opsporen.

1. Het snelmenu openen en selecteer **Inschakelen voor foutopsporing in**. Wanneer u wordt gevraagd als u zeker weet of u wilt inschakelen voor foutopsporing in op de virtuele machine, selecteert u **Ja**.

    Azure installeert u de externe foutopsporing extensie op de virtuele machine foutopsporing inschakelen.

    ![VM foutopsporing opdracht inschakelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure activiteit log](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Nadat de externe foutopsporing extensie is voltooid installeren, opent u het contextmenu van de virtuele machine en selecteert u **Foutopsporing bijvoegen**

    Azure ontvangt een lijst met de processen op de virtuele machine en worden aangegeven in de bijlage toevoegen aan het dialoogvenster proces.

    ![De foutopsporingsopdracht bijvoegen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Selecteer **selecteert u** de lijst met resultaten om weer te geven van de soorten code die u wilt opsporen beperken in het dialoogvenster **bijlage toevoegen aan het proces** . U kunt fouten opsporen in 32 - of 64-bits beheerde code, systeemeigen code of beide.

    ![Het dialoogvenster type code selecteren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Selecteer de processen die u wilt foutopsporing op de virtuele machine en selecteer vervolgens **toevoegen**. U kunt bijvoorbeeld het proces w3wp.exe kiezen als u wilt een web-app op de virtuele machine foutopsporing. [Fouten opsporen in een of meer processen in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) en [Azure rol architectuur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) Zie voor meer informatie.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Een webproject en een virtuele machine voor foutopsporing maken

Voordat u uw Azure project publiceert, u mogelijk handiger om deze te testen in een container-omgeving die ondersteuning biedt voor foutopsporing en testen van scenario's en waar u testen en controleren of programma's kunt installeren. Eén manier hiervoor is op afstand fouten opsporen in uw app op een virtuele machine.

Visual Studio ASP.NET-projecten bieden een optie voor het maken van een handige virtuele machine die u voor het testen van de app gebruiken kunt. De virtuele machine bevat meestal nodig eindpunten zoals PowerShell, extern bureaublad en WebDeploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Een webproject en een virtuele machine voor foutopsporing maken

1. Visual Studio, maak een nieuwe ASP.NET-webtoepassing.

1. Kies in het dialoogvenster Nieuw ASP.NET-Project, klikt u in de sectie Azure **virtuele machines** in de vervolgkeuzelijst. Laat het selectievakje **externe bronnen maken** is geselecteerd. Selecteer **OK** om door te gaan.

    Het dialoogvenster **maken virtuele machine op Azure** wordt weergegeven.


    ![Dialoogvenster voor ASP.NET web project maken](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Notitie:** U wordt gevraagd aan te melden bij uw Azure-account als u nog niet bent aangemeld.

1. Selecteer de verschillende instellingen voor de virtuele machine en selecteer vervolgens **OK**. Zie [virtuele Machines]( http://go.microsoft.com/fwlink/?LinkId=623033) voor meer informatie.

    De naam die u voor DNS-naam invoert is de naam van de virtuele machine. 

    ![VM op Azure dialoogvenster maken](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure wordt gemaakt van de virtuele machine en klik vervolgens op bepalingen en configureert de eindpunten, zoals Extern bureaublad- en Web implementeren



1. Nadat de virtuele machine volledig is geconfigureerd, selecteert u de VM knooppunt in Server Explorer.

1. Het snelmenu openen en selecteer **Inschakelen voor foutopsporing in**. Wanneer u wordt gevraagd als u zeker weet of u wilt inschakelen voor foutopsporing in op de virtuele machine, selecteert u **Ja**. 

    Azure installeert u de externe foutopsporing extensie in de virtuele machine foutopsporing inschakelen.

    ![VM foutopsporing opdracht inschakelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure activiteit log](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Uw project publiceren, zoals wordt beschreven in [hoe: implementeren een Web Project met één klik publiceren in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx). Omdat u wilt opsporen op de virtuele machine, op de pagina **Instellingen** van de wizard **Web publiceren** , selecteert u **fouten opsporen in** als de configuratie. Dit zorgt ervoor dat de code symbolen, terwijl foutopsporing beschikbaar zijn.

    ![Publicatie-instellingen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. In de **Opties voor het publiceren van bestand**, selecteer **Extra bestanden op bestemming verwijderen** als het project is al geïmplementeerd op een eerder moment.

1. Nadat het project, in het contextmenu van de virtuele machine in Server Explorer publiceert, selecteert u **Foutopsporing bijvoegen**

    Azure ontvangt een lijst met de processen op de virtuele machine en worden aangegeven in de bijlage toevoegen aan het dialoogvenster proces.

    ![De foutopsporingsopdracht bijvoegen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Selecteer **selecteert u** de lijst met resultaten om weer te geven van de soorten code die u wilt opsporen beperken in het dialoogvenster **bijlage toevoegen aan het proces** . U kunt fouten opsporen in 32 - of 64-bits beheerde code, systeemeigen code of beide.

    ![Het dialoogvenster type code selecteren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Selecteer de processen die u wilt foutopsporing op de virtuele machine en selecteer vervolgens **toevoegen**. U kunt bijvoorbeeld het proces w3wp.exe kiezen als u wilt een web-app op de virtuele machine foutopsporing. [Fouten opsporen in een of meer processen in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) Zie voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Gebruik **Intellitrace** voor het verzamelen van een logboek van gesprekken en gebeurtenissen van een server release. Zie [voor foutopsporing in een gepubliceerde Cloudservice met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).
- Gebruik **Diagnostisch hulpprogramma Azure** aan te melden gedetailleerde informatie uit de code die wordt uitgevoerd binnen rollen, of de rollen worden uitgevoerd in de ontwikkelomgeving of in Azure wordt aangegeven. Zie [verzamelen gegevens met behulp van Azure diagnostische gegevens vastleggen](http://go.microsoft.com/fwlink/p/?LinkId=400450).
