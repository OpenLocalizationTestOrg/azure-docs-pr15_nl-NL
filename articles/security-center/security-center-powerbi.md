<properties
   pageTitle="Inzicht krijgen van Beveiligingscentrum Azure-gegevens met Power BI | Microsoft Azure"
   description="Het pakket van Azure beveiliging Center Power BI-inhoud maakt het gemakkelijk te vinden zijn beveiligingsmeldingen, aanbevelingen, aanval van resources en trends weer, op basis van een gegevensset die is gemaakt voor uw rapporten."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Inzicht krijgen van Beveiligingscentrum Azure-gegevens met Power BI
Het [Power BI Dashboard](http://aka.ms/azure-security-center-power-bi) voor Azure Beveiligingscentrum kunt u visualiseren, analyseren en filteren aanbevelingen en beveiligingswaarschuwingen vanaf elke locatie, zoals uw mobiele apparaat. Gebruik het Power BI-dashboard geven trends en patronen - beveiligingsmeldingen weergave door een resource of bron IP-adres en niet geadresseerd beveiligingsrisico's door een resource of leeftijd aanvallen. 

U kunt ook samenvoegen Beveiligingscentrum aanbevelingen en beveiligingswaarschuwingen met andere gegevens op interessante manieren, bijvoorbeeld met gegevens van [Azure controlelogboeken bijhouden](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) en [Azure SQL Database controle](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/). Beide bieden Power BI-Dashboards en u kunt ook deze gegevens naar Excel exporteren voor eenvoudig rapporten over de beveiligingsstatus van uw resources cloud.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Azure Beveiligingscentrum dashboard gebruiken voor toegang tot Power BI
U kunt ook het dashboard Azure Beveiligingscentrum gebruiken voor toegang tot Power BI-rapporten. Volg de stappen voor het uitvoeren van deze taak: 

1. Klik in het dashboard **Beveiligingscentrum Azure** op de knop **verkennen in Power BI** .

    ![Verbinding maken met Azure Beveiligingscentrum met behulp van Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. Het **verkennen van Power BI** -blad geopend aan de rechterkant, zoals wordt weergegeven in het volgende scherm:

    ![Verbinding maken met Azure Beveiligingscentrum met behulp van Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Als u het Power BI-dashboard voor het eerst maakt, kunt u een van de volgende opties kiezen in het blad **verkennen in Power BI** : 

    - **Beveiliging inzichten dashboard**: Kies deze optie als u wilt maken van een dashboard dat beveiligingsstatus, threads en detectie bevat. Deze optie is een meer algemene voor DevOps rol die verantwoordelijk is voor het analyseren van hun status beveiliging en waarschuwingen over de abonnementen gedetecteerd.
    - **Beleid-Beheerdashboard**: Kies deze optie als u wilt verkennen management en afdwingen beleid.  Deze optie is een meer algemene voor centraal IT die meer zijn gericht op beheermodel. Zichtbaarheid en inzichten op beveiliging beleid naleving toegang binnen hun organisatie, kunnen ze dit dashboard gebruiken.
    - Als u al een Power BI-dashboard hebt, klikt u op **Ga naar uw huidige Power BI-dashboard**.

4. Klik op de optie **beveiliging inzichten dashboard** voor dit voorbeeld. Als dit de eerste keer, maakt u een Power BI-dashboard voor Beveiligingscentrum u wordt gevraagd om de inhoud pack te installeren. Zoals in het volgende scherm, klikt u op de knop **ophalen** in het venster **inhoud packs voor Power BI** :

    ![Azure beveiliging Center beveiliging inzichten dashboard](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. Het venster **verbinding maken met Azure beveiliging Center beveiliging inzichten** weergegeven. Controleer of **de verificatiemethode** is **oAuth2** zoals hieronder wordt weergegeven en klik op **aanmelden** .
    
    ![Verificatie](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. U mogelijk gevraagd om te verifiëren opnieuw met uw Azure referenties. Na het verifiëren van uw dashboard wordt gemaakt. Nadat het dashboard is gemaakt ziet u een rapport met de vergelijkbare structuur, zoals wordt weergegeven in het volgende scherm:

    ![Power BI-Dashboard](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] Een vernieuwing van het rapport volgens de planning moet plaatsvinden dagelijks. Als u een fout bij deze vernieuwen ondervindt, Lees [Vernieuwen problemen met het Azure beveiliging Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/)voor meer informatie over het oplossen.

Hier ziet u het aantal beveiligingsmeldingen en aanbevelingen en het aantal VMs, Azure SQL-databases en netwerk resources wordt gevolgd door Beveiligingscentrum Azure.

Een koppeling naar Beveiligingscentrum Azure wordt u omgeleid naar de Azure-portal. De grafieken om eenvoudig te visualiseren informatie over beveiligingsaanbevelingen en waarschuwingen, waaronder:

- Status van de beveiliging
- In behandeling aanbevelingen
- VM aanbevelingen
- Waarschuwingen na verloop van tijd
- Aangevallen Resources
- Aangevallen IP-adressen

Achter elke grafiek, zijn er extra inzichten. Selecteer een tegel voor meer informatie. Bijvoorbeeld, ziet de **Status van de beveiliging** -tegel u meer informatie over wachtende aanbevelingen door alle resources zoals wordt weergegeven in het volgende scherm:

![Aanbevelingen](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Als u op elke regel van deze grafiek, klikt u op wilt de andere grijs en u de focus verplaatsen alleen op de sectie die u hebt geselecteerd. Als u wilt teruggaan naar het dashboard, klikt u op **Azure Beveiligingscentrum** onder de optie **Dashboards** in het linkerdeelvenster van deze pagina.

> [AZURE.NOTE] Als u uw rapporten extra velden toevoegen of wijzigen van bestaande visuele elementen aanpassen wilt, kunt u het rapport kunt bewerken. Lees [een rapport in de bewerkingsweergave in Power BI beheren](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) voor meer informatie.

De tegels **Aanvaller IP -adressen** en **waarschuwingen over een periode, Resources aanval** heeft de vergelijkbare uitvoer na het klikken op elkaar ervan. Dit gebeurt omdat het rapport informatie over deze drie variabelen verzamelt en deze **Resources onder aanval** belt zoals wordt weergegeven in het volgende scherm:

![Resources onder aanval](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Nu kunt u ook een kopie van dit rapport opslaan, afdrukken of op het web publiceert met behulp van de beschikbare opties in het menu **bestand** .

![Bestandsmenu](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Het verkennen van uw Beveiligingscentrum Azure-gegevens met Power BI-services

Verbinding maken met de [Power BI-inhoud Pack Services](https://msit.powerbi.com/groups/me/getdata/services) in Power BI en de volgende stappen uitvoeren:

1. Klik in het venster **Inhoud Pack voor Power BI** ziet u twee opties zoals hieronder wordt weergegeven.

    ![Inhoud pack voor Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Als u al uitgevoerd het eerste deel van dit artikel ziet u slechts één optie, dat wil beheer van Azure beveiligingsbeleid Center zeggen.

2. **In dit voorbeeld, klikt u op in de tegel **Beheer van Azure beveiligingsbeleid Center** .**

3. Klik in het venster **verbinding maken met het beheer van Azure beveiligingsbeleid Center** zorg **oAuth2** onder zoals vervolgkeuzelijst **Verificatiemethode** onderstaande selecteren en klik op **aanmelden** .

    ![Venster voor beheer van beleid](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. U wordt omgeleid naar een verificatiepagina waar u de referenties waarmee u verbinding maakt met Azure Beveiligingscentrum werkt moet typt. Nadat de verificatie voltooid is, start Power BI het importeren van gegevens als u wilt uw rapporten samenstelt. Tijdens deze periode mogelijk ziet u het volgende bericht op de rechterhoek van uw browser:

    ![Verbinding maken met Azure Beveiligingscentrum met behulp van Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Wanneer het dashboard voor het eerst wordt gemaakt kan langer duren dan normaal, hoofdzakelijk voor scenario's waarin u meerdere abonnementen hebt. 

5. Zodra het proces is voltooid, wordt uw dashboard Azure beveiliging Center Power BI geladen bij het **Beheer van** rapport dat vergelijkbaar is met de hieronder wordt weergegeven:

    ![Beleid-Beheerdashboard](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Zie ook
In dit document, hebt u geleerd hoe Power BI gebruiken in Beveiligingscentrum Azure. Zie de volgende onderwerpen voor meer informatie over Azure Beveiligingscentrum:

- [Bewerkingen handleiding en Azure beveiliging Center plannen](security-center-planning-and-operations-guide.md) , informatie over het plannen van Azure Beveiligingscentrum adoptie.
- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) , informatie over het configureren van beveiligingsinstellingen in Azure Beveiligingscentrum
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) , informatie over het beheren en reageren op beveiligingsmeldingen
- [Veelgestelde vragen over beheercentrum voor Azure-beveiliging](security-center-faq.md) : veelgestelde vragen over het gebruik van de service zoeken
- [Azure Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) , zoeken weblogberichten over Azure beveiliging en naleving
