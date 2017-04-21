#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Het gebruik van de Azure opdrachtregel hulpprogramma's voor Mac en Linux

Deze handleiding wordt beschreven hoe u met de hulpmiddelen Azure opdrachtregelopties voor Mac en Linux maken en beheren van services in Azure wordt aangegeven. De scenario's waarvoor zijn **voor het installeren van de hulpmiddelen voor** **het importeren van uw publicatie-instellingen**, **maken en beheren van Azure Websites**en **maken en beheren van Azure virtuele Machines**. Zie voor uitgebreide naslagwerk documentatie [opdrachtregel hulpprogramma Azure voor Mac en Linux documentatie][reference-docs]. 

##<a name="table-of-contents"></a>Inhoudsopgave
* [Wat zijn de Azure opdrachtregel hulpprogramma's voor Mac en Linux](#Overview)
* [Het installeren van de Azure opdrachtregel hulpprogramma's voor Mac en Linux](#Download)
* [Het maken van een Azure-account](#CreateAccount)
* [Het downloaden en importeren publicatie-instellingen](#Account)
* [Het maken en beheren van een Azure-website](#WebSites)
* [Het maken en beheren van een Azure virtuele machines](#VMs)


<h2><a id="Overview"></a>Wat zijn de Azure opdrachtregel hulpprogramma's voor Mac en Linux</h2>

Het hulpprogramma's voor opdrachtregel Azure voor Mac en Linux zijn een aantal opdrachtregel hulpprogramma's voor de implementatie en Azure services beheren.
 
De ondersteunde taken zijn:

* Publicatie-instellingen importeren.
* Maken en beheren van Azure-Websites.
* Maken en beheren van Azure virtuele Machines.

Voor een volledige lijst met ondersteunde opdrachten, typt u `azure -help` via de opdrachtregel na de installatie van stukken gereedschap, of raadpleegt u de [documentatie][reference-docs].

<h2><a id="Download">Het installeren van de Azure opdrachtregel hulpprogramma's voor Mac en Linux</a></h2>

De volgende lijst bevat informatie voor het installeren van de opdrachtregel hulpmiddelen, afhankelijk van het besturingssysteem:

* **Mac**: het [installatieprogramma van Azure SDK]downloaden[mac-installer]. Open het bestand gedownloade .pkg en voltooi de installatiestappen zoals u wordt gevraagd.

* **Linux**: Installeer de meest recente versie van [Node.js] [ nodejs-org] (Zie [Node.js installeren via Package Manager][install-node-linux]), voert u de volgende opdracht uit:

        npm install azure-cli -g

    **Opmerking**: mogelijk moet u deze opdracht uitvoert met verhoogde bevoegdheden:

        sudo npm install azure-cli -g

* **Windows**: Voer het Winows installatieprogramma (MSI-bestand), die hier beschikbaar is: [Azure opdrachtregel extra][windows-installer].


Als u wilt testen de installatie, typ `azure` bij de opdrachtprompt. Als de installatie voltooid is, ziet u een lijst met alle beschikbare `azure` opdrachten.

<h2><a id="CreateAccount"></a>Het maken van een Azure-account</h2>

Als u wilt gebruiken de Azure opdrachtregel hulpprogramma's voor Mac en Linux, moet u een Azure-account.

Open een webbrowser en bladert u naar [http://www.windowsazure.com] [ windowsazuredotcom] en klik op **gratis proefversie** in de rechterbovenhoek.

![Azure website][Azure Web Site]

Volg de instructies voor het maken van een account.

<h2><a id="Account"></a>Het downloaden en importeren publicatie-instellingen</h2>

Als u wilt beginnen, moet u eerst downloaden en importeren uw publicatie-instellingen. Hierdoor kunt u het hulpprogramma gebruiken om te maken en beheren van Azure Services. Downloaden van uw publicatie-instellingen, voert u de `account download` opdracht:

    azure account download

Hiermee wordt uw standaardbrowser openen en u aanmelden bij de beheerportal wordt gevraagd. Na het aanmelden, uw `.publishsettings` bestand wordt gedownload. Noteer waar dit bestand is opgeslagen.

Importeer vervolgens de `.publishsettings` bestand door de volgende opdracht uit te voeren vervangen `{path to .publishsettings file}` door het pad naar uw `.publishsettings` bestand:

    azure account import {path to .publishsettings file}

Kunt u de informatie die is opgeslagen door de <code>import</code> command met behulp van de <code>account clear</code> opdracht:

    azure account clear

Voor een overzicht van opties voor `account` opdrachten, maken gebruik van de `-help` optie:

    azure account -help

Na het importeren van uw publicatie-instellingen en u moet verwijderen de `.publishsettings` bestand vanwege de beveiliging.

> [AZURE.NOTE] Wanneer u importeert publicatie-instellingen, de referenties voor toegang tot uw Azure abonnement worden opgeslagen in uw `user` map. Uw `user` map door het besturingssysteem is beveiligd. Maar het wordt aanbevolen dat u extra stappen uitvoeren om te versleutelen uw `user` map. U kunt dit doen in de volgende manieren:    
> 
> - Klik op Windows, de mapeigenschappen wijzigen of BitLocker gebruiken.
> - Schakel op de Mac, FileVault voor de map.
> - Gebruik de functie van de map versleutelde start op Ubuntu. Andere onderzoeken Linux bieden equivalente functies.

U bent nu klaar om te worden maken en beheren van Azure Websites en Azure virtuele Machines.  

<h2><a id="WebSites"></a>Het maken en beheren van een Azure-Website</h2>

###<a name="create-a-website"></a>Een Website maken

Als u wilt een Azure website hebt gemaakt, moet u eerst een lege map genaamd maakt `MySite` en bladert u naar deze map.

Voer de volgende opdracht uit:

    azure site create MySite --git

De uitvoer van deze opdracht bevat de standaard-URL voor de nieuwe website. De `--git` optie kunt u cijfer publiceren naar uw website door te maken van cijfer opslagplaatsen in beide telefoonboek van uw lokale toepassing en in het datacenter van uw website gebruiken. Houd er rekening mee dat als uw lokale map al een cijfer-bibliotheek is, de opdracht een nieuwe externe worden toegevoegd aan de bestaande opslagplaats, die wijst naar de bibliotheek in datacenter van uw website.

Opmerking die u kunt uitvoeren de `azure site create` command met een van de volgende opties:

* `--location [location name]`. Deze optie kunt u de locatie van het datacenter waarin uw website, (bijvoorbeeld ' West ons") wordt gemaakt. Als u deze optie weglaat, zult u promted een locatie te kiezen.
* `--hostname [custom host name]`. Deze optie kunt u een aangepaste hostname voor uw website opgeven.

U kunt vervolgens inhoud toevoegen aan het telefoonboek van uw website. Gebruik de stroom normale cijfer (`git add`, `git commit`) om door te voeren van uw inhoud. Gebruik de volgende opdracht uit cijfer naar uw website-inhoud naar Azure push: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>Publicatie van GitHub instellen

Als u doorlopend publiceren uit een bibliotheek GitHub instelt, gebruikt u de `--GitHub` option bij het maken van een site:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Als u een lokale kopie van een bibliotheek GitHub hebt of als u een bibliotheek met één externe verwijzing naar een bibliotheek GitHub hebt, wordt deze opdracht automatisch code publiceren in de bibliotheek GitHub aan uw site. Daarna wordt verplaatst naar de bibliotheek GitHub wijzigingen automatisch worden gepubliceerd naar uw site.

Wanneer u de publicatie hebt ingesteld in GitHub, is de standaard-vertakking gebruikt de basispagina tak. Als u wilt een ander tak opgeven, voer de volgende opdracht uit uw lokale bibliotheek:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>App-instellingen configureren

App-instellingen zijn sleutel-waardeparen die beschikbaar voor uw toepassing gedurende runtime zijn. Als de waarde voor een Website Azure, app instellingswaarden wordt overschreven door instellingen met dezelfde sleutel die zijn gedefinieerd in uw siteniveau. Voor Node.js en PHP-toepassingen zijn de instellingen van de app beschikbaar als omgevingsvariabelen. Het volgende voorbeeld ziet u hoe u een paar sleutelwaarden instellen:

    azure site config add <key>=<value> 

Voor een overzicht van alle sleutel/waardeparen, gebruikt u de volgende handelingen uit:

    azure site config list 

Of als u de sleutel weten en wilt zien van de waarde, u kunt gebruiken:

    azure site config get <key> 

Als u wilt wijzigen van de waarde van een bestaande sleutel moet u eerst de bestaande sleutel wissen en vervolgens opnieuw toe te voegen. De opdracht wissen is:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>Een lijst met en sites weergeven

U kunt uw websites gebruiken de volgende opdracht uit:

    azure site list

Als u gedetailleerde informatie over een site, gebruikt de `site show` opdracht. Het volgende voorbeeld ziet u details voor `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Stoppen, starten of opnieuw starten van een site

U kunt stoppen, starten of opnieuw starten van een site met de `site stop`, `site start`, of `site restart` opdrachten:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Een site verwijderen

Tot slot kunt u een site met verwijderen de `site delete` opdracht:

    azure site delete MySite

Als u een van bovenstaande opdrachten uit in de map waarop u hebt uitgevoerd wordt uitgevoerd `site create`, hoeft u niet kunt u de naam van de site opgeven `MySite` als laatste parameter.

Om te zien van een volledige lijst van `site` opdrachten, maken gebruik van de `-help` optie:

    azure site -help 

<h2><a id="VMs"></a>Het maken en beheren van een Azure virtuele machines</h2>

een Azure virtuele machines wordt gemaakt van een virtuele machine afbeelding (VHD-bestand) dat u opgeeft of die beschikbaar is in de galerie met afbeelding. Als u wilt zien van afbeeldingen die beschikbaar zijn, gebruikt u de `vm image list` opdracht:

    azure vm image list

U kunt inrichten en een virtuele machine starten vanuit een van de beschikbare afbeeldingen met de `vm create` opdracht. Het volgende voorbeeld ziet u hoe u een Linux virtuele machine maken (genoemd `myVM`) uit een afbeelding in de galerie met afbeelding (CentOS 6.2). De hoofdmap-gebruikersnaam en wachtwoord voor de virtuele machine zijn `myusername` en `Mypassw0rd` respectievelijk. (Houd er rekening mee dat het `--location` parameter geeft de Datacenter waarin de virtuele machine is gemaakt. Als u weglaat de `--location` parameter, wordt u gevraagd een locatie te kiezen.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Kunt u overwegen doorgeven de `--ssh` vlag (Linux) of `--rdp` markering (Windows) `vm create` externe verbindingen met de zojuist gemaakte virtuele machine inschakelen.

Als u liever van een virtuele machine uit een aangepaste afbeelding inrichten wilt, kunt u een afbeelding uit een VHD-bestand met de `vm image create` opdracht en gebruik de `vm create` opdracht voor het inrichten van de virtuele machine. Het volgende voorbeeld ziet u het maken van een afbeelding Linux (genoemd `myImage`) uit een lokale VHD-bestand. (De `--location` parameter geeft de gegevens waarin de afbeelding is opgeslagen.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

In plaats van een afbeelding uit een lokale VHD maakt, kunt u een afbeelding maken van een VHD die zijn opgeslagen in Azure-blobopslag. U kunt hiervoor met de `blob-url` parameter:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Na het maken van een afbeelding, kunt u een virtuele machine van de afbeelding inrichten met behulp van `vm create`. De opdracht hieronder Hiermee maakt u een virtuele machine genoemd `myVM` van de bovenstaande afbeelding (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Nadat u een virtuele machine ingericht hebt, wilt u mogelijk eindpunten voor externe toegang aan uw virtuele computer (bijvoorbeeld) maken. Het volgende voorbeeld wordt de `vm create endpoint` opdracht bij het openen externe poort 22 en lokale poort 22 `myVM`:

    azure vm endpoint create myVM 22 22

Vind u gedetailleerde informatie over een virtuele machine (inclusief IP-adres, de naam van de DNS- en eindpuntgegevens) met de `vm show` opdracht:

    azure vm show myVM

Afsluiten, starten, of de virtuele machine opnieuw, een van de volgende opdrachten gebruiken:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

En ten slotte, als u wilt verwijderen de VM, gebruikt u de `vm delete` opdracht:

    azure vm delete myVM

Voor een volledige lijst met opdrachten voor het maken en beheren van virtuele machines, gebruikt u de `-h` optie:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

