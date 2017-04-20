<properties
    pageTitle="Inhoud van de synchronisatie van een cloud-map met Azure App-Service"
    description="Informatie over het implementeren van uw app naar Azure App-Service via synchronisatie van inhoud van een cloud-map."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Inhoud van de synchronisatie van een cloud-map met Azure App-Service

Deze zelfstudie ziet u hoe u implementeren naar [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) door het synchroniseren van uw inhoud van populaire cloudservices opslag zoals Dropbox en OneDrive. 

## <a name="overview"></a>Overzicht van de implementatie van inhoud synchroniseren

De inhoud synchroniseren op aanvraag-implementatie is mogelijk gemaakt door de [implementatie-engine Kudu](https://github.com/projectkudu/kudu/wiki) geïntegreerd met App-Service. Klik in de [Portal van Azure](https://portal.azure.com)kunt u een map in uw cloudopslag, werken met uw app-code en de inhoud in die map en synchroniseren met de App Service met een muisklik. Inhoud synchroniseren maakt gebruik van het proces Kudu voor het maken en uitvoeren. 
    
## <a name="contentsync"></a>Het inschakelen van inhoud synchroniseren-implementatie
Als u wilt inschakelen inhoud synchroniseren op basis van de [Azure-Portal](https://portal.azure.com), als volgt te werk:

1. Klik op **Instellingen**in van uw app blade in de Portal Azure, > **Implementatie bron**. Klik op **Bron kiezen**en selecteer vervolgens **OneDrive** of **Dropbox** als bron voor implementatie. 

    ![Inhoud synchroniseren](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Vanwege de onderliggende verschillen in de API's, wordt **OneDrive voor bedrijven** niet ondersteund op dit moment. 

2. Hiermee voert u de werkstroom autorisatie om in te schakelen van de App-Service voor toegang tot een specifiek vooraf gedefinieerde aangewezen pad voor OneDrive of Dropbox waar al uw App Service-inhoud wordt opgeslagen.  
    Na autorisatie voor de App-Service krijgt platform u de optie om een map inhoud onder het opgegeven pad van de inhoud te maken of om een bestaande inhoud map onder deze aangewezen inhoud pad te kiezen. De aangewezen inhoud paden onder uw cloud-opslag-accounts gebruikt voor het synchroniseren van de App Service zijn de volgende:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Dropbox**:`Dropbox\Apps\Azure`

3. Na de eerste synchronisatie van de inhoud kan de inhoud synchroniseren op verzoek van de Azure portal worden gestart. Geschiedenis van de implementatie is beschikbaar met het blad **implementaties** .

    ![Implementatie geschiedenis](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
Meer informatie voor Dropbox-implementatie is beschikbaar onder [Deploy vanuit Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 


