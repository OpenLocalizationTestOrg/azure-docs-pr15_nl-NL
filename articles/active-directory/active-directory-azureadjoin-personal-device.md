

<properties
    pageTitle="Deelnemen aan een persoonlijk apparaat naar uw organisatie | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe gebruikers hun persoonlijke Windows 10-apparaten met hun bedrijfsnetwerk kunnen registreren en implementatiestappen bevat voor een scenario BYOD."
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

# <a name="join-a-personal-device-to-your-organization"></a>Deelnemen aan een persoonlijk apparaat naar uw organisatie

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Om een Windows-10-apparaat te koppelen aan uw organisatie

1.  Selecteer **Instellingen**in het menu **Start** .
2.  Selecteer **Accounts**en klik vervolgens op **uw account**.
3.  Klik op **toevoegen werk- of schoolaccount**en typ vervolgens in uw organisatie-account.
4.  Klik op de aanmeldingspagina voor uw organisatie, Voer uw gebruikersnaam en wachtwoord en klik vervolgens op **OK**.
5.  U wordt gevraagd voor een uitdaging meervoudige verificatie. (Deze uitdaging kan worden geconfigureerd door een IT-beheerder.)
6.  Azure Active Directory (Azure AD) Hiermee wordt gecontroleerd of het apparaat mobiele apparaat management registratie vereist.
7.  Windows registreert van het apparaat in de adreslijst van de organisatie in Azure AD en schrijft u deze zich in de mobiele apparaten beheren, indien nodig.
8.  Als u een beheerde gebruiker bent, gaat Windows u naar het bureaublad tot en met de automatische aanmelden.
9.  Als u een federatieve gebruiker, wordt u op een Windows-aanmelding scherm worden gehouden met voert u uw referenties.

## <a name="additional-information"></a>Aanvullende informatie
* [Windows 10 voor de onderneming: manieren om apparaten voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Identiteiten zonder wachtwoorden via Microsoft Passport verifiëren](active-directory-azureadjoin-passport.md)
* [Meer informatie over het gebruik scenario's voor het deelnemen aan Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Deelnemen aan Azure AD instellen](active-directory-azureadjoin-setup.md)
