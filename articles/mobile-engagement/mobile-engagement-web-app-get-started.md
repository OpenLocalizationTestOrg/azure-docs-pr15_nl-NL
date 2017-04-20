<properties
    pageTitle="Aan de slag met Azure Mobile betrokkenheid voor Web Apps | Microsoft Azure"
    description="Informatie over het gebruiken van Azure Mobile betrokkenheid met analyses en push-meldingen voor Web Apps."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Aan de slag met Azure Mobile betrokkenheid voor Web Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp ziet u hoe u Azure Mobile betrokkenheid gebruiken voor meer informatie over het gebruik van uw Web App.

Deze zelfstudie is het volgende nodig:

+ Visual Studio-2015 of een andere editor van uw keuze
+ [Web SDK](http://aka.ms/P7b453) 

Deze SDK Web is in de proefversie van en alleen Analytics ondersteunt momenteel en verzendende browser of in de app push-meldingen nog niet worden ondersteund. 

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started)voor meer informatie.

##<a name="setup-mobile-engagement-for-your-web-app"></a>Mobiele betrokkenheid van de instelling voor uw Web-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app op de backend Mobile betrokkenheid

Een "eenvoudige integratie,' dat wil de minimale set vereist zeggen voor het verzamelen van gegevens worden in deze zelfstudie weergegeven.

We gaan een eenvoudige web-app maken met Visual Studio om te laten zien van de integratie Hoewel bij een webtoepassing gemaakt buiten Visual Studio ook kunt u de stappen volgen. 

###<a name="create-a-new-web-app"></a>Een nieuwe Web-App maken

De volgende stappen wordt ervan uitgegaan dat het gebruik van Visual Studio-2015 Hoewel de stappen identiek in eerdere versies van Visual Studio zijn. 

1. Visual Studio Start en selecteer in **het beginscherm** **Nieuw Project**.

2. Selecteer in het pop-upvenster **Web** -> **ASP.Net-webtoepassing**. Vul de app **naam**, **locatie** en **naam van de oplossing**, en klik vervolgens op **OK**.

3. Selecteer **lege** onder **ASP.Net 4.5 sjablonen** in de **een sjabloon selecteren** wordt weergegeven, en klik op **OK**. 

U hebt nu een nieuw, leeg Web App project waarin de Azure Mobile betrokkenheid Web SDK integreert gemaakt.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Uw app verbinden met mobiele betrokkenheid backend

1. Een nieuwe map genaamd **javascript** in uw oplossing maken en toevoegen van het Web SDK JS-bestand **azure-engagement.js** erin. 

2. Een nieuw bestand genaamd **main.js** in deze javascript-map met de volgende code toevoegen. Zorg ervoor dat de verbindingsreeks bijwerken. Dit `azureEngagement` object wordt gebruikt voor toegang tot Web SDK methoden. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio met js-bestanden][1]

##<a name="enable-real-time-monitoring"></a>Realtime controle inschakelen

Om te beginnen met het verzenden van gegevens en ervoor te zorgen dat de gebruikers actieve, moet u ten minste één activiteit verzenden naar de betrokkenheid van de Mobile-end. Een activiteit in de context van een web-app is een pagina met webonderdelen. 

1. Een nieuwe pagina **home.html** genoemd in de oplossing maken en dit als de beginpagina voor uw web-app instellen. 
2. De twee JavaScript die we eerder in deze pagina hebt toegevoegd door toe te voegen van de volgende handelingen uit in de hoofdtekst tag opnemen. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Bijwerken van de tag hoofdtekst om te bellen van EngagementAgent `startActivity` methode
        
        <body onload="engagement.agent.startActivity('Home')">

4. Hier ziet u hoe uw **home.html** eruit zullen
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Verbinding maken met de app met realtime bewaken

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Analytics uitbreiden

Hier zijn alle methoden beschikbaar bij Web SDK die u voor analytics gebruiken kunt:

1. Activiteiten/webpagina's:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Gebeurtenissen
        
        engagement.agent.sendEvent(name, extras);

3. Fouten

        engagement.agent.sendError(name, extras);

4. Taken

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

