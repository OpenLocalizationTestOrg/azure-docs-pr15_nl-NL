<properties
    pageTitle="Een nieuwe apparaat met Azure AD tijdens de installatie instellen | Microsoft Azure"
    description="Een onderwerp waarin wordt uitgelegd hoe gebruikers kunnen instellen Azure AD deelnemen aan tijdens de eerste uitvoeren ervaring."
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

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Een nieuwe apparaat met Azure AD tijdens de installatie instellen

Klik in Windows 10 kunnen gebruikers lid zijn of haar apparaten met Azure Active Directory (Azure AD) in de eerste sessie (FRX). Hiermee kan organisaties krimp apparaten om over hun werknemers of leerlingen/studenten te distribueren of laten kiezen hun eigen apparaten (CYOD).
Als edities van Windows 10 Professional of Windows 10 Enterprise op een apparaat is geïnstalleerd, wordt de ervaring standaard de installatieprocedure voor apparaten eigendom bij het bedrijf.

## <a name="to-join-a-device-to-azure-ad"></a>Om een apparaat te koppelen aan Azure AD


1. Wanneer u het nieuwe apparaat inschakelen en de installatieprocedure start, ziet u het bericht **Klaar ophalen** . Volg de aanwijzingen voor het instellen van uw apparaat.
2. Begin met het aanpassen van uw land en taal. Accepteer de licentievoorwaarden voor Microsoft-Software.
![Voor uw regio aanpassen](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Selecteer het netwerk dat u gebruiken wilt om verbinding te maken met Internet.
4. Selecteer of u een persoonlijk apparaat of een apparaat eigendom bij het bedrijf gebruikt. Als het eigendom bij het bedrijf, klikt u op **Dit apparaat mijn organisatie behoort**. Hiermee start u de deelnemen aan Azure AD-ervaring. Hier volgt een scherm dat u ziet als u Windows 10 Professional gebruikt.
<center>
![Eigenaar van dit scherm PC](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  Voer de referenties die aan u zijn verstrekt door uw organisatie.
<center>
![Aanmeldingsscherm](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  Nadat u uw gebruikersnaam in te voeren hebt ingevoerd, wordt een overeenkomende tenant bevindt zich in Azure AD. Als u zich in een gefedereerd domein, wordt u omgeleid naar uw on-premises implementatie Secure Token Service (STS) server--bijvoorbeeld Active Directory Federation Services (AD FS).
7. Als u een gebruiker in een niet-federatief domein bent, voert u uw referenties rechtstreeks op de Azure AD gehoste pagina. Als bedrijf huisstijl is geconfigureerd, wordt u ook het logo van uw organisatie zien en tekst ondersteunen.
8.  U wordt gevraagd voor een uitdaging meervoudige verificatie. Dit probleem kan worden geconfigureerd door een IT-beheerder.
9.  Azure AD Hiermee wordt gecontroleerd of deze gebruiker/apparaat is vereist voor registratie in mobiele apparaten beheren.
10. Windows registreert van het apparaat in de adreslijst van de organisatie in Azure AD en schrijft u deze zich in de mobiele apparaten beheren, indien nodig.
11. Als u een beheerde gebruiker bent, Windows Hiermee gaat u naar het bureaublad in het proces voor automatische aanmelding.
12. Als u een federatieve gebruiker bent, u, worden doorgestuurd naar het Windows-aanmeldingsscherm uw referenties invoeren.

> [AZURE.NOTE] Deelnemen aan een on-premises Windows Server Active Directory-domein in de modus van Windows kant-en-klare wordt niet ondersteund. Daarom als u van plan bent om een computer te koppelen aan een domein, moet u de koppeling **Windows met een lokale account instellen** in plaats daarvan. U kunt vervolgens deelnemen aan het domein van de instellingen op uw computer als u eerder hebt uitgevoerd.

## <a name="additional-information"></a>Aanvullende informatie
* [Windows 10 voor de onderneming: manieren om apparaten voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Identiteiten zonder wachtwoorden via Microsoft Passport verifiëren](active-directory-azureadjoin-passport.md)
* [Meer informatie over het gebruik scenario's voor het deelnemen aan Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Deelnemen aan Azure AD instellen](active-directory-azureadjoin-setup.md)
