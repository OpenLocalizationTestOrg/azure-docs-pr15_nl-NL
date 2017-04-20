<properties
    pageTitle="Web-Apps op Azure stapel - bekende problemen en probleemoplossing | Microsoft Azure"
    description="Gedetailleerde richtlijnen voor het Web-Apps Azure gestapelde implementeren"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Web Apps Resource Provider - bekende problemen en probleemoplossing

> [AZURE.NOTE] De volgende informatie is alleen van toepassing op Azure stapel TP1 implementaties.

Als u problemen ondervindt bij het implementeren of het werken met Web Apps op Azure Stack raadpleegt u de onderstaande richtlijnen.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure stapel Web-Apps niet beschikbaar in de portal

![Azure stapel WebApps maken nieuw WebApp][1]

Nadat u kunt de [registratie van de Provider Azure stapel Web-Apps](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) hebt voltooid, als u niet kunt met het Web + mobiele blade vinden vervolgens probeert u het volgende:
* **Meld u af van de portal** en **Sluit de browser** en drukt u in een **nieuwe browser venster aanmelding bij de portal** en probeer het opnieuw.

Als u zich nog steeds zien niet op het web en mobiele blade, probeert u het volgende:

1.  Zoek op de **Azure stapel Host Machine** in **Hyper-V-beheer** de **xRPVM** en **verbinding maken** met de VM.
2.  Open een **opdrachtprompt als Administrator** en **IISRESET** uitvoeren
3.  Ga terug naar de **ClientVM** en open een **nieuw browservenster**, **Meld u aan bij de portal** en probeer het opnieuw.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Implementatie mislukt bij het maken van een Web-App in een nieuwe resourcegroep

In de eerste Preview van Web-Apps, worden **Alle Web-Apps** gemaakt in dezelfde resourcegroep als de Web-Apps Resource-Provider (**AppService-LOCAL**).  Dit is een bekend probleem en in een toekomstige preview wordt opgelost.

## <a name="other-issues"></a>Andere problemen

Als u andere problemen met het Web-Apps op Azure Stack boek in [het Forum Azure stapel] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) waar leden van ons team berichten wilt controleren.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
