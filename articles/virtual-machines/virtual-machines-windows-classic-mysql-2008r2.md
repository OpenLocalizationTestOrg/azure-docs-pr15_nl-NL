<properties
    pageTitle="Maken van een VM uitgevoerd MySQL | Microsoft Azure"
    description="Maak een Azure virtuele machines met Windows Server 2012 R2 en het gebruik van het implementatiemodel klassieke MySQL-database."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>MySQL installeren op een virtuele machine die zijn gemaakt met het klassieke implementatiemodel met Windows Server 2012 R2

[MySQL](http://www.mysql.com) is een bron voor populaire openen, de SQL-database. Deze zelfstudie ziet u hoe u installeren en uitvoeren van de community-versie van MySQL 5.6.23 als een MySQL-Server op een virtuele machine met Windows Server 2012 R2. Raadpleeg voor instructies over het installeren van MySQL op Linux,: [het installeren van MySQL op Azure](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>Een virtuele machine met Windows Server 2012 R2 maken

Als u een met Windows Server 2012 R2 VM nog geen hebt, kunt u deze [zelfstudie](virtual-machines-windows-classic-tutorial.md) de virtuele machine maken. 

## <a name="attach-a-data-disk"></a>Een gegevensschijf koppelen

Nadat de virtuele machine is gemaakt, kunt u desgewenst een schijf aanvullende gegevens koppelen. Dit wordt aanbevolen voor productie werkbelasting en om te voorkomen gebrek aan ruimte op de schijf OS (C:), waaronder het besturingssysteem.

Lees [hoe u een gegevensschijf aan een virtuele Windows-computer als bijlage toevoegen](virtual-machines-windows-classic-attach-disk.md) en volg de instructies voor het koppelen van een lege schijf. De instelling van de cache host ingesteld op **geen** of **alleen-lezen**.

## <a name="log-on-to-the-virtual-machine"></a>Meld u aan bij de virtuele machine

Vervolgens gaat u [Meld u aan bij de virtuele machine](virtual-machines-windows-classic-connect-logon.md) zodat u MySQL kunt installeren.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Installeren en uitvoeren van MySQL-Community-Server op de virtuele machine

Ga als volgt te werk als u wilt installeren, configureren en uitvoeren van de Community-versie van MySQL-Server:

> [AZURE.NOTE] Deze stappen zijn voor de 5.6.23.0 Community-versie van MySQL- en Windows Server 2012 R2. Uw ervaring mogelijk verschillende voor verschillende versies van MySQL- of Windows Server.

1.  Nadat u hebt verbonden met de virtuele machine met extern bureaublad, klikt u op **Internet Explorer** vanuit het startscherm.
2.  Selecteer de knop **Extra** in de rechterbovenhoek (het wielpictogram cogged) en klik vervolgens op **Internetopties**. Klik op het tabblad **beveiliging** , klik op het pictogram **Vertrouwde websites** en klik vervolgens op de knop **Sites** . Http://*toevoegen. mysql.com aan de lijst met vertrouwde websites. Klik op * *sluiten**, en klik vervolgens op * *OK**.
3.  In de adresbalk van Internet Explorer, typ http://dev.mysql.com/downloads/mysql/.
4.  De MySQL-site gebruiken om te zoeken en downloaden van de nieuwste versie van het installatieprogramma MySQL voor Windows. Bij het kiezen van het installatieprogramma van MySQL, moet u de versie die de volledige set (bijvoorbeeld de mysql-installer-community-5.6.23.0.msi met een bestandsgrootte van 282.4 MB) bestanden en sla het installatieprogramma heeft downloaden.
5.  Wanneer het installatieprogramma van het downloaden is voltooid, klikt u op **uitvoeren** om setup starten.
6.  Klik op de pagina **Licentieovereenkomst** accepteer de licentieovereenkomst en klik op **volgende**.
7.  Klik op het gewenste installatietype op de pagina **Type installatie kiezen** en klik vervolgens op **volgende**. De volgende stappen wordt ervan uitgegaan dat de selectie van het type van de instelling **alleen Server** .
8.  Klik op de pagina **installatie** op **uitvoeren**. Wanneer de installatie is voltooid, klikt u op **volgende**.
9.  Klik op **volgende**op de pagina **Configuratie van Product** .
10. Geef het gewenste type- en -connectiviteit opties, waaronder de TCP-poorten indien nodig op de pagina **Type en netwerken** . Selecteer **Geavanceerde opties weergeven**en klik vervolgens op **volgende**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. Geef een sterk wachtwoord voor MySQL-hoofdmap op de pagina **Accounts en rollen** . Zo nodig extra MySQL-gebruikersaccounts toevoegen en klik vervolgens op **volgende**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. Wijzigingen in de standaardinstellingen opgeven voor het uitvoeren van de MySQL-Server als een Windows-service zo nodig op de pagina **Windows-Service** en klik vervolgens op **volgende**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. Geef de gewenste wijzigingen in opties voor logboekregistratie op op de pagina **Geavanceerde opties** en klik vervolgens op **volgende**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. Klik op de pagina **Serverconfiguratie toepassen** op **uitvoeren**. Wanneer de volgende configuratiestappen uit voltooid zijn, klikt u op **Voltooien**.
15. Klik op **volgende**op de pagina **Configuratie van Product** .
16. Klik op de pagina voor de **Installatie is voltooid** , klikt u op **Log kopiëren naar het Klembord** als u wilt deze later bekijken en klik vervolgens op **Voltooien**.
17. Vanuit het startscherm, typ **mysql**en klik vervolgens op **MySQL 5.6 opdrachtregel Client**.
18. Voer het wachtwoord van hoofdmap dat u hebt opgegeven in stap 11 en er moet worden weergegeven met een prompt waar kunt u de opdrachten voor het configureren van MySQL uitgeven. Zie voor een overzicht van opdrachten en syntaxis, [MySQL-referentiehandleidingen](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. U kunt ook server configuration standaardinstellingen, zoals de base en gegevens mappen en stations, configureren met vermeldingen in het bestand C:\Program Files (x86) \MySQL\MySQL Server 5.6\my-default.ini. Zie voor meer informatie [5.1.2 Server Configuration standaard](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Eindpunten configureren

Als u wilt dat de MySQL-Server-service kan worden gecontroleerd voor MySQL-clientcomputers op Internet, moet u een eindpunt configureert voor de TCP-poorten waarop u de MySQL-Server-service luistert en maak een extra regel van Windows Firewall. Dit is TCP-poorten 3306 tenzij u een andere poort op de pagina **Type en netwerken** (stap 10 van de vorige procedure).


> [AZURE.NOTE] Overweeg de beveiligingsrisico's hierdoor, omdat dit de MySQL-Server-service beschikbaar voor alle computers op Internet stelt. U kunt de set bron IP-adressen die zijn toegestaan met een Access (Toegangsbeheerlijst) gebruik van het eindpunt definiëren. Lees [hoe u omhoog eindpunten instellen met een virtuele Machine](virtual-machines-windows-classic-setup-endpoints.md)voor meer informatie.


Een eindpunt voor de MySQL-Server-service configureren:

1.  In de klassieke portal Azure **virtuele Machines**klikt u op, klik op de naam van uw MySQL virtuele machine en klik vervolgens op **eindpunten**.
2.  In de opdrachtenbalk, klikt u op **toevoegen**.
3.  Klik op de pagina **toevoegen een eindpunt met een virtuele machine** op de pijl-rechts.
4.  Als u de standaard MySQL TCP-poorten van 3306 gebruikt, klikt u op **MySQL** in het vak **naam**en klik vervolgens op het vinkje.
5.  Als u een andere TCP-poort gebruikt, typt u een unieke naam in het vak **naam**. Selecteer **TCP** in protocol, typt u het poortnummer in zowel **Openbare poort** als **Privé poort**en klik vervolgens op het vinkje.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Een regel voor Windows Firewall toestaan MySQL-verkeer toevoegen

Als u wilt toevoegen van een regel voor Windows Firewall waarmee MySQL-verkeer van Internet, voer de volgende opdracht bij de opdrachtprompt met verhoogde bevoegdheid Windows PowerShell op de MySQL-server virtuele machine.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>Uw verbinding met extern testen


Als u wilt testen van uw externe verbinding met de service MySQL-Server wordt uitgevoerd op de Azure virtuele machine, moet u eerst de DNS-naam die overeenkomt met de cloudservice met de virtuele machine met MySQL-Server te bepalen.

1.  Klik in de klassieke portal Azure **virtuele Machines**klikt u op, klik op de naam van uw VM MySQL-server en klik vervolgens op **Dashboard**.
2.  Noteer de **Naam van de DNS** -waarde onder de sectie **Snelle bekijken** vanuit het dashboard VM. Hier volgt een voorbeeld:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  Vanaf een lokale computer MySQL of de MySQL-client uitgevoerd, voer de volgende opdracht aan te melden als een MySQL-gebruiker.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    Bijvoorbeeld, voor de MySQL gebruiker naam dbadmin3 en de naam van de DNS-testmysql.cloudapp.net voor de virtuele machine, gebruikt u de volgende opdracht uit.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het uitvoeren van MySQL, de [MySQL-documentatie](http://dev.mysql.com/doc/).
