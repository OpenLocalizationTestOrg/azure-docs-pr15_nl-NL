<properties
    pageTitle="Web App klonen met behulp van Azure-Portal"
    description="Leer hoe u uw Web-Apps naar nieuwe Web-Apps met behulp van Azure Portal klonen."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure App Service App klonen Azure Portal gebruiken#

De klonen functie in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) kunt u eenvoudig klonen bestaande WebApps naar een nieuwe app in een andere regio of in dezelfde regio. Hiermee schakelt u klanten kunnen een aantal apps gebruiken tussen de verschillende regio's snel en gemakkelijk.

App klonen is momenteel alleen ondersteund voor premium laag app service-abonnementen. De nieuwe functie dezelfde beperkingen als functie van de back-up van Web Apps gebruikt, raadpleegt u [Back-up van een WebApp in Azure App-Service](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Een bestaande App klonen ##

De web-app moet worden uitgevoerd in de modus **Premium** om u te maken voor de web-app.

1. Open uw web-app blade in de [Portal van Azure](https://portal.azure.com/).
2. Klik op **hulpmiddelen**. Klikt u in het blad **hulpmiddelen** op **Klonen App**.

    ![][1]

    > [AZURE.NOTE]
    > Als de web-app nog niet is in de **Premium** -modus, ontvangt u een bericht waarin wordt aangegeven dat de ondersteunde modi voor app klonen. U hebt nu de optie voor het selecteren van de **Upgrade**.
    
3. Geef in het blad **Klonen App** een naam van de nieuwe web-app, resourcegroep en App-Service plannen. Ook de gebruiker is mogelijk opgeven of u een aantal instellingen voor gegevensbron web-app klonen of niet.

    ![][2]

4. Nadat u hebt geklikt **maken** begint het platform werken op de bron-web-app klonen.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Een bestaande App in een App-Service-omgeving klonen##

Klik in het blad **Klonen App** heeft de klant de optie voor het kiezen van een app-toepassingen in een App-Service-omgeving.

## <a name="current-restrictions"></a>Huidige beperkingen ##

Deze functie is momenteel in de proefversie, we werken als u wilt toevoegen van nieuwe mogelijkheden na verloop van tijd, de volgende lijst worden de bekende beperkingen voor de huidige ondersteuning van de app klonen in Azure-Portal:

- Azure verkeer Manager-instellingen worden niet gekopieerd.
- Automatisch schaalinstellingen worden niet gekopieerd
- Back-schema-instellingen worden niet gekopieerd.
- VNET instellingen worden niet gekopieerd.
- App inzichten worden niet automatisch ingesteld op de bestemming WebApp
- Eenvoudig Auth-instellingen worden niet gekopieerd.
- Kudu extensie niet worden gekopieerd
- TiP regels worden niet gekopieerd.
- Database-inhoud niet worden gekopieerd


### <a name="references"></a>Verwijzingen ###
- [Web App klonen via PowerShell](app-service-web-app-cloning.md)
- [Back-up van een WebApp in Azure App-Service](web-sites-backup.md)
- [Het maken van een App Service-omgeving](app-service-web-how-to-create-an-app-service-environment.md)
- [Een WebApp in een omgeving van de Service-App maken](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Inleiding tot het App-omgeving](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png