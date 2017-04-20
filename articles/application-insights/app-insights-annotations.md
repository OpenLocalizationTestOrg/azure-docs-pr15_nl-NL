<properties
    pageTitle="Los aantekeningen voor toepassing inzichten | Microsoft Azure"
    description="Implementatie toevoegen of markeringen om uw doelstellingen explorer-grafieken in de toepassing inzichten te maken."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>Release aantekeningen in de toepassing inzichten

Laat aantekeningen naar [Aan de doelstellingen Explorer](app-insights-metrics-explorer.md) grafieken weergeven waarin u een nieuwe build geïmplementeerd. Ze kunnen u eenvoudig om te zien of de wijzigingen geen invloed op de prestaties van de toepassing hadden. Ze automatisch kunnen worden gemaakt met [Visual Studio Team Services systeem maken](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs)en u kunt ook [deze PowerShell maken](#create-annotations-from-powershell).

![Voorbeeld van aantekeningen met zichtbare correlatie met server antwoord tijd](./media/app-insights-annotations/00.png)

Release aantekeningen zijn van een functie van de build cloudgebaseerde en de toets los-service van de Visual Studio Team Services. 

## <a name="install-the-annotations-extension-one-time"></a>Installeren van de extensie aantekeningen (één keer)

Als u wilt kunnen release aantekeningen maken, moet u een van de vele Team extensies beschikbaar in de Visual Studio-marktplaats te installeren.

1. Meld u aan bij uw project [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) .
2. In Visual Studio Marketplace, [krijgen de extensie Release aantekeningen](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), en deze aan uw Team Services-account toevoegen.

![AT belangrijkste rechts van de webpagina Team Services, open Marketplace. Selecteer visuele Team Services en klik onder opbouwen en release-programma's Kies meer zien.](./media/app-insights-annotations/10.png)

Alleen moet u één keer deze stap herhalen voor uw Visual Studio Team Services-account. Release aantekeningen kunnen nu worden geconfigureerd voor een project in uw account. 

## <a name="get-an-api-key-from-application-insights"></a>Een API-sleutel krijgen van toepassing inzichten

U moet deze stap herhalen voor elk release-sjabloon die u wilt release aantekeningen maken.


1. Meld u aan bij de [Portal van Microsoft Azure](https://portal.azure.com) en open de toepassing inzichten resource die uw toepassing bewaakt. (Of [er nu een maken](app-insights-overview.md), als u dit nog niet hebt gedaan nog).
2. Open **API Access**en een kopie van de **Toepassing inzichten-Id**.

    ![Open de toepassing inzichten resource in portal.azure.com, en kies instellingen. Open API-toegang. Kopieer de toepassings-ID](./media/app-insights-annotations/20.png)

2. Open (of maak) in een afzonderlijk browservenster, op de release-sjabloon die worden beheerd door uw implementaties van Visual Studio Team Services. 

    Een taak toevoegen en selecteer de toepassing inzichten Release aantekening-taak in het menu.

    Plak de **Toepassings-Id** die u hebt gekopieerd uit het blad API-toegang.

    ![In Visual Studio Team Services, Release openen, selecteert u de definitie van een release en kiest u bewerken. Klik op taak toevoegen en selecteer toepassing inzichten Release aantekeningen. Plak de inzichten toepassings-id.](./media/app-insights-annotations/30.png)

3. Het veld **APIKey** instellen op een variabele `$(ApiKey)`.

4. Terug in het blad API Access een nieuwe API-sleutel maken en uitvoeren van een kopie van deze.

    ![Klik in het blad API Access in het Azure-venster, op API-sleutel maken. Geef een opmerking, schrijven aantekeningen controleren en klik op code genereren. Kopieer de nieuwe sleutel.](./media/app-insights-annotations/40.png)

4. Open het tabblad configuratie van de release-sjabloon.

    Maken van een variabele definitie voor `ApiKey`.

    Plak uw API-sleutel aan de ApiKey variabele definitie.

    ![Selecteer het tabblad configuratie en op variabele toevoegen in het venster Team Services. Stel de naam ApiKey en in de waarde, plak de sleutel die u zojuist hebt gegenereerd.](./media/app-insights-annotations/50.png)


5. De definitie release ten slotte **Opslaan** .

## <a name="create-annotations-from-powershell"></a>Aantekeningen maken op basis van PowerShell

U kunt ook aantekeningen maken van een proces dat u tevreden bent met (zonder tegenover Team System). 

De [Powershell-script uit GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)ophalen.

Gebruik deze als volgt:

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

Krijgen de `applicationId` en een `apiKey` uit uw toepassing inzichten resource: instellingen openen, API Access en de id van de toepassing kopiëren Klik op API-sleutel maken en de toets kopiëren. 

## <a name="release-annotations"></a>Release aantekeningen

Wanneer u de sjabloon release gebruiken om te implementeren van een nieuwe versie, wordt er nu een aantekening worden verzonden naar toepassing inzichten. De aantekeningen wordt van grafieken in aan de doelstellingen Explorer weergegeven.

Klik op een markering aantekeningen voor meer informatie over de release, met inbegrip van die persoon, bron besturingselement tak openen, los definitie, -omgeving en meer.


![Klik op een markering voor release aantekeningen.](./media/app-insights-annotations/60.png)
