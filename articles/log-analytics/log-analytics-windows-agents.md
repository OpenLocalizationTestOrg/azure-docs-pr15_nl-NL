<properties
    pageTitle="Windows-computers verbinden met Log Analytics | Microsoft Azure"
    description="In dit artikel leest de stappen in de Windows-computers in de infrastructuur van uw on-premises implementatie rechtstreeks naar OMS verbindt met behulp van een aangepaste versie van de Microsoft Monitoring Agent MMA ()."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>


# <a name="connect-windows-computers-to-log-analytics"></a>Windows-computers verbinden met Log Analytics

In dit artikel leest de stappen om te verbinden met de computers Windows in de infrastructuur van uw on-premises implementatie rechtstreeks OMS werkruimten met behulp van een aangepaste versie van de Microsoft Monitoring Agent MMA (). U moet installeren en verbinden met agenten voor alle computers gewenste ingebouwde naar OMS in volgorde deze gegevens te verzenden naar OMS en naar het werken met die gegevens in de portal OMS. Elke agent kunt melden aan meerdere werkruimten.

U kunt met configuratie, opdrachtregel agenten installeren of met de gewenste staat configuratie (DSC) in Azure automatisering.  

>[AZURE.NOTE] Voor virtuele machines uitgevoerd in Azure wordt aangegeven dat u kunt de installatie vereenvoudigen met behulp van de [extensie VM](log-analytics-azure-vm-extension.md).

Op computers met Internet connectivity, wordt de agent gebruikt u de verbinding met Internet gegevens te verzenden naar OMS. Voor computers waarop geen internetverbinding hebt, kunt u een proxy of de OMS Log Analytics-doorstuurservers.

Verbinden met uw Windows-computers OMS is ingewikkelde 3 eenvoudige stappen uit te voeren:

1. Het installatiebestand agent downloaden
2. De agent met behulp van de methode die u kiest u installeren
3. De agent configureren of extra werkruimten, indien nodig toevoegen

In het volgende diagram ziet u de relatie tussen uw Windows-computers en OMS nadat u hebt geïnstalleerd en geconfigureerd agenten.

