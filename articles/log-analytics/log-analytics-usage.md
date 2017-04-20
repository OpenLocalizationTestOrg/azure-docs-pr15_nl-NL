<properties
    pageTitle="Gegevensgebruik in Log Analytics analyseren | Microsoft Azure"
    description="U kunt de pagina gebruik van in Log Analytics om weer te geven hoeveel gegevens wordt verzonden naar de OMS-service."
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
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="analyze-data-usage-in-log-analytics"></a>Gegevensgebruik in Log Analytics analyseren

Log Analytics in de bewerkingen Management Suite Kantoorbeheersysteem verzamelt gegevens en regelmatig verzonden naar de OMS-service.  U kunt de pagina **Gebruik** van om weer te geven hoeveel gegevens wordt verzonden naar de OMS-service. De pagina **Gebruik** van ziet u ook over hoeveel gegevens wordt dagelijks verzonden door oplossingen en hoe vaak uw servers gegevens wordt verzonden.

>[AZURE.NOTE] Als u een gratis-account gemaakt met behulp van de [OMS-website](http://www.microsoft.com/oms)hebt, bent u maximaal 500 MB aan gegevens naar de OMS-service dagelijks verzendt. Als u de dagelijkse limiet heeft bereikt, wordt de gegevensanalyse stoppen en cv aan het begin van de volgende dag. U moet ook is niet geaccepteerd of verwerkt door OMS gegevens opnieuw te verzenden.

U kunt uw gebruik weergeven met behulp van de tegel **Gebruik** op het dashboard **Overzicht** in OMS.

![Gebruik-tegel](./media/log-analytics-usage/usage-tile.png)

Als u hebt uw dagelijkse gebruik limiet overschreden, of als u zich bij uw limiet, kunt u desgewenst een oplossing als u wilt verkleinen van de hoeveelheid gegevens die u naar de OMS-service verzendt verwijderen. Zie voor meer informatie over het verwijderen van oplossingen [oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).

![Gebruik dashboard](./media/log-analytics-usage/usage-dashboard.png)

De pagina **Gebruik** van bevat de volgende gegevens:

- Gemiddelde gebruik per dag
- Gegevensgebruik voor elke oplossing via de afgelopen 30 dagen
- Hoeveel gegevens de servers in uw omgeving wordt verzonden naar de OMS-service via de afgelopen 30 dagen
- De data-abonnement prijzen van laag en geschatte kosten
- Informatie over uw SLA service level agreement (), inclusief hoe lang het duurt OMS om uw gegevens te verwerken

## <a name="to-work-with-usage-data"></a>Werken met gegevens over zoekgebruik

1. Klik op de tegel **Gebruik** op de pagina **Overzicht** .
2. Klik op de pagina **Gebruik** van de gebruikscategorieën weergeven die gebieden weergeven als dat u bang bent.
3. Als u een oplossing die te veel van de quota voor uw dagelijkse upload verbruikt hebt, kunt u overwegen dat oplossing verwijderen.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>De geschatte kosten en factureringsgegevens weergeven
1. Klik op de tegel **Gebruik** op de pagina **Overzicht** .
2. Klik op de pagina **Gebruik** van onder **Gebruik**, klikt u op de punthaken (**>**) naast de **geschatte kosten**.
3. In de uitgevouwen details van **uw data-abonnement** , kunt u uw geschatte maandelijks kosten zien.  
    ![De data-abonnement](./media/log-analytics-usage/usage-data-plan.png)
4. Als u uw factureringsgegevens weergeven wilt, klikt u op **Mijn factuur weergeven** als de informatie van uw abonnement wilt weergeven.
    - Klik op de pagina abonnementen op uw abonnement als details en de lijst met een lijn-items van gebruik wilt bekijken.  
        ![abonnement](./media/log-analytics-usage/usage-sub01.png)
    - U kunt een groot aantal taken voor het beheren en meer details van uw abonnement bekijken uitvoeren op de pagina Accountoverzicht voor uw abonnement.  
        ![details van abonnement](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Batches gegevens moeten worden weergegeven voor uw SLA
1. Klik op de tegel **Gebruik** op de pagina **Overzicht** .
2. Klik op **downloaden SLA details** **Service Level Agreement**.
3. Een Excel-XLSX-bestand is gedownload u moet controleren.  
    ![SLA details](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Volgende stappen

- Zie [Log zoekopdrachten in Log Analytics](log-analytics-log-searches.md) om gedetailleerde informatie verzameld door oplossingen te bekijken.
