<properties 
    pageTitle="Extern bureaublad voor cloudservices (Node.js) inschakelen" 
    description="Leer hoe u extern bureaublad toegang inschakelen voor de virtuele machines uw Azure Node.js-toepassing te hosten." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="enabling-remote-desktop-in-azure"></a>Extern bureaublad in Azure inschakelen

Extern bureaublad kunt u toegang tot het bureaublad van een rol exemplaar uitgevoerd in Azure wordt aangegeven. U kunt een verbinding met extern bureaublad Configureer de virtuele machine of problemen oplossen met de toepassing.

> [AZURE.NOTE] In dit artikel is van toepassing op Node.js toepassingen die worden gehost als een Azure-Cloudservice.


## <a name="prerequisites"></a>Vereisten voor

- Installeren en configureren van [Azure Powershell](../powershell-install-configure.md).
- Een app Node.js dashboard implementeren naar een Azure-Cloudservice. Zie [bouwen en implementeren van een toepassing Node.js naar een Cloudservice Azure](cloud-services-nodejs-develop-deploy-app.md)voor meer informatie.


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Stap 1: Gebruik Azure PowerShell voor het configureren van de service voor extern bureaublad-toegang

Als u wilt Remote Desktop gebruikt, moet u de servicedefinitie van Azure-en configuratie bijwerken met een gebruikersnaam, wachtwoord en certificaat. 

De volgende stappen uitvoeren vanaf een computer waarop de bronbestanden voor de app bevat.

1. **Windows PowerShell** als Administrator uitvoeren. (In het **Menu Start** of het **Startscherm**, zoekt u **Windows PowerShell**.)

2.  Navigeer naar de map waarin de definitie van de service (.csdef) en bestanden van de service-configuratie (.cscfg).

3. Voer in het volgende PowerShell-cmdlet:

        Enable-AzureServiceProjectRemoteDesktop

4. Voer een gebruikersnaam en wachtwoord bij de prompt.

    ![inschakelen azureserviceprojectremotedesktop][enable-rdp]

3.  Voer in het volgende PowerShell-cmdlet om de wijzigingen te publiceren:

        Publish-AzureServiceProject

    ![publiceren azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Stap 2: Verbinding maken met het exemplaar van de rol

Nadat u de definitie van de service update publiceert, kunt u een verbinding maken met het exemplaar van de rol.

1.  Klik in de [portal van Azure klassieke] **Cloud Services** selecteren en selecteer uw service.

    ![Azure klassieke portal][cloud-services]

2.  **Exemplaren**op en klik vervolgens op **productie** of **tijdelijke** als u wilt zien van de exemplaren voor uw service. Selecteer een exemplaar en klik op **verbinding maken met** onderaan op de pagina.

    ![De pagina exemplaren][3]

2.  Als u **verbinding maken met**klikt, wordt de webbrowser vraagt u een RDP-bestand wilt opslaan. Dit bestand niet openen. (Bijvoorbeeld als u Internet Explorer gebruikt, klikt u op **openen**.)

    ![vragen om te openen of opslaan van het RDP-bestand][4]

3.  Wanneer het bestand wordt geopend, wordt de volgende beveiligingsprompt weergegeven:

    ![Beveiligingsprompt voor Windows][5]

4.  Klik op **verbinding maken**, en een beveiligingsprompt voor het invoeren van referenties voor toegang tot het exemplaar wordt weergegeven. Voer het wachtwoord die u hebt gemaakt in [stap 1] [stap 1: de service voor extern bureaublad toegang via Azure PowerShell configureren], en klik vervolgens op **OK**.

    ![gebruikersnaam en wachtwoord gevraagd][6]

Wanneer de verbinding wordt gemaakt, geeft verbinding met extern bureaublad het bureaublad van het exemplaar in Azure wordt aangegeven. 

![Extern bureaublad-sessies][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Stap 3: De service als u wilt uitschakelen van extern bureaublad access configureren 

Wanneer u extern bureaublad, verbindingen in de exemplaren van de rol in de cloud niet meer nodig hebt, moet u externe bureaublad toegang via [Azure PowerShell]uitschakelen.

1.  Voer in het volgende PowerShell-cmdlet:

        Disable-AzureServiceProjectRemoteDesktop

2.  Voer in het volgende PowerShell-cmdlet om de wijzigingen te publiceren:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Aanvullende informatie

- [Op afstand toegang krijgen tot het exemplaren van de rol in Azure wordt aangegeven] 
- [Gebruik van extern bureaublad met Azure rollen]
- [Node.js Developer Center](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure klassieke portal]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Op afstand toegang krijgen tot het exemplaren van de rol in Azure wordt aangegeven]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Gebruik van extern bureaublad met Azure rollen]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 