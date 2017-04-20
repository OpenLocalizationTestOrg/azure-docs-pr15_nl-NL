<properties
    pageTitle="Foutopsporing op afstand met continue bezorging inschakelen | Microsoft Azure"
    description="Leer hoe u foutopsporing op afstand inschakelen wanneer u met de bezorging van continue implementeren naar Azure"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Foutopsporing op afstand inschakelen wanneer u een doorlopend bezorging publiceren naar Azure gebruikt

U kunt foutopsporing op afstand inschakelen in Azure wordt aangegeven, voor cloudservices of virtuele machines, wanneer u [doorlopend bezorging](cloud-services-dotnet-continuous-delivery.md) gebruikt voor het publiceren naar Azure door deze stappen uit.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Externe foutopsporing voor cloudservices inschakelen

1. Klik op de agent opbouwen instellen de eerste omgeving voor Azure zoals wordt beschreven in de [Opdrachtregel voor Azure opbouwen](http://msdn.microsoft.com/library/hh535755.aspx).
2. Omdat de externe foutopsporing runtime (msvsmon.exe) vereist voor het pakket is, installeert u de [Remote Tools voor Visual Studio-2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (of de [Externe hulpprogramma's voor Microsoft Visual Studio 2013 Update 5](https://www.microsoft.com/en-us/download/details.aspx?id=48156) als u Visual Studio 2013 gebruikt). Als alternatief, kunt u de externe foutopsporing binaire bestanden kopiëren van een systeem met Visual Studio is geïnstalleerd.
3. Een digitaal certificaat maken zoals beschreven in het [Overzicht van certificaten voor Azure-Cloudservices](cloud-services-certs-create.md). Houd de .pfx en RDP certificaatvingerafdruk en uploadt u het certificaat in de doel-cloudservice.
4. Gebruik de volgende opties in de opdrachtregel MSBuild voor bouwen en pakket met externe foutopsporing ingeschakeld. (SUBSTITUEREN werkelijke paden naar uw systeem en project-bestanden voor de hoek tussen haakjes items.)

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`het pad naar de map die msvsmon.exe in de externe Tools voor Visual Studio is.
    `RemoteDebuggerConnectorVersion`is de versie van Azure SDK in uw cloudservice. Dit moet ook overeenkomen met de versie is geïnstalleerd met Visual Studio.

5. Publiceren naar de doel-cloudservice met behulp van het pakket en .cscfg bestand gegenereerd in de vorige stap.
6. Importeer het certificaat (.pfx-bestand) naar de computer waarop Visual Studio met Azure SDK voor .NET is geïnstalleerd. Zorg ervoor dat om te importeren in de `CurrentUser\My` certificaat store, anders koppelen aan de foutopsporing in Visual Studio, mislukt.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Externe foutopsporing voor virtuele machines inschakelen

1. Maak een Azure virtuele machines. Zie [een virtuele Machine met Windows Server maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) of [maken en beheren van Azure virtuele Machines in Visual Studio](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. Klik op de [Azure klassieke portalpagina](http://go.microsoft.com/fwlink/p/?LinkID=269851)van de virtuele machine-dashboard om te zien van de virtuele machine **RDP CERTIFICAATVINGERAFDRUK**te bekijken. Deze waarde wordt gebruikt voor de `ServerThumbprint` waarde in de configuratie van de extensie.
3. Een clientcertificaat maken, zoals beschreven in het [Overzicht van certificaten voor Azure-Cloudservices](cloud-services-certs-create.md) (laten de .pfx en RDP certificaatvingerafdruk).
4. Azure Powershell installeren (versie 0.7.4 of hoger) zoals beschreven in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).
5. Voer de volgende script om in te schakelen van de extensie RemoteDebug. Vervang de paden en persoonlijke gegevens door uw eigen, zoals de naam van abonnement, de servicenaam van de en vingerafdruk.

    >[AZURE.NOTE] Dit script is geconfigureerd voor Visual Studio-2015. Als u Visual Studio 2013 gebruikt, wijzigt u de `$referenceName` en `$extensionName` toewijzingen onderstaande gebruik `RemoteDebugVS2013` (in plaats van `RemoteDebugVS2015`).

    <pre>
   Toevoegen AzureAccount

    Selecteer-AzureSubscription "Mijn abonnement op Microsoft"

    $vm = "mytestvm1" get-AzureVM - servicenaam-naam "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    foreach ($endpoint in $endpoints) {toevoegen-AzureEndpoint - VM $vm-$endpoint een naam. Name - Protocol tcp - PublicPort $endpoint. PublicPort - LokalePoort $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1.*" $publicConfiguration = "<PublicConfig>< Connector.Enabled > waar < /Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension `
    -ReferenceName $referenceName ` 
    -Publisher $publisher `
    -ExtensionName $extensionName ` 
    -versie $version ' - PublicConfiguration $publicConfiguration

    foreach ($extension in $vm. VM. ResourceExtensionReferences) {als (($extension. Verwijzing - eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - en ($extension. Een naam geven - eq $extensionName) '- en ($extension. Versie - eq $version)) {$extension. ResourceExtensionParameterValues [0]. Toets = 'config.txt' einde}}

    $vm | Update-AzureVM </pre>

6. Importeer het certificaat (.pfx) naar de computer waarop Visual Studio met Azure SDK voor .NET is geïnstalleerd.
