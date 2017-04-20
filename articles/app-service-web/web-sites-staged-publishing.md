<properties
    pageTitle="Tijdelijke omgevingen voor web-apps in Azure-Service voor App instellen"
    description="Leer hoe u gefaseerde publiceren gebruiken voor web-apps in Azure App-Service."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Tijdelijke omgevingen voor web-apps in Azure-Service voor App instellen
<a name="Overview"></a>

Wanneer u uw web-app met [App-Service](http://go.microsoft.com/fwlink/?LinkId=529714)implementeert, kunt u implementeren naar een slot aparte implementatie in plaats van de standaard productie slot als actief in de **standaard** - of **Premium** App Service abonnement-modus. Implementatie sleuven zijn daadwerkelijk live WebApps gebruiken met hun eigen hostnamen. Web app-inhoud en configuraties elementen kunnen worden omgewisseld tussen twee implementatie sleuven, inclusief de productie slot. Uw toepassing naar een slot implementatie implementeert heeft de volgende voordelen:

- U kunt wijzigingen van de web-app in een tijdelijk opslaan implementatie slot valideren voordat u deze met de productie boven verwisselen.

- Zorgt ervoor dat op alle exemplaren van de slot zijn temperatuur voordat het wordt omgewisseld in gebruik neemt u eerst een WebApp implementatie in een slot en deze Exchange verwisselen. Hierdoor downtime wanneer u uw web-app implementeren. De omleiding verkeer naadloze is en geen aanvragen worden verbroken grond omwisselen bewerkingen. Deze hele werkstroom kan worden geautomatiseerd door te configureren [Automatisch uitwisselen](#configure-auto-swap-for-your-web-app) wanneer oude omwisselen gegevensvalidatie niet nodig is.

- Na een omwisselen bevat de slot met eerder gefaseerde WebApp nu de vorige productie WebApp. Als de wijzigingen die is omgezet in de productie slot zijn niet zoals verwacht, kunt u de dezelfde omwisselen onmiddellijk om ervoor zorgen dat uw "laatste bekende goede site' uitvoeren terug.

Elke App Service abonnement modus ondersteunt een verschillend aantal implementatie sleuven. Als u wilt weten hoeveel sleuven uw web-app modus ondersteunt, raadpleegt u [Prijzen van App-Service](/pricing/details/app-service/).

- Wanneer uw web-app meerdere elementen heeft, kunt u de modus niet wijzigen.

- Schaalbaarheid is niet beschikbaar voor niet-productieve sleuven.

- Gekoppelde resourcebeheer wordt niet ondersteund voor niet-productieve sleuven. Klik in de [Portal van Azure](http://go.microsoft.com/fwlink/?LinkId=529715) alleen kunt u deze mogelijke invloed op een slot productie voorkomen door tijdelijk de niet-productieve slot verplaatsen naar een andere App Service abonnement modus. Houd er rekening mee dat de niet-productieve slot opnieuw dezelfde modus met de slot productie delen moet voordat u kunt de twee sleuven uitwisselen.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>Een slot implementatie toevoegen aan een web-app ##

De web-app moet worden uitgevoerd in de **standaard** - of de **Premium** -modus in voordat u kunt meerdere implementatie sleuven inschakelen.

1. Open uw web-app blade in de [Portal van Azure](https://portal.azure.com/).
2. Klik op **Instellingen**en klik vervolgens op **implementatie sleuven**. Klikt u in het blad **implementatie sleuven** op **Slot toevoegen**.

    ![Een nieuwe implementatie slot toevoegen][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Als de web-app nog niet is in de **standaard** - of **Premium** -modus, ontvangt u een bericht waarin wordt aangegeven dat de ondersteunde modi voor het inschakelen van gefaseerde publiceren. U hebt nu de optie voor het **upgraden** selecteren en Ga naar het tabblad **schaal** van uw web-app voordat u verdergaat.

2. In het blad **toevoegen een slot** , Geef een naam op voor de slot en selecteer of u wilt de configuratie van de web-app van een andere bestaande implementatie slot klonen. Klik op het vinkje om door te gaan.

    ![Configuratie van gegevensbron][ConfigurationSource1]

    De eerste keer dat u een slot toevoegt, moet u alleen twee keuzes: klonen configuratie van de standaard-slot in productie of helemaal niet.

    Nadat u verschillende sleuven hebt gemaakt, kunt u wel configuratie van een slot behalve het gedeelte in productie klonen:

    ![Configuratie bronnen][MultipleConfigurationSources]

5. Klik in het blad **implementatie sleuven** op de implementatie-slot als u wilt een blade voor de slot, met een set aan de doelstellingen en geconfigureerd, net als een andere WebApp te openen. **Your-Web-App-name-Deployment-slot-name** wordt weergegeven boven aan blade herinneren dat u over de implementatie-slot beschikt.

    ![Implementatie Slot titel][StagingTitle]

5. Klik op de app-URL in van de slot blade. U ziet de implementatie-slot heeft een eigen hostname en is ook een live-app. Als u wilt beperken openbare toegang tot de implementatie-slot, Zie [App Service Web App-web-toegang tot de implementatie van niet-productieve sleuven blokkeren](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Er is geen inhoud na implementatie slot maken. U kunt dashboard implementeren naar de slot uit een andere opslagplaats tak of een heel anders opslagplaats. U kunt ook de configuratie van de slot wijzigen. Gebruik de publiceren-profiel of implementatie-referenties die zijn gekoppeld aan de implementatie-slot voor inhoudsupdates.  U kunt bijvoorbeeld [publiceren naar deze slot met cijfer](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>Configuratie voor implementatie sleuven ##
Wanneer u de configuratie van een andere implementatie slot klonen, is de gekloonde configuratie kan worden bewerkt. Bepaalde elementen configuratie wordt bovendien de inhoud over een omwisselen (kan niet in slot worden specifieke) Volg terwijl andere elementen configuratie aanwezig in de dezelfde slot na een omwisselen (slot specifieke blijven). De volgende lijsten weergeven de configuratie die wordt gewijzigd wanneer u sleuven uitwisselen.

**Instellingen die worden vervangen**:

- Algemene instellingen - zoals framework-versie, 32/64-bits, Web sockets
- App-instellingen (kan worden geconfigureerd als u wilt blijven een slot)
- Verbindingstekenreeksen (kan worden geconfigureerd als u wilt blijven een slot)
- Handlertoewijzingen
- Controle- en diagnostische instellingen
- WebJobs inhoud

**Instellingen die niet zijn omgewisseld**:

- Publicerende eindpunten
- Aangepaste domeinnamen
- SSL-certificaten en bindingen
- Schaalinstellingen
- WebJobs planners

Toegang tot het blad **Toepassingsinstellingen** voor een specifieke slot om te configureren op een app-instelling of verbindingstekenreeks aan een slot (niet omgewisseld), en selecteert u het vak **Slot instelling** voor de configuratie-elementen die de slot moet blijven. Houd er rekening mee dat een configuratie-element als markeren slot specifieke heeft het effect van het tot stand brengen van dat element als niet verwisselbare over alle van de implementatie-sleuven die is gekoppeld aan de web-app.

![Instellingen voor slot][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Implementatie sleuven uitwisselen ##

>[AZURE.IMPORTANT] Voordat u een web-app van een slot implementatie van Exchange verwisselt, zorg dat alle niet-slot bepaalde instellingen zijn geconfigureerd exact in zoals u wilt dat dit in de doellijst uitwisselen.

1. Als u wilt uitwisselen implementatie sleuven, klikt u op de knop **wisselen** in de opdrachtenbalk van de web-app of in de opdrachtenbalk van een slot implementatie. Zorg dat de omwisselen bron- en omwisselen doel juist zijn ingesteld. Het doel omwisselen zou meestal de productie slot.  

    ![Knop wisselen][SwapButtonBar]

3. Klik op **OK** om de bewerking te voltooien. Wanneer de bewerking is voltooid, hebben de implementatie-sleuven zijn omgewisseld.

## <a name="configure-auto-swap-for-your-web-app"></a>Automatisch uitwisselen configureren voor uw web-app ##

Automatisch omwisselen stroomlijnt DevOps scenario's waar u uw web-app met nul koudwatersystemen begin- en nul downtime continu implementeren voor klanten met einde van de web-app. Wanneer een slot implementatie is geconfigureerd voor automatisch omwisselen in bedrijf, telkens wanneer u de code-update naar die slot push, wordt App-Service automatisch de web-app uitwisselen Exchange nadat deze is al op temperatuur in de slot.

>[AZURE.IMPORTANT] Wanneer u automatische uitwisselen voor een slot inschakelt, zorg er dan voor dat de configuratie slot is precies de configuratie die zijn bedoeld voor de doeltoepassing slot (meestal de productie slot).

Automatisch uitwisselen configureren voor een slot gaat heel eenvoudig. Volg de onderstaande stappen:

1. In het blad **Implementatie sleuven** , selecteert u een niet-productieve slot, klikt u op **Alle instellingen** voor de die slot blade.  

    ![][Autoswap1]

2. Klik op **Toepassingsinstellingen**. Selecteer **op** voor het **Automatisch uitwisselen**, selecteert u de gewenste doel slot in **Automatisch uitwisselen Slot**en klik op **Opslaan** in de opdrachtenbalk. Controleer of de configuratie voor de slot is exact de configuratie die zijn bedoeld voor de doeltoepassing slot.

    Het tabblad **meldingen** wordt een groene **SUCCESS** flash zodra de bewerking voltooid is.

    ![][Autoswap2]

    >[AZURE.NOTE] Als u wilt testen Auto uitwisselen voor uw web-app, kunt u eerst een niet-productieve doel slot in **Automatisch uitwisselen Slot** om vertrouwd te raken met de functie selecteren.  

3. Een push code aan die slot implementatie uitvoeren. Automatisch omwisselen gebeurt na een korte tijd en de update worden weergegeven op de URL van uw doel slot.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Meerdere fasen omwisselen gebruiken voor uw web-app ##

Meerdere fasen omwisselen is beschikbaar voor validatie in de context van de configuratie-elementen die zijn ontworpen voor een slot zoals verbindingstekenreeksen met de blijven vereenvoudigen. In dat geval is het mogelijk nuttige informatie voor deze configuratie-elementen via het doel-omwisselen toepassen op de bron omwisselen en valideren voordat omwisselen daadwerkelijk van kracht. Zodra omwisselen doel configuratie-elementen zijn toegepast op de bron omwisselen welke acties beschikbaar zijn voor het voltooien van de omwisselen of terug te keren naar de oorspronkelijke configuratie voor het omwisselen bron ook met het effect van de omwisselen heffen. Voorbeelden voor het beschikbaar voor meerdere fasen omwisselen Azure PowerShell-cmdlets worden opgenomen in de Azure PowerShell-cmdlets voor implementatie sleuven sectie.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Ongedaan maken van een app productie na uitwisselen ##

Als er fouten worden vermeld in na het omwisselen van een slot, terugdraaien de sleuven naar de oude omwisselen staat door de dezelfde twee sleuven onmiddellijk verwisselen.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Aangepaste opgewarmd voordat uitwisselen ##

Sommige apps mogelijk aangepaste opgewarmd acties. Het element applicationInitialization configuratie in web.config kunt u aangepaste initialisatie acties opgeven die moeten worden uitgevoerd voordat een aanvraag wordt ontvangen. De bewerking omwisselen wacht voor deze aangepaste opgewarmd om te voltooien. Hier volgt een voorbeeld web.config fragment.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>Verwijderen van een slot implementatie##

Klik in het blad voor een slot implementatie, op **verwijderen** in de opdrachtenbalk.  

![Verwijderen van een Slot implementatie][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Azure PowerShell-cmdlets voor implementatie sleuven

Azure PowerShell is een module die cmdlets uit om het beheren van Azure via Windows PowerShell, inclusief ondersteuning voor het beheren van web app-implementatie sleuven in Azure App-Service biedt.

- Zie voor informatie over het installeren en configureren van Azure PowerShell en klik op het verifiëren van Azure PowerShell met uw Azure abonnement [installeren en configureren van Microsoft Azure PowerShell](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>WebApp maken

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>Een slot implementatie voor een web-app maken

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Meerdere fasen omwisselen initiëren en doel slot configuratie toepassen op bron slot

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Herstel de eerste fase van meerdere fasen uitwisselen en bron slot configuratie herstellen

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Implementatie sleuven uitwisselen

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Implementatie slot verwijderen

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Azure opdrachtregel (Azure CLI) opdrachten voor implementatie sleuven

De CLI Azure biedt platforms opdrachten voor het werken met Azure, inclusief ondersteuning voor het beheren van Web App-implementatie sleuven.

- Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md)voor instructies over het installeren en configureren van de Azure CLI, met inbegrip van informatie over het koppelen van Azure CLI aan uw Azure-abonnement.

-  Als u de opdrachten die beschikbaar zijn voor Azure App-Service in de CLI Azure, bellen `azure site -h`.

----------
### <a name="azure-site-list"></a>Azure sitelijst
Voor informatie over de web-apps in het huidige abonnement, belt u **azure sitelijst**, zoals in het volgende voorbeeld.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Azure-site maken
Als u wilt maken van een slot implementatie, belt u **azure site maken** en geef de naam van een bestaande web-app en de naam van de slot maken, zoals in het volgende voorbeeld.

`azure site create webappslotstest --slot staging`

Om te schakelen bronbeheer voor de nieuwe slot, gebruikt u de optie **--cijfer** , zoals in het volgende voorbeeld.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>Azure site uitwisselen
Als u de bijgewerkte implementatie sleuf van de app productie, gebruikt u de opdracht **azure site uitwisselen** een bewerking omwisselen, zoals in het volgende voorbeeld uit te voeren. De app productie wordt niet naar beneden tijd optreden, noch worden deze een koudwatersystemen begin onderworpen aan.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>Azure site verwijderen
Als u wilt verwijderen van een slot implementatie die niet meer nodig is, gebruikt u de opdracht **verwijderen van azure-site** , zoals in het volgende voorbeeld.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="next-steps"></a>Volgende stappen ##
[Azure App Service Web App-web-toegang tot de implementatie van niet-productieve sleuven blokkeren](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Gratis evaluatieversie van Microsoft Azure](/pricing/free-trial/)

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
