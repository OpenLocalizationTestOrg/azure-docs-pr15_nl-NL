<properties
    pageTitle="Aan de slag met Log Analytics | Microsoft Azure"
    description="U kunt werken met Log Analytics in de Microsoft bewerkingen Management Suite Kantoorbeheersysteem openen in minuten."
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
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="get-started-with-log-analytics"></a>Aan de slag met Log Analytics

U kunt werken met Log Analytics in de Microsoft bewerkingen Management Suite Kantoorbeheersysteem openen in minuten. Bij het kiezen van het maken van een werkruimte OMS die vergelijkbaar is met een account hebt u twee opties:

- Website van Microsoft bewerkingen Management Suite
- Microsoft Azure-abonnement

U kunt een gratis OMS werkruimte met de website OMS maken. Of u een Microsoft Azure-abonnement kunt gebruiken om een werkruimte OMS te maken. Beide werkruimten hebben hetzelfde resultaat, behalve dat een gratis OMS-werkruimte alleen 500 MB aan gegevens dagelijks naar de OMS-service verzenden kunt. Als u een Azure-abonnement gebruikt, kunt u ook abonnement gebruiken voor toegang tot andere Azure-services. Ongeacht de methode die u kunt maken van de werkruimte, kunt u de werkruimte wilt maken met een Microsoft-account of organisatieaccount.

Hier volgt een overzicht van het proces:

![Onboarding-diagram](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Meld u aan analyses vereisten en overwegingen bij de implementatie

- U moet een betaald abonnement Microsoft Azure volledig gebruik Log Analytics. Als u geen een Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) waarmee u toegang tot een Azure-service. Of u kunt maken van een gratis OMS-account op de website van de [Bewerkingen Management Suite](http://microsoft.com/oms) en klikt u op **gratis proberen**.
- Een werkruimte OMS
- Elke Windows-computer die u verzamelen gegevens van Windows Server 2008 SP1 wilt, moet uitvoeren of hoger
- Webadressen [firewall](log-analytics-proxy-firewall.md) toegang tot de OMS van service
- Een server [OMS Log Analytics doorstuurservers](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (Gateway) naar het verkeer van doorsturen servers naar OMS, als internettoegang niet beschikbaar van computers is
- Als u Operations Manager, Log Analytics ondersteunt Operations Manager 2012 SP1 UR6 gebruikt en hoger en Operations Manager 2012 R2 UR2 en hoger. Proxy-ondersteuning is in Operations Manager 2012 SP1 UR7 en Operations Manager 2012 R2 UR3 toegevoegd. Hiermee bepaalt u hoe deze worden geïntegreerd met OMS.
- Bepalen of uw computers directe internettoegang hebt. Zo niet, vereisen ze een gateway-server voor toegang tot de OMS-service-websites. Alle toegang is via HTTPS.
- Hiermee bepaalt u welke technologieën en servers gegevens naar OMS verzonden worden. Bijvoorbeeld domeincontroller, SQL Server, enzovoort.
- Toestemming verlenen aan gebruikers in OMS en Azure.
- Als u betrokken over het gegevensgebruik van, afzonderlijk implementeren van elke oplossing en de invloed op de prestaties te testen voordat u extra oplossingen toevoegt.
- Houd uw gegevensgebruik en de prestaties naarmate u oplossingen en functies aan Log Analytics toevoegt. Dit geldt ook voor gebeurtenis siteverzameling, log siteverzameling, prestatiegegevens verzamelen, enzovoort. Het is beter beginnen met minimale siteverzameling tot gegevensgebruik of invloed op de prestaties is geconstateerd.
- Controleren of Windows-agenten niet ook worden beheerd met Operations Manager anders dubbele gegevens zijn ingevoegd. Dit geldt ook voor Azure-gebaseerd-agenten waarvoor Azure diagnostische hulpprogramma's die zijn ingeschakeld.
- Nadat u agenten installeert, controleert u of dat de agent correct werkt. Als niet controleren om ervoor te zorgen dat de cryptografische API: volgende generatie (CNG) sleutel moeten worden geïsoleerd niet is uitgeschakeld via Groepsbeleid.
- Sommige Log Analytics-oplossingen hebt aanvullende vereisten



## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>Registreren in 3 stappen met de bewerkingen Management-Suite

1. Ga naar de website [Bewerkingen Management Suite](http://microsoft.com/oms) en klik op **gratis proberen**. Meld u aan met uw Microsoft-account, zoals Outlook.com of met een organisatieaccount die is verstrekt door uw bedrijf of onderwijsinstelling voor gebruik met Office 365 of andere Microsoft-services.
2. Geef een unieke werkruimte naam. Een werkruimte is een logische container waarin uw gegevens beheren is opgeslagen. Deze biedt u een manier om partitiegegevens tussen verschillende teams in uw organisatie, zoals de gegevens die zich exclusief naar de werkruimte. Geef een e-mailadres en het gebied waar u uw gegevens hebt opgeslagen.  
    ![werkruimte maken en het koppelen van abonnement](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Vervolgens kunt u een nieuwe Azure abonnement of een koppeling naar een bestaand Azure-abonnement. Als u doorgaan met de gratis proefversie wilt, klikt u op **Nu niet**.  
  ![werkruimte maken en het koppelen van abonnement](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

U kunt nu aan de slag met de bewerkingen Management Suite-portal.

U kunt meer informatie over het instellen van uw werkruimte en het koppelen van bestaande Azure-mailaccounts naar werkruimten die zijn gemaakt met de bewerkingen Management Suite bij [beheren toegang tot Log Analytics](log-analytics-manage-access.md).

## <a name="sign-up-quickly-using-microsoft-azure"></a>Snel met Microsoft Azure registreren

1. Ga naar de [Azure-portal](https://portal.azure.com) en meld u aan, bladeren in de lijst met services en selecteer **Log Analytics Kantoorbeheersysteem**.  
    ![Azure-portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Klik op **toevoegen**en selecteer vervolgens opties voor de volgende items:
    - De naam van de **Werkruimte OMS**
    - **Abonnement** - als er meerdere abonnementen, kiest u het thema dat u wilt koppelen aan de nieuwe werkruimte.
    - **Resourcegroep**
    - **Locatie**
    - **Prijzen van laag**  
        ![snel aan maken](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klik op **maken** en ziet u de details van de werkruimte in de portal van Azure.       
    ![details van de werkruimte](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Klik op de koppeling **OMS-Portal** als u wilt de bewerkingen Management Suite-website met uw nieuwe werkruimte openen.

U bent klaar om te beginnen met behulp van de portal bewerkingen Management Suite.

U kunt meer informatie over het instellen van uw werkruimte en het koppelen van bestaande werkruimten die u hebt gemaakt met de bewerkingen Management Suite op Azure-abonnementen op [beheren toegang tot Log Analytics](log-analytics-manage-access.md).

## <a name="get-started-with-the-operations-management-suite-portal"></a>Aan de slag met de bewerkingen Management Suite-portal
Klik op de tegel **Instellingen** om te kiezen oplossingen en verbinding maken met de servers die u wilt beheren, en volg de stappen in deze sectie.  

![aan de slag](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Oplossingen toevoegen** - uw geïnstalleerde oplossingen weergeven.  
    ![oplossingen](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Klik op **Ga naar de galerie** om toe te voegen meer oplossingen.  
    ![oplossingen](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Selecteer een oplossing en klik vervolgens op **toevoegen**.
2. **Verbinding maken met een bron** - kiezen hoe u verbinding maakt met uw server-omgeving voor het verzamelen van gegevens:
    - Verbinding maken met een Windows Server of -client rechtstreeks door een agent installeren.
    - Sluit Linux-servers met de OMS-Agent voor Linux.
    - Gebruik een Azure opslag-account dat is geconfigureerd met de Windows- of Linux Azure diagnostische VM extensie.
    - Systeem Center Operations Manager gebruiken om te koppelen van uw groepen management of de hele Operations Manager-implementatie.
    - Windows-Telemetrielogboek gebruik van de Upgrade Analytics inschakelen.
        ![verbonden bronnen](./media/log-analytics-get-started/oms-onboard-data-sources.png)    

3. **Gegevens verzamelen** Ten minste één gegevensbron in te vullen gegevens om uw werkruimte te configureren. Wanneer u klaar bent, klikt u op **Opslaan**.    

    ![gegevens verzamelen](./media/log-analytics-get-started/oms-onboard-logs.png)    


## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>(Optioneel) Maak verbinding tussen servers rechtstreeks naar de bewerkingen Management Suite door te installeren, een agent

Het volgende voorbeeld ziet u hoe u een Windows-agent installeert.

1. Klik op de tegel **Instellingen** , klik op het tabblad **Verbonden bronnen** , klikt u op een tabblad voor het brontype die u wilt toevoegen en een van beide downloaden een agent of meer informatie over het inschakelen van een agent. Klik bijvoorbeeld op **Windows-Agent downloaden (64-bits)**. Als u Windows wilt toevoegen, kunt u alleen de agent op Windows Server 2008 SP 1 of hoger en Windows 7 SP1 of hoger te installeren.
2. De agent installeren op een of meer servers. U kunt installeren agenten één voor één of een bestaande verdeling oplossing voor software die u moet mogelijk een meer geautomatiseerde methode gebruikt met een [aangepast script](log-analytics-windows-agents.md), of u kunt gebruiken.
3. Nadat u akkoord met de licentieovereenkomst gaat en u uw installatiemap kiezen, selecteert u **verbinding maken met de agent aan Azure Log Analytics Kantoorbeheersysteem**.   
    ![Agent-instelling](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Op de volgende pagina, wordt u gevraagd voor uw werkruimte-ID en de werkruimte-toets. Uw werkruimte-ID en de toets worden weergegeven op het scherm waar u het bestand agent hebt gedownload.  
    ![Agent toetsen](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![servers bijvoegen](./media/log-analytics-get-started/oms-onboard-key.png)
5. Tijdens de installatie, kunt u klikken op **Geavanceerd** om desgewenst instellen van uw proxyserver en geef de verificatie-informatie. Klik op de knop **volgende** om terug te keren naar het scherm van de informatie werkruimte.
6. Klik op **volgende** om te valideren van uw werkruimte-ID en de toets. Als er fouten zijn gevonden, kunt u **terug** te verbeteren. Wanneer uw werkruimte-ID en de toets worden gevalideerd, klikt u op **installeren** om de agent-installatie te voltooien.
7. Klik in het Configuratiescherm op Microsoft Monitoring Agent > Azure Log Analytics Kantoorbeheersysteem tabblad. Een groen vinkje-pictogram wordt weergegeven wanneer de agenten met de bewerkingen Management Suite-service communiceren. Hiermee gaat in eerste instantie ongeveer 5-10 minuten.

>[AZURE.NOTE] De capaciteit beheer en de configuratie assessment oplossingen worden momenteel niet ondersteund door de servers die rechtstreeks verbonden met de bewerkingen Management-Suite.


U kunt ook de agent naar System Center Operations Manager 2012 SP1 en hoger verbinding te maken. Als u wilt doen, selecteert u **verbinding maken met de System Center Operations Manager-agent**. Wanneer u de optie u gegevens naar de service verzenden zonder extra hardware of laden van uw groepen management kiezen.

U vindt meer agenten verbinding kunt maken met de bewerkingen Management Suite op [verbinding maken met Windows-computers Log Analytics](log-analytics-windows-agents.md).

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>(Optioneel) verbinding maken met System Center Operations Manager servers

1. Selecteer in de console Operations Manager **beheer**.
2. Vouw het knooppunt **Operationele inzichten** en selecteer **Operationele inzichten verbinding**.

  >[AZURE.NOTE] Afhankelijk van welke Update Rollup van SCOM u gebruikt, ziet u mogelijk een knooppunt voor *Systeem Center Advisor*, *Operationele inzichten*of *Bewerkingen Management Suite*.

3. Klik op de koppeling **registreren operationele inzicht krijgen in** richting van de rechtsboven en volg de instructies.
4. Na het voltooien van de wizard registreren, klikt u op de koppeling van de **Computer/groep toevoegen** .
5. In het dialoogvenster **Computer zoeken** kunt u zoeken naar computers of groepen gecontroleerd door Operations Manager. Selecteer de optie computers of groepen om over te ingebouwde ze Log analyses, klikt u op **toevoegen**en klik vervolgens op **OK**. U kunt controleren of dat de OMS-service is ontvangen van gegevens door naar de tegel voor **Gebruik** in de portal bewerkingen Management Suite te gaan. Gegevens worden weergegeven in ongeveer 5-10 minuten.

U vindt meer Operations Manager verbinding kunt maken met de bewerkingen Management Suite op [Verbinding maken met Operations Manager naar Log Analytics](log-analytics-om-agents.md).

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>(Optioneel) gegevens van cloudservices in Microsoft Azure analyseren

Met de bewerkingen Management Suite, kunt u snel gebeurtenis en IIS-logboeken voor cloudservices en virtuele machines doordat diagnostische hulpprogramma's voor Azure-Cloudservices zoeken. U kunt ook extra inzichten voor uw Azure virtuele machines ontvangen door de installatie van de Microsoft-Agent bewaken. U kunt meer informatie over het configureren van uw Azure-omgeving voor het gebruik van de bewerkingen Management Suite op [verbinding maken met Azure opslag Log Analytics](log-analytics-azure-storage.md).


## <a name="next-steps"></a>Volgende stappen

- [Oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md) voor functionaliteit toevoegen en gegevens te verzamelen.
- Vertrouwd raken met [log zoekopdrachten](log-analytics-log-searches.md) om gedetailleerde informatie verzameld door oplossingen te bekijken.
- [Dashboards](log-analytics-dashboards.md) op te slaan en het weergeven van uw eigen aangepaste zoekacties gebruiken.
