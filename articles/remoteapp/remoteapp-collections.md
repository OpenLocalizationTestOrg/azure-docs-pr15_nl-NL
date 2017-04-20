<properties 
    pageTitle="Welk soort siteverzameling hebt u nodig voor Azure RemoteApp? | Microsoft Azure" 
    description="Informatie over het typen van collecties met Azure RemoteApp beschikbaar." 
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



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Welk soort siteverzameling hebt u nodig voor Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp kunt u apps en bronnen delen met gebruikers op elk apparaat. We dit doen door de verzamelingen waarin de apps en bronnen maken en klik vervolgens u deze verzamelingen delen met gebruikers. Er zijn 2 verschillende siteverzamelingen opties, met verschillende netwerk- en verificatieopties - die geschikt is voor u?

Laten we Doorloop de verschillende overwegingen en de opties die u aanbrengen wilt in het optimaal gebruik van de Azure RemoteApp-verzameling. 


## <a name="quick-differences-between-the-collection-types"></a>Snelle verschillen tussen het typen van de siteverzameling

|           | Cloud | Hybride |
|-----------|-------|--------|
|Een bestaande VNET gebruiken| Ja| Ja|
|AD-verbonden accounts (DirSync) vereist| Nee| Ja|
|Vereist aan domein toevoegen| Nee| Ja|
|Domeincontroller toegankelijk zijn voor VNET vereist| Nee| Ja|

## <a name="cloud-collections"></a>Cloud-verzamelingen
- Snelle maken - de verzameling snel is ingericht, wat betekent dat uw apps krijgen aan gebruikers.
- Uw eigen apps weer of onze delen. U kunt een aangepaste afbeelding (samengesteld op basis van een VM Azure) of een van de afbeeldingen die wordt geleverd bij uw abonnement.
- U hoeft niet te configureren van een verbinding tussen uw siteverzameling en uw on-premises domein.
- Maar u kunt desgewenst uw eigen VNET Azure toegang wilt bieden naar uw on-premises omgeving voor het delen van gegevens of niet-Windows-verificatie in resources zoals SQL Server (met behulp van de database-verificatie) gebruiken.


OK, hoe maak ik een?

- Alleen cloud? Maken met de optie **Snelle maken** in de portal.
- Cloud + VNET? Maken met de optie **maken met VNET** , maar niet wilt deelnemen aan een domein.

## <a name="hybrid-collections"></a>Hybride verzamelingen
- Volledige toegang tot on-premises netwerk + Azure VNET bieden.
- Domein join toegang voor apps en gegevens bevat. Externe toepassingen kunnen verifiëren ten opzichte van uw lokale Active Directory - ze vervolgens toegang tot bronnen in uw domein.
- Geavanceerde voor controle en beheer met bestaande System Center oplossingen en Groepsbeleid van Windows (via een aangepaste afbeelding die is gebaseerd op Windows Server 2012 R2) inschakelen
- Ondersteuning voor [ExpressRoute](https://azure.microsoft.com/services/expressroute/) voor uw VNET Azure verbinding met uw lokale VNET.

Met de optie **maken met VNET** maken en toevoegen aan een domein moet kiezen.

## <a name="authentication-options"></a>Verificatieopties voor
Azure RemoteApp ondersteunt zowel Microsoft-accounts en Azure Active Directory-accounts, maar niet alle collecties ondersteunen alle methoden. 

| Accounttype                      |                                                             | Cloud | Cloud + VNET | Hybride |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Microsoft-Account                 |                                                             | Ja   | Ja          | Nee     |
| Azure Active Directory (Azure AD) |                                                             |       |              |        |
|                                   | Azure AD alleen                                               | Ja   | Ja          | Nee     |
|                                   | AD Connect met Wachtwoordsynchronisatie                               | Ja   | Ja          | Ja    |
|                                   | AD Connect zonder Wachtwoordsynchronisatie                            | Ja   | Ja          | Nee     |
|                                   | AD Connect met AD FS                                       | Ja   | Ja          | Ja    |
|                                   | 3e derden Azure ondersteund identiteitsprovider (zoals Ping) | Ja   | Ja          | Ja    |
| Meervoudige verificatie       |                                                             | Ja   | Ja          | Ja    |



### <a name="cloud-and-cloud--vnet"></a>Cloud en Cloud + VNET 
Met collecties van de cloud, kunt u Microsoft-accounts, Azure AD-accounts of een combinatie van beide. Gebruik de accounts die het meest voor uw gebruikers geschikt.

Er zijn geen specifieke vereisten voor het gebruik van Microsoft-accounts. 

Als u gebruiken Azure AD-accounts wilt, moet u om ervoor te zorgen dat uw Azure AD-tenant overeenkomt met de naam die is gekoppeld aan uw abonnement. Wanneer u uw Azure RemoteApp-abonnement hebt gemaakt, is de Azure AD-tenant u automatisch gekoppeld aan uw abonnement. Elke Azure AD-gebruiker die u machtigen moet die dezelfde tenant. Indien nodig kunt u [de Azure AD-tenant wijzigen](remoteapp-changetenant.md) die is gekoppeld aan uw abonnement.
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybride (of cloud + Azure AD + AD)

Gebruik van Azure AD + lokale Active Directory is vereist voor een siteverzameling hybride. U moet AD Connect gebruiken om te integreren van de twee mappen. Maar ik heb een keuze wanneer u gaat naar hoe u AD Connect configureren. 

Er zijn 2 AD Connect-scenario's - Wachtwoordsynchronisatie of gebruiken van AD-Federatie. Bekijk de [informatie in AD Connect](../active-directory/active-directory-aadconnect.md) om te bepalen welke van deze werkt het best voor u.

U kunt ook Azure AD + AD gebruiken met een cloud-siteverzameling. Zorg ervoor dat u volgt dezelfde stappen instellen.

Bekijk [Azure AD + Active Directory-vereisten voor Azure RemoteApp](remoteapp-ad.md) voor de benodigde stappen voor het configureren van Azure AD en Active Directory.

## <a name="go-create-your-collection"></a>Ga op de verzameling maken!
OK, ik denk dat we hebt kunt deze uitvoeren nu, dus er slechts één criterium nog do is-uw eerste Azure RemoteApp-verzameling maken.

[Een cloud-siteverzameling maken](remoteapp-create-cloud-deployment.md) of [een hybride-verzameling maken](remoteapp-create-hybrid-deployment.md) - krijgen net maken.
