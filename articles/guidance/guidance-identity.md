<properties
   pageTitle="Identiteit in Azure beheren | Microsoft Azure"
   description="Dit artikel wordt uitgelegd en verschilt van de verschillende methoden voor het beheren van identiteit in hybride-systemen die de grens op-premises/cloud met Azure's beslaan."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Identiteit in Azure beheren

In de meeste enterprise-systemen op basis van Windows, gebruikt u AD (Active Directory) identiteit managementdiensten leveren aan uw toepassingen. AD werkt ook in een on-premises omgeving, maar als u de infrastructuur van uw netwerk in de cloud uitbreidt er enkele belangrijke beslissingen nemen met betrekking tot het beheren van identiteit. Moet u de domeinen van uw on-premises implementatie om op te nemen VMs in de cloud uitvouwen? Moet u nieuwe domeinen in de cloud maken en zo ja, hoe? Moet u uw eigen bos implementeren in de cloud of moet u ervoor van Azure Active Directory (AAD) gebruiken?

In dit artikel worden enkele algemene opties voor de vergadering de uitdagingen dat van dit scenario uitgaat en kunt die u bepalen welke oplossing het beste aan uw wensen voldoet op basis van uw vereisten.

## <a name="using-azure-active-directory"></a>Gebruik van de Azure Active Directory

U kunt AAD maken van een AD-domein in de cloud en dit koppelen aan een on-premises AD-domein. AAD kunt u eenmalige aanmelding (SSO) configureren voor gebruikers die werken met toepassingen via de cloud.

[! [0]][0]

AAD is een eenvoudige manier om een beveiligingsdomein implementeren in de cloud. Deze wordt gebruikt door veel Microsoft-toepassingen, zoals Microsoft Office 365. 

Voordelen van het gebruik van AAD:

- Er is niet nodig voor het behoud van een Active Directory-infrastructuur in de cloud. AAD is volledig beheerde en beheerd door Microsoft.

- AAD biedt dezelfde identiteitsgegevens beschikbaar on-premises.

- Verificatie kan optreden in Azure wordt aangegeven, dat u een externe toepassingen en gebruikers contact opnemen met de on-premises domein wordt afgetrokken.

Aandachtspunten bij het gebruik van AAD wordt verwezen:

- Identiteit services zijn beperkt tot gebruikers en groepen. Er is geen mogelijkheid om te verifiëren service en computer-accounts.

- U moet connectiviteit configureren met uw on-premises domein wilt behouden de AAD-adreslijst gesynchroniseerd. 

- U bent verantwoordelijk voor het publiceren van toepassingen die gebruikers toegang in de cloud tot en met AAD tot hebben.

Lees voor gedetailleerde informatie [Implementatie van Azure Active Directory][implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>Met Active Directory in de cloud die zijn gekoppeld aan een on-premises implementatie bos

U kunt lokale AD Directory Services (AD DS) hosten kan, maar in een hybride scenario waar elementen van een toepassing in Azure bevinden zich efficiënter deze functionaliteit en de AD-opslagplaats repliceren in de cloud. Deze methode verkleint de latentie die worden veroorzaakt door het verzenden van verificatie en voor lokale autorisatieaanvragen vanuit de cloud weer terug naar AD DS on-premises implementatie uitgevoerd. 

[! [1]][1]

Deze methode is vereist dat u uw eigen domein in de cloud maken en aan de on-premises implementatie bos toevoegen. U maakt VMs als host voor de AD DS-services.

Voordelen van het gebruik van een afzonderlijk domein in de cloud:

- Biedt de mogelijkheid om te verifiëren gebruiker, service en computer accounts on-premises implementatie en in de cloud.

- Biedt toegang tot dezelfde identiteitsgegevens beschikbaar on-premises.

- Er is niet nodig voor het beheren van een afzonderlijke AD-bos; het domein in de cloud kunt deel uitmaakt van de on-premises implementatie bos.

- U kunt toepassen Groepsbeleid gedefinieerd door de on-premises implementatie GPO objecten op het domein in de cloud.

Overwegingen bij het gebruik van een afzonderlijk domein in de cloud:

- Moet u uw eigen AD DS-servers en domein in de cloud maken en beheren.

- Er zijn mogelijk enkele latentie synchronisatie tussen de domeinservers in de cloud en de on-premises implementatie-servers.

Zie voor informatie over het configureren van deze architectuur [Uitbreiden Active Directory Directory Services (Hiermee) naar Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Active Directory gebruikt met een afzonderlijke bos

Een organisatie die wordt uitgevoerd AD (Active Directory) on-premises implementatie wellicht een bos die bestaat uit veel verschillende domeinen. U kunt domeinen moeten worden geïsoleerd tussen functiegebieden die moet worden geïsoleerd, mogelijk vanwege de beveiliging bieden, maar u kunt gegevens delen tussen domeinen door in te stellen vertrouwensrelaties.

[! [2]][2]

Een organisatie die gebruikmaakt van afzonderlijke domeinen kunt profiteren van Azure door een of meer van deze domeinen verplaatsen in een afzonderlijk bos in de cloud. U kunt ook een organisatie mogelijk wilt bewaren alle cloud resources logisch afzonderlijk vanuit deze aangehouden on-premises en informatie over cloud resources in hun eigen adreslijst opslaan als onderdeel van een bos die ook gehouden in de cloud.

Voordelen van het gebruik van een afzonderlijke bos in de cloud:

- U kunt implementeren van on-premises implementatie identiteiten en scheidt u identiteiten van Azure alleen-lezen.

- Er is geen moeten worden gerepliceerd vanuit de on-premises AD bos naar Azure, waardoor de gevolgen van netwerklatentie.

Overwegingen:

- Verificatie voor on-premises implementatie identiteiten in de cloud voert extra netwerk *hops* naar de on-premises implementatie AD-servers.

- Moet u uw eigen AD DS-servers en bos implementeren in de cloud, en de juiste vertrouwensrelaties definiëren tussen forests.

Het [maken van een Active Directory Directory Services (Hiermee) resource bos in Azure wordt aangegeven] document[ adds-forest-in-azure] wordt beschreven hoe u deze methode uitgebreider implementeren.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Gebruik van Active Directory Federation Services (ADFS)-met Azure

ADFS on-premises implementatie kan worden uitgevoerd kan, maar in een hybride scenario waar toepassingen in Azure bevinden zich efficiënter deze functionaliteit implementeren in de cloud, zoals hieronder wordt weergegeven.

[! [3]][3]

Deze architectuur is vooral handig voor:

- Oplossingen die gebruikmaken van federatieve autorisatie om weer te geven van webtoepassingen naar partnerorganisaties.

- Systemen die ondersteuning bieden voor toegang via webbrowsers uitgevoerd buiten de organisatie-firewall.

- Systemen waarmee gebruikers kunnen de toegang tot uw webtoepassingen door verbinding te maken van geautoriseerde externe apparaten, zoals een externe computers, notitieblokken en andere mobiele apparaten. 

Voordelen van het gebruik van ADFS met Azure:

- U kunt gebruikmaken van claims toepassingen.

- Deze biedt de mogelijkheid om te vertrouwen externe partners voor verificatie.

- Dit is compatibel met grote verzameling protocollen voor verificatie.

Overwegingen voor het gebruik van ADFS met Azure:

- Dit moet u uw eigen Hiermee, ADFS en toepassingsproxy ADFS-Web-servers implementeren in de cloud.

- Deze architectuur kan zijn complexe te configureren.

Voor meer informatie lezen [Implementeren Active Directory Federation Services (ADFS) in Azure wordt aangegeven][adfs-in-azure].

## <a name="next-steps"></a>Volgende stappen

De volgende informatiebronnen wordt uitgelegd hoe u het implementeren van de architectuur in dit artikel beschreven.

- [Implementatie van Azure Active Directory][implementing-aad]
- [Active Directory-adreslijstservices (Hiermee) uitbreiden naar Azure][extending-adds]
- [Maken van een Active Directory Directory Services (Hiermee) resource bos in Azure wordt aangegeven][adds-forest-in-azure]
- [Implementatie van Azure Active Directory Federation Services (ADFS)][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Cloud identiteit architectuur met Azure Active Directory"
[1]: ./media/guidance-identity/figure2.png "Secure hybride netwerkarchitectuur met Active Directory"
[2]: ./media/guidance-identity/figure3.png "Secure hybride netwerkarchitectuur met afzonderlijk AD-domeinen en forests"
[3]: ./media/guidance-identity/figure4.png "Hybride netwerkarchitectuur met ADFS Secure"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md