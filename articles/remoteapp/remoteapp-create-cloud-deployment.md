<properties 
    pageTitle="Het maken van een wolk verzameling Azure RemoteApp | Microsoft Azure" 
    description="Informatie over het maken van een implementatie van Azure RemoteApp dat gegevens worden opgeslagen in de cloud Azure." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Het maken van een wolk verzameling Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Er zijn twee soorten [Azure RemoteApp siteverzamelingen](remoteapp-collections.md): 

- Cloud: bevindt zich volledig in Azure wordt aangegeven. U kunt alle gegevens opslaan in de cloud (zodat een verzameling alleen de cloud) of aan de verzameling verbinden met een VNET en opslaan van gegevens.   
- Hybride: bevat een virtueel netwerk voor lokale toegang - hiervoor is vereist het gebruik van Azure AD en een on-premises Active Directory-omgeving.

Deze zelfstudie begeleidt u bij het proces van het maken van een siteverzameling cloud. Er zijn vier stappen: 

1.  Een verzameling Azure RemoteApp maken.
2.  (Optioneel) configureren directory-synchronisatie. Als u van Azure AD gebruikmaakt + Active Directory, die u hebt voor het synchroniseren van gebruikers, contactpersonen en wachtwoorden vanuit uw lokale Active Directory aan bij uw Azure AD-tenant.
5.  Apps publiceren.
6.  Configureer de toegang van gebruikers.


**Voordat u begint**

U moet het volgende voordat u de verzameling maakt:

