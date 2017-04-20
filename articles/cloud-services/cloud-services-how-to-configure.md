<properties 
    pageTitle="Het configureren van een cloudservice (klassieke portal) | Microsoft Azure" 
    description="Informatie over het configureren van cloudservices in Azure wordt aangegeven. Leer hoe u de configuratie van de cloud-service bijwerken en configureren van externe toegang tot rol exemplaren." 
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

U kunt de meest gebruikte instellingen voor een cloudservice configureren in de portal van Azure klassieke. Of, als u tevreden bent met de configuratiebestanden rechtstreeks bijwerkt, downloadt u een service-configuratiebestand wilt bijwerken, klikt u vervolgens de bijgewerkte bestand uploaden en de cloudservice bijwerken met wijzigingen in de configuratie. In beide gevallen zijn de configuratie-updates in alle exemplaren van de rol geactiveerd.

De portal van Azure klassieke kunt u ook [Verbinding met extern bureaublad voor een rol in Azure Cloud Services](cloud-services-role-enable-remote-desktop.md) inschakelen

Azure kunt alleen ervoor zorgen dat de beschikbare service 99.95 percentage tijdens de configuratie-updates hebt u ten minste twee instanties van de rol voor elke rol. Waarmee één virtuele machine clientaanvragen verwerken terwijl de andere wordt bijgewerkt. Zie [Niveau serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/)voor meer informatie.

## <a name="change-a-cloud-service"></a>Een cloudservice wijzigen

1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**, klikt u op de naam van de cloudservice en klik vervolgens op **configureren**.

    ![Configuratiepagina](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    Klik op de pagina **configureren** kunt u configureren monitoring, rolinstellingen bijwerken en kies het besturingssysteem van de Gast en familie naar exemplaren van de rol. 

2. Stel het niveau van de controle op uitgebreid of minimale in **monitoring**, en de hulpprogramma's voor diagnose verbindingstekenreeksen die vereist voor het uitgebreide controle zijn configureren.

3. Voor service rollen (gegroepeerd per rol), kunt u de volgende instellingen kunt bijwerken:
    
    >**Instellingen**  
    >Wijzig de waarden van diverse configuratie-instellingen die zijn opgegeven in de *ConfigurationSettings* elementen van de service-configuratiebestand (.cscfg).
    >
    >**Certificaten**  
    >Wijzig de vingerafdruk van het certificaat dat wordt gebruikt in SSL-versleuteling voor een rol. Als u wilt wijzigen van een certificaat, moet u eerst het nieuwe certificaat (Klik op de pagina **certificaten** ) uploaden. Vervolgens de vingerafdruk in de certificaattekenreeks weergegeven in de rolinstellingen bijwerken

4. In **besturingssysteem gebruikt**, kunt u het besturingssysteem familie of de versie voor rol exemplaren wijzigen of kies **Automatische** automatische updates van de huidige versie van besturingssysteem inschakelen. De instellingen van het besturingssysteem van toepassing op web rollen en werknemer rollen, maar niet van invloed zijn op de virtuele Machines.

    De meest recente versie van besturingssysteem op alle exemplaren van de rol is geïnstalleerd tijdens de implementatie en de besturingssystemen worden automatisch bijgewerkt al dan niet standaard. 
    
    Als u nodig voor uw cloudservice om uit te voeren op een ander besturingssysteemversie vanwege compatibiliteitsvereisten in code hebt, kunt u een besturingssysteem familie en versie. Als u een specifiek besturingssysteemversie kiest, worden automatisch besturingssysteem updates voor de cloudservice geschorst. U moet zorgen dat de besturingssystemen updates ontvangen.
    
    Als u alle compatibiliteitsproblemen die uw apps met de meest recente versie van besturingssysteem hebt oplossen, kunt u besturingssysteem automatische updates inschakelen door in te stellen van de versie van het besturingssysteem op **automatisch**. 
    
    ![Instellingen voor OS](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. Als u wilt opslaan van uw configuratieinstellingen en deze in de rol-exemplaren push, klikt u op **Opslaan**. (Klik op **negeren** om de wijzigingen te annuleren.) **Opslaan** en **negeren** zijn toegevoegd aan de opdrachtbalk nadat u een instelling hebt gewijzigd.

## <a name="update-a-cloud-service-configuration-file"></a>Bijwerken van een configuratiebestand cloud-service

1. Download een cloud configuratie servicebestand (.cscfg) met de huidige configuratie. Klik op de pagina **configureren** voor de cloudservice klikt u op **downloaden**. Klik op **Opslaan**of klik op **OpslaanAls** om het bestand.

2. Nadat u de service-configuratiebestand bijwerkt, uploaden en de configuratie-updates toepassen:

    1. Klik op **uploaden**op de pagina **configureren** .
    
        ![Configuratie uploaden](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. In **configuratiebestand**gebruiken **Bladeren** om de bijgewerkte .cscfg-bestand te selecteren.
    
    3. Als uw cloudservice alle functies die slechts één exemplaar hebt bevat, selecteert u het selectievakje **configuratie toepassen, zelfs als een of meer rollen één exemplaar bevatten** om in te schakelen van de configuratie-updates voor de rollen om verder te gaan.
    
        Tenzij u ten minste twee instanties van elke rol definieert, Azure ten minste 99.95 procent kan niet garanderen beschikbaarheid van de cloudservice tijdens de configuratie-service-updates. Zie [Niveau serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/)voor meer informatie.
    
    4. Klik op **OK** (vinkje). 


## <a name="next-steps"></a>Volgende stappen

* Leer hoe u [een cloudservice](cloud-services-how-to-create-deploy.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name.md)configureren.
* [Uw cloudservice beheren](cloud-services-how-to-manage.md).
* [Verbinding met extern bureaublad voor een rol in Azure Cloudservices inschakelen](cloud-services-role-enable-remote-desktop.md)
* [Ssl-certificaten](cloud-services-configure-ssl-certificate.md)configureren.
