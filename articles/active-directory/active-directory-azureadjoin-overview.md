<properties
    pageTitle="Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden | Microsoft Azure"
    description="Een gedetailleerd overzicht van hoe Windows 10-apparaten van Azure AD Join gebruikmaken kunnen moet worden geregistreerd op Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden

## <a name="what-is-azure-active-directory-join"></a>Wat is de Azure Active Directory deelnemen?
Azure Active Directory deelnemen aan (Azure AD deelnemen aan), is de functionaliteit gebruiken die een apparaat eigendom bij het bedrijf in Azure Active Directory registreert zodat Centraal beheer van het apparaat. Deze kan gebruikers zoals werknemers en leerlingen/studenten verbinding maken met de cloud enterprise via Azure Active Directory. Hiermee worden vereenvoudigd implementaties van Windows en toegang tot de organisatie-apps en resources vanaf elk apparaat met Windows, beide bedrijfs-eigenaar en persoonlijk eigendom (BYOD).

Azure AD-Join is bedoeld voor ondernemingen die eerste/cloud-alleen de cloud zijn--meestal kleine en middelgrote bedrijven die ik heb een on-premises implementatie Windows Server Active Directory-infrastructuur. Dat genoemd, Azure AD-Join kunt en ook wordt worden gebruikt door grote organisaties op apparaten die niet in staat te doen een join traditionele domein (bijvoorbeeld mobiele apparaten) of voor gebruikers die hoofdzakelijk moeten toegang hebben tot de Office 365 of andere apps Azure AD SaaS zijn.

Hoewel de join traditionele domein nog steeds dat de beste biedt on-premises implementatie zoekervaring op apparaten die kunnen lid worden van domein, is Azure AD Join geschikt voor apparaten die niet aan domein toevoegen. Azure AD-Join is ook geschikt voor het beheren van gebruikers in de cloud. Dit gebeurt met behulp van mogelijkheden voor het beheer van mobiele apparaat in plaats van met behulp van hulpmiddelen voor projectbeheer traditionele domein, zoals Groepsbeleid en System Center Configuration Manager (SCCM).