- [Registreren](https://azure.microsoft.com/services/remoteapp/) voor Azure RemoteApp. 
- Verzamel informatie over de gebruikers die u wilt verlenen van toegang tot. Dit is de gegevens van de Microsoft-account of Active Directory werk accountgegevens voor gebruikers.
- Deze procedure wordt ervan uitgegaan dat u een van beide gaan om een van de sjabloonafbeeldingen die is opgegeven als onderdeel van uw abonnement te gebruiken of dat u al hebt geüpload de afbeelding van de sjabloon die u wilt gebruiken. Als u nodig hebt om de afbeelding van een andere sjabloon te uploaden, kunt u dat doen vanaf de pagina afbeeldingen van sitesjablonen. Alleen op **de afbeelding van een sjabloon uploaden** en volg de stappen in de wizard. 
- Wilt u de Office 365 ProPlus afbeelding gebruiken? Bekijk de info [hier](remoteapp-officesubscription.md).
- Wilt u aangepaste apps geven of LOB-programma's? Maak een nieuwe [afbeelding](remoteapp-imageoptions.md) en deze gebruiken in de cloud-verzameling.
- Bepalen of u moet verbinding maken met een VNET. Als u verbinding maakt met een VNET kiest, controleert u of het voldoet aan de [richtlijnen van de formaatgrepen](remoteapp-vnetsizing.md) en dat deze [kunnen worden verbonden met RemoteApp](remoteapp-vnet.md). Bekijk de [planning VNET-artikel ](remoteapp-planvnet.md)voor meer informatie.
- Als u een VNET gebruikt, kunt u bepalen of u wilt toevoegen aan uw lokale Active Directory-domein.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Stap 1: Een verzameling cloud - maken met of zonder een VNET##


Gebruik de volgende stappen om te **maken van een siteverzameling alleen de cloud**:

1. Ga naar de pagina RemoteApp in de beheerportal.
2. Klik op **Nieuw > snel maken**.
3. Voer een naam voor uw siteverzameling en selecteer uw regio.
4. Kies het abonnement dat u use wilt-standaard of eenvoudige.
5. De sjabloon wilt gebruiken voor deze siteverzameling kiezen. 

    **Tip:** Uw abonnement voor RemoteApp wordt geleverd met [afbeeldingen van sitesjablonen](remoteapp-images.md) met Office 365 of Office 2013 (voor proefabonnement gebruik)-programma's, enkele gepubliceerd (zoals Word) en anderen wilt publiceren. U kunt ook een nieuwe [afbeelding](remoteapp-imageoptions.md) maken en gebruiken in de cloud-verzameling.


1. Klik op **maken RemoteApp-siteverzameling**.
    
    **Belangrijke:** Het kan maximaal 30 minuten voor het inrichten van uw siteverzameling duren.

Nadat de verzameling Azure RemoteApp is gemaakt, dubbelklikt u op de naam van de siteverzameling. Die u krijgt de pagina **Snel aan de slag** : dit is waar u klaar bent met het configureren van de siteverzameling.

Gebruik de volgende stappen om te maken van een **wolk + VNET siteverzameling**:

1. Ga naar de pagina Azure RemoteApp in de beheerportal.
2. Klik op **nieuwe** > **met VNET maken**.
3. Voer een naam voor uw siteverzameling.
4. Kies het abonnement dat u use wilt-standaard of eenvoudige.
5. Kies de VNET die u al hebt gemaakt. Weet u niet hoe u dat doen? Nu zijn de stappen in het onderwerp [hybride](remoteapp-create-hybrid-deployment.md) .
6. Bepalen of u wilt deelnemen aan de verzameling naar uw domein. Indien Ja, moet u AD Connect gebruiken om te integreren van Azure AD en uw Active Directory-omgeving. Die valt onder in **stap 2**.
6. Klik op **maken RemoteApp-siteverzameling**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Stap 2: Configureer Active Directory directory-synchronisatie (optioneel) ##

Als u gebruiken, Active Directory wilt, moet Azure RemoteApp directory-synchronisatie tussen Azure Active Directory en uw lokale Active Directory voor het synchroniseren van gebruikers, contactpersonen en wachtwoorden aan bij uw Azure Active Directory-tenant. Zie [Active Directory voor Azure RemoteApp configureren](remoteapp-ad.md) voor het plannen van informatie. U kunt ook rechtstreeks naar [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) voor informatie gaan.

## <a name="step-3-publish-apps"></a>Stap 3: Apps publiceren ##

Een app Azure RemoteApp is de app of een programma dat u aan uw gebruikers opgeeft. Deze bevindt zich in de afbeelding van de sjabloon die u hebt geüpload voor de collectie. Wanneer een gebruiker een app opent, wordt de app wordt weergegeven om uit te voeren in hun lokale omgeving, maar dit echt wordt uitgevoerd op een virtuele machine in Azure wordt aangegeven. 

Voordat u uw gebruikers hebben toegang tot apps, moet u ze te publiceren: apps kunt publiceren uw gebruikers rechtstreeks toegang tot de apps via de extern-bureaubladclient.
 
U kunt meerdere apps publiceren naar de Azure RemoteApp-verzameling. Klik op de pagina publiceren op **publiceren** als een programma wilt toevoegen. U kunt een publiceren vanuit het menu **Start** van de afbeelding van de sjabloon of door het opgeven van het pad op de afbeelding van de sjabloon voor de app. Als u toevoegen vanuit het menu **Start wilt** , kiest u de app te publiceren. Als u besluit om het pad naar de app, Geef een naam voor de app en het pad naar waar dit is geïnstalleerd op de afbeelding van de sjabloon.

## <a name="step-4-configure-user-access"></a>Stap 4: Gebruikerstoegang configureren ##

Nu u de verzameling hebt gemaakt, moet u de gebruikers die u wilt kunnen gebruiken van uw externe bronnen toevoegen. Als u Active Directory gebruikt, de gebruikers dat u opgeeft dat toegang tot moet bestaat niet in de Active Directory-tenant die is gekoppeld aan het abonnement dat u gebruikt om te maken van deze siteverzameling.

1.  Klik op **configureren de toegang van gebruikers**op de pagina snel starten. 
2.  Voer de werkaccount (uit Active Directory) of Microsoft-account dat u toegang wilt verlenen voor.

    **Notities:** 

    Zorg ervoor dat u gebruikt de “user@domain.com” opmaken.

    Als u Office 365 ProPlus in uw siteverzameling zijn gebruikt, moet u de Active Directory-identiteiten gebruiken voor uw gebruikers. Hiermee kunt valideren licenties. 

3.  Nadat de gebruikers worden gevalideerd, klikt u op **Opslaan**.


## <a name="next-steps"></a>Volgende stappen ##

Dat is het: u hebt gemaakt en geïmplementeerd van de Azure RemoteApp cloud-verzameling. De volgende stap is dat uw gebruikers downloaden en installeren van de client extern bureaublad. U vindt de URL voor de client op de pagina Azure RemoteApp snel starten. Vervolgens hebben gebruikers zich aanmelden bij de client en toegang tot de apps die u hebt gepubliceerd.

### <a name="help-us-help-you"></a>Help ons te helpen u 
Wist u naast de beoordeling van dit artikel en het toevoegen van opmerkingen omlaag onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreken? Iets mis? Ik iets dat is alleen verwarrend schrijven? Omhoog schuiven en klik op **bewerken op GitHub** als u wijzigingen wilt - die wordt verzonden naar ons voor revisie en vervolgens zodra we zich op deze afmelden, ziet u uw wijzigingen en verbeteringen hier.