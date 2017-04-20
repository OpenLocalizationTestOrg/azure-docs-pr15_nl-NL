<properties 
    pageTitle="Azure RemoteApp Veelgestelde vragen over | Microsoft Azure" 
    description="Meer informatie vindt u antwoorden op de meest Veelgestelde vragen over Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Azure RemoteApp-Veelgestelde vragen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

We ken de volgende vragen over Azure RemoteApp. Anderen hebben? Ga naar de [RemoteApp-forums](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) en laat het ons weten wat u moet weten of een opmerking vervolgmenu onder.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Niet vinden wat u zoekt? Hebt u een vraag die we het antwoord niet?
Als u niet over de beschikt moet u of u een extra vraag die we bent die niet hier hebt, Ga naar het [forum van Azure RemoteApp](http://aka.ms/araforum) en uw vraag er. We kunt hier meer antwoorden altijd toevoegen.

## <a name="what-is-azure-remoteapp"></a>Wat is Azure RemoteApp? ##


- **Wat is Azure RemoteApp?** RemoteApp is dat een Azure-service helpt u veilige, externe toegang bieden tot toepassingen vanaf veel verschillende apparaten. Lees meer over [Azure RemoteApp](remoteapp-whatis.md).
- **Wat zijn de implementatieopties?** Er zijn twee soorten RemoteApp verzamelingen: cloud en hybride. Welke monitor u nodig hebt, is afhankelijk van een aantal factoren, zoals het of u nodig hebt aan domein toevoegen. We gaan alle deze beslissingen uitleggen [hier](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Snelle tips over het gebruik van Azure RemoteApp ##
- **Hoe lang totdat ik ben verbroken? Hoe lang ik inactief kan zijn voordat u mij het opstarten geven?** 4 uur. Als u of een van uw gebruikers is niet actief geweest voor 4 uur, wordt u automatisch worden afgemeld bij Azure RemoteApp. Bekijk de andere standaardinstellingen in [Azure-abonnement en limieten van de Service, quota, en voorwaarden](../azure-subscription-service-limits.md).
- **Kan ik deze service gratis uitproberen?** Ja. Er is een gratis proefversie beschikbaar voor 30 dagen. Nadat de proefperiode is afgelopen, kunt u overstappen naar een betaald-account (die u kunt gebruiken in productie) of stoppen met het gebruik van de service. Uw gratis proefversie starten door te gaan naar [portal.azure.com](http://portal.azure.com) : Maak een nieuw exemplaar van RemoteApp. U kunt met de gratis proefversie, 2 exemplaren van RemoteApp maken met 10 gebruikers per exemplaar. Houd er rekening mee dat deze evaluatieversie bevinden zich alleen voor 30 dagen.
## <a name="azure-remoteapp-subscription-details"></a>Details van Azure RemoteApp-abonnement ##

- **Wat zijn de beperkingen van de service?** U kunt meer informatie over de standaardinstellingen en service limieten van Azure RemoteApp in [Azure-abonnement en limieten van de Service, quota, en voorwaarden](../azure-subscription-service-limits.md). Laat ons weten als u meer vragen hebt.
- **Hoeveel gebruikers moet ik hebben?** Er is een minimum van 20 gebruikers. Ik wil dat geweldig duidelijkheid herhalen: de MINIMUMWAARDE is 20. U wordt gefactureerd voor 20. 
- **Wat kost RemoteApp?** Bekijk de [Details van Azure RemoteApp-prijzen ](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Kost één type siteverzameling meer dan een ander?** Ja, kan het, afhankelijk van uw vereisten voor de siteverzameling. Een verzameling hybride vereist een verbinding van Azure RemoteApp naar uw on-premises netwerk. Als u een bestaande VNET/Express Route gebruikt, moet u er gratis is. Maar als u een nieuwe Azure VNET en een gateway of Express Route gebruikt, moet u betalen voor de [gateway VPN](https://azure.microsoft.com/pricing/details/vpn-gateway) of [Express doorsturen](https://azure.microsoft.com/pricing/details/expressroute/). Deze kosten (Zie de koppelingen) is boven aan uw maandelijkse Azure RemoteApp kosten.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Verzamelingen - wat wordt ondersteund, welke moet gebruikt u en anderen
- **Worden ondersteund aangepaste-LOB (LOB)-toepassingen?** Ja. Als u een maatwerktoepassing in Azure RemoteApp, een [afbeelding van de aangepaste sjabloon](remoteapp-create-custom-image.md)maken en uploadt dit naar de RemoteApp-verzameling.
- **Werken mijn aangepaste LOB-toepassing in Azure RemoteApp?** De beste manier om de afbeelding die is om deze te testen. Bekijk het [RD compatibiliteit Center](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Welke implementatiemethode (cloud of hybride) meest geschikt is voor mijn organisatie?** Hybride verzamelingen over de meest volledige ervaring beschikken als u wilt dat de volledige integratie met eenmalige aanmelding (SSO) en beveiligde on-premises implementatie netwerkconnectiviteit. Cloud verzamelingen bieden een agile en eenvoudige manier te isoleren van uw implementatie met behulp van meerdere verificatiemethoden. Meer informatie over de [implementatieopties voor](remoteapp-whatis.md).
- **We hebben SQL of een andere database van een on-premises implementatie of in Azure wordt aangegeven. Welk implementatietype moeten we gebruiken?** Dat hangt af van waar uw SQL- of backend-database. Als de database op een privé-netwerk, gebruikt u de hybride-verzameling. Als de database wordt blootgesteld aan Internet en client verbindingen tot stand te brengen toestaat, kunt u de cloud-verzameling.
- **Hoe zit het met de toewijzing van station, USB- en seriële poort, Klembord delen en printeromleiding van de?** Al deze functies worden ondersteund in Azure RemoteApp. Klembord delen en printer omleiding is standaard ingeschakeld. U kunt meer informatie over de omleiding van [hier](remoteapp-redirection.md). 


## <a name="template-images"></a>Afbeeldingen van sitesjablonen
- **Kan ik gebruiken een cloud of bestaande VM als de sjabloon voor mijn RemoteApp-siteverzameling?** Ja! U kunt maken van een afbeelding op basis van een VM Azure, gebruikt u een van de afbeeldingen die wordt geleverd bij uw abonnement of een aangepaste afbeelding maken. Bekijk de [Afbeeldingsopties RemoteApp](remoteapp-imageoptions.md).


## <a name="network-options"></a>Opties voor netwerken
- **De verzameling hybride vereist een VNET. Kan we onze bestaande VNET gebruiken?** U kunt als de bestaande VNET een VNET Azure is. Zie ' stap 1: het virtuele netwerk instellen "in de [hybride siteverzameling instructies](remoteapp-create-hybrid-deployment.md) voor meer informatie.
- **Kan ik een VNET gebruiken met een verzameling cloud?** U kunt daadwerkelijk. Bekijk [een cloud-verzameling maken](remoteapp-create-cloud-deployment.md), met name stap 1, voor meer informatie.

## <a name="authentication-options"></a>Verificatieopties voor



- **Hoe zit verificatie? Welke methoden worden ondersteund?** De verzameling cloud ondersteunt Microsoft-accounts en Azure Active Directory-accounts die ook Office 365-accounts zijn. De verzameling hybride ondersteunt alleen Azure Active Directory-accounts die zijn gesynchroniseerd (met behulp van een hulpmiddel zoals [Azure Active Directory-synchronisatie](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) van een Windows Server Active Directory-implementatie; specifiek, hetzij gesynchroniseerd met de optie Wachtwoordsynchronisatie of gesynchroniseerd met Active Directory Federation Services (AD FS) Federatie geconfigureerd. U kunt ook de [Meervoudige verificatie MFA ()](https://azure.microsoft.com/services/multi-factor-authentication/).

>[AZURE.NOTE]De Azure Active Directory-gebruikers moeten zich in de tenant dat is gekoppeld aan uw abonnement. (U kunt weergeven en wijzigen van uw abonnement op het tabblad **Instellingen** in de portal. Zie [de Azure Active Directory-tenant wijzigen die wordt gebruikt door RemoteApp](remoteapp-changetenant.md) voor meer informatie.)

- **Waarom kan niet ik toegang tot mijn Azure Active Directory-account geven?** De Azure Active Directory-gebruikers moeten zich in de map die is gekoppeld aan uw abonnement. U kunt bekijken of wijzigen van deze map op het tabblad instellingen in de portal. Zie [de Azure Active Directory-tenant wijzigen die wordt gebruikt door RemoteApp](remoteapp-changetenant.md) voor meer informatie.

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Clients - welk apparaat kan ik gebruiken voor toegang tot Azure RemoteApp?
Hier vindt u informatie over de goede client, met inbegrip van de stappen voor het installeren van de verschillende clients bij het [openen van uw apps in Azure RemoteApp](remoteapp-clients.md).

- **Welke apparaten en besturingssystemen de clienttoepassingen ondersteunen?**
Eerste, computers en -tablets: 
    - Windows-10 (client preview)
    - Windows 8.1 en Windows 8
    - Windows 7 servicepack 1
    - Mac OS X
    - Windows RT
    - Android-tablets
    - iPads en de telefoonnummers:
    - iPhone
    - Android-telefoon
    - Windows Phone
 
    [Download](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) nu een RemoteApp-client.
- **Ondersteunt Azure RemoteApp dunne Clients?** Ja, de volgende Windows ingesloten dunne clients worden ondersteund:
    - Windows Embedded Standard 7
    - Windows 8 standaard ingesloten
    - Windows 8.1 industrie ingesloten Pro
    - Windows 10 IoT Enterprise

- **Welke versie van Windows Server wordt ondersteund voor het externe bureaublad sessie Host (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Ondersteuning en feedback


- **Wat is het abonnement ondersteuning voor RemoteApp?** Ondersteuning voor facturering en abonnementen management wordt gratis aangeboden. Technische ondersteuning is beschikbaar via de [Azure service-abonnementen](https://azure.microsoft.com/support/plans/). U kunt ook ondersteuning voor gratis community tot en met onze [Azure discussieforum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
- **Hoe verzend ik feedback?** Ga naar het [Feedbackforum](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Wie kan ik meer informatie over Azure RemoteApp communiceren?** U kunt deelnemen aan de wekelijkse [Ask de webinar Experts](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), waarbij we het gaan over alles RemoteApp hebben naast onze [discussieforum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), dat wil een prima plek om te vragen posten zeggen.
- **Hoe zit het met RemoteApp-documentatie?** We blij dat u wordt gevraagd. Naast de help-inhoud in de portal help voor vellen (Klik op de **?** Klik op een pagina in de portal van) de volgende artikelen zijn beschikbaar om u te leren alles over RemoteApp:
    - **Aan de slag:**
        - [Wat is RemoteApp?](remoteapp-whatis.md)
        - [Wat staat er in de sjabloonafbeeldingen RemoteApp?](remoteapp-images.md)
        - [Hoe licensing werkt?](remoteapp-licensing.md)
        - [Hoe werken RemoteApp- en Office samen?](remoteapp-o365.md)
        - [Hoe omleiding werkt in RemoteApp](remoteapp-redirection.md)?
    - **Implementeren:**
        - [Afbeelding van een aangepaste sjabloon maken](remoteapp-create-custom-image.md)
        - [Een hybride-verzameling maken](remoteapp-create-hybrid-deployment.md)
        - [Een cloud-verzameling maken](remoteapp-create-cloud-deployment.md)
        - [Azure Active Directory configureren voor RemoteApp](remoteapp-ad.md)
        - [Een app publiceren in RemoteApp](remoteapp-publish.md)
    - **Beheren:**
        - [Gebruikers toevoegen](remoteapp-user.md)
        - [Aanbevolen procedures voor het configureren en gebruiken van RemoteApp](remoteapp-bestpractices.md)  

    Video's! We hebben ook een aantal video's over RemoteApp. Sommige kunt Inleiding ([Inleiding tot Azure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) u anderen u begeleid bij implementatie ([Cloud-implementatie](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) en [hybride implementatie](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Bekijk deze.

 
### <a name="help-us-help-you"></a>Help ons te helpen u 
Wist u naast de beoordeling van dit artikel en het toevoegen van opmerkingen omlaag onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreken? Iets mis? Ik iets dat is alleen verwarrend schrijven? Omhoog schuiven en klik op **bewerken op GitHub** als u wijzigingen wilt - die wordt verzonden naar ons voor revisie en vervolgens zodra we zich op deze afmelden, ziet u uw wijzigingen en verbeteringen hier.
