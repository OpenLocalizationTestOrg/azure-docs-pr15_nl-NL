<properties
   pageTitle="Integratie van Azure log Veelgestelde vragen over | Microsoft Azure"
   description="Deze Veelgestelde vragen vindt u antwoorden op vragen over het logboek met Azure-integratie."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Integratie van Azure log veelgestelde Veelgestelde vragen

Deze Veelgestelde vragen vindt u antwoorden op vragen over de integratie van de Azure log, een service waarmee u kunt het integreren van onbewerkte logboeken van uw Azure resources in uw on-premises implementatie-systemen voor beveiligingsinformatie en gebeurtenis Management (SIEM). Deze integratie biedt een geïntegreerd dashboard voor alle activa, on-premises implementatie of in de cloud, zodat u kunt aggregeren, relateren analyseren en Waarschuw voor beveiligingsgebeurtenissen die zijn gekoppeld aan uw toepassingen.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Hoe kan ik de opslag-accounts waaruit Azure log integratie trekt aan Azure VM Logboeken uit zien?

De opdracht **azlog bronlijst**uitvoeren.

## <a name="how-can-i-update-the-proxy-configuration"></a>Hoe kan ik de proxyconfiguratie bijwerken?

Als uw proxy-instellingen Azure opslag access niet rechtstreeks mogelijk, opent u de **AZLOG. EXE. CONFIG** bestand in **c:\Program Files\Microsoft Azure Log integratie**. Het bestand als u wilt opnemen van de sectie **defaultProxy** met de proxyadres van uw organisatie bijwerken. Wanneer update klaar is, stoppen en start de service via opdrachten **netto stoppen azlog** en **netto start azlog**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Hoe kan ik de abonnementsgegevens in Windows-gebeurtenissen zien?

De **subscriptionid** toevoegen aan de beschrijvende naam bij het toevoegen van de bron.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

De gebeurtenis XML heeft de metagegevens, zoals hieronder wordt weergegeven, inclusief de abonnements-id.

![Gebeurtenis XML][1]

## <a name="error-messages"></a>Foutberichten

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Wanneer u zich de opdracht **azlog createazureid**, waarom krijg ik het volgende foutbericht weergegeven?

Fout:

  *Kan niet worden gemaakt van de toepassing AAD - Tenant 72f988bf-86f1-41af-91ab-2d7cd011db37-reden = 'Niet toegestaan' - bericht = 'Onvoldoende machtigingen om de bewerking te voltooien.'*

**Azlog createazureid** probeert te maken van een service principal in alle Azure AD-tenants voor de abonnementen waarop de Azure login toegang tot heeft. Als uw Azure aanmelding alleen een gastgebruiker in die Azure AD-tenant is, klikt u vervolgens mislukt de opdracht met 'Onvoldoende machtigingen om de bewerking te voltooien.' Aanvragen Tenant-beheerder uw account toevoegen als een gebruiker in de tenant.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Wanneer u zich de opdracht **azlog Autoriseer**, waarom krijg ik het volgende foutbericht weergegeven?

Fout:

  *Waarschuwing over het maken van roltoewijzing - AuthorizationFailed: de client janedo@microsoft.com' met object-id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' geen toestemming om uit te voeren actie 'Microsoft.Authorization/roleAssignments/write' bereik '/ abonnementen/70 d 95299-d689-4c 97-b971-0d8ff0000000'.*

De rol van lezer bij de hoofdsom Azure AD-service (gemaakt met **Azlog createazureid**) **Azlog Autoriseer** opdracht toegewezen aan de opgegeven abonnementen. Als de Azure aanmelding niet de beheerder van een collega of een eigenaar van het abonnement is, mislukt het foutbericht 'Autorisatie mislukte'. Azure Rolgebaseerd toegangsbeheer (RBAC) van collega beheerder of de eigenaar is vereist voor deze actie niet voltooien.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Waar vind ik de definitie van de eigenschappen in controlelogboek?

Zie:

- [Bewerkingen met resourcemanager controleren](../resource-group-audit.md)
- [Lijst van de management gebeurtenissen in een abonnement in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Waar vind ik meer informatie op waarschuwingen van Azure Beveiligingscentrum?

Zie [beheren en beveiligingswaarschuwingen in Azure Beveiligingscentrum beantwoorden](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Hoe kan ik wijzigen wat met VM diagnostische gegevens worden verzameld?

Zie [PowerShell gebruiken om in te schakelen Azure diagnostische gegevens in een virtuele machine waarop Windows wordt uitgevoerd](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) voor meer informatie over het ophalen, wijzigen, en het instellen van de Azure diagnostische hulpprogramma's in Windows *(af)* configuratie. Hier volgt een voorbeeld:

### <a name="get-the-wad-config"></a>Ophalen van de zoekconfiguratie af

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Wijzigen van de zoekconfiguratie af

Het volgende voorbeeld is een configuratie waar alleen EventID 4624 en EventId 4625 worden verzameld uit het gebeurtenissenlogboek van beveiliging. Microsoft Antimalware gebeurtenissen zijn verzameld uit het gebeurtenislogboek systeem. Zie [door andere gebeurtenissen] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) voor meer informatie over het gebruik van XPath-expressies.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>De configuratie af instellen

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Nadat u wijzigingen aanbrengt, schakelt u het opslag-account om ervoor te zorgen dat de juiste gebeurtenissen die zijn verzameld.

Stuur een e-mail naar als u vragen over Azure Log integratie hebt, [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
