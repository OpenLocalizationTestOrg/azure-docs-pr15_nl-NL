<properties
    pageTitle="Bronnen voor Web-App verplaatsen naar een andere Resource-groep"
    description="Beschrijving van de scenario's waarin u Web Apps en App Services uit één resourcegroep naar een andere verplaatsen kunt."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Ondersteunde verplaatsen configuraties

U kunt de [ARM verplaatsen Resources Api](../resource-group-move-resources.md)gebruiken Azure Web App-resources verplaatsen.

De volgende scenario's voor verplaatsen van Azure Web-Apps worden momenteel ondersteund:

* De volledige inhoud van een resourcegroep (WebApps, app service-abonnementen en certificaten) verplaatsen naar een andere resourcegroep 
    * Opmerking: De resourcegroep bestemming mag geen Microsoft.Web bronnen in dit scenario
* Afzonderlijke-WebApps verplaatsen naar een andere resourcegroep, terwijl ze nog steeds hostingprovider in hun huidige app serviceplan (het abonnement dat app staat, blijft in de oude resourcegroep)