![Overzicht van zakelijke apparaten en persoonlijke apparaten met lokale Active Directory en Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Waarom moeten ondernemingen vast Azure AD deelnemen?

* **Bedrijven die grotendeels in de cloud zijn**: als u aangekomen bent of wilt verplaatsen naar een model waarin u uw on-premises implementatie op het milieu worden afgetrokken en wilt werken in de cloud, deelnemen aan Azure AD kan voordelen. U kunt wellicht Azure AD-accounts hebt gemaakt handmatig of via synchronisatie van uw lokale Active Directory. In beide gevallen hebt u een account in Azure AD en u deze kunt gebruiken om aan te melden bij Windows 10. Uw gebruikers kunnen deelnemen aan zijn of haar computer naar Azure AD via een van beide de kant-en-klare ervaring (OOBE) of via het menu instellingen. Sitelid, kunnen profiteren van eenmalige aanmelding (SSO) toegang tot cloud bronnen zoals Office 365, in de browser of in Office-toepassingen.

* **Onderwijsinstellingen**: een scenario's beschreven we over vaak horen is dat onderwijsinstellingen twee gebruikerstypen: faculteit en leerlingen/studenten. Faculteitsleden worden beschouwd als van langere leden van de organisatie, waardoor on-premises implementatie-accounts maken voor hen wenselijk is. Maar leerlingen/studenten korterlopende deel uitmaken van de organisatie en kan dus kunnen worden beheerd in Azure AD. Dit betekent dat directory schaal naar de cloud in plaats van opgeslagen on-premises implementatie verplaatst kunt. Ook betekent dit dat leerlingen/studenten kunnen bij Windows met hun Azure AD-account aanmelden en toegang tot Office 365-informatiebronnen, in de browser of in Office-toepassingen krijgen.

* **Detailhandel bedrijven**: een ander scenario we over van klanten ken is hun willen seizoensgebonden werknemers eenvoudiger te beheren.  Accounts voor langere termijn, fulltime werknemers worden opnieuw, meestal gemaakt als lokale accounts op computers domein behoren. Maar seizoensgebonden werknemers korterlopende lid zijn van de organisatie, zodat u wenselijk voor het beheren van deze waar gebruikerslicenties eenvoudiger rond kunnen worden verplaatst. Deze gebruikersaccounts maken in de cloud met Office 365-licenties kan de gebruikers krijgen van de voordelen aan te melden bij Windows en Office-toepassingen met een Azure AD-account. Ondertussen onderhouden u meer flexibiliteit met hun licenties nadat ze verlaten.
* **Andere bedrijven**: hoewel u gebruikers in uw lokale Active Directory behouden, kunt u nog steeds profiteren van dat gebruikers worden Azure AD samengevoegd. Dat komt doordat Azure AD biedt een eenvoudigere functionaliteit voor gekoppelde, efficiënt Apparaatbeheer, automatische mobiele apparaat management registratie en mogelijkheid voor eenmalige aanmelding voor Azure AD en on-premises bronnen.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Azure AD deelnemen aan welke mogelijkheden biedt?
Met Azure AD deelnemen aan krijgt u het volgende:

* **Selfservice inrichting van bedrijfs-eigendom apparaten**: met Windows 10, kunnen gebruikers een gloednieuwe, krimp apparaat in de modus kant-en-klare, zonder IT betrokkenheid.


* **Ondersteuning voor moderne formulier factoren**: Azure AD Join werkt op apparaten waarop niet het traditionele domein mogelijkheden deelnemen.  


* **Ondersteuning voor bestaande organisatie-accounts**: gebruikers niet meer hoeft te maken en onderhouden een een persoonlijk Microsoft-account om de beste ervaring op bedrijf uitgegeven apparaten, net zo met Windows 8. Ze kunnen hun bestaande werk-accounts in plaats daarvan in Azure AD gebruiken. In veel organisaties wordt betekent dit in principe dat gebruikers kunnen instellen en meld u aan bij Windows met dezelfde referenties waarmee ze toegang tot Office 365.


* **Automatische mobiele apparaat management registratie**: apparaten kunnen automatisch worden ingeschreven voor mobiele apparaten beheren wanneer verbinding hebt met Azure AD. Dit proces werkt met Microsoft Intune en partner mobiele apparaat managementoplossingen. Wanneer Apparaatbeheer met Intune klaar is, IT-beheerders kunnen monitor/Azure AD-die zijn gekoppeld apparaten beheren samen met een domein behoren apparaten in de beheerconsole SCCM.


* **Eenmalige aanmelding naar bedrijfsresources**: gebruikers eenmalige aanmelding van het Windows-bureaublad-apps en bronnen in de cloud, zoals Office 365 en duizenden bedrijfstoepassingen die afhankelijk van Azure AD voor authenticatie via [Azure AD Connect zijn](active-directory-azureadjoin-deployment-aadjoindirect.md)te rekenen. Zakelijk eigendom apparaten die zijn gekoppeld aan Azure AD ook profiteren van eenmalige aanmelding van on-premises implementatie resources wanneer het apparaat is op een bedrijfsnetwerk en vanaf elke locatie wanneer deze resources worden weergegeven via de [Azure AD-toepassingsproxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx).


* **OS staat Roaming**: instellingen voor toegankelijkheid, websites, Wi-Fi wachtwoorden en andere instellingen die worden gesynchroniseerd tussen apparaten bedrijfs-eigendom zonder een persoonlijk Microsoft-account.


* **Gereed voor Enterprise Windows Store**: de Windows Store app acquisition en licenties met Azure AD-accounts ondersteunt. Organisaties kunnen volumelicentie apps en deze beschikbaar maken voor gebruikers in hun organisatie.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Hoe werken verschillende apparaten met Azure AD deelnemen?

| Bedrijfs-apparaat (lid is van de on-premises domein)                                                                                                                                                                                         | Bedrijfs-apparaat (lid is van de cloud)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Persoonlijk apparaat                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Gebruikers kunnen zich aanmelden bij Windows met werk referenties (zoals ze vandaag doen).                                                                                                                                                                        | Gebruikers kunnen zich aanmelden bij Windows met werk referenties die worden beheerd in Azure AD. Dit geldt voor zakelijke apparaten in drie zaken: 1) de organisatie geen Active Directory on-premises (bijvoorbeeld een kleine bedrijven). 2) de organisatie niet alle gebruikersaccounts maken in Active Directory (bijvoorbeeld accounts voor leerlingen en studenten, consultants of seizoensgebonden werknemers zijn niet gemaakt in Active Directory). 3) de organisatie heeft zakelijke apparaten die kunnen niet worden toegevoegd aan een (on-premises)-domein, zoals telefoons of tablets waarop een Mobile SKU (bijvoorbeeld een secundaire apparaat die u hebt gemaakt voor een factory/detailhandel) wordt uitgevoerd. Azure AD-Join ondersteunt deelnemen aan van zakelijke apparaten voor beheerde en federatieve organisaties. | Gebruikers met hun persoonlijke Microsoft-accountreferenties (niet veranderd), moet u zich aanmelden bij Windows.                                                |
| Gebruikers hebben toegang tot roaming instellingen en de onderneming Windows Store. Deze services geschikt is voor werk-accounts en een persoonlijk Microsoft-account niet vereisen. U moet hiervoor organisaties hun lokale Active Directory verbinden met Azure AD.                                        | Selfservice-instelling u kunt doen. Ze kunnen leest u over de eerste sessie (FRX) via hun werkaccount als alternatief voor IT-inrichten met de apparaten, hoewel beide methoden worden ondersteund.                                                                                                                                                                                                                                                                                                                                                                             | Gebruikers kunnen eenvoudig met het toevoegen van een werkaccount dat wordt beheerd in Active Directory of Azure AD.                                                      |
| Gebruikers kunnen eenmalige aanmelding kunnen vanaf het bureaublad werken apps, websites en resources--inclusief zowel on-premises implementatie resources en cloud-apps die gebruikmaken van Azure AD voor verificatie.                                                                                                            | Apparaten worden automatisch worden geregistreerd in de enterprise-adreslijst (Azure AD) en automatisch worden geregistreerd in mobiele apparaten beheren. (Azure AD Premium-functie).                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Gebruikers hebben SSO mogelijkheid alle apps en websites/invoegen met dit werkaccount.                                              |
| Gebruikers kunnen hun persoonlijke Microsoft-account voor toegang tot hun persoonlijke afbeeldingen en bestanden zonder die invloed hebben op bedrijfsgegevens toevoegen. (Instellingen roaming blijven werken met hun werk-accounts.) Het Microsoft-account kunt eenmalige aanmelding en niet meer stations de roaming-instellingen.  | U kunt doen selfservice wachtwoordherstel (SSPR) op winlogon, wat betekent dat ze een vergeten wachtwoord opnieuw kunnen instellen. (Azure AD Premium-functie).                                                                                                                                                                                                                                                                                                                                                                                                                                   | Gebruikers hebben toegang tot de onderneming Windows Store, zodat ze kunnen ophalen en LOB-apps op hun persoonlijke apparaten gebruiken. |                                                               |


## <a name="additional-information"></a>Aanvullende informatie
* [Windows 10 voor de onderneming: manieren om apparaten voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Identiteiten zonder wachtwoorden via Microsoft Passport verifiëren](active-directory-azureadjoin-passport.md)
* [Meer informatie over het gebruik scenario's voor het deelnemen aan Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Deelnemen aan Azure AD instellen](active-directory-azureadjoin-setup.md)
