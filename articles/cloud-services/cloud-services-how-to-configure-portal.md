<properties 
    pageTitle="Het configureren van een cloudservice (portal) | Microsoft Azure" 
    description="Informatie over het configureren van cloudservices in Azure wordt aangegeven. Leer hoe u de configuratie van de cloud-service bijwerken en configureren van externe toegang tot rol exemplaren. In deze voorbeelden wordt de Azure-portal gebruiken." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Het configureren van Cloudservices

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-configure-portal.md)
- [Azure klassieke portal](cloud-services-how-to-configure.md)

U kunt de meest gebruikte instellingen voor een cloudservice configureren in de portal van Azure. Of, als u tevreden bent met de configuratiebestanden rechtstreeks bijwerkt, downloadt u een service-configuratiebestand wilt bijwerken, klikt u vervolgens de bijgewerkte bestand uploaden en de cloudservice bijwerken met wijzigingen in de configuratie. In beide gevallen zijn de configuratie-updates in alle exemplaren van de rol geactiveerd.

U kunt ook de exemplaren van uw cloud service rollen of extern bureaublad in deze beheren.

Azure kunt alleen ervoor zorgen dat de beschikbare service 99.95 percentage tijdens de configuratie-updates hebt u ten minste twee instanties van de rol voor elke rol. Waarmee één virtuele machine clientaanvragen verwerken terwijl de andere wordt bijgewerkt. Zie [Niveau serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/)voor meer informatie.

## <a name="change-a-cloud-service"></a>Een cloudservice wijzigen

Na het openen van de [Azure-portal](https://portal.azure.com/), navigeer naar de cloudservice. Hier kunt beheren u verschillende aspecten ervan. 

![Pagina-instellingen](./media/cloud-services-how-to-configure-portal/cloud-service.png)

De **instellingen van** of **alle** koppelingen wordt het blad **Instellingen** kunt u de **Eigenschappen**, wijzigen van de **configuratie**, de **certificaten**, setup **waarschuwingsregels**, beheren en het beheren van **gebruikers** die toegang hebben tot dit cloudservice geopend.

![Azure cloud service-instellingen blade](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Het besturingssysteem voor de cloudservice kan niet worden gewijzigd met behulp van de **Azure-portal**, kunt u deze instelling via de [portal van Azure klassieke](http://manage.windowsazure.com/)alleen wijzigen. Dit is gedetailleerde [hier](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Cmdlets voor controle

U kunt meldingen toevoegen aan uw cloudservice. Klik op **Instellingen** > **Waarschuwing regels** > **melding toevoegen**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Hier kunt u een melding instellen. U kunt een waarschuwing voor de volgende soorten gegevens instellen met de vervolgkeuzelijst **Mertic** .

- Schijf lezen
- Schijf schrijven
- Klik in het netwerk
- Netwerk af
- CPU-percentage 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Cmdlets voor controle van een metrische tegel configureren

In plaats van met **Instellingen** > **Waarschuwingsregels**, kunt u klikken op een van de metrische tegels in het gedeelte **controle** van het blad **cloudservice** .

![Cloudservice bewaken](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Hier kunt u de grafiek gebruikt met de tegel aanpassen of een waarschuwing regel toevoegen.


## <a name="reboot-reimage-or-remote-desktop"></a>Opnieuw opstarten, reimage of extern bureaublad

U kunt Extern bureaublad met behulp van de **Azure-portal**niet configureren op dit moment. Maar kunt u instellen deze via de [portal van Azure klassieke](cloud-services-role-enable-remote-desktop.md) [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), of via [Visual Studio](../vs-azure-tools-remote-desktop-roles.md). 

Klik eerst op het exemplaar van de cloud.

![Exemplaar van de cloud-Service](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Van het blad dat wordt geopend uou kunt starten van een verbinding met extern bureaublad, start opnieuw op afstand op het exemplaar of extern reimage (met een afbeelding van vers) het exemplaar beginnen.

![Cloud Service exemplaar knoppen](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>Uw .cscfg configureren

Mogelijk moet u opnieuw configureren service cloud tot en met de [service](cloud-services-model-and-package.md#cscfg) -configuratiebestand (cscfg). U moet eerst uw .cscfg-bestand downloaden, wijzigen en klik vervolgens uploaden.

1. Klik op het pictogram **Instellingen** of de koppeling **alle instellingen** om het blad **Instellingen** te openen.

    ![Pagina-instellingen](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Klik op het item **configuratie** .

    ![Configuratie Blade](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Klik op de knop **downloaden** .

    ![Downloaden](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Nadat u de service-configuratiebestand bijwerkt, uploaden en de configuratie-updates toepassen:

    ![Uploaden](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Selecteer het bestand .cscfg en klik op **OK**.

            
## <a name="next-steps"></a>Volgende stappen

* Leer hoe u [een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md)configureren.
* [Uw cloudservice beheren](cloud-services-how-to-manage-portal.md).
* [Ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md)configureren.