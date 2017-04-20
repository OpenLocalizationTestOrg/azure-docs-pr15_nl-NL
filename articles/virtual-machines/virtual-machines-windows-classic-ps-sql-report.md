<properties 
    pageTitle="PowerShell gebruiken voor het maken van een VM waarbij een rapportserver Native modus | Microsoft Azure"
    description="In dit onderwerp wordt beschreven en begeleidt u bij de implementatie en configuratie van de rapportserver van een SQL Server Reporting Services-native modus in een Azure Virtual Machine. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management"/>
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>PowerShell gebruiken voor het maken van een Azure VM waarbij een rapportserver Native modus

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

In dit onderwerp wordt beschreven en begeleidt u bij de implementatie en configuratie van de rapportserver van een SQL Server Reporting Services-native modus in een Azure Virtual Machine. De stappen in dit document gebruik een combinatie van de handmatige stappen om de virtuele machine en een Windows PowerShell-script om Reporting Services configureren op de VM te maken. De configuratiescript bevat een firewallpoort voor HTTP- of HTTPs te openen.

>[AZURE.NOTE] Als u geen **HTTPS** op de rapportserver, **slaat u stap 2**vereist.
>
>Nadat de VM is gemaakt in stap 1, gaat u naar de sectie script gebruiken voor het configureren van de rapportserver en HTTP. Nadat u het script uitvoeren, zijn de rapportserver is klaar voor gebruik.

## <a name="prerequisites-and-assumptions"></a>Vereisten en hypothesen

- **Azure-abonnement**: Controleer of het aantal cores beschikbaar in uw Azure-abonnement. Als u de aanbevolen VM grootte van **A3**maakt, moet u de beschikbare cores **4** . Als u de grootte van een VM van **A2**gebruikt, moet u de beschikbare cores **2** .
    
    - Om te controleren of de limiet core van uw abonnement, in de klassieke Azure portal, klik op instellingen op te geven in het linkerdeelvenster en vervolgens klikt u op gebruik in het bovenste menu.
    
    - Als u wilt vergroten het quotum voor core, neemt u contact [Azure-ondersteuning](https://azure.microsoft.com/support/options/). Zie voor informatie over de grootte van VM [VM grootten voor Azure](virtual-machines-linux-sizes.md).

- **Windows PowerShell-scripts**: het onderwerp wordt ervan uitgegaan dat er een basiskennis werken van Windows PowerShell. Zie de volgende onderwerpen voor meer informatie over het gebruik van Windows PowerShell:

    - [Windows PowerShell zo starten op Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Aan de slag met Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Stap 1: Een Azure virtuele machines inrichten

1. Blader naar de Azure klassieke portal.

1. Klik op **virtuele Machines** in het linkerdeelvenster.

    ![Microsoft azure virtuele machines](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. Klik op **Nieuw**.

    ![de knop Nieuw](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Klik op **van de galerie**.

    ![nieuwe vm uit galerie](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Klik op **SQL Server 2014 RTM Standard, Windows Server 2012 R2** en klik vervolgens op de pijl om door te gaan.

    ![volgende](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Als u de Reporting Services-functie voor abonnementen voor gegevensgestuurde nodig hebt, kiest u **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. Zie [Functies worden ondersteund door de edities van SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting)voor meer informatie over de edities van SQL Server en ondersteuningsfunctie.

1. Bewerk de volgende velden op de pagina **configuratie VM** :
                                    
    - Als er meer dan één **versie datum**is, selecteert u de meest recente versie.
    
    - **De naam van de VM**: de computernaam wordt ook gebruikt op de volgende pagina van de configuratie als de standaardnaam van de DNS Service Cloud. De DNS-naam moet uniek zijn in de Azure-service. Houd rekening met de VM configureren met de naam van een computer die wordt beschreven wat de VM voor wordt gebruikt. Bijvoorbeeld ssrsnativecloud.
    
    - **Laag**: standaard
    
    - **Grootte: A3** is de grootte van de aanbevolen VM voor SQL Server-werkbelastingen. Als een VM alleen als een rapportserver gebruikt wordt, is het een VM hoeveelheid A2 voldoende, tenzij de rapportserver een grote werkbelasting ervaringen. Zie [Virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/)voor VM prijsinformatie.
    
    - **Nieuwe gebruikersnaam in te voeren**: de naam die u opgeeft als administrator op de VM wordt gemaakt.
    
    - **Een nieuw wachtwoord** en **Bevestig**. Dit wachtwoord wordt gebruikt voor de nieuwe beheerdersaccount en het wordt aanbevolen dat een sterk wachtwoord.
    
    - Klik op **volgende**. ![volgende](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Klik op de volgende pagina bewerken de volgende velden:

    - **Cloudservice**: Selecteer **een nieuwe Cloudservice maken**.
    
    - **De naam van de DNS-Service cloud**: dit is de openbare DNS-naam van de Cloudservice die is gekoppeld aan de VM. De standaardnaam is de naam die u hebt opgegeven in voor de naam VM. Als u in de volgende stappen van het onderwerp, u een vertrouwde SSL-certificaat maken en vervolgens de DNS-naam voor de waarde van de '**verleend aan**' van het certificaat is gebruikt.
    
    - **Land/affiniteit groep/virtuele netwerk**: Kies het dichtst bevindt bij uw eindgebruikers regio.
    
    - **Opslag-Account**: een automatisch gegenereerde opslag gebruikt.
    
    - **Beschikbaarheid instellen**: geen.
    
    - **EINDPUNTEN** Houd de eindpunten **Extern bureaublad** en **PowerShell** en voegt u een HTTP of HTTPS-eindpunt, afhankelijk van uw omgeving.

        - **HTTP**: de standaard-openbare en persoonlijke-poort **80**zijn. Opmerking: als u een persoonlijke poort dan 80, wijzigen **$HTTPport = 80** in het http-script.

        - **HTTPS**: de standaard-openbare en persoonlijke-poort **443**zijn. Veiligheidsoverwegingen is de privé poort wijzigen en uw firewall en de rapportserver om de persoonlijke poort configureren. Zie voor meer informatie over de eindpunten, [het instellen van communicatie met een virtuele Machine](virtual-machines-windows-classic-setup-endpoints.md). Als u een andere poort dan 443, veranderen de parameter **$HTTPsport = 443** in het script HTTPS.
    
    - Klik op volgende. ![volgende](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Op de laatste pagina van de wizard, houd de standaard **de VM-agent installeren** geselecteerd. De stappen in dit onderwerp kunnen geen gebruik van de VM-agent, maar als u van plan bent om te houden dit VM, de VM-agent en uitbreidingen kunt u verbeteren hij CM.  Zie voor meer informatie over de VM-agent, [VM Agent en uitbreidingen – deel 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Een van de standaard-extensies geïnstalleerd advertentie met is de extensie "BGINFO" die moet worden weergegeven op het bureaublad VM, systeeminformatie zoals interne IP- en vrije schijfruimte.

1. Klik op voltooien. ![OK](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. De **Status** van de VM Hiermee wordt weergegeven als **starten (Provisioning)** tijdens het inrichten en vervolgens wordt weergegeven als **actief** wanneer de VM is ingericht en klaar voor gebruik.

## <a name="step-2-create-a-server-certificate"></a>Stap 2: Een certificaat maken

>[AZURE.NOTE] Als u geen HTTPS op de rapportserver vereist, kunt u **slaat u stap 2** en Ga naar de sectie **script gebruiken voor het configureren van de rapportserver en HTTP**. Gebruik het HTTP-script snel configureren de rapportserver en wordt de rapportserver klaar voor gebruik.

HTTPS op de VM gebruiken, moet u een vertrouwde SSL-certificaat. Afhankelijk van uw scenario, kunt u een van de volgende twee methoden:

- Een geldig SSL-certificaat uitgegeven door een certificeringsinstantie (CA) en vertrouwd door Microsoft. De CA-basiscertificaten zijn vereist om te worden gedistribueerd via het basiscertificaatprogramma van Microsoft. Zie voor meer informatie over dit programma [Windows en Windows Phone 8 SSL Basiscertificaatprogramma (lid CA's)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) en [Inleiding tot het basiscertificaatprogramma van Microsoft](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).

- Een zelfondertekend certificaat. Zelfondertekende certificaten worden niet aanbevolen voor productieomgevingen.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Gebruik een certificaat dat is gemaakt door een vertrouwde certificeringsinstantie (CA)

1. **Een certificaat voor de website bij een certificeringsinstantie aanvragen**. 

    U kunt de Wizard certificaat gebruiken om een certificaat verzoek bestand (Certreq.txt) die u naar een certificeringsinstantie verzendt te genereren, of om een aanvraag voor een online certificeringsinstantie te genereren. Bijvoorbeeld Microsoft Certificate Services in Windows Server 2012. Afhankelijk van het niveau van uw identiteit door het certificaat van uw server worden aangeboden, is het aantal dagen naar verschillende maanden voor de certificeringsinstantie goedkeurt van uw aanvraag kunt invullen en ontvangt u een certificaatbestand. 

    Zie de volgende onderwerpen voor meer informatie over het aanvragen van een servercertificaten: 
    
    - Gebruik [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
    
    - Beveiliging hulpmiddelen voor het beheren van Windows Server 2012.

    [Beveiliging hulpmiddelen voor het beheren van Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] Het veld **uitgegeven aan** van de vertrouwde SSL-certificaat moet hetzelfde als de **Cloud Service DNS-naam** die u voor de nieuwe VM gebruikt.

1. **Het servercertificaat op de webserver installeren**. De webserver is in dit geval de VM waarop de rapportserver en de website wordt gemaakt in de volgende stappen wanneer u Reporting Services configureren. Zie [een Server-certificaat installeren](https://technet.microsoft.com/library/cc740068)voor meer informatie over het installeren van de servercertificaat op de webserver met behulp van het certificaat MMC-module.
    
    Als u gebruiken het script in dit onderwerp is inbegrepen wilt, om te configureren de rapportserver, de waarde van de certificaten **vingerafdruk** is vereist als een parameter van het script. Zie de volgende sectie voor meer informatie over het ophalen van de vingerafdruk van het certificaat.

1. Het servercertificaat toewijzen aan de rapportserver. De toewijzing is voltooid in het volgende gedeelte als u de rapportserver configureert.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Het virtuele Machines zelfondertekende certificaat gebruiken

Een zelfondertekend certificaat is gemaakt op de VM wanneer de VM is ingericht. Het certificaat heeft dezelfde naam als de naam van de VM DNS. Om te vermijden certificaatfouten, is het vereist dat het certificaat vertrouwd op de VM zelf, en ook door alle gebruikers van de site wordt.

1. Als u de basiscertificeringsinstantie van het certificaat op de lokale VM vertrouwt, moet u het certificaat toevoegen aan de **Vertrouwde hoofdsite certificeringsinstanties**. Ga als volgt een overzicht van de vereiste stappen. Zie [een Server-certificaat installeren](https://technet.microsoft.com/library/cc740068)voor meer informatie over hoe u kunt de Certificeringsinstantie vertrouwen.

    1. Vanuit de Azure klassieke-portal, selecteert u de VM en klik op verbinding maken. Afhankelijk van de browserconfiguratie van uw, wordt u mogelijk gevraagd om op te slaan een RDP-bestand om verbinding te maken voor VM.
    
        ![verbinding maken met azure virtuele machines](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Gebruik de gebruikersnaam VM, gebruikersnaam en wachtwoord die u bij het maken van de VM hebt geconfigureerd. 
    
        Bijvoorbeeld, in de volgende afbeelding, de naam van de VM **ssrsnativecloud** is en de gebruikersnaam is **testuser**.
        
        ![vm-aanmeldingsnaam zijn opgenomen](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Mmc.exe uitvoeren. Zie voor meer informatie [hoe: certificaten weergeven met de module](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. In het menu **bestand** van console toepassing de module **certificaten** toevoegen, selecteer **Computeraccount** wanneer hierom wordt gevraagd en klik vervolgens op **volgende**.
    
    1. Selecteer de **Lokale Computer** om te beheren en klik vervolgens op **Voltooien**.
    
    1. Klik op **Ok** en vouwt u de **certificaten - persoonlijke** knooppunten en klik vervolgens op **certificaten**. Het certificaat is de naam van de DNS-naam van de VM en eindigt met **cloudapp.net**. Met de rechtermuisknop op de certificaatnaam van het en klik op **kopiëren**.
    
    1. Vouw het knooppunt **Certificeringsinstanties die vertrouwde basiscertificaten** uit en klik vervolgens met de rechtermuisknop op **certificaten** en klik op **Plakken**.
    
    1. Om te valideren, dubbelklikt u op de naam van het certificaat onder **Certificeringsinstanties die vertrouwde basiscertificaten** en controleer of er geen fouten zijn en u ziet dat uw certificaat. Als u gebruiken het HTTPS-script wordt geleverd bij dit onderwerp wilt, om te configureren de rapportserver, de waarde van de certificaten **vingerafdruk** is vereist als een parameter van het script. **De vingerafdrukwaarde te halen**, voert u de volgende handelingen uit. Er is ook een steekproef PowerShell om op te halen de vingerafdruk in sectie [script gebruiken voor het configureren van de rapportserver en HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).
        
        1. Dubbelklik op de naam van het certificaat, bijvoorbeeld ssrsnativecloud.cloudapp.net.
        
        1. Klik op het tabblad **Details** .
        
        1. Klik op **vingerafdruk**. De waarde van de vingerafdruk wordt weergegeven in het veld details, bijvoorbeeld a6 08 3c aantal vrijheidsgraden f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.
        
        1. Kopieer de vingerafdruk en de waarde opslaan voor later of het script nu bewerken.
        
        1. (*) Voordat u het script uitvoert, verwijdert de spaties tussen de waardeparen. De vingerafdruk al eerder is vermeld zou bijvoorbeeld nu a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
        
        1. Het servercertificaat toewijzen aan de rapportserver. De toewijzing is voltooid in het volgende gedeelte als u de rapportserver configureert.

Als u een zelfondertekend SSL-certificaat gebruikt, wordt in de naam van het certificaat al overeenkomt met de hostnaam van VM. De DNS-records van de computer wordt daarom globaal al is geregistreerd en zijn toegankelijk vanaf elke client.

## <a name="step-3-configure-the-report-server"></a>Stap 3: De rapportserver configureren

In dit gedeelte begeleidt u bij het configureren van de VM als een rapportserver van Reporting Services native modus. U kunt een van de volgende methoden gebruiken voor het configureren van de rapportserver:

- Gebruik het script voor het configureren van de rapportserver

- Configuratiemanager gebruiken voor het configureren van de rapportserver.

Zie voor meer gedetailleerde stappen voor het gedeelte [verbinding maken met de virtuele Machine en Start de Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Verificatie notitie:** Windows-verificatie is de aanbevolen verificatiemethode en de standaard Reporting Services-verificatie. Alleen gebruikers die zijn geconfigureerd op de VM toegang heeft tot Reporting Services en zijn toegewezen aan rollen voor Reporting Services.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Gebruik van script de rapportserver en HTTP configureren

Als u de Windows PowerShell-script wilt configureren de rapportserver, voert u de volgende stappen uit. De configuratie bevat HTTP, niet HTTPS:

1. Vanuit de Azure klassieke-portal, selecteert u de VM en klik op verbinding maken. Afhankelijk van de browserconfiguratie van uw, wordt u mogelijk gevraagd om op te slaan een RDP-bestand om verbinding te maken voor VM.

    ![verbinding maken met azure virtuele machines](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Gebruik de gebruikersnaam VM, gebruikersnaam en wachtwoord die u bij het maken van de VM hebt geconfigureerd. 

    Bijvoorbeeld, in de volgende afbeelding, de naam van de VM **ssrsnativecloud** is en de gebruikersnaam is **testuser**.
    
    ![vm-aanmeldingsnaam zijn opgenomen](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Open op de VM, **Windows filter wissen** met beheerdersbevoegdheden. De PowerShell ISE is al dan niet standaard op Windows server 2012 geïnstalleerd. Het wordt aanbevolen dat u de wissen gebruiken in plaats van een standaard Windows PowerShell-venster, zodat u kunt het script in de wissen plakken, wijzig het script en vervolgens het script uitvoeren.

1. In Windows PowerShell wissen, klikt u op het menu **Beeld** en klik op **Script-veld weergeven**.

1. Het volgende script Kopieer en plak het script in het filter wissen van Windows script-veld.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Als u de VM met een HTTP-poort dan 80 gemaakt, wijzigt u de parameter $HTTPport = 80.

1. Het script is momenteel geconfigureerd voor Reporting Services. Als u het script uitvoeren voor Reporting Services wilt, moet u het gedeelte versie met het pad naar de naamruimte "V11,:" in het overzicht Get-WmiObject aanpassen.

1. Het script uitvoeren.

**Gegevensvalidatie**: om te bevestigen dat de functionaliteit van de server basisrapport werkt, raadpleegt u de sectie [de configuratie controleren](#verify-the-configuration) verderop in dit onderwerp.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Gebruik van script de rapportserver en HTTPS configureren

Als u Windows PowerShell wilt configureren de rapportserver, voert u de volgende stappen uit. De configuratie omvat HTTPS, niet HTTP.

1. Vanuit de Azure klassieke-portal, selecteert u de VM en klik op verbinding maken. Afhankelijk van de browserconfiguratie van uw, wordt u mogelijk gevraagd om op te slaan een RDP-bestand om verbinding te maken voor VM.

    ![verbinding maken met azure virtuele machines](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Gebruik de gebruikersnaam VM, gebruikersnaam en wachtwoord die u bij het maken van de VM hebt geconfigureerd. 

    Bijvoorbeeld, in de volgende afbeelding, de naam van de VM **ssrsnativecloud** is en de gebruikersnaam is **testuser**.

    ![vm-aanmeldingsnaam zijn opgenomen](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Open op de VM, **Windows filter wissen** met beheerdersbevoegdheden. De PowerShell ISE is al dan niet standaard op Windows server 2012 geïnstalleerd. Het wordt aanbevolen dat u de wissen gebruiken in plaats van een standaard Windows PowerShell-venster, zodat u kunt het script in de wissen plakken, wijzig het script en vervolgens het script uitvoeren.

1. Als u de lopende scripts, voert u de volgende Windows PowerShell-opdracht:

        Set-ExecutionPolicy RemoteSigned

    Vervolgens kunt u de volgende handelingen uit om te controleren of het beleid uitvoeren:

        Get-ExecutionPolicy

1. In **Windows PowerShell wissen**, klikt u op het menu **Beeld** en klik op **Script-veld weergeven**.

1. Het volgende script Kopieer en plak deze in het filter wissen van Windows script-veld.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Wijzig de parameter **$certificatehash** in het script:

    - Dit is een **vereiste** parameter. Als u de waarde van het certificaat niet uit de vorige stap hebt opgeslagen, een van de volgende methoden gebruiken om de hashwaarde certificaat kopiëren uit de vingerafdruk certificaten.:

        Open Windows filter wissen op het VM, en voer de volgende opdracht:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        De uitvoer ziet er ongeveer als volgt uit. Als het script als resultaat een lege regel, de VM niet een certificaat dat is geconfigureerd, bijvoorbeeld hebt, raadpleegt u de sectie [het virtuele Machines zelfondertekende certificaat gebruiken](#to-use-the-virtual-machines-self-signed-certificate).
    
    OF-BEWERKING
    
    - Voer op de VM mmc.exe en voegt u de module **certificaten** .
    
    - Dubbelklik op de certificaatnaam van uw onder het knooppunt **Hoofdsite certificeringsinstanties vertrouwde** . Als u het zelfondertekende certificaat van VM gebruikt, wordt het certificaat is de naam van de DNS-naam van de VM en eindigt met **cloudapp.net**.
    
    - Klik op het tabblad **Details** .
    
    - Klik op **vingerafdruk**. De waarde van de vingerafdruk wordt weergegeven in het veld details, bijvoorbeeld af 11 60 b6 4b 28 8 d 89 0a 82 12 ge 6 ter a9 c3 66 4f 31 90 48
    
    - **Voordat u het script uitvoert**, verwijdert de spaties tussen de waardeparen. Bijvoorbeeld af1160b64b288d890a8212ff6ba9c3664f319048

1. Wijzig de parameter **$httpsport** : 

    - Als u poort 443 voor het eindpunt HTTPS gebruikt, hoeft niet u deze parameter in het script bijwerken. Anders gebruik de poortwaarde van de die u hebt geselecteerd wanneer u het privé HTTPS-eindpunt voor de VM hebt geconfigureerd.

1. Wijzig de parameter **$DNSName** : 

    - Het script is geconfigureerd voor een jokerteken certificaat $DNSName = 'en'. Als u geen wilt configureren voor een jokerteken certificaat binding, opmerking af $DNSName ="+ 'en inschakelen van de volgende regel, de volledige $DNSNAme verwijzing, ## $DNSName="$server.cloudapp.net'.

        Wijzig de waarde $DNSName als u niet wilt dat de VM DNS-naam voor Reporting Services gebruiken. Als u de parameter gebruikt, wordt deze naam moet ook gebruiken door het certificaat en u de naam van globaal in een DNS-server hebt geregistreerd.

1. Het script is momenteel geconfigureerd voor Reporting Services. Als u het script uitvoeren voor Reporting Services wilt, moet u het gedeelte versie met het pad naar de naamruimte "V11,:" in het overzicht Get-WmiObject aanpassen.

1. Het script uitvoeren.

**Gegevensvalidatie**: om te bevestigen dat de functionaliteit van de server basisrapport werkt, raadpleegt u de sectie [de configuratie controleren](#verify-the-connection) verderop in dit onderwerp. Voor de verificatie van het certificaat binding open een opdrachtprompt met beheerdersbevoegdheden en voer de volgende opdracht uit:

    netsh http show sslcert

Het resultaat bevat het volgende:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Configuratiemanager gebruiken voor het configureren van de rapportserver

Als u niet uitvoeren van de PowerShell-script om te configureren de rapportserver wilt, volgt u de stappen in deze sectie om de Reporting Services native modus configuratiemanager gebruiken voor het configureren van de rapportserver.

1. Vanuit de Azure klassieke-portal, selecteert u de VM en klik op verbinding maken. Gebruik de gebruikersnaam en wachtwoord die u bij het maken van de VM hebt geconfigureerd.

    ![verbinding maken met azure virtuele machines](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. Windows update uitvoeren en installeer updates voor VM. Als een herstart van VM moet ondernomen worden, start de VM opnieuw en opnieuw verbinding maken met de VM van de Azure klassieke portal.

1. Vanuit het menu Start op de VM, typt u **Reporting Services** en open **Reporting Services Configuration Manager**.

1. Laat de standaardwaarden voor **De naam van de Server** en **Rapport Server-instantie**. Klik op **verbinding maken**.

1. Klik in het linkerdeelvenster op **Webservice-URL**.

1. Standaard is RS geconfigureerd voor HTTP-poort 80 met IP-"Alle toegewezen". HTTPS toevoegen:

    1. In **SSL-certificaat**: Selecteer het certificaat dat u gebruiken wilt, bijvoorbeeld [naam van de VM]. cloudapp.net. Als er geen certificaten weergegeven zijn, raadpleegt u de sectie **stap 2: een Server digitaal certificaat maken** voor informatie over het installeren en het certificaat op de VM vertrouwen.
    
    1. Klik onder **SSL-poort**: Kies 443. Als u het privé HTTPS-eindpunt geconfigureerd in de VM met een andere privé poort, hier die waarde gebruiken.
    
    1. Klik op **toepassen** en wacht totdat de bewerking is voltooid.

1. Klik in het linkerdeelvenster op **Database**.

    1. Klik op **wijzigen Databas**e.
    
    1. Klik op **een nieuwe report server-database maken** en klik op **volgende**.
    
    1. Laat de standaard **Servernaam**: als de VM Geef een naam en laat u de standaard **Verificatietype** als **Huidige gebruiker** – **Geïntegreerde beveiliging**. Klik op **volgende**.
    
    1. Laat de standaard **Databasenaam** als **ReportServer** en klik op **volgende**.
    
    1. Laat de standaard **Verificatietype** als **Servicereferenties** en klik op **volgende**.
    
    1. Klik op **volgende** op de pagina **Summary** .
    
    1. Wanneer de configuratie voltooid is, klikt u op **Voltooien**.

1. Klik in het linkerdeelvenster op **Report Manager URL**. Laat de standaard **Virtuele map** als **rapporten** en klikt u op **toepassen**.

1. Klik op **Afsluiten** om te sluiten van de Reporting Services Configuration Manager.

## <a name="step-4-open-windows-firewall-port"></a>Stap 4: Open Windows Firewall poort

>[AZURE.NOTE] Als u een van de scripts voor het configureren van de rapportserver gebruikt, kunt u dit gedeelte overslaan. Het script opgenomen een stap om de firewallpoort te openen. De standaardinstelling is poort 80 voor HTTP en poort 443 voor HTTPS.

Als u wilt extern verbinding maken met Report Manager of de rapportserver op de virtuele machine, is op de VM een TCP-eindpunt vereist. Dit is vereist voor het openen van dezelfde poort in van de VM firewall. Het eindpunt is gemaakt wanneer de VM is ingericht.

Deze sectie bevat algemene informatie voor het openen van de firewallpoort. Zie [configureren een Firewall voor toegang tot rapporten-Server](https://technet.microsoft.com/library/bb934283.aspx) voor meer informatie

>[AZURE.NOTE] Als u het script voor het configureren van de rapportserver gebruikt, kunt u dit gedeelte overslaan. Het script opgenomen een stap om de firewallpoort te openen.

Als u een persoonlijke poort geconfigureerd voor HTTPS dan 443, wijzigt u het volgende script correct. Als u wilt openen poort **443** op Windows Firewall, voert u het volgende:

1. Een Windows PowerShell-venster openen met beheerdersbevoegdheden.

1. Als u een andere poort dan 443 gebruikt wanneer u het eindpunt HTTPS voor de VM hebt geconfigureerd, de poort in de volgende opdracht bijwerken en vervolgens de opdracht uitvoeren:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. Wanneer de opdracht is voltooid, **Ok** wordt weergegeven in de opdrachtprompt.

Een Windows PowerShell-venster openen om te bevestigen dat de poort is geopend, en voer de volgende opdracht:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Controleer de configuratie

Open uw browser met beheerdersbevoegdheden en blader naar de volgende rapport server ad report manager URL's om te bevestigen dat de functionaliteit van de server basisrapport nu werkt:

- Op de VM, bladert u naar de URL van de rapportserver:

        http://localhost/reportserver

- Op de VM, bladert u naar de URL van de Manager rapport:

        http://localhost/Reports

- Blader naar de **externe** lijst Manager op de VM vanaf de lokale computer. Werk de DNS-naam in het volgende voorbeeld zo nodig. Wanneer u hierom wordt gevraagd om een wachtwoord, gebruikt u de van beheerdersreferenties die u hebt gemaakt tijdens de VM is ingericht. De gebruikersnaam is in de [domein]\[gebruikersnaam] waar het domein de naam van de computer VM, bijvoorbeeld ssrsnativecloud\testuser is-indeling. Als u niet HTTP**S**gebruikt, verwijdert u de **s** in de URL. Zie de volgende sectie voor informatie over het maken van andere gebruikers op VM.

        https://ssrsnativecloud.cloudapp.net/Reports

- Uw lokale computer, blader naar de URL van de externe rapportserver. Werk de DNS-naam in het volgende voorbeeld zo nodig. Als u niet HTTPS gebruikt, verwijdert u de s in de URL.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Maken gebruikers en rollen toewijzen

Na configureren en de rapportserver verifiëren, is een algemene administratieve taak te maken van een of meer gebruikers toewijzen aan gebruikers aan rollen voor Reporting Services. Zie de volgende onderwerpen voor meer informatie:

- [Een lokale gebruikersaccount maken](https://technet.microsoft.com/library/cc770642.aspx)

- [Verlenen gebruiker toegang tot een rapportserver (Report Manager)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Maken en beheren van roltoewijzingen](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Maken en publiceren van rapporten op de Azure virtuele Machine

De volgende tabel worden enkele van de opties die beschikbaar zijn voor het publiceren van bestaande rapporten vanaf een lokale computer op de rapportserver die worden gehost op Microsoft Azure Virtual Machine:

- **RS.exe-script**: gebruik RS.exe-script om het rapportitems uit en bestaande rapportserver kopiëren naar uw Microsoft Azure Virtual Machine. Zie het gedeelte 'Native modus systeemeigen modus – Microsoft Azure virtuele machines' in de [steekproef Reporting Services-rs.exe Script om inhoud migreren tussen rapportservers](https://msdn.microsoft.com/library/dn531017.aspx)voor meer informatie.

- **Report Builder**: de virtuele machine bevat de Klik-eenmaal versie van Microsoft SQL Server Report Builder. Rapport opbouwfunctie voor de eerste keer start op de virtuele machine:

    1. Start uw browser met beheerdersbevoegdheden.
    
    1. Blader naar Rapportbeheer op de virtuele machine en **Report Builder** klikt op het lint.

    Zie [installeren, wanneer u, en ondersteunende Report Builder](https://technet.microsoft.com/library/dd207038.aspx)voor meer informatie.

- **SQL Server Data Tools: VM**: als u de VM met SQL Server 2012 hebt gemaakt, wordt SQL Server Data Tools is geïnstalleerd op de virtuele machine en kunnen worden gebruikt om het **Rapport serverprojecten** en rapporten op de virtuele machine maken. SQL Server Data Tools kunt u de rapporten publiceren naar de rapportserver op de virtuele machine.
    
    Als u de VM met SQL server 2014 hebt gemaakt, kunt u SQL Server Data Tools - BI voor visual Studio installeren. Zie de volgende onderwerpen voor meer informatie:

    - [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [SQL Server Data Tools en SQL Server Business Intelligence (BI SSDT)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: externe**: op uw lokale computer, kunt u een Reporting Services-project maakt in SQL Server Data Tools die Reporting Services-rapporten bevat. Het project verbinding maken met de URL van de web-service configureren.

    ![de Projecteigenschappen SSDT voor SQL Server Reporting Services-project](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Gebruik script**: gebruik-script om rapport serverinhoud te kopiëren. Zie [voorbeeld Reporting Services-rs.exe Script om inhoud migreren tussen rapportservers](https://msdn.microsoft.com/library/dn531017.aspx)voor meer informatie.

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Als u geen gebruik van de VM kosten minimaliseren

>[AZURE.NOTE] Afsluiten om te beperken kosten voor uw Azure virtuele Machines wanneer u niet in gebruik, de VM van de Azure klassieke portal. Als u de opties voor de Windows-power binnen een VM afsluiten de VM gebruikt, wordt u nog steeds hetzelfde bedrag geheven voor de VM. Als u wilt beperken kosten, moet u de VM in de portal van Azure klassieke afsluiten. Als u de VM niet meer nodig hebt, moet u de VM en de bijbehorende VHD-bestanden om te voorkomen dat opslag kosten verwijderen. Zie het gedeelte Veelgestelde vragen aan [Virtuele Machines prijzen Details](https://azure.microsoft.com/pricing/details/virtual-machines/)voor meer informatie.

## <a name="more-information"></a>Meer informatie

### <a name="resources"></a>Resources

- Zie [Windows PowerShell gebruiken om een Azure VM met SQL Server BI en SharePoint 2013 te maken](https://msdn.microsoft.com/library/azure/dn385843.aspx)voor soortgelijke inhoud gerelateerd aan een enkele server-implementatie van SQL Server Business Intelligence en SharePoint 2013.

- Zie voor soortgelijke inhoud gerelateerd aan een meerdere server-implementatie van SQL Server Business Intelligence en SharePoint 2013, [Implementeren SQL Server Business Intelligence in Azure virtuele Machines](https://msdn.microsoft.com/library/dn321998.aspx).

- Zie voor algemene informatie over implementaties van SQL Server Business Intelligence in Azure virtuele Machines, [SQL Server Business Intelligence in Azure virtuele Machines](virtual-machines-windows-classic-ps-sql-bi.md).

- Voor meer informatie over de kosten van Azure kosten berekenen, raadpleegt u het tabblad virtuele Machines van [Azure prijzen Rekenmachine](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Community-inhoud

- Zie [Hostingprovider SQL Service voor het rapporteren van Azure virtuele machines](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html)voor stapsgewijze instructies voor het maken van een Reporting Services systeemeigen modus rapportserver zonder script te gebruiken.

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Koppelingen naar andere bronnen voor SQL Server in Azure VMs

[SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md)
