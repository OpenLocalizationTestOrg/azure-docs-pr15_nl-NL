<properties
    pageTitle="Azure AD-rechten identiteitsbeheer | Microsoft Azure"
    description="Een onderwerp waarin wordt uitgelegd wat Azure AD bevoegdheden identiteitsbeheer is en hoe PIM gebruiken om uw cloud-beveiliging te verbeteren."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Azure AD-rechten identiteitsbeheer

Met bevoegdheden identiteitsbeheer Azure Active Directory (AD), kunt u beheren, wilt instellen en controleren van access binnen uw organisatie. Dit geldt ook voor toegang tot bronnen in Azure AD en andere Microsoft online services zoals Office 365 of Microsoft Intune.  

> [AZURE.NOTE] Bevoegdheden identiteitsbeheer is alleen beschikbaar met de Premium-P2-editie van Azure Active Directory. Zie [Azure Active Directory-edities](active-directory-editions.md)voor meer informatie.

Organisaties wilt minimaliseren van het aantal personen die toegang tot beveiligde gegevens of resources hebben, omdat die verkleint u de kans van een kwaadwillende gebruiker die toegang aan. Echter moeten gebruikers nog steeds verrichten bevoegdheden in Azure, Office 365 of SaaS-apps. Organisaties geven bevoegdheden toegang van gebruikers in Azure AD zonder wat die gebruikers mee bezig zijn met hun beheerdersbevoegdheden voor controle. Azure AD rechten identiteitsbeheer kunt u dit risico oplossen.  

Azure AD rechten identiteitsbeheer kunt u:  

- Zie Azure AD-beheerders zijn
- Op aanvraag, "just in time" beheerderstoegang tot Microsoft Online Services, zoals Office 365 en Intune inschakelen
- Rapporten over de beheerder van access geschiedenis en wijzigingen in beheerder toewijzingen ophalen
- Meldingen over de toegang tot een bevoegdheden rol ontvangen

Azure AD bevoegdheden identiteitsbeheer kunt beheren de ingebouwde Azure AD organisatorische rollen, waaronder:  

- Globale beheerder
- Factureringsbeheerder
- Service-beheerder  
- Gebruikersbeheerder
- Wachtwoordbeheerder

## <a name="just-in-time-administrator-access"></a>Alleen in tijd beheerderstoegang

In het verleden, kunt u een gebruiker toewijzen aan een beheerdersrol via de portal van Azure klassieke of Windows PowerShell. Hierdoor wordt die gebruiker een **permanente beheerder**, in de toegewezen rol altijd actief. Azure AD rechten identiteitsbeheer maakt u kennis met het concept van een **in aanmerking komend beheerder**. In aanmerking komend beheerders moeten gebruikers die bevoegdheden toegang nodig hebben nu en dan, maar niet elke dag. De rol is niet actief totdat de gebruiker access, moet en ze een activering te voltooien en maakt u een actieve beheerder voor een vooraf ingestelde tijdsduur.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Bevoegdheden identiteitsbeheer inschakelen voor uw adreslijst

U kunt starten met behulp van Azure AD bevoegdheden identiteitsbeheer in de [portal van Azure](https://portal.azure.com/).

>[AZURE.NOTE] U moet een globale beheerder met een organisatie-account (bijvoorbeeld @yourdomain.com), niet in een Microsoft-account (bijvoorbeeld @outlook.com), om in te schakelen Azure AD bevoegdheden Identiteitsbeheer voor een map.

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/) als een globale beheerder van de map.
2. Als uw organisatie meer dan één map heeft, selecteert u uw gebruikersnaam in de rechterbovenhoek van de Azure-portal. Selecteer de map waar u Azure AD bevoegdheden identiteitsbeheer gebruikt.
3. Selecteer **meer services** en het tekstvak Filter gebruiken om te zoeken naar **Azure AD bevoegdheden identiteitsbeheer**.
4. **Vastmaken aan dashboard** controleren en klik vervolgens op **maken**. De toepassing bevoegdheden identiteitsbeheer wordt geopend.

Als u de eerste persoon Azure AD bevoegdheden identiteitsbeheer gebruiken in uw adreslijst, klikt u vervolgens begeleidt de [beveiligingswizard](active-directory-privileged-identity-management-security-wizard.md) u bij de eerste toewijzing-ervaring. Na die worden u automatisch de eerste **Beveiligingsbeheerder** en de **beheerder van de bevoegdheden van de rol** van de map.

Alleen de beheerder van een bevoegdheden rol kunt toegang voor andere beheerders beheren. U kunt [andere gebruikers de mogelijkheid om te beheren in PIM geven](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Bevoegdheden identiteitsbeheer dashboard

Azure AD rechten identiteitsbeheer biedt een dashboard waarmee u belangrijke informatie, zoals:

- Waarschuwingen die verwijzen naar gelegenheden beveiliging te verbeteren
- Het aantal gebruikers die zijn toegewezen aan elke bevoegdheden rol  
- Het aantal in aanmerking komend en permanente beheerders
- Doorlopende toegang reviseert

![PIM dashboard - schermafbeelding][2]

## <a name="privileged-role-management"></a>Bevoegdheden rollenbeheer

Met Azure AD bevoegdheden identiteitsbeheer, kunt u de beheerders beheren door toevoegen of verwijderen van permanente of in aanmerking komend beheerders aan elke rol.

![Beheerders van PIM toevoegen/verwijderen - schermafbeelding][3]

## <a name="configure-the-role-activation-settings"></a>De rol activering-instellingen configureren

Met de [rolinstellingen](active-directory-privileged-identity-management-how-to-change-default-settings.md) kunt u de eigenschappen van het in aanmerking komend rol-activering inclusief configureren:

- De duur van de rol activering termijn
- De rol activering-melding
- De informatie die een gebruiker moet op te geven tijdens het activeringsproces rol  

![Schermafbeelding van PIM - beheerder activering - instellingen][4]

Houd er rekening mee dat in de afbeelding, de knoppen voor **Meervoudige verificatie** zijn uitgeschakeld. Zeer, voor bepaalde rollen, bevoegdheden we MFA vereisen voor betere beveiliging.

## <a name="role-activation"></a>Rol activering  

Naar het [activeren van een rol](active-directory-privileged-identity-management-how-to-activate-role.md)vraagt een in aanmerking komend beheerder een tijd-gebonden "activering" voor de rol. De activering kan worden aangevraagd met de optie **activeren mijn rol** in Azure AD bevoegdheden identiteitsbeheer.

Een beheerder die wil activeren van een rol moet geïnitialiseerd Azure AD bevoegdheden identiteitsbeheer in de portal van Azure.

Activering van de rol kan worden aangepast. In de instellingen PIM, kunt u de lengte van de activering en wat bepalen informatie de beheerder opgeven moet om het activeren van de rol.

![PIM beheerder verzoek rol activering - schermafbeelding][5]

## <a name="review-role-activity"></a>Rol activiteit controleren

Er zijn twee manieren om te controleren hoe uw werknemers en beheerders bevoegdheden rollen gebruikt. De eerste optie ook via het [controlerapport](active-directory-privileged-identity-management-how-to-use-audit-log.md). De controlegeschiedenis logboeken op wijzigingen bijhouden in bevoegdheden roltoewijzingen en rol activering geschiedenis.

![PIM activering geschiedenis - schermafbeelding][6]

De tweede optie is voor het instellen van de normale [access reviseert](active-directory-privileged-identity-management-how-to-start-security-review.md). Deze beoordelingen toegang kunnen worden uitgevoerd door en toegewezen revisor (zoals de teammanager van een) of de werknemers zelf kunnen bekijken. Dit is de beste manier om te controleren die nog steeds toegang is vereist en wie niet meer doet.


## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
