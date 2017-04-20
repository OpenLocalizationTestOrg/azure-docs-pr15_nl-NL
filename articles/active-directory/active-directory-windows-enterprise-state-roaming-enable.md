<properties
    pageTitle="Inschakelen Enterprise staat Roaming in Azure Active Directory | Microsoft Azure"
    description="Veelgestelde vragen over Enterprise staat Roaming instellingen in Windows-apparaten. Enterprise staat Roaming gebruikers met een geïntegreerde ervaring biedt via de Windows-apparaten en Hiermee reduceert u de tijd die nodig is voor het configureren van een nieuwe apparaat."
    services="active-directory"
    keywords="Enterprise staat roaming, windows cloud, het inschakelen van onderneming staat roaming"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Inschakelen Enterprise staat Roaming in Azure Active Directory

Enterprise staat Roaming is beschikbaar voor een organisatie met een abonnement Premium Azure Active Directory (Azure AD). Zie voor meer informatie over hoe u een Azure AD-abonnement, de [productpagina Azure AD](https://azure.microsoft.com/services/active-directory).

Wanneer u de Enterprise-staat Roaming inschakelt, worden uw organisatie automatisch toegewezen licenties voor een abonnement gratis, beperkt gebruik Azure Rights Management. Dit gratis abonnement is beperkt tot versleutelen en ontsleutelen enterprise-instellingen en toepassingsgegevens die zijn gesynchroniseerd door de service Enterprise staat Roaming; u moet een betaald abonnement gebruik van de volledige functionaliteit van Azure Rights Management.

Volg deze stappen om het inschakelen van onderneming staat Roaming nadat een Premium Azure AD-abonnement:

1. Meld u aan bij van de Azure klassieke portal.
2. Aan de linkerkant, selecteer **ACTIVE DIRECTORY**en selecteer vervolgens de map waarvoor u wilt inschakelen Enterprise staat Roaming.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Ga naar het tabblad **configureren** op de bovenkant.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  Schuif omlaag op de pagina en selecteer **gebruikers mogelijk SYNCHRONISEREN instellingen en gegevens van de ENTERPRISE-APP**en klik vervolgens op **Opslaan**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Voor een Windows 10-apparaat in instellingen met de service Enterprise staat Roaming staat, moet het apparaat met een Azure AD-identiteit verifiëren. Voor apparaten die zijn gekoppeld aan Azure AD, is de primaire gebruikersaanmelding de Azure AD-identiteit, zodat er geen aanvullende configuratie vereist is. Voor apparaten met een traditionele lokale Active Directory, moet de IT-beheerder [verbindt u de apparaten die een domein zijn gekoppeld aan Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Synchronisatie-gegevensopslag
Enterprise staat Roaming gegevens wordt gehost in een of meer [Azure regio's](https://azure.microsoft.com/regions/ ) die best overeenstemt met de land/regio-waarde instellen in het exemplaar van de Azure Active Directory. Gegevens Enterprise staat Roaming is partities op basis van de drie belangrijkste geografische regio's: Noord-Amerika, EMEA en APAC. Gegevens voor de tenant Enterprise staat Roaming lokaal bevindt met de geografische regio en niet worden gerepliceerd naar regio's.  Bijvoorbeeld klanten die hun land/regio-waarde ingesteld op een van de EMEA-landen, zoals "Frankrijk" of "Zambia" hun gegevens die worden gehost heeft in één of van de Azure regio's in Europa hebben.  Klanten die hun land/regio-waarde in Azure AD naar een van Noord-Amerika landen, instellen zoals "Verenigde Staten" of "Canada" wordt hun gegevens die worden gehost op een of meer van de Azure regio's binnen de Verenigde Staten hebben.  Klanten die hun land/regio-waarde in Azure AD naar een van de APAC landen instellen zoals "Australië" of "Nieuw-Zeeland" hun gegevens die worden gehost op een of meer van de Azure gebieden binnen Azië heeft.  Zuid-Amerikaanse landen en Antarctica gegevens wordt in een of meer Azure regio's binnen de Verenigde Staten worden gehost.  De waarde van het land/regio is ingesteld als onderdeel van de Azure AD-proces voor het maken van directory en vervolgens kan niet worden gewijzigd. 

Als u meer informatie over de opslaglocatie van gegevens, Rapporteer u een tickets met [Azure ondersteunen](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Enterprise staat Roaming beheren
Azure AD globale beheerders kunnen en Enterprise staat Roaming uitschakelen in de portal van Azure klassieke.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globale beheerders kunnen beperken instellingen synchroniseren met specifieke beveiligingsgroepen.

Globale beheerders kunnen ook een statusrapport per gebruiker apparaat synchroniseren weergeven door een bepaalde gebruiker in de lijst met Active Directory instantie **gebruikers** selecteren en te klikken op het tabblad **apparaten** en **apparaten worden gesynchroniseerd instellingen en gegevens van de app enterprise**-upweergave selecteren.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>Gegevensretentie
Gegevens die zijn gesynchroniseerd met Azure via Enterprise staat Roaming bewaard voor onbepaalde tijd tenzij een handmatige verwijderbewerking is uitgevoerd of de betreffende gegevens wordt bepaald moeten verlopen. 

**Expliciete verwijdering:** De gegevens verwijderd wanneer een Azure beheerder Hiermee verwijdert u een gebruiker of een map of een beheerder expliciet aanvraagt dat er gegevens worden verwijderd.

- **Het verwijderen van gebruiker**: wanneer een gebruiker in Azure AD wordt verwijderd, het gebruikersaccount roaming gegevens worden gemarkeerd voor het verwijderen en tussen 90 tot en met 180 dagen worden verwijderd. 
- **Directory verwijdering**: een hele map verwijderen in Azure AD is een onmiddellijke bewerking. Alle instellingsgegevens die zijn gekoppeld met die map worden gemarkeerd voor het verwijderen en tussen 90 tot en met 180 dagen worden verwijderd. 
- **Klik op verzoek verwijdering**: als de beheerder Azure AD wil handmatig verwijderen van een specifieke gebruiker gegevens of instellingsgegevens, de beheerder een tickets met [Azure ondersteunen](https://azure.microsoft.com/support/)kunt indienen. 

**Verouderde gegevensverwijdering**: gegevens die niet is geopend voor één jaar ("de bewaarperiode"), worden behandeld als verlopen en kan worden verwijderd uit Azure. De bewaarperiode kan worden gewijzigd, maar worden niet minder dan 90 dagen. De verouderde gegevens mogelijk een specifieke reeks Windows/toepassingsinstellingen of alle instellingen voor een gebruiker. Bijvoorbeeld:
 
- Als er geen apparaten toegang hebt tot een bepaalde instellingen voor siteverzameling (bijvoorbeeld een toepassing van het apparaat is verwijderd of een groep instellingen zoals 'Thema' is uitgeschakeld voor alle apparaten van een gebruiker), en vervolgens die verzameling verouderde na de bewaarperiode worden en kan worden verwijderd. 
- Als een gebruiker heeft uitgeschakeld in instellingen synchroniseren op al zijn of haar apparaten, klikt u vervolgens geen van de instellingsgegevens worden gebruikt en alle instellingsgegevens voor die gebruiker worden verlopen en kan worden verwijderd na de bewaarperiode. 
- Als de beheerder van de map Azure AD uitgeschakeld Enterprise staat Roaming voor de hele adreslijst, worden alle gebruikers worden in die map worden niet meer gesynchroniseerd van instellingen en alle instellingsgegevens voor alle gebruikers op opmerkingen worden verlopen en kan worden verwijderd na de bewaarperiode. 

**Verwijderde gegevens herstellen**: het bewaarbeleid voor gegevens kan niet worden geconfigureerd. Nadat de gegevens worden blijvend verwijderd, is deze niet worden hersteld. Het is echter Houd er rekening mee dat de instellingsgegevens alleen worden verwijderd uit Azure niet het apparaat van de eindgebruiker. Als u elk apparaat opnieuw verbinding later maakt met de Enterprise-staat-roamingservice, wordt de instellingen opnieuw gesynchroniseerd en opgeslagen in Azure wordt aangegeven.


## <a name="related-topics"></a>Verwante onderwerpen
- [Overzicht Enterprise staat Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
- [Instellingen en gegevens roaming Veelgestelde vragen](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Beleid en MDM groepsinstellingen voor instellingen synchroniseren](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows 10 roaming instellingen verwijzing](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
