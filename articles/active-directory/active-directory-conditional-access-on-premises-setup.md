<properties
    pageTitle="Instellen van on-premises implementatie voorwaardelijke toegang tot een Azure Active Directory-apparaatregistratie | Microsoft Azure"
    description="Een stapsgewijs voorwaardelijke toegang tot on-premises implementatie-toepassingen Active Directory Federation Services (AD FS) gebruiken in Windows Server 2012 R2 in te schakelen."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>On-premises implementatie voorwaardelijke toegang tot een Azure Active Directory-apparaatregistratie instellen

Persoonlijk kunnen eigendom apparaten van uw gebruikers worden gemarkeerd bekende naar uw organisatie doordat de gebruikers in plaats van werk join zijn of haar apparaten met de apparaatregistratie van Azure Active Directory-service. Hieronder ziet u een stapsgewijze handleiding voorwaardelijke toegang tot on-premises implementatie-toepassingen Active Directory Federation Services (AD FS) gebruiken in Windows Server 2012 R2 in te schakelen.

> [AZURE.NOTE]
> Office 365-licentie of Azure AD Premium is vereist als apparaten met een geregistreerd in Azure Active Directory-apparaatregistratie service voorwaardelijke-beleid. Dit geldt ook voor beleid van Active Directory Federation Services (AD FS) naar on-premises implementatie bronnen.

Zie [deelnemen met werkplek vanaf elk apparaat voor eenmalige aanmelding en naadloze tweede Factor verificatie via bedrijfstoepassingen](https://technet.microsoft.com/library/dn280945.aspx)voor meer informatie over de voorwaardelijke toegang scenario's voor on-premises implementatie.

Deze functies zijn beschikbaar voor klanten die een licentie Azure Active Directory Premium aanschaffen.

<a name="supported-devices"></a>Ondersteunde apparaten
-------------------------------------------------------------------------
* Windows 7 domein gekoppeld apparaten.
* Windows 8.1 persoonlijk en domein gekoppeld apparaten.
* iOS 6 en hoger, Safari-browser
* Android 4.0 of hoger, Samsung GS3 of boven telefoons, Samsung Note2 of boven tablets.


<a name="scenario-prerequisites"></a>Scenario vereisten
------------------------------------------------------------------------
* Abonnement op Office 365 of Azure Active Directory Premium
* Een Azure Active Directory-tenant
* Windows Server Active Directory (Windows Server 2008 of hoger)
* Bijgewerkte schema in Windows Server 2012 R2
* De licentie voor Premium Azure Active Directory
* Windows Server 2012 R2 Federation Services, geconfigureerd voor eenmalige aanmelding van Azure AD
* Windows Server 2012 R2 Web Application-Proxy Microsoft Azure Active Directory-verbinding (Azure AD Connect). [Download Azure AD Connect hier](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* Geverifieerd-domein.

<a name="known-issues-in-this-release"></a>Bekende problemen in deze release
-------------------------------------------------------------------------------
* Apparaat op basis van voorwaardelijke clienttoegangsbeleid vereisen apparaat object terugschrijven met Active Directory van Azure Active Directory. Maximaal uur 3 voor apparaatobjecten worden geschreven terug naar Active Directory
* apparaten met iOS 7 wordt altijd de gebruiker gevraagd om een certificaat tijdens verificatie via clientcertificaat selecteren.
* Sommige versies van IOS 8, voordat iOS 8.3 niet werken.

## <a name="scenario-assumptions"></a>Scenario hypothesen
Dit scenario wordt ervan uitgegaan dat u hebt een hybride omgeving die bestaat uit een Azure AD-tenant en een lokale Active Directory. Deze tenants moeten worden verbonden met Azure AD Connect en met een geverifieerd-domein en AD FS voor eenmalige aanmelding. De onderstaande controlelijst kunt u configureren van uw omgeving naar de fase hierboven is beschreven.

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>Controlelijst: Vereisten voor voorwaardelijke toegang scenario
--------------------------------------------------------------
Sluit uw Azure AD-tenant met uw lokale Active Directory.

## <a name="configure-azure-active-directory-device-registration-service"></a>Azure Active Directory apparaat registratie-Service configureren
Gebruik deze handleiding om te implementeren en Azure Active Directory apparaat registratie-Service configureren voor uw organisatie.

Deze handleiding wordt ervan uitgegaan dat u hebt geconfigureerd Windows Server Active Directory en u zich hebt geabonneerd op Microsoft Azure Active Directory. Zie de bovenstaande voorwaarden.

Als u wilt implementeren Azure Active Directory apparaat registratieservice met uw Azure Active Directory-tenant, voert u de taken in de volgende controlelijst, in volgorde. Wanneer u een koppeling gaat u naar een conceptuele onderwerp, Ga terug naar deze controlelijst nadat u het conceptuele onderwerp bekijken, zodat u kunt doorgaan met de resterende taken in deze controlelijst. Sommige taken bevat een scenario validatiestap waarmee u kunt bepalen of dat de stap is voltooid.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Deel 1: Inschakelen Azure Active Directory-apparaatregistratie

Volg de onderstaande inschakelen en configureren van de Azure Active Directory apparaat registratie-Service controlelijst.

| Taak                                                                                                                                                                                                                                                                                                                                                                                             | Verwijzing                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Schakel de apparaatregistratie in uw Azure Active Directory-tenant toe te staan dat apparaten om over te nemen aan de werkplek. Meervoudige verificatie is standaard niet ingeschakeld voor de service. Meervoudige verificatie is echter aangeraden wanneer u een apparaat registreren. Voordat u meervoudige verificatie in ADRS inschakelt, moet u ervoor zorgen dat AD FS is geconfigureerd voor een meervoudige verificatie-provider. | [Azure Active Directory-apparaatregistratie inschakelen](active-directory-conditional-access-device-registration-overview.md)               |
| Apparaten ontdekt uw Azure Active Directory apparaat registratieservice herkent bekende DNS-records. U moet de DNS van uw bedrijf zodanig configureren dat apparaten uw Azure Active Directory apparaat registratieservice kunnen detecteren.                                                                                                                                                   | [Azure Active Directory-apparaatregistratie discovery configureren.](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Deel 2: Implementeren en Windows Server 2012 R2 Active Directory Federation Services configureren en hiervoor een relatie Federatie met Azure AD instellen

| Taak                                                                                                                                                                                                                                                                                                                                                                                             | Verwijzing                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Active Directory Domain Services domein met de Windows Server 2012 R2 schema-uitbreidingen implementeren. U hoeft niet alle domeincontroller upgrade naar Windows Server 2012 R2. De upgrade schema is de enige vereiste. | [Upgrade van uw Active Directory Domain Services-Schema](#upgrade-your-active-directory-domain-services-schema)               |
| Apparaten ontdekt uw Azure Active Directory apparaat registratieservice herkent bekende DNS-records. U moet de DNS van uw bedrijf zodanig configureren dat apparaten uw Azure Active Directory apparaat registratieservice kunnen detecteren.                                                                                                                                                   | [Voorbereiden van uw Active Directory-ondersteuning voor apparaten](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>Deel 3: Inschakelen apparaat terugschrijven in Azure AD

| Taak                                                                                                                                                                                                                                                                                                                                                                                             | Verwijzing                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Stap 2 van inschakelen apparaat write-backs in Azure AD Connect uitvoeren. Na voltooiing, retourneren dit deze handleiding. | [Apparaat write-backs in Azure AD Connect inschakelen](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[Optionele] Deel 4: Inschakelen meervoudige verificatie

Het is raadzaam dat u een van de verschillende opties voor meervoudige verificatie configureren. Als u dat MFA wilt, raadpleegt u [kiezen de beveiligingsoplossing meervoudige voor u](../multi-factor-authentication/multi-factor-authentication-get-started.md). Het bevat een beschrijving van elke oplossing, en koppelingen waarmee u kunt configureren van de oplossing van uw keuze.

## <a name="part-5-verification"></a>Deel 5: verificatie

De implementatie is voltooid. U kunt nu bepaalde scenario's uitproberen. Volg de onderstaande koppelingen voor experimenteren met de service en vertrouwd te raken met de functies


| Taak                                                                                                                                                                                                                         | Verwijzing                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Deelnemen aan sommige apparaten met uw werk via Azure Active Directory-apparaatregistratie. U kunt deelnemen aan iOS, Windows en Android-apparaten                                                                                         | [Deelnemen aan apparaten met uw werk via Azure Active Directory-apparaatregistratie](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| U kunt weergeven en geregistreerde apparaten met behulp van de Portal-beheerder in-/ uitschakelen. In deze taak wordt u sommige geregistreerde apparaten met behulp van de Portal beheerder weergeven.                                                        | [Overzicht van de registratie Azure Active Directory-apparaat](active-directory-conditional-access-device-registration-overview.md)                             |
| Controleer of dat apparaatobjecten naar worden geschreven terug van Azure Active Directory Windows Server Active Directory.                                                                                                                  | [Controleer of geregistreerde apparaten zijn geschreven terug naar Active Directory](#verify-registered-devices-are-written-back-to-active-directory)                  |
| Nu dat gebruikers hun apparaten registreren kunnen, kunt u de toepassing maken-beleid in AD FS waarmee alleen geregistreerde apparaten. In deze taak u maakt openen een access-regel van toepassing en een aangepaste geweigerd bericht | [Maak een beleid van toepassing toegang en aangepaste toegang geweigerd](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Azure Active Directory integreren met lokale Active Directory
Hierdoor kunt u uw Azure AD-tenant integreren met uw lokale active directory, met Azure AD Connect. Hoewel de stappen in de portal van Azure klassieke beschikbaar zijn, noteer eventuele speciale instructies in deze sectie vermelde.

1.  Meld u aan bij de Azure klassieke portal met een account die is van een globale beheerder in Azure AD.
2.  Selecteer in het linkerdeelvenster, **Active Directory**.
3.  Selecteer het telefoonboek van uw op het tabblad **map** .
4.  Selecteer het tabblad **Directory-integratie** .
5.  Volg de stappen 1 tot en met 3 Azure Active Directory integreren met uw on-premises adreslijst onder sectie **implementeren en te beheren** .
  1.    Domeinen toevoegen.
  2.    Installeren en uitvoeren van Azure AD Connect: installeren Azure AD Connect met de volgende instructies, [aangepaste installatie van Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).
  3. Controleer en directory-synchronisatie beheren. Instructies voor eenmalige aanmelding zijn beschikbaar in deze stap.

  > [AZURE.NOTE]
  > Federatie met AD FS configureren, zoals in het document dat is gekoppeld hierboven wordt beschreven. U hoeft niet te configureren op een van de preview-functies.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Upgrade van uw schema Active Directory Domain Services

> [AZURE.NOTE]
> Een upgrade van uw Active Directory-schema kan niet ongedaan worden gemaakt. Het wordt aanbevolen dat u eerst dit in een testomgeving uitvoert.

1. Meld u aan bij uw domeincontroller met een account met zowel de ondernemingsadministrator en de beheerder van het Schema rechten.
2. Kopieer de **[media] \support\adprep** directory en de onderliggende mappen naar een van uw Active Directory-domeincontroller.
3. Waar is [media] het pad naar de Windows Server 2012 R2 installatiemedia.
4. Ga naar de map adprep vanaf een opdrachtprompt en uitvoeren: **adprep.exe/forestprep**. Volg de aanwijzingen om de upgrade schema te voltooien.

## <a name="prepare-your-active-directory-to-support-devices"></a>Uw Active Directory ter ondersteuning van apparaten voorbereiden

>[AZURE.NOTE] Dit is een eenmalige bewerking die u uitvoeren moet om het voorbereiden van uw Active Directory-bos apparaten worden ondersteund. U moet zijn aangemeld met beheerdersbevoegdheden voor enterprise en uw Active Directory-bos het schema van Windows Server 2012 R2 naar deze procedure hebt voltooid moet hebben.


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>Uw Active Directory-bos ter ondersteuning van apparaten voorbereiden

> [AZURE.NOTE]
>Dit is een eenmalige bewerking die u uitvoeren moet om het voorbereiden van uw Active Directory-bos apparaten worden ondersteund. U moet zijn aangemeld met beheerdersbevoegdheden voor enterprise en uw Active Directory-bos het schema van Windows Server 2012 R2 naar deze procedure hebt voltooid moet hebben.

### <a name="prepare-your-active-directory-forest"></a>Uw Active Directory-bos voorbereiden

1.  Open op de federatieserver, een Windows PowerShell-opdrachtvenster en typ: initialisatie-ADDeviceRegistration
2.  Wanneer u wordt gevraagd voor **ServiceAccountName**, typt u de naam van de serviceaccount dat u hebt geselecteerd als de serviceaccount voor AD FS. Als dit een gMSA-account is, voert u het account in de indeling **domain\accountname$** . Voor een domeinaccount, gebruikt u de opmaak **domain\accountname**.



### <a name="enable-device-authentication-in-ad-fs"></a>Apparaat-verificatie in AD FS inschakelen

1. Klik op de federatieserver, open de AD FS-beheerconsole en navigeer naar **AD FS** > **Beleidsregels voor verificatie**.
2. Selecteer **globale primaire verificatie bewerken** in het deelvenster **Acties** .
3. Controleer **apparaat-verificatie inschakelen** en selecteer vervolgens**OK**.
4. Standaard worden niet-gebruikte apparaten met AD FS geregeld verwijderd uit Active Directory. Bij gebruik van Azure Active Directory-apparaatregistratie zodat apparaten kunnen worden beheerd in Azure wordt aangegeven, moet u deze taak uitschakelen.


### <a name="disable-unused-device-cleanup"></a>Niet-gebruikte apparaat opruimen uitschakelen
1. Open op de federatieserver, een Windows PowerShell-opdrachtvenster en typ: Set-AdfsDeviceRegistration - MaximumInactiveDays 0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Azure AD Connect voorbereiden op een apparaat write-backs

1.  Vul in deel 1: Azure AD voorbereiden verbinding maken.


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>Deelnemen aan apparaten met uw werk via Azure Active Directory-apparaatregistratie

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>Deelnemen aan een iOS-apparaat met Azure Active Directory-apparaatregistratie

Azure Active Directory-apparaatregistratie gebruikt het registratieproces Over-the-Air profiel voor iOS-apparaten. Dit proces begint met de gebruiker die verbinding maken met de registratie-URL van het profiel met de webbrowser Safari. Het URL-indeling is als volgt:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Waar `yourdomainname` is de naam van het domein dat u hebt geconfigureerd met Azure Active Directory. Als uw domeinnaam contoso.com is, zou de URL bijvoorbeeld:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Zijn er veel verschillende manieren om te communiceren deze URL aan uw gebruikers. Het is raadzaam deze URL in een maatwerktoepassing toegang krijgen tot publiceren geweigerd bericht in AD FS. Hier vindt u in de komende sectie: [een access-beleid van toepassing maken en aangepaste toegang geweigerd](#create-an-application-access-policy-and-custom-access-denied-message).

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>Deelnemen aan een Azure Active Directory-apparaatregistratie met Windows 8.1-apparaat

1. Ga op uw apparaat met Windows 8.1 **PC-instellingen** > **netwerk** > **bedrijf**.
2. Voer uw gebruikersnaam in UPN-indeling. Bijvoorbeeld dan@contoso.com..
3. Selecteer **deelnemen**.
4. Wanneer u wordt gevraagd, aanmelden met uw referenties. Het apparaat is nu gekoppeld.

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>Deelnemen aan een Azure Active Directory-apparaatregistratie met Windows 7-apparaat
Als u wilt Windows 7-domein hebt geregistreerd gekoppeld apparaten moet u het apparaat registratie software-pakket implementeren. Het softwarepakket heet werkplek deelnemen aan voor Windows 7 en is beschikbaar op de [website van Microsoft Connect](https://connect.microsoft.com/site1164). Instructies die het gebruik van het pakket vindt u op [Automatische apparaatregistratie configureren voor Windows 7 domein apparaten die zijn gekoppeld](active-directory-conditional-access-automatic-device-registration-windows7.md).

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>Deelnemen aan een Android-apparaat met Azure Active Directory-apparaatregistratie

De [Azure verificator voor Android onderwerp](active-directory-conditional-access-azure-authenticator-app.md) bevat instructies over het installeren van Azure verificator app op uw Android-apparaat en een werkaccount toevoegen. Wanneer een werkaccount is gemaakt op een Android-apparaat, is dat apparaat bedrijf deel uitmaakt van de organisatie.

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>Controleer of geregistreerde apparaten zijn geschreven terug naar Active Directory
U kunt weergeven en controleer of dat de apparaatobjecten van uw zijn geschreven terug naar uw Active Directory met LDP.exe of ADSI bewerken. Beide zijn beschikbaar in de Active Directory-beheerder-hulpmiddelen.

Standaard wordt in hetzelfde domein als uw AD FS-farm apparaatobjecten die geschreven terug van Azure Active Directory zijn worden geplaatst.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Maak een beleid van toepassing toegang en aangepaste toegang geweigerd
De volgende scenario: U een Relying partijen vertrouwen in AD FS-toepassing maken en configureren van een regel voor het uitgifte autorisatie waarmee alleen geregistreerde apparaten. Nu mogen alleen apparaten die zijn geregistreerd voor toegang tot de toepassing. Zodat deze gemakkelijk voor uw gebruikers toegang te krijgen tot de toepassing, moet u een aangepaste toegang geweigerd bericht bevat instructies voor het deelnemen aan hun apparaat configureren. Uw gebruikers beschikken nu over een naadloze manier hun apparaten registreren voor toegang tot een toepassing.

De volgende stappen wordt uitgelegd u hoe u dit scenario implementeert.

>[AZURE.NOTE]
In deze sectie wordt ervan uitgegaan dat u al hebt geconfigureerd een te vertrouwen partijen vertrouwen voor uw toepassing in AD FS.

1. Open het hulpmiddel AD FS MMC en navigeer naar de AD FS > vertrouwensrelaties > partijen vertrouwensrelaties te vertrouwen.
2. Zoek de toepassing waarvan deze nieuwe access-regel van toepassing. Met de rechtermuisknop op de toepassing en selecteer claimen regels bewerken...
3. Selecteer het tabblad **Uitgiftebevoegdheidsregels** en selecteer vervolgens **Regel toevoegen**
4. In de **regel claimen** sjabloon vervolgkeuzelijst, selecteert u **toestaan of weigeren gebruikers op basis van een binnenkomende claimen**. Selecteer **volgende**.
5. In de Claim Regelnaam: veldtype: **toegang vanaf geregistreerde apparaten toestaan**
6. Van de binnenkomende claimen type: vervolgkeuzelijst, selecteert u **Is geregistreerd gebruiker**.
7. In de binnenkomende claimen waarde: veld: **waar**
8. Selecteer het keuzerondje **toegang voor gebruikers met deze binnenkomende claim toestaan** .
9. Selecteer **Voltooien** en selecteer **toepassen**.
10. Verwijder alle regels die zijn meer strikte dan de regel die u zojuist hebt gemaakt. Bijvoorbeeld de standaard **Toestaan dat toegang tot alle gebruikers** regel verwijderen.

Uw toepassing is nu geconfigureerd voor toegang tot toestaan alleen wanneer de gebruiker is die afkomstig zijn uit een apparaat dat ze geregistreerd en deel uitmaakt van het bedrijf. Voor meer geavanceerde-beleid, raadpleegt u [Risico's beheren met toegangsbeheer meervoudige](https://technet.microsoft.com/library/dn280949.aspx).

Vervolgens wordt u een aangepast foutbericht configureren voor uw toepassing. Het foutbericht wordt gebruikers wilt laten weten dat ze hun apparaat aan de werkplek deelneemt moeten voordat ze toegang hebben tot de toepassing. U kunt een maatwerktoepassing toegang geweigerd met aangepaste HTML en Windows PowerShell maken.

Open een opdrachtvenster Windows PowerShell en typt u de volgende opdracht uit op de federatieserver. Gedeelten van de opdracht vervangen door items die specifiek voor uw systeem zijn:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Voordat u deze toepassing kunt openen, moet u uw apparaat registreren.

**Als u een iOS-apparaat, selecteert u deze koppeling deel te nemen aan uw apparaat**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Dit iOS-apparaat te koppelen aan uw bedrijf.


**Als u een apparaat met Windows 8.1 gebruikt**, u kunt deelnemen aan uw apparaat door te gaan naar **PC-instellingen ** >  **netwerk** > **bedrijf**.


Waarbij '**te vertrouwen partijen vertrouwen naam**' is de naam van uw toepassingen te vertrouwen partijen vertrouwen object in AD FS.
Waarbij **yourdomain.com** is de naam van het domein dat u hebt geconfigureerd met Azure Active Directory. Bijvoorbeeld contoso.com.
Zorg ervoor dat eventuele regeleinden (indien aanwezig) verwijderen uit de html-inhoud die u voor de cmdlet **Set-AdfsRelyingPartyWebContent** doorgeven.


Als gebruikers toegang uw toepassing vanaf een apparaat die niet is geregistreerd bij de Azure Active Directory apparaat registratie-Service tot, krijgen ze nu een pagina die op de schermafbeelding die u lijkt hieronder.

![Screeshot van een fout wanneer gebruikers hun apparaat dat nog niet hebt geregistreerd met Azure AD](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