![OMS-direct-agent-diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## <a name="system-requirements-and-required-configuration"></a>Systeemvereisten en vereiste configuratie
Lees de volgende informatie om ervoor te zorgen u voldoen aan vereisten voordat u installeert of agenten implementeren.

- U kunt alleen de MMA OMS installeren op computers met Windows Server 2008 SP 1 of hoger of Windows 7 SP1 of hoger.
- U moet een OMS-abonnement.  Zie [aan de slag met Log Analytics](log-analytics-get-started.md)voor aanvullende informatie.
- Elke Windows-computer moet kunnen verbinding hebt met Internet via HTTPS. Deze verbinding kan worden direct, via een proxyserver of via de OMS Log Analytics-doorstuurservers.
- U kunt de MMA OMS installeren op zelfstandige computers, servers en virtuele machines. Als u gehoste Azure virtuele machines OMS verbinding wilt maken, raadpleegt u [verbinding maken met Azure virtuele machines naar Log Analytics](log-analytics-azure-vm-extension.md).
- De agent moet TCP-poort 443 gebruiken voor verschillende bronnen. Zie voor meer informatie, [proxy en firewall instellingen configureren in Log Analytics](log-analytics-proxy-firewall.md).

## <a name="download-the-agent-setup-file-from-oms"></a>Het installatiebestand agent uit het Kantoorbeheersysteem downloaden
1. Klik op de tegel van de **Instellingen** in de portal OMS, klikt u op de pagina **Overzicht** .  Klik op het tabblad **Bronnen verbonden** aan de bovenkant.  
    ![Tabblad met verbonden bronnen](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Onder **Computers rechtstreeks bijvoegen**, klikt u op **De Windows-Agent downloaden** van toepassing op uw computer processortype om te downloaden van het installatiebestand.
3. Aan de rechterkant van de **Werkruimte-ID**, klik op het pictogram kopiëren en plak de-ID in Kladblok.
4. Aan de rechterkant van de **Primaire sleutel**, klik op het pictogram kopiëren en plak de toets in Kladblok.     
    ![Werkruimte-ID en de primaire sleutel kopiëren](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## <a name="install-the-agent-using-setup"></a>De agent met configuratie installeren
1. Voer Setup uit om de agent installeren op een computer waarop u wilt beheren.
2. Klik op de welkomstpagina op **volgende**.
3. Klik op de pagina licentievoorwaarden lezen van de licentie en klik op **ik ga akkoord**.
4. Klik op de pagina doelmap wijzigen of de standaardmap voor de installatie en klik vervolgens op **volgende**.
5. U kunt verbinding maken met de agent aan Azure Log Analytics Kantoorbeheersysteem, Operations Manager, of u kunt de opties leeg laat als u wilt dat de agent later configureren op de pagina Opties van de instelling Agent. Klik op **volgende**.   
    - Als u ervoor kiest om verbinding maken met naar Azure Log Analytics Kantoorbeheersysteem, de **ID van de werkruimte** en de **Werkruimte toets (primaire)** die u in de vorige procedure hebt gekopieerd in Kladblok plakken en klik op **volgende**.  
        ![Werkruimte-ID en de primaire sleutel plakken](./media/log-analytics-windows-agents/connect-workspace.png)
    - Als u verbinding maakt met Operations Manager kiest, typ de **Naam van de groep Management**, servernaam van **Management** en **Serverpoort Management**en klik vervolgens op **volgende**. Klik op de accountpagina Agent actie kiest u de lokale systeemaccount of een lokale domein-account en klik op **volgende**.  
        ![configuratie van de groep Management](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agent actie-account](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Klik op de pagina toepassing installeren, Controleer uw keuzen en klik op **installeren**.
7. Pagina van de configuratie is voltooid, klikt u op **Voltooien**.
8. Als alles compleet is, wordt de **Microsoft Monitoring Agent** weergegeven in **Het Configuratiescherm**. U kunt uw configuratie er bekijken en controleer of de agent aan operationele inzichten Kantoorbeheersysteem is aangesloten. Wanneer u verbinding hebt met OMS, de agent wordt weergegeven in een bericht ontvangt: **de Microsoft Monitoring Agent verbonden is met de service Microsoft bewerkingen Management Suite.**

## <a name="install-the-agent-using-the-command-line"></a>De agent via de opdrachtregel installeren
- Wijzigen en gebruikt u het volgende voorbeeld de agent via de opdrachtregel installeren.

    >[AZURE.NOTE] Als u upgraden van een agent wilt, moet u de Log-analyses uitvoeren van scripts API gebruiken. Zie de volgende sectie voor het upgraden van een agent.

    ```
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## <a name="upgrade-the-agent-and-add-a-workspace-using-a-script"></a>Upgrade van de agent en de toevoeging van een werkruimte met een script
U kunt een agent upgraden en toevoegen van een werkruimte door middel van het logboek-analyses uitvoeren van scripts API met het volgende PowerShell-voorbeeld.

```
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

>[AZURE.NOTE] Als u hebt gebruikt de opdrachtregel of script eerder installeren of configureren van de agent `EnableAzureOperationalInsights` is vervangen door `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>De agent DSC gebruiken in Azure automatisering installeren

>[AZURE.NOTE] In dit voorbeeld procedure en script wordt een bestaande agent niet bijgewerkt.

1. De xPSDesiredStateConfiguration DSC Module uit [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) importeren in Azure automatisering.  
2.  Azure automatisering variabele activa voor *OPSINSIGHTS_WS_ID* en *OPSINSIGHTS_WS_KEY*maken. *OPSINSIGHTS_WS_ID* ingesteld op uw OMS Log Analytics werkruimte-ID en stel *OPSINSIGHTS_WS_KEY* op de primaire sleutel van de werkruimte.
3.  Gebruik de onderstaande script en opslaan als MMAgent.ps1
4.  Wijzigen en gebruikt u het volgende voorbeeld de agent DSC gebruiken in Azure automatisering installeren. Importeren MMAgent.ps1 in Azure automatisering met behulp van de Azure Automation-interface of de cmdlet.
5.  Een knooppunt toewijzen aan de configuratie. Binnen 15 minuten het knooppunt de configuratie moeten worden gecontroleerd en de MMA wordt verplaatst naar het knooppunt.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "http://download.microsoft.com/download/0/C/0/0C072D6E-F418-4AD4-BCB2-A362624F400A/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Een agent handmatig configureren of toevoegen van extra werkruimten
Als u agenten hebt geïnstalleerd maar deze niet hebt geconfigureerd of als u wilt dat de agent melden aan meerdere werkruimten, kunt u de volgende informatie aan een agent inschakelen of configureert u deze opnieuw. Nadat u de agent hebt geconfigureerd, wordt met de agentservice registreren en krijgt benodigde configuratiegegevens en management packs die oplossingsinformatie bevatten.

1. Nadat u de Microsoft-Agent Monitoring hebt geïnstalleerd, open **Het Configuratiescherm**.
2. Open **Microsoft Monitoring Agent** en klik vervolgens op het tabblad **Azure Log Analytics Kantoorbeheersysteem** .   
3. Klik op **toevoegen** om het dialoogvenster **toevoegen een logboek Analytics-werkruimte** te openen.
4. Plak de **Werkruimte-ID** en de **Werkruimte toets (primaire)** die u hebt gekopieerd in Kladblok in een vorige procedure voor de werkruimte die u wilt toevoegen en klik vervolgens op **OK**.  
    ![Operationele inzichten configureren](./media/log-analytics-windows-agents/add-workspace.png)

Nadat de gegevens worden verzameld van computers gecontroleerd door de agent, wordt het aantal computers gecontroleerd door OMS in de portal OMS op het tabblad **Verbonden bronnen** in **Instellingen** weergegeven als **Servers verbonden**.


## <a name="to-disable-an-agent"></a>Een agent uitschakelen
1. Na de installatie van de agent, open **Het Configuratiescherm**.
2. Open Microsoft Monitoring Agent en klik vervolgens op het tabblad **Azure Log Analytics Kantoorbeheersysteem** .
3. Selecteer een werkruimte en klik vervolgens op **verwijderen**. Herhaal deze stap voor alle andere werkruimten.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>(Optioneel) agenten rapporteren aan een groep met Operations Manager management configureren

Als u Operations Manager in uw IT-infrastructuur gebruikt, kunt u ook de MMA-agent gebruiken als een agent aan Operations Manager.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>MMA agenten rapporteren aan een groep met Operations Manager management configureren
1.  Klik op de computer waarop de-agent is geïnstalleerd, open **Het Configuratiescherm**.
2.  Open **Microsoft Monitoring Agent** en klik vervolgens op het tabblad **Operations Manager** .
    ![Microsoft Monitoring Agent Operations Manager-tabblad](./media/log-analytics-windows-agents/om-mg01.png)
3.  Als uw Operations Manager-servers integratie met Active Directory, klikt u op **automatisch bijwerken toewijzingen voor het groeperen van AD DS management**.
4.  Klik op **toevoegen** om het dialoogvenster **beheer groep toevoegen** te openen.  
    ![Microsoft Monitoring Agent een groep Management toevoegen](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  Typ de naam van uw groep management in **de naam van de groep Management** .
6.  Typ de naam van de computer van de primaire management server in het vak **primaire management server** .
7.  Typ in het vak **serverpoort Management** het poortnummer TCP.
8.  Kies onder **Agent actie-Account**, de lokale systeemaccount of een lokale domein-account.
9.  Klik op **OK** om te sluiten in het dialoogvenster **beheer groep toevoegen** en klik vervolgens op **OK** om te sluiten in het dialoogvenster **Eigenschappen van Microsoft Agent bewaken** .

## <a name="optionally-configure-agents-to-use-the-oms-log-analytics-forwarder"></a>Desgewenst kunt vinden voor het gebruik van de OMS Log Analytics-doorstuurservers configureren

Als u servers of clients die ik heb een verbinding met Internet hebt, kunt u deze gegevens verzenden naar OMS met behulp van de OMS Log Analytics-doorstuurservers nog steeds.  Wanneer u de doorstuurservers gebruikt, worden alle gegevens van agenten wordt verzonden via één server die toegang heeft tot Internet. De doorstuurservers overzet gegevens van de agenten naar OMS rechtstreeks zonder het analyseren van een van de gegevens die worden overgebracht.

Zie [OMS Log Analytics doorstuurservers](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) voor meer informatie over de doorstuurservers, inclusief installatie en configuratie.

Zie [proxy en firewall-instellingen in Log Analytics configureren](log-analytics-proxy-firewall.md)voor informatie over het configureren van uw agenten om een proxyserver, dat wil in dit geval de doorstuurservers OMS zeggen, te gebruiken.

## <a name="optionally-configure-proxy-and-firewall-settings"></a>(Optioneel) proxy en firewall-instellingen configureren
Als u proxyservers of firewalls in uw omgeving die het beperken van toegang met Internet hebt, raadpleegt u [de proxy en firewall instellingen configureren in Log Analytics](log-analytics-proxy-firewall.md) zodat uw agenten om te communiceren met de service OMS.

## <a name="next-steps"></a>Volgende stappen

- [Oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md) voor functionaliteit toevoegen en gegevens te verzamelen.
- [Proxy- en firewall instellingen configureren in Log Analytics](log-analytics-proxy-firewall.md) als uw organisatie gebruikmaakt van een proxyserver of firewall zodat agenten met de Log Analytics-service communiceren kunnen.
