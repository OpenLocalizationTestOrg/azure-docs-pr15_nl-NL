<properties
 pageTitle="Taken aan een taalpakket HPC cluster in Azure indienen | Microsoft Azure"
 description="Meer informatie over het instellen van de computer van een on-premises implementatie taken kunnen verzenden naar een HPC Pack cluster in Azure wordt aangegeven"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>HPC taken vanaf een lokale computer aan een HPC Pack cluster geïmplementeerd in Azure wordt aangegeven

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Een lokale clientcomputer taken met een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure configureren. In dit artikel leest u hoe u een lokale computer met clienthulpprogramma's instelt om in te dienen taak via HTTPS aan het cluster in Azure wordt aangegeven. Op deze manier kunnen meerdere cluster gebruikers taken naar een HPC Pack cloudgebaseerde mogelijk, maar zonder verbinding maken met het hoofd knooppunt VM rechtstreeks of toegang krijgen tot een Azure-abonnement verzenden.

![Een taak aan een cluster in Azure verzenden][jobsubmit]

## <a name="prerequisites"></a>Vereisten voor

* **HPC Pack hoofd knooppunt geïmplementeerd in een VM Azure** - is raadzaam dat u geautomatiseerde hulpprogramma's zoals een [Azure quickstart sjabloon](https://azure.microsoft.com/documentation/templates/) of een [Azure PowerShell-script](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) gebruikt de hoofd knooppunt en cluster implementeren. Moet u de DNS-naam van het hoofd knooppunt en de referenties van de beheerder van een cluster de stappen in dit artikel.

* **Clientcomputer** - moet u een clientcomputer voor Windows of Windows Server die HPC Pack client hulpprogramma's (Zie [systeemvereisten](https://technet.microsoft.com/library/dn535781.aspx)) kan worden uitgevoerd. Als u alleen de HPC Pack webportal of REST API gebruiken om in te dienen taken wilt, kunt u elke clientcomputer van uw keuze.

* **Installatiemedia HPC Pack** - voor het installeren van de HPC Pack clienthulpprogramma's, het gratis installatiepakket te gaan naar de meest recente versie van HPC Pack (HPC Pack 2012 R2) is verkrijgbaar via het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=328024). Zorg ervoor dat u dezelfde versie van HPC Pack dat is geïnstalleerd op het hoofd knooppunt VM downloaden.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Stap 1: Installeren en configureren van de webonderdelen op het hoofd knooppunt

Zorg ervoor dat de HPC Pack web components zijn geconfigureerd op de kop knooppunt HPC Pack zodat een REST API interface taken kunnen verzenden naar het cluster via HTTPS. Als ze worden niet al is geïnstalleerd, moet u eerst de web components installeren door het installatiebestand HpcWebComponents.msi uit te voeren. Configureer de onderdelen vervolgens door het HPC PowerShell-script **Set-HPCWebComponents.ps1**uit te voeren.

Zie de [installatie van Microsoft HPC Pack Web Components](http://technet.microsoft.com/library/hh314627.aspx)voor gedetailleerde procedures.

>[AZURE.TIP] Bepaalde sjablonen Azure quickstart voor HPC Pack installeren en configureren van de web components automatisch. Als u het [HPC Pack IaaS implementatiescript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) maken van het cluster gebruikt, kunt u desgewenst installeren en configureren van de web components als onderdeel van de implementatie.

**Voor het installeren van de web components**

1. Verbinding maken met het hoofd knooppunt VM met behulp van de referenties van de beheerder van een cluster.

2. Uit de map HPC Pack Setup HpcWebComponents.msi worden uitgevoerd op het hoofd knooppunt.

3. Volg de stappen in de wizard voor het installeren van de web components

**Voor het configureren van de web components**

1. Klik op het knooppunt hoofd HPC PowerShell als een beheerder te starten.

2. Als u de map naar de locatie van het configuratiescript, typt u de volgende opdracht uit:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Typ de volgende opdracht uit om de REST-interface configureren en start de webservice HPC:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Wanneer u hierom wordt gevraagd om een certificaat te selecteren, kiest u het certificaat dat met de naam van het hoofd knooppunt overeenkomt. Bijvoorbeeld als u het hoofd knooppunt VM met het implementatiemodel klassieke implementeert, de certificaatnaam eruit CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Als u het implementatiemodel resourcemanager gebruikt, de certificaatnaam eruit CN =&lt;*HeadNodeDnsName*&gt;. &lt; *regio*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] U selecteert dit certificaat later wanneer u taken aan het hoofd knooppunt vanaf een lokale computer toevoegen. Niet selecteren of een certificaat dat met de naam van de computer van het hoofd knooppunt in de Active Directory-domein overeenkomt configureren (bijvoorbeeld CN =*MyHPCHeadNode.HpcAzure.local*).

