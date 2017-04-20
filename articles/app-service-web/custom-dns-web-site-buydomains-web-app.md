<properties
    pageTitle="Kopen van een aangepaste domeinnaam in Azure App Service Web Apps"
    description="Leer hoe u een aangepaste domeinnaam met een web-app in Azure App Service kopen."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>Kopen en configureren van een aangepaste domeinnaam in Azure App-Service

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Wanneer u een web-app maakt, wordt het in Azure toegewezen aan een subdomein van azurewebsites.net. Als uw web-app **contoso heet**, is de URL bijvoorbeeld **contoso.azurewebsites.net**. Azure toegewezen ook een virtueel IP-adres.

Voor een productie WebApp wilt u waarschijnlijk gebruikers moeten zien van een aangepaste domeinnaam. In dit artikel wordt uitgelegd hoe kopen en configureren van een aangepast domein met de [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>Overzicht

Als u een domeinnaam voor uw web-app niet hebt, kunt u eenvoudig een op [Azure-Portal](https://portal.azure.com/)kopen. Tijdens het kunt u WWW en hoofdsite de DNS-records worden toegewezen aan uw web-app automatisch hebt. Ook kunt u uw domein rechts binnen Azure-Portal beheren.


Gebruik de volgende stappen uit om te kopen domeinnamen en toewijzen aan uw web-app.

1. Open de [Portal van Azure](https://portal.azure.com/)in uw browser.

2. Klik op de naam van uw web-app op het tabblad **Web Apps** , selecteer **Instellingen**en selecteer **aangepaste domeinen**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. Klik in het blad **aangepaste domeinen** op **domeinen kopen**.

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. In het blad **Domeinen kopen** , gebruik u het tekstvak met het typen van de domeinnaam die u wilt kopen en druk op Enter. De domeinen van de voorgestelde beschikbaar wordt weergegeven onder het tekstvak. Selecteer welke domein dat u wilt kopen. U kunt meerdere domeinen in één keer aanschaffen. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Klik op de **Contactgegevens** en van het domein contactgegevens formulier invullen.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] Is het belangrijk dat u Vul alle verplichte velden met zo nauwkeurig mogelijk, met name het e-mailadres. Bij het aanschaffen van het domein zonder 'Privacy beveiliging', wordt u mogelijk gevraagd om te controleren of uw e-mail voordat het domein geactiveerd wordt. In sommige gevallen treden onjuiste gegevens voor contactgegevens nalatigheid bij het aanschaffen van domeinen. 

6. Nu kunt u,

    een) ' automatisch verlengen"uw domein elk jaar
    
    b) Opt-in voor de "Privacy beveiliging' die is opgenomen in de inkoopprijs gratis (behalve topniveaudomein wie bevindt zich register niet ondersteunen Privacy. For example:. co.in,. co.uk enz.)  
    
    c) "toewijst standaard hostnamen" voor WWW en hoofddomein naar de huidige Web-App. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] Optie C Hiermee configureert u DNS-bindingen en Hostname bindingen automatisch voor u.  Op deze manier kunnen uw Web-App worden geopend met aangepaste domein zodra de aankoop voltooid is (DNS-doorgeven vertragingen in sommige gevallen baring). Geval, uw Web-App is achter Azure verkeer Manager, worden er geen optie voor het toewijzen van hoofddomein, zoals A-Records niet met het verkeer Manager werken. U kunt altijd toewijzen de domeinen/subformulier / domains gekocht via een Web-App naar een andere Web App en vice versa. Zie stap 8 voor meer informatie. 
    
