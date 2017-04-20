<properties
    pageTitle="Overzicht van de Azure portal navigeren"
    description="Meer van de andere gebruikerservaringen voor Service-Web-App tussen de beheerportal en de Azure-Portal"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>Overzicht van de Azure portal navigeren

Azure-Websites worden nu [App Service-WebApps](http://go.microsoft.com/fwlink/?LinkId=529714)genoemd. We bent bijwerken van alle onze documentatie aan deze naamswijziging en instructies voor de Portal Azure te geven. Totdat dat proces is voltooid, kunt u dit document als een handleiding voor het werken met Web Apps in de portal van Azure.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>De toekomst van de Portal van Azure-klassiek

Terwijl u de bestaande huisstijl wijzigingen in de klassieke Azure-Portal ziet, wordt die portal wordt vervangen door de Azure-Portal. Als de klassieke portal geleidelijk, wordt de focus de ontwikkeling van nieuwe bij de Portal Azure verschuiven. Alle komende nieuwe functies voor Web Apps komen in de Portal Azure. Begin met behulp van de Azure-Portal om te profiteren van de meest recente en grootste dat WebApps hebben om te bieden.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Indeling verschillen tussen de klassieke Azure-Portal en Azure-Portal

Klik in de klassieke portal vindt u alle Azure services op de linkerzijde. Navigatie in de klassieke portal volgt boomstructuur, waar u starten vanuit de service en navigeren in elk element. Deze structuur werkt prima wanneer onafhankelijke onderdelen beheren. Toepassingen is gebaseerd op Azure zijn echter een verzameling met onderling verbonden services, en deze boomstructuur is ideaal voor het werken met collecties van services niet. 

De portal van Azure kunt u gemakkelijk maken van toepassingen end-to-end met onderdelen uit meerdere services. De portal is ingedeeld als *reizen*. Een *reis* is een reeks *bladen*, containers voor de verschillende onderdelen. Bijvoorbeeld instellen automatisch schalen voor een web-app een *reis* waarmee u verschillende bladen is zoals wordt weergegeven in het volgende voorbeeld: het **website** blad (dat blade titel niet nog is bijgewerkt als u wilt gebruiken van de nieuwe terminologie), het blad **Instellingen** en het blad **schalen af** . In het voorbeeld wordt automatisch schalen ingesteld afhankelijk is van CPU-gebruik, zodat er ook een blade **CPU Percentage is** . De onderdelen binnen de *bladen* worden genoemd *onderdelen*, dat eruitziet als tegels. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigatie-voorbeeld: een WebApp maken

Maken van nieuwe WebApps is nog steeds zo eenvoudig als 1-2-3. De volgende afbeelding ziet u de klassieke portal en de portal naast elkaar om te laten zien dat niet veel is gewijzigd in het aantal stappen nodig om een WebApp en uit te voeren. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Klik in de portal kunt u kiezen uit de meest voorkomende van web-apps, waaronder populaire galerie-toepassingen zoals WordPress. Voor een volledige lijst met beschikbare toepassingen, gaat u naar de [Azure Marketplace].

Als u een web-app hebt gemaakt, kunt u opgeven URL, App-abonnement, en de locatie in de portal net als in de klassieke portal. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Bovendien definieert de portal u andere algemene instellingen. Bijvoorbeeld kunt [resourcegroepen](../azure-resource-manager/resource-group-overview.md) u eenvoudig Zie en verwante Azure bronnen beheren. 

## <a name="navigation-example-settings-and-features"></a>Voorbeeld van de navigatie: instellingen en functies

De instellingen en functies die worden nu logisch gegroepeerd in een enkel blade, van waaruit u kunt navigeren.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

U kunt bijvoorbeeld aangepaste domeinen maken door te klikken op **aangepaste domeinen en SSL** in het blad **Instellingen** .

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Als u een melding voor een controle wilt, op **vergaderverzoeken en fouten** en klik vervolgens **Waarschuwing toevoegen**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Als u wilt inschakelen diagnostische hulpprogramma's, klikt u op **Diagnostische logboeken** in het blad **Instellingen** .

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
Als u wilt configureren toepassingsinstellingen, klikt u op **Toepassingsinstellingen** in het blad **Instellingen** . 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Dan de naam van de huisstijl zijn een paar dingen in de portal verwijderd of hernoemd gegroepeerd anders om eenvoudiger te zoeken. Hieronder wordt bijvoorbeeld een schermafbeelding van de bijbehorende pagina voor app-instellingen (**configureren**) in de klassieke portal.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Meer informatiebronnen

[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)
 