5. Als u wilt configureren de webportal voor indiening van de taak, typt u de volgende opdracht uit:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Nadat het script is voltooid, stoppen en de HPC taak Scheduler-Service opnieuw starten door te typen van de volgende opdrachten:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Stap 2: De hulpprogramma's HPC Pack-client installeren op een lokale computer

Als u de hulpprogramma's HPC Pack-client installeren op uw computer wilt, kunt u de installatiebestanden HPC Pack (volledige installatie) downloaden vanuit het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=328024). Wanneer u begint met de installatie, kiest u de optie voor het **hulpprogramma's HPC Pack-client**.

Als u wilt gebruiken het HPC Pack clienthulpprogramma's taken kunnen verzenden naar het hoofd knooppunt VM, moet u ook een certificaat exporteren vanuit het hoofd knooppunt en installeer het op de clientcomputer. Het certificaat moet in. CER-indeling.

**Het certificaat exporteren vanuit het hoofd knooppunt**

1. Klik op het knooppunt hoofd toevoegen de module Certificaten aan een Microsoft Management Console voor een account van de lokale Computer. Zie [de module Certificaten aan een MMC toevoegen](https://technet.microsoft.com/library/cc754431.aspx)voor stappen om toe te voegen de module.

2. Vouw in de consolestructuur **certificaten – lokale Computer** > **persoonlijke**, en klik vervolgens op **certificaten**.

3. Zoek het certificaat dat u hebt geconfigureerd voor de webonderdelen HPC Pack in [stap 1: installeren en configureren van de webonderdelen op het hoofd knooppunt](#step-1:-install-and-configure-the-web-components-on-the-head-node) (bijvoorbeeld CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Met de rechtermuisknop op het certificaat en klik op **Alle taken** > **exporteren**.

5. Klik in de Wizard Certificaat exporteren op **volgende**en zorg ervoor dat de optie **Nee, de persoonlijke sleutel niet exporteren** is geselecteerd.

6. De overige stappen van de wizard exporteren van het certificaat in binaire X.509 DER-codering (. CER)-indeling.


**Het certificaat op de clientcomputer importeren**


1. Kopieer het certificaat dat u hebt geïmporteerd uit het hoofd knooppunt naar een map op de clientcomputer.

2. Voer op de clientcomputer certmgr.msc.

3. Klik in Certificaatbeheer uitvouwen **certificaten-huidige gebruiker** > **Certificeringsinstanties die vertrouwde basiscertificaten**, met de rechtermuisknop op **certificaten**en klik vervolgens op **Alle taken** > **importeren**.

4. Klik in de Wizard Certificaat importeren Klik op **volgende** en volg de stappen voor het importeren van het certificaat dat u hebt geïmporteerd uit het hoofd knooppunt naar de hoofdsite certificeringsinstanties vertrouwde store.



>[AZURE.TIP] Ziet u mogelijk een beveiligingswaarschuwing, omdat de certificeringsinstantie op het hoofd knooppunt wordt niet herkend door de clientcomputer. Voor testdoeleinden, kunt u deze waarschuwing negeren en het certificaat importeren te voltooien.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Stap 3: Uitvoeren testtaken op het cluster

Om te controleren of uw configuratie, probeert u de taken van de lokale computer waarop het cluster in Azure wordt aangegeven. U kunt bijvoorbeeld HPC Pack grafische interface of opdrachtregel opdrachten taken kunnen verzenden naar het cluster gebruiken. U kunt ook een web-e-portal gebruiken om in te dienen taken.


**Taak indiening opdrachten uitvoeren op de clientcomputer**


1. Klik op een clientcomputer waarop de HPC Pack client hulpprogramma's zijn geïnstalleerd, start u een opdrachtprompt.

2. Typ de opdracht van een steekproef. Als alle taken op het cluster, typt u bijvoorbeeld een opdracht die vergelijkbaar is met een van de volgende opties, afhankelijk van de volledige DNS-naam van het hoofd knooppunt u:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    of
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] Gebruik de volledige DNS-naam van het hoofd knooppunt, niet het IP-adres, in de URL scheduler. Als u het IP-adres opgeeft, lijkt een fout op "certificaat van de server moet hebben een geldige reeks vertrouwen of in de winkel vertrouwde basiscertificeringsinstantie worden geplaatst."

3. Wanneer u wordt gevraagd, typt u de gebruikersnaam in te voeren (in het formulier &lt;domeinnaam&gt;\\&lt;gebruikersnaam&gt;) en het wachtwoord van de HPC cluster-beheerder of een ander cluster-gebruiker die u hebt geconfigureerd. U kunt kiezen voor de opslag van de referenties lokaal voor meer taakbewerkingen.

    Er verschijnt een lijst met taken.


**Gebruik van HPC taak Manager op de clientcomputer**

1. Als u geen domeinreferenties voor een gebruiker cluster eerder opslaan bij het verzenden van een taak, kunt u de referenties in Referentiebeheer toevoegen.

    een. Klik in het Configuratiescherm op de clientcomputer start Referentiebeheer.

    b. Klik op **Windows-referenties** > **toevoegen een algemene referentie**.

    c. Geef het internetadres (bijvoorbeeld https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler of https://&lt;HeadNodeDnsName&gt;.&lt; regio&gt;.cloudapp.azure.com/HpcScheduler), en de gebruikersnaam in te voeren (&lt;domeinnaam&gt;\\&lt;gebruikersnaam&gt;) en het wachtwoord van de cluster-beheerder of een ander cluster-gebruiker die u hebt geconfigureerd.

2. Start HPC taak Manager op de clientcomputer.

3. Typ de URL voor het hoofd knooppunt in Azure wordt aangegeven in het dialoogvenster **Selecteer hoofd knooppunt** (bijvoorbeeld https://&lt;HeadNodeDnsName&gt;. cloudapp.net of https://&lt;HeadNodeDnsName&gt;.&lt; regio&gt;. cloudapp.azure.com).

    HPC taak Manager wordt geopend en wordt een lijst met taken op het hoofd knooppunt.

**Gebruik van de webportal op het hoofd knooppunt**

1. Start een webbrowser op de clientcomputer en voer een van de volgende adressen, afhankelijk van de volledige DNS-naam van het hoofd knooppunt:

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    of
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Typ in het beveiligingsdialoogvenster dat wordt weergegeven, de domeinreferenties van de beheerder van de cluster HPC. (U kunt ook andere cluster gebruikers toevoegen in verschillende rollen. Zie [Cluster-gebruikers beheren](https://technet.microsoft.com/library/ff919335.aspx).)

    De webportal wordt geopend aan de taak lijstweergave.

3. Als u wilt de taak van een steekproef die resulteert in de tekenreeks "Hallo wereld" uit het cluster verzenden, klikt u op **nieuwe taak** in het linkernavigatievenster.

4. Klik op de pagina **Nieuwe taak** **van indiening pagina's**, klik op **Hallo wereld**. De taak indiening pagina wordt weergegeven.

5. Klik op **verzenden**. Als u wordt gevraagd, vindt u de domeinreferenties van de beheerder van de cluster HPC. De taak wordt ingediend en de taak-ID wordt weergegeven op de pagina **Mijn taken** .

6. U kunt de resultaten van de taak die u hebt verzonden, klikt u op de taak-ID en klik vervolgens op **Taken weergeven** om de uitvoer van de opdracht (onder **uitvoer**) weer te geven.

## <a name="next-steps"></a>Volgende stappen

* U kunt ook taken aan het Azure cluster met het [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx)indienen.

* Als u verzenden cluster taken van een client Linux wilt, raadpleegt u de steekproef Python in de [HPC Pack 2012 R2 SDK en Code van de steekproef](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