7. Klik op de **selecteren** op **Domeinen kopen** blade, ziet u de gegevens op de bladeserver **bevestiging aanschaffen** . Als u akkoord met de juridische voorwaarden en klikt u op **kopen**, uw bestelling worden verzonden en u kunt het aanschaffen proces op **melding**controleren. Domein aankoop kan enkele minuten duren. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Als u een domein voor het succes besteld, kunt u het domein beheren en toewijzen aan uw web-app. Klik op de **'...'** aan de rechterkant van uw domein. Vervolgens kunt u **aankoop annuleren** of **domein beheren**. Klik op **beheren domein**, klikt u vervolgens we **subdomein** kunt verbinden met onze WebApp op **Manage domain** blade. Voer deze stap uit binnen de context van de desbetreffende Web-App als u wilt een **subdomein** maken naar een andere Web-App. Hier u kiezen voor het toewijzen van het domein naar verkeer manager eindpunt (als Web App achter TM is) door gewoon selecteren verkeer manager de naam in de vervolgkeuzelijst. Dit doet, worden domein/subdomein automatisch toegewezen aan de Web-Apps achter dat verkeer Manager-eindpunt. 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] U kunt 'aanschaffen 'binnen annuleren 5 dagen voor volledige restitutie. Na 5 dagen die geval doet u het niet mogelijk om te 'Annuleren aanschaffen', in plaats daarvan ziet u een optie voor het 'Verwijderen' het domein. Bij het verwijderen van het domein zijn ingevoegd om deze uit uw abonnement zonder restitutie vrijgeven en worden de beschikbare domein. 

Wanneer de configuratie is voltooid, wordt de aangepaste domeinnaam worden vermeld in de sectie **Hostname bindingen** van uw web-app.

Nu moet u mogelijk voert de aangepaste domeinnaam in de browser en ziet u dat deze met succes u naar uw web-app gaat.
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>Wat gebeurt er met het aangepaste domein dat u hebt gekocht

Het aangepaste domein dat u hebt gekocht in het blad **aangepaste domeinen en SSL** is gekoppeld aan het abonnement dat Azure. Als een Azure-bron is dit aangepaste domein afzonderlijke en onafhankelijke vanuit de App Service-app die u het domein dat voor het eerst hebt gekocht. Dit betekent dat:

- Binnen de Azure-portal, kunt u het aangepaste domein dat u hebt gekocht voor meer dan één App Service-app en niet alleen betrekking heeft op de app die u het aangepaste domein voor het eerst hebt gekocht. 
- Alle aangepaste domeinen die u in het abonnement dat Azure door te gaan naar het blad **aangepaste domeinen en SSL** van *een* App Service-app in een abonnement hebt gekocht, kunt u beheren.
- U kunt een App Service-app van hetzelfde Azure abonnement toewijzen aan een subdomein binnen dat aangepaste domein.
- Als u wilt verwijderen van een App Service-app, kunt u niet te verwijderen van het aangepaste domein dat deze is gebonden aan als u wilt blijven gebruiken voor andere apps.

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>Als u het aangepaste domein niet ziet u hebt gekocht

Als u het aangepaste domein vanuit het blad **aangepaste domeinen en SSL** hebt gekocht, maar het aangepaste domein onder **beheerde domeinen**wordt niet weergegeven, controleert u de volgende onderdelen:

- Het maken van aangepaste domein nog niet volledig. De melding Bel boven aan de Azure-portal om de voortgang controleren.
- Het maken van aangepaste domein mogelijk om een bepaalde reden mislukt. De melding Bel boven aan de Azure-portal om de voortgang controleren.
- Het aangepaste domein is mogelijk hebt voltooid, maar het blad kan niet worden vernieuwd. Probeert u het blad **aangepaste domeinen en SSL** opent u deze opnieuw.
- U kunt het aangepaste domein op een gegeven moment hebt verwijderd. De controlelogboeken controleren door te klikken op **Instellingen** > **Logboeken aan de controle** van de belangrijkste blade van uw app. 
- Het **aangepaste domeinen en SSL** blad dat u op zoek bent in mogelijk deel uitmaakt van een app die u in een ander abonnement van Azure maakt. Overschakelen naar een andere app in een ander abonnement en schakel de **aangepaste domeinen en SSL** blade.  
  In de portal niet mogelijk kunnen zien of aangepaste domeinen die zijn gemaakt in een ander abonnement van de Azure dan de app beheren. Echter als u een **Geavanceerd beheer** in van het domein **beheren domein** blade klikt, wordt u omgeleid naar de website van de domeinprovider, waar u kunt wel   [handmatig configureren van uw aangepaste domein, zoals een externe aangepast domein](web-sites-custom-domain-name.md)  
   voor apps in een ander abonnement van Azure hebt gemaakt. 


