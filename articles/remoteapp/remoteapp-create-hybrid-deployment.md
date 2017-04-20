<properties
    pageTitle="Het maken van een siteverzameling hybride voor Azure RemoteApp | Microsoft Azure"
    description="Informatie over het maken van een implementatie van RemoteApp die is verbonden met het interne netwerk."
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

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Het maken van een siteverzameling hybride voor Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Er zijn twee soorten Azure RemoteApp siteverzamelingen:

- Cloud: bevindt zich volledig in Azure wordt aangegeven. U kunt alle gegevens opslaan in de cloud (zodat een verzameling alleen de cloud) of aan de verzameling verbinden met een VNET en opslaan van gegevens.   
- Hybride: bevat een virtueel netwerk voor lokale toegang - hiervoor is vereist het gebruik van Azure AD en een on-premises Active Directory-omgeving.

Niet weet welke u nodig hebt? Bekijk [welk siteverzameling hebt u nodig voor Azure RemoteApp](remoteapp-collections.md).

Deze zelfstudie begeleidt u bij het proces van het maken van een hybride-siteverzameling. Er zijn acht stappen:

1.  Bepalen welke [afbeelding](remoteapp-imageoptions.md) wilt gebruiken voor uw siteverzameling. U kunt een aangepaste afbeelding maken of gebruik een van de Microsoft-afbeeldingen die wordt geleverd bij uw abonnement.
2. Het virtuele netwerk instellen. Bekijk de informatie [VNET planning](remoteapp-planvnet.md) en [formaat wijzigen](remoteapp-vnetsizing.md) in.
2.  Een verzameling maken.
2.  Deelnemen aan de verzameling naar uw lokale domein.
3.  De Sjabloonafbeelding van een toevoegen aan uw siteverzameling.
4.  Directory-synchronisatie configureren. Azure RemoteApp is vereist dat u integreren met Azure Active Directory op een van beide 1) configureren Azure Active Directory-synchronisatie met de optie Wachtwoordsynchronisatie of 2) configureren Azure Active Directory-synchronisatie zonder de optie Wachtwoordsynchronisatie maar werkt met een domein dat is gekoppeld aan AD FS. Bekijk de [informatie over de configuratie voor Active Directory met RemoteApp](remoteapp-ad.md).
5.  Publiceren RemoteApp-apps.
6.  Configureer de toegang van gebruikers.

**Voordat u begint**

U moet het volgende voordat u de verzameling maakt:

