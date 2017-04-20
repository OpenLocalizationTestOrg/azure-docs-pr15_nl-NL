<properties 
    pageTitle="Een Java-toepassing tot Azure App Service Web Apps toevoegen" 
    description="Deze zelfstudie ziet u hoe u een pagina of de toepassing op uw exemplaar van Azure App Service Web Apps die al is geconfigureerd voor gebruik van Java toevoegen." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Een Java-toepassing tot Azure App Service Web Apps toevoegen

Zodra u hebt uw Java-web-app in [Azure App Service][] geïnitialiseerd, zoals beschreven op [een Java-web-app in Azure App Service maken](web-sites-java-get-started.md), kunt u uw toepassing door te plaatsen van uw WAR in de map **webapps** kunt uploaden.

De navigatie-pad naar de map **webapps** verschilt op basis van hoe u uw exemplaar van de Web-Apps instellen.

- Als u uw web-app hebt ingesteld met behulp van de Azure Marketplace, luidt het pad naar de map **webapps** in het formulier **d:\home\site\wwwroot\bin\application\_server\webapps**, waarbij **toepassing\_server** is de naam van de toepassingsserver van kracht voor uw exemplaar van de Web Apps. 
- Als u uw web-app hebt ingesteld met behulp van de Azure UI-configuratie, is het pad naar de map **webapps** in het formulier **d:\home\site\wwwroot\webapps**. 

Houd er rekening mee dat u bronbeheer gebruiken kunt voor het uploaden van uw toepassing of webpagina's, met inbegrip van [continue integratie scenario's](app-service-continuous-deployment.md). FTP is ook een optie voor het uploaden van uw toepassing of webpagina's.

Notitie voor Tomcat WebApps: nadat u uw WAR-bestand hebt geüpload naar de map **webapps** , de toepassingsserver Tomcat wordt gedetecteerd dat u deze hebt toegevoegd en automatisch wordt geladen. Houd er rekening mee dat als u bestanden (behalve WAR-bestanden) naar de hoofdmap kopieert, de toepassingsserver opnieuw worden gestart moeten voordat de bestanden die worden gebruikt. De functionaliteit voor automatisch laden voor de Tomcat Java-web-apps waarop Azure is gebaseerd op een nieuwe WAR-bestand wordt toegevoegd, of nieuwe bestanden of mappen die zijn toegevoegd aan de map **webapps** . 

## <a name="next-steps"></a>Volgende stappen

Zie het [Java Developer Center](/develop/java/)voor meer informatie.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App-Service]: http://go.microsoft.com/fwlink/?LinkId=529714
