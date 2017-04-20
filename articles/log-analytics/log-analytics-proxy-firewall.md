<properties
    pageTitle="Proxy- en firewall-instellingen configureren in Log Analytics | Microsoft Azure"
    description="Proxy- en firewall-instellingen configureren wanneer uw agenten of OMS services moeten specifieke poorten gebruiken."
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
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>

# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Proxy- en firewall-instellingen configureren in Log Analytics

Acties die nodig zijn voor het configureren van proxy en firewallinstellingen van analysegegevens Log in OMS verschillen wanneer u Operations Manager en haar partners versus Microsoft Monitoring agenten die rechtstreeks verbinden met servers. Zie de volgende secties voor het type agent die u gebruikt.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>Proxy-en firewallinstellingen configureren met de Microsoft-Agent bewaken

Voor Microsoft Monitoring Agent te verbinden met de OMS-service registreren, moet deze hebben toegang tot het poortnummer van uw domeinen en de URL's. Als u een proxyserver voor communicatie tussen de agent en de OMS-service gebruiken, moet u ervoor zorgen dat de juiste bronnen toegankelijk zijn. Als u een firewall gebruikt voor het beperken van toegang met Internet, moet u uw firewall om toegang te verlenen aan OMS configureren. De volgende tabellen bevatten de poorten die OMS nodig heeft.

|**Agent-bron**|**Poorten**|**Overslaan HTTPS controle**|
|--------------|-----|--------------|
|\*. ods.opinsights.azure.com|443|Ja|
|\*. oms.opinsights.azure.com|443|Ja|
|\*. blob.core.windows.net|443|Ja|
|ods.systemcenteradvisor.com|443| |

