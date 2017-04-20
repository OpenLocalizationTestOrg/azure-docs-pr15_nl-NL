
<properties 
    pageTitle="Azure AD + Active Directory-vereisten voor Azure RemoteApp | Microsoft Azure" 
    description="Leer hoe u Active Directory instellen voor gebruik met Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + Active Directory-vereisten voor Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.


Voor uw siteverzameling van de hybride Azure RemoteApp of voor een cloud-siteverzameling die u wilt communiceren met AD Connect, moet u als volgt te werk.

### <a name="connect-azure-ad-and-active-directory"></a>Verbinding maken met Azure AD en Active Directory

Als u verbinden met uw Azure AD-tenant en uw on-premises Active Directory-omgevingen wilt, gebruikt u AD Connect. Het kost u alleen [4 klikken](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) om verbinding maken met de twee mappen.

Notitie - adreslijstsynchronisatie is vereist voor hybride siteverzamelingen.

### <a name="make-sure-your-domaincom-match"></a>Zorg ervoor dat uw "@domain.com" overeenkomen met
Controleer voordat u begint of dat de UPN voor uw on-premises implementatie bos overeenkomt met het achtervoegsel van uw domein Azure AD. 

Nadat u de UPN-domeinachtervoegsel in Azure AD instelt, alle gebruikers logboekregistratie in Azure RemoteApp wordt Meld u aan als “user@ <the suffix you set up>". Zorg ervoor dat gebruikers zich ook met dezelfde aanmelden kunnen user@suffix in de on-premises domein. In bepaalde gevallen kunt u een domeinnaam instellen in Azure AD tijdens het geven van een ander domeinachtervoegsel voor de gebruiker op prem. In dit geval uw gebruikers kunnen geen verbinding maken met een domein behoren computers en bronnen via Azure RemoteApp.

Als u uw domein UPN-achtervoegsel in AAD als contoso.com hebt ingesteld, maar sommige gebruikers op de lokale/AD zijn geconfigureerd dat Meld u aan met bijvoorbeeld @contoso.uk, en vervolgens deze gebruikers worden niet correct Meld u aan bij de verzameling ARA. Gebruikers UPN in AAD en AD moeten hetzelfde voor de aanmelding mogelijk"

### <a name="create-objects-for-azure-remoteapp"></a>Objecten maken voor Azure RemoteApp
U moet ook de volgende lokale Active Directory-objecten maken:

- Een serviceaccount voor toegang tot domein resources voor RemoteApp-programma's voor het samenvoegen van de eindpunten RDSH naar de on-premises domein.
- Een organisatie-eenheid RemoteApp machine objecten bevatten. Gebruik van de organisatie-eenheid wordt aanbevolen (maar niet vereist) te isoleren de accounts en het beleid dat u met RemoteApp gebruiken wilt.

U deze beide objecten nodig hebt bij het maken van uw siteverzameling RemoteApp daarom moet u eerst te doen deze stappen.