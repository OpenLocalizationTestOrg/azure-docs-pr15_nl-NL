<properties 
pageTitle="Verbinding met extern bureaublad voor een rol in Azure Cloudservices inschakelen" 
description="Het configureren van uw azure cloud-servicetoepassing toe te staan dat verbindingen met extern bureaublad" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Verbinding met extern bureaublad voor een rol in Azure Cloudservices inschakelen

>[AZURE.SELECTOR]
- [Azure klassieke portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Extern bureaublad kunt u toegang tot het bureaublad van een rol uitgevoerd in Azure wordt aangegeven. U kunt een verbinding met extern bureaublad gebruiken voor het oplossen van problemen met uw toepassing terwijl deze wordt uitgevoerd. 

U kunt een verbinding met extern bureaublad in uw rol tijdens de ontwikkeling inschakelen door de modules extern bureaublad te nemen in de servicedefinitie van uw of u kunt Extern bureaublad met het externe bureaublad Extension inschakelen. De voorkeur benadering is de extensie extern bureaublad gebruiken als u extern bureaublad inschakelen kunt, zelfs nadat de toepassing wordt geïmplementeerd zonder dat u moet uw toepassing opnieuw distribueren. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Extern bureaublad van de Azure klassieke portal configureren
De portal van Azure klassieke gebruikt de benadering extern bureaublad extensie zodat u extern bureaublad inschakelen kunt, zelfs nadat de toepassing wordt geïmplementeerd. De pagina **configureren** voor uw cloudservice, kunt u extern bureaublad inschakelen, wijzigt u het lokale beheerdersaccount waarmee verbinding met de virtuele machines, het certificaat in verificatie gebruikt en de vervaldatum instellen. 


1. Klik op **Cloudservices**, klikt u op de naam van de cloudservice en klik vervolgens op **configureren**.

2. Klik op **externe**.
    
    ![Externe cloudservices](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Alle exemplaren van de rol wordt opgestart wanneer u eerst Extern bureaublad inschakelen en klik op OK (vinkje). Als u wilt voorkomen dat opnieuw opstarten, moet het certificaat dat is gebruikt voor het coderen van het wachtwoord op de rol zijn geïnstalleerd. Om te voorkomen dat een herstart, [een certificaat voor de cloudservice uploaden](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) en Ga terug naar dit dialoogvenster.
    

3. Selecteer de rol die u wilt bijwerken of selecteer **alle** voor alle rollen in **rollen**.

4. Een van de volgende wijzigingen aanbrengen:
    
    - Als u extern bureaublad, selecteert u het selectievakje **Extern bureaublad inschakelen** . Schakel het selectievakje in als u wilt uitschakelen van extern bureaublad.
    
    - Maak een account in extern bureaublad-verbindingen met de rol exemplaren gebruiken.
    
    - Werk het wachtwoord voor het bestaande account.
    
    - Selecteer een geüploade certificaat wilt gebruiken voor verificatie (uploaden het certificaat dat met **uploaden** op de pagina **certificaten** ) of een nieuw certificaat maken. 
    
    - Wijzig de vervaldatum voor de configuratie Extern bureaublad.

5. Wanneer u klaar bent met de updates van uw configuratie, klikt u op **OK** (vinkje).


## <a name="remote-into-role-instances"></a>Remote in rol exemplaren
Zodra extern bureaublad is ingeschakeld op de rollen kunt remote u in een exemplaar van de rol via verschillende hulpmiddelen.

Verbinding maken met een exemplaar van de rol van de Azure klassieke portal:
    
  1.   Klik op **exemplaren** om de pagina **exemplaren** te openen.
  2.   Selecteer een rol exemplaar met extern bureaublad geconfigureerd.
  3.   Klik op **verbinding maken**en volg de instructies voor het openen van het bureaublad. 
  4.   Klik op **openen** en klik vervolgens **koppelen** om te starten van de verbinding met extern bureaublad. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Gebruik Visual Studio om externe in een exemplaar van de rol

In Visual Studio Server Explorer:

1. Vouw de **Azure\\Cloudservices\\[naam van de cloud-service]** knooppunt.
2. **Hetzij **Staging** - of**uitvouwen.
3. Vouw de afzonderlijke rol.
4. Met de rechtermuisknop op een van de rol exemplaren, klikt u op **verbinding maken met behulp van extern bureaublad...**en voer vervolgens de gebruikersnaam en wachtwoord. 

![Server explorer extern bureaublad](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>PowerShell gebruiken om het RDP-bestand
U kunt de cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) gebruiken om op te halen het RDP-bestand. U kunt het RDP-bestand vervolgens verbinding met extern bureaublad gebruiken voor toegang tot de cloudservice.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Het bestand RDP via de Service Management REST API via een programma te downloaden
U kunt de bewerking van de REST [RDP-bestand downloaden](https://msdn.microsoft.com/library/jj157183.aspx) gebruiken om de RDP-bestand te downloaden. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Extern bureaublad in het definitiebestand service configureren

Deze methode kunt u extern bureaublad inschakelen voor de toepassing tijdens de ontwikkeling. Deze methode is vereist versleutelde wachtwoorden worden opgeslagen in de configuratie van uw service Bestands- en alle updates in het externe bureaublad configuratie vereist een opnieuw installeren van de toepassing. Als u wilt voorkomen dat deze de nadelen moet u het externe bureaublad extensie op basis van aanpak hierboven is beschreven.  

U kunt Visual Studio [inschakelen van een verbinding met extern bureaublad](../vs-azure-tools-remote-desktop-roles.md) met de service definitie bestand aanpak.  
De onderstaande stappen worden de wijzigingen die nodig zijn voor de service-modelbestanden om extern bureaublad beschreven. Visual Studio worden deze wijzigingen automatisch gemaakt wanneer u publiceert.

### <a name="set-up-the-connection-in-the-service-model"></a>De verbinding in het servicemodel instellen 
Met de **invoer** -element kunt u de module **Remote Access** en de module **RemoteForwarder** importeren naar het bestand [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) .

De definitie servicebestand moet er ongeveer als het volgende voorbeeld met de `<Imports>` element toegevoegd.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Het bestand [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) moet zijn vergelijkbaar met het volgende voorbeeld, houd rekening met de `<ConfigurationSettings>` en `<Certificates>` elementen. Het certificaat dat u opgeeft moet worden [geüpload naar de cloudservice](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Aanvullende informatie

[Het configureren van Cloudservices](cloud-services-how-to-configure.md)