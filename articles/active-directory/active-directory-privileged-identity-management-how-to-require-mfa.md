<properties
   pageTitle="Hoe u meervoudige verificatie vereisen | Microsoft Azure"
   description="Leer hoe u meervoudige verificatie (MFA) vereisen voor bevoegdheden identiteiten met de extensie Azure Active Directory bevoegdheden identiteitsbeheer."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Hoe u MFA Azure AD bevoegdheden identiteitsbeheer vereisen

Het is raadzaam dat u meervoudige verificatie (MFA) voor al uw beheerders vereist. Hierdoor wordt de kans op pesterijen een aanval vanwege een beschadigde wachtwoord.

U kunt vereisen dat gebruikers een uitdaging MFA voltooien wanneer ze zich aanmeldt. Het blogbericht [MFA voor Office 365 en MFA voor Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) worden vergeleken wat is inbegrepen in abonnementen voor Office en Azure, met de onderdelen van de Microsoft Azure meervoudige verificatie aanbod.

U kunt ook vereisen dat gebruikers een uitdaging MFA voltooien wanneer ze een rol in Azure AD PIM activeren. Op deze manier als de gebruiker kan een uitdaging MFA niet hebt voltooid wanneer ze zich aangemeld, wordt ze gevraagd dat te doen door PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>MFA Azure AD-rechten identiteitsbeheer vereisen

Wanneer u identiteiten in PIM als de beheerder van een bevoegdheden rol beheert, ziet u mogelijk waarschuwingen die MFA voor bevoegdheden accounts aanraden. Klik op de Beveiligingsmelding van in het dashboard PIM en een nieuwe blade wordt geopend met een lijst met administrator-accounts waarvoor MFA vereist.  U kunt opgeven dat MFA moeten door meerdere rollen selecteren en vervolgens op de knop **herstellen** , of u kunt op de drie puntjes naast afzonderlijke rollen en klik op de knop **herstellen** .

> [AZURE.IMPORTANT] Rechts voorlopig Azure MFA alleen werkt met werk of schoolaccounts: geen Microsoft-accounts (meestal een persoonlijk account die wordt gebruikt voor het aanmelden bij Microsoft-services zoals Skype, Xbox, Outlook.com, enz.). Reden kan iedereen met een Microsoft-account in aanmerking komend beheerder niet, omdat ze niet MFA gebruiken om te activeren van hun rol. Als deze gebruikers gaan met het beheren van werkbelasting met een Microsoft-account wilt, worden omgezet ze in permanente beheerders voorlopig.

Bovendien kunt u de vereiste MFA voor een specifieke rol wijzigen door erop te klikken in de sectie van het dashboard PIM. Klik vervolgens op **Instellingen** in het blad rol en klik vervolgens onder meervoudige verificatie selecteren **inschakelen** .

## <a name="how-azure-ad-pim-validates-mfa"></a>Hoe MFA is gevalideerd met Azure AD PIM

Er zijn twee opties voor het valideren van MFA wanneer een gebruiker een rol activeren.

De eenvoudigste optie is afhankelijk van Azure MFA voor gebruikers die een bevoegdheden rol wordt geactiveerd. Klik hiertoe Controleer eerst of die gebruikers zijn licentie, indien nodig, en hebt geregistreerd voor Azure MFA. Meer informatie over hoe u dit doet, is in [aan de slag met Azure meervoudige verificatie in de cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Dit is aanbevolen, maar niet verplicht, Azure AD om af te dwingen MFA voor deze gebruikers wanneer ze zich aanmelden te configureren. Dit komt omdat de controles MFA door Azure AD PIM zelf komen.

Als gebruikers worden geverifieerd on-premises implementatie kunt u ook hebt uw identiteitsprovider verantwoordelijk voor MFA. Als u AD hebt geconfigureerd bevat Federation Services smartcard gebaseerde verificatie voor de toegang tot Azure AD, [beveiligen cloud resources met Azure meervoudige verificatie en AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) vereisen bijvoorbeeld instructies voor het configureren van AD FS voor het verzenden van claims naar Azure AD. Wanneer een gebruiker probeert te activeren van een rol, accepteert Azure AD PIM dat MFA is al gevalideerd voor de gebruiker zodra deze de juiste claims ontvangt.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
