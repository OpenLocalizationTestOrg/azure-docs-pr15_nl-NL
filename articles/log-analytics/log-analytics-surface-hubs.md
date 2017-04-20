<properties
    pageTitle="Surface Hubs met Log Analytics controleren | Microsoft Azure"
    description="Gebruik de oplossing oppervlak Hub voor het bijhouden van de status van uw Hubs oppervlak en begrijpen hoe ze worden gebruikt."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="monitor-surface-hubs-with-log-analytics"></a>Monitor met een oppervlak Hubs met Log Analytics

In dit artikel wordt beschreven hoe u de oplossing oppervlak Hub in Log Analytics kunt gebruiken om de Hub van Microsoft Surface-apparaten met het Microsoft bewerkingen Management Suite Kantoorbeheersysteem te houden. Log Analytics kunt u de status van uw Hubs oppervlak alsmede begrijpen hoe ze worden gebruikt bijhouden.

Elke oppervlak-Hub heeft de Microsoft Monitoring-Agent is geïnstalleerd. De door de agent u gegevens kunt verzenden vanuit uw oppervlak-Hub naar OMS. Logboekbestanden worden opgelezen vanaf uw oppervlak Hubs en worden vervolgens worden verzonden naar de OMS-service. Factoren, zoals servers offline is, wordt de agenda die niet zijn gesynchroniseerd, of als het apparaat-account niet kan aanmelden bij Skype in OMS worden weergegeven in het dashboard oppervlak Hub. Met behulp van de gegevens in het dashboard, kunt u de apparaten die niet worden uitgevoerd, of die zijn andere problemen en potentieel gelden correcties voor de aangetroffen problemen identificeren.


## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende informatie voor het installeren en configureren van de oplossing. Om te kunnen beheren uw Hubs oppervlak van de Microsoft bewerkingen Management Suite Kantoorbeheersysteem, hebt u het volgende nodig:

- Een geldig abonnement op [OMS](http://www.microsoft.com/oms).
- Een abonnement voor [OMS](https://azure.microsoft.com/pricing/details/log-analytics/) die ondersteuning voor het aantal apparaten bieden die u wilt controleren. OMS prijzen varieert afhankelijk van hoe veel apparaten worden geregistreerd en hoeveel gegevens deze processen. Wilt u hiermee rekening bij het plannen van uw oppervlak Hub-implementatie.

Vervolgens wordt u een abonnement OMS toevoegen aan uw bestaande Microsoft Azure-abonnement of maak een nieuwe werkruimte rechtstreeks via de portal OMS. Gedetailleerde instructies voor het gebruik van een methode loopt [aan de slag met Log Analytics](log-analytics-get-started.md). Nadat het abonnement OMS is ingesteld, zijn er twee manieren om in te schrijven van uw Hub Surface-apparaten:

- Automatisch via InTune
- Handmatig via de **Instellingen** op uw apparaat oppervlak Hub.

## <a name="set-up-monitoring"></a>Cmdlets voor controle instellen

U kunt de servicestatus en activiteit van uw oppervlak-Hub Log Analytics gebruiken in OMS controleren. Met behulp van InTune of lokaal met behulp van **Instellingen** op de Hub oppervlak, kunt u de Hub oppervlak in OMS inschrijven.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Surface Hubs verbinden met OMS tot en met InTune

U moet de werkruimte-ID en de werkruimte-toets voor de werkruimte OMS die uw Hubs oppervlak beheert. U kunt krijgen van de portal OMS.

InTune is een Microsoft-product waarmee u kunt het centrale beheer van de configuratie-instellingen voor OMS die zijn toegepast op een of meer van uw apparaten. Volg deze stappen om uw apparaten via InTune configureren:

1. Meld u aan bij InTune.
2. Ga naar **Instellingen** > **verbonden bronnen**.
3. Maak of bewerk een beleid op basis van de sjabloon oppervlak Hub.
4. Ga naar de sectie OMS (Azure operationele inzichten) van het beleid en voeg de *Werkruimte-ID* en de *Werkruimte sleutel* aan het beleid.
5. Sla het beleid.
6. Het beleid koppelen aan de juiste groep van apparaten.

  ![InTune beleid](./media/log-analytics-surface-hubs/intune.png)

De instellingen OMS InTune vervolgens gesynchroniseerd met de apparaten in de doelgroep, ze in uw werkruimte OMS inschrijven.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Surface Hubs verbinden met OMS met de app-instellingen

U moet de werkruimte-ID en de werkruimte-toets voor de werkruimte OMS die uw Hubs oppervlak beheert. U kunt krijgen van de portal OMS.

Als u niet InTune gebruikt voor het beheren van uw omgeving, kunt u de apparaten handmatig via de **Instellingen** op elke oppervlak-Hub kunt inschrijven:

1. Open **Instellingen**in de Hub oppervlak.
2. Voer de referenties van de beheerder apparaat wanneer u wordt gevraagd.
3. Klik op **Dit apparaat**, en de onder **controle**, klikt u op **OMS-instellingen configureren**.
4. Selecteer **controle inschakelen**.
6. In het dialoogvenster Instellingen van OMS, typt u de **Werkruimte-ID** en typt u de **Werkruimte-toets**.  
  ![Instellingen](./media/log-analytics-surface-hubs/settings.png)
7. Klik op **OK** om de configuratie te voltooien.

Een bevestiging wordt weergegeven dat u al dan niet de configuratie OMS is toegepast op het apparaat. Als dat het geval is, wordt er een bericht weergegeven dat de-agent is verbonden met de OMS-service. Het apparaat begint vervolgens gegevens worden verzonden naar OMS waar u kunt weergeven en ondernemen.

## <a name="monitor-surface-hubs"></a>Monitor met een oppervlak Hubs

Uw Hubs oppervlak bewaken lijkt OMS gebruiken veel andere geregistreerde apparaten cmdlets voor controle.

1. Meld u aan bij de portal OMS.
2. Ga naar het oppervlak Hub oplossing pack dashboard.
3. De status van uw apparaat wordt weergegeven.

  ![Surface Hub-dashboard](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

U kunt [meldingen](log-analytics-alerts.md) op basis van bestaande of aangepaste log zoekopdrachten maken. Met de gegevens die de OMS van uw Hubs oppervlak verzamelt, kunt u zoeken naar problemen en een waarschuwing voor de voorwaarden die u voor uw apparaten definieert.


## <a name="next-steps"></a>Volgende stappen

- Gebruik [zoekopdrachten in het logboek in Log Analytics](log-analytics-log-searches.md) gedetailleerde oppervlak Hub gegevens moeten worden weergegeven.
- [Waarschuwingen](log-analytics-alerts.md) om u te waarschuwen wanneer er zich problemen bij uw Hubs oppervlak voordoen maken.