U kunt de volgende procedure voor het configureren van proxy-instellingen voor Microsoft Monitoring Agent via het Configuratiescherm. U moet het gebruik van de procedure voor elke server. Hebt u veel servers die u wilt configureren, is het mogelijk handiger een script gebruiken om dit proces te automatiseren. Als dat het geval is, raadpleegt u de volgende procedure [voor het configureren van proxy-instellingen voor de Microsoft Monitoring-Agent met een script](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Voor het configureren van proxy-instellingen voor Microsoft Monitoring Agent via het Configuratiescherm

1. Open **het Configuratiescherm**.

2. Open **Microsoft Agent cmdlets voor controle**.

3. Klik op het tabblad **Proxy-instellingen** .<br>  
  ![tabblad proxy-instellingen](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Selecteer **een proxyserver gebruiken** en typ de URL en poortnummer, indien nodig, zoals in het voorbeeld weergegeven. Als uw proxyserver is verificatie vereist, typt u de gebruikersnaam en wachtwoord voor toegang tot de proxyserver.

Gebruik de volgende procedure om te maken van een PowerShell-script die u uitvoeren kunt om in te stellen van de proxy-instellingen voor elke agent die rechtstreeks met servers verbonden.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Proxy-instellingen voor de Microsoft Monitoring-Agent met een script configureren

Kopieer in het onderstaande voorbeeld, bijwerken met gegevens die specifiek zijn voor uw omgeving en deze opslaan onder PS1 extensie vervolgens het script uitvoeren op elke computer die rechtstreeks met de OMS-service verbonden.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>Proxy-en firewallinstellingen configureren met Operations Manager

Voor een Operations Manager management groep verbinden en registreert met de OMS-service, moet deze hebben toegang tot de poortnummers van uw domeinen en URL's. Als u een proxyserver voor communicatie tussen de server voor Operations Manager en de OMS-service gebruiken, moet u ervoor zorgen dat de juiste bronnen toegankelijk zijn. Als u een firewall gebruikt voor het beperken van toegang met Internet, moet u uw firewall om toegang te verlenen aan OMS configureren. Zelfs als een Operations Manager management server niet achter een proxyserver is, mogelijk haar agenten. In dit geval moet de proxyserver op dezelfde manier worden geconfigureerd agenten zijn om te kunnen inschakelen en beveiliging toestaan en gegevens van de oplossing Log-beheer krijgen verzonden naar de OMS webservice.

In de volgorde voor Operations Manager agenten om te communiceren met de OMS-service, moet de infrastructuur van uw Operations Manager (inclusief agenten) de juiste proxy-instellingen en versie hebben. De proxy-instellingen voor agenten is opgegeven in de Operations Manager-console. Uw versie moet een van de volgende opties:

- Operations Manager 2012 SP1 Update Rollup 7 of hoger
- Operations Manager 2012 R2 Update Rollup 3 of hoger


De volgende tabellen bevatten de poorten die betrekking hebben op de volgende taken kunnen uitvoeren.

>[AZURE.NOTE] Enkele van de volgende bronnen Advisor en operationele inzichten vermelden, beide eerdere versies van OMS zijn. Bronnen in de lijst wordt gewijzigd in de toekomst echter.

Hier volgt een lijst met bronnen voor agent en poorten:<br>

|**Agent-bron**|**Poorten**|
|--------------|-----|
|\*. ods.opinsights.azure.com|443|
|\*. oms.opinsights.azure.com|443|
|\*.BLOB.Core.Windows.NET/\*|443|
|ods.systemcenteradvisor.com|443|
<br>
Hier volgt een lijst met management server-bronnen en poorten:<br>

|**Management server-bron**|**Poorten**|**Overslaan HTTPS controle**|
|--------------|-----|--------------|
|service.systemcenteradvisor.com|443| |
|\*. service.opinsights.azure.com|443| |
|\*. blob.core.windows.net|443|Ja| 
|Data.systemcenteradvisor.com|443| | 
|ods.systemcenteradvisor.com|443| | 
|\*. ods.opinsights.azure.com|443|Ja| 
<br>
Hier volgt een lijst met bronnen voor OMS en Operations Manager-console en poorten.<br>

|**OMS en Operations Manager-console-resource**|**Poorten**|
|----|----|
|service.systemcenteradvisor.com|443|
|\*. service.opinsights.azure.com|443|
|\*. live.com|Poort 80 en 443|
|\*. microsoft.com|Poort 80 en 443|
|\*. microsoftonline.com|Poort 80 en 443|
|\*. mms.microsoft.com|Poort 80 en 443|
|Login.Windows.NET|Poort 80 en 443|
<br>

Gebruik de volgende procedures uit uw Operations Manager management groep met de OMS-service registreren. Als u communicatie tussen de groep management en de OMS-service problemen, gebruikt u de procedures gegevensvalidatie problemen met de gegevensoverdracht van naar de OMS-service.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Uitzonderingen voor de eindpunten van de service OMS aanvragen

1. Gebruik de informatie uit de eerste tabel die eerder gepresenteerd om ervoor te zorgen dat de bronnen die u nodig hebt voor de server voor Operations Manager zijn toegankelijk via een firewalls hebt.
2. Gebruik de informatie uit de tweede tabel gepresenteerd eerder om ervoor te zorgen dat de bronnen die u nodig hebt voor de bewerkingen-console in Operations Manager en OMS zijn toegankelijk via een firewalls hebt.
3. Als u een proxyserver met Internet Explorer gebruikt, zorg ervoor dat is geconfigureerd en goed werkt. Om te verifiëren, kunt u een beveiligde Webverbinding kunnen worden gegeocodeerd (HTTPS), bijvoorbeeld [https://bing.com](https://bing.com)openen. Als de beveiligde Webverbinding kunnen worden gegeocodeerd niet in een browser werkt, werkt waarschijnlijk niet in de beheerconsole Operations Manager met webservices in de cloud.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>De proxyserver configureren in de Operations Manager-console

1. Open de Operations Manager-console en selecteert u het **beheer van de** werkruimte.

2. Vouw **Operationele inzichten**en selecteer vervolgens **Operationele inzichten verbinding**.<br>  
    ![Operations Manager OMS verbinding](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. Klik in de weergave OMS verbinding op **Proxyserver configureren**.<br>  
    ![Operations Manager OMS verbinding proxyserver configureren](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. In operationele inzichten Wizard instellingen: Proxyserver, selecteer **een proxyserver voor toegang tot de operationele inzichten-webservice**en typt u de URL met de poort nummeren, bijvoorbeeld **http://myproxy:80**.<br>  
    ![Operations Manager OMS proxyadres](./media/log-analytics-proxy-firewall/proxy-om03.png)


### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Referenties opgeven als de proxy-server is verificatie vereist.
 Referenties voor de proxyserver en instellingen moeten doorgegeven aan beheerde computers OMS wordt gerapporteerd. Deze servers moeten zich in de *Microsoft System Center Advisor Monitoring Server groep*. Referenties zijn versleuteld in het register van elke server in de groep.

1. Open de Operations Manager-console en selecteert u het **beheer van de** werkruimte.
2. Selecteer onder **RunAs configuratie**, **profielen**.
3. Open het **Systeem Center Advisor uitvoeren als profiel Proxy** -profiel.  
    ![afbeelding van het systeem Center Advisor uitvoeren als Proxy-profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Klik op **toevoegen** om te gebruiken van een account uitvoeren als in de uitvoeren als profielwizard. U kunt een nieuw account uitvoeren als maken of een bestaand account gebruiken. Dit account heeft onvoldoende machtigingen om door te geven via de proxyserver nodig.  
    ![afbeelding van de Wizard uitvoeren als profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Kies het account voor het beheren van stelt **een geselecteerde klasse, de groep of het object** om het Object zoeken te openen.  
    ![afbeelding van de Wizard uitvoeren als profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Zoek en selecteer vervolgens **Microsoft systeem Center Advisor Monitoring Server groep**.  
    ![afbeelding van het vak Object zoeken](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Klik op **OK** om te sluiten van het vak uitvoeren als account toevoegen.  
    ![afbeelding van de Wizard uitvoeren als profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Voltooi de wizard en de wijzigingen op te slaan.  
    ![afbeelding van de Wizard uitvoeren als profiel](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Voor het valideren van die management OMS packs worden gedownload

Als u de oplossingen aan OMS hebt toegevoegd, kunt u deze in de Operations Manager-console weergeven als management packs onder **beheer van de**. Zoek naar *Systeem Center Advisor* om snel te berekenen.  
    ![Management packs gedownload](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) of u kunt ook voor OMS management packs controleren met behulp van de volgende Windows PowerShell-opdracht in de server voor Operations Manager:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Valideren die Operations Manager worden gegevens verzonden naar de OMS-service

1. In de server voor Operations Manager, open prestaties Monitor (perfmon.exe) en selecteer **Prestaties Monitor**.
2. Klik op **toevoegen**en selecteer **Systeemstatus Service Management groepen**.
3. Alle items die met **HTTP beginnen**toevoegen.  
    ![items toevoegen](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Als uw configuratie Operations Manager gemaakt is, ziet u de activiteit voor items van de servicestatus servicebeheer voor gebeurtenissen en andere gegevensitems, gebaseerd op de management packs die u hebt toegevoegd in OMS en het beleid voor het verzamelen van geconfigureerde log.  
    ![Activiteiten van prestaties controleren weergeven](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## <a name="next-steps"></a>Volgende stappen

- [Oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md) voor functionaliteit toevoegen en gegevens te verzamelen.
- Vertrouwd raken met [log zoekopdrachten](log-analytics-log-searches.md) om gedetailleerde informatie verzameld door oplossingen te bekijken.