- [Registreren](https://azure.microsoft.com/services/remoteapp/) voor Azure RemoteApp.
- Maak een gebruikersaccount in Active Directory wilt gebruiken als de serviceaccount Azure RemoteApp. De machtigingen voor dit account beperken zodat deze kan alleen worden toegevoegd aan machines op het domein.
- Verzamel informatie over uw on-premises netwerk: IP-adres informatie en details van VPN-apparaat.
- Installeer de [Azure PowerShell](../powershell-install-configure.md) -module.
- Verzamel informatie over de gebruikers die u wilt verlenen van toegang tot. Moet u de Azure Active Directory-UPN (bijvoorbeeld name@contoso.com) voor elke gebruiker. Zorg ervoor dat de UPN komt overeen met tussen Azure AD en Active Directory.
- Kies de afbeelding van uw sjabloon. Een afbeelding van de sjabloon Azure RemoteApp bevat de apps en -programma's die u wilt publiceren voor uw gebruikers. Zie [Azure RemoteApp afbeelding van de opties](remoteapp-imageoptions.md) voor meer informatie.
- Wilt u de Office 365 ProPlus afbeelding gebruiken? Bekijk de info [hier](remoteapp-officesubscription.md).
- [Active Directory voor RemoteApp configureren](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Stap 1: Het virtuele netwerk instellen
Kunt u een verzameling hybride die gebruikmaakt van een bestaand Azure virtuele netwerk implementeren, of u een nieuwe virtueel netwerk kunt maken. Een virtueel netwerk kunt uw access-gegevens van gebruikers in uw lokale netwerk via RemoteApp externe bronnen. Gebruikmaakt van een Azure virtuele netwerk biedt uw siteverzameling direct netwerktoegang tot andere Azure services en virtuele machines geïmplementeerd in deze virtuele netwerk.

Zorg ervoor dat u de informatie [VNET plannen](remoteapp-planvnet.md) en [VNET grootte](remoteapp-vnetsizing.md) bekijken voordat u uw VNET maakt.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Een VNET Azure maken en toevoegen aan uw Active Directory-implementatie

Beginnen met het maken van een [virtueel netwerk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Dit gebeurt op het tabblad **netwerk** in de portal van Azure. U moet uw virtuele netwerk verbinden met de Active Directory-implementatie die is gesynchroniseerd met uw Azure Active Directory-tenant.

Zie [een virtueel netwerk met behulp van de Azure portal maken](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) voor meer informatie.

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Zorg ervoor dat uw virtuele netwerk is gereed voor Azure RemoteApp
Voordat u uw siteverzameling maken, moet u laten we controleren of dat uw nieuwe virtuele netwerk gereed is. U kunt dit controleren door het volgende te doen:

1. Maak een Azure virtuele machines binnen het subnet van het virtuele netwerk dat u zojuist hebt gemaakt voor RemoteApp.
2. Extern bureaublad verbinding maken met de virtuele machine gebruiken. (Klik op **verbinding maken**.)
3. Toevoegen aan de dezelfde Active Directory-implementatie die u wilt gebruiken voor RemoteApp.

Werkte die? Het virtuele netwerk en subnet zijn gereed voor Azure RemoteApp!

U vindt meer informatie over het maken van Azure virtuele machines en verbinding met extern bureaublad [als volgt](https://msdn.microsoft.com/library/azure/jj156003.aspx).

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Stap 2: Een verzameling Azure RemoteApp maken ##



1. Ga naar de pagina Azure RemoteApp in de [portal van Azure](http://manage.windowsazure.com).
2. Klik op **Nieuw > maken met VNET**.
3. Voer een naam voor uw siteverzameling.
4. Kies het abonnement dat u use wilt-standaard of eenvoudige.
5. Kies uw VNET in de vervolgkeuzelijst lijst en klik vervolgens op uw subnet.
6. Kies om deze te koppelen aan uw domein.
5. Klik op **maken RemoteApp-siteverzameling**.

Nadat de verzameling Azure RemoteApp is gemaakt, dubbelklikt u op de naam van de siteverzameling. Die u krijgt de pagina **Snel aan de slag** : dit is waar u klaar bent met het configureren van de siteverzameling.

Is iets fout gegaan gebleven? Bekijk de [informatie over probleemoplossing hybride-siteverzameling](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Stap 3: De verzameling koppelen aan het lokale domein ##


1. Klik op **deelnemen aan een lokaal domein**op de pagina **Snel starten** .
2. Het Azure RemoteApp-service-account toevoegen aan uw lokale Active Directory-domein. Moet u de domeinnaam, afdeling, service-account-gebruikersnaam en wachtwoord.

    Dit is de informatie die u als u de stappen in de [Active Directory configureren voor Azure RemoteApp](remoteapp-ad.md)gevolgd verzameld.


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Stap 4: Koppeling naar een Azure RemoteApp-afbeelding ##

Een afbeelding van de sjabloon Azure RemoteApp bevat de programma's die u wilt delen met gebruikers. U kunt geen nieuwe [Sjabloonafbeelding](remoteapp-imageoptions.md) maken of koppelen aan een bestaande afbeelding (één al geïmporteerd of geüpload naar Azure RemoteApp). U kunt ook een koppeling maken naar een van de Azure RemoteApp- [afbeeldingen van sitesjablonen](remoteapp-images.md) met Office 365 of Office 2013 (voor proefabonnement gebruik)-programma's.

Als u de nieuwe afbeelding uploaden wilt, moet u Voer de naam en kies de locatie voor de afbeelding. Klik op de volgende pagina van de wizard u ziet dat een reeks PowerShell-cmdlets - kopiëren en uitvoeren van deze cmdlets vanaf een verhoogde Windows PowerShell-prompt de opgegeven afbeelding uploaden.

Als u een koppeling naar een bestaande Sjabloonafbeelding, geef dan de afbeelding van de naam, locatie en bijbehorende Azure abonnement.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Stap 5: Directory-synchronisatie van Active Directory configureren ##

Azure RemoteApp is vereist dat u integreren met Azure Active Directory op een van beide 1) configureren Azure Active Directory-synchronisatie met de optie Wachtwoordsynchronisatie of 2) configureren Azure Active Directory-synchronisatie zonder de optie Wachtwoordsynchronisatie maar werkt met een domein dat is gekoppeld aan AD FS.

Zie [verbinding maken met AD](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) - in dit artikel vindt u directory-integratie in 4 stappen instellen.

Zie [Active Directory-synchronisatie: roadmap](http://msdn.microsoft.com//library/azure/hh967642.aspx) voor het plannen van informatie en gedetailleerde stappen.

## <a name="step-6-publish-apps"></a>Stap 6: Apps publiceren ##

Een app Azure RemoteApp is de app of een programma dat u aan uw gebruikers opgeeft. Deze bevindt zich in de afbeelding van de sjabloon die u hebt geüpload voor de collectie. Wanneer een gebruiker een app opent, wordt deze wordt weergegeven om uit te voeren in hun lokale omgeving, maar dit echt wordt uitgevoerd in Azure wordt aangegeven.

Voordat u uw gebruikers hebben toegang tot de apps, moet u ze te publiceren – Hiermee kunt uw gebruikers toegang krijgen tot de apps via de extern bureaublad-client.

U kunt meerdere apps publiceren naar uw siteverzameling. Klik op de pagina publiceren op **publiceren** als een app wilt toevoegen. U kunt een publiceren vanuit het menu **Start** van de afbeelding van de sjabloon of door het opgeven van het pad op de afbeelding van de sjabloon voor de app. Als u toevoegen vanuit het menu **Start wilt** , kiest u wilt toevoegen. Als u besluit om het pad naar de app, Geef een naam voor de app en het pad naar waar dit is geïnstalleerd op de afbeelding van de sjabloon.

## <a name="step-7-configure-user-access"></a>Stap 7: De toegang van gebruikers configureren ##

Nu u de verzameling hebt gemaakt, moet u de gebruikers die u wilt kunnen gebruiken van uw externe bronnen toevoegen. De gebruikers die toegang tot moet bestaat niet in de Active Directory-tenant die is gekoppeld aan het abonnement op te geven die u gebruikt deze Azure RemoteApp-verzameling maken.

1.  Klik op **configureren de toegang van gebruikers**op de pagina snel starten.
2.  Voer de werkaccount (uit Active Directory) of Microsoft-account dat u toegang wilt verlenen voor.

    **Notities:**

    Zorg ervoor dat u gebruikt de “user@domain.com” opmaken.

    Als u Office 365 ProPlus in uw siteverzameling zijn gebruikt, moet u de Active Directory-identiteiten gebruiken voor uw gebruikers. Hiermee kunt valideren licenties.


3.  Zodra de gebruikers worden gevalideerd, klikt u op **Opslaan**.


## <a name="next-steps"></a>Volgende stappen ##
Dat is het: u hebt gemaakt en geïmplementeerd van uw siteverzameling van de hybride Azure RemoteApp. De volgende stap is dat uw gebruikers downloaden en installeren van de client extern bureaublad. U vindt de URL voor de client op de pagina Azure RemoteApp snel starten. Vervolgens hebben gebruikers zich aanmelden bij de client en toegang tot de apps die u hebt gepubliceerd.



### <a name="help-us-help-you"></a>Help ons te helpen u
Wist u naast de beoordeling van dit artikel en het toevoegen van opmerkingen omlaag onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreken? Iets mis? Ik iets dat is alleen verwarrend schrijven? Omhoog schuiven en klik op **bewerken op GitHub** als u wijzigingen wilt - die wordt verzonden naar ons voor revisie en vervolgens zodra we zich op deze afmelden, ziet u uw wijzigingen en verbeteringen hier.
