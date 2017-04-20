<properties
    pageTitle="# Automatische apparaatregistratie voor apparaten met Windows 7-domein gekoppeld configureren | Microsoft Azure"
    description="Stappen voor het configureren van uw domein met Windows 7 gekoppeld apparaten automatisch met Azure AD kunnen registreren. en stappen voor het implementeren van het apparaat registratie software-pakket naar uw domein met Windows 7-apparaten met behulp van een verdeling systeem voor software zoals System Center Configuration Manager die zijn gekoppeld."
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
    ms.date="09/21/2016"
    ms.author="MarkVi"/>

# <a name="configure-automatic-device-registration-for-windows-7-domain-joined-devices"></a>Automatische apparaatregistratie voor apparaten met Windows 7-domein gekoppeld configureren

U kunt uw apparaten voor het domein toegevoegd van Windows 7 automatisch registreren met Azure AD configureren als IT-beheerder. Als u wilt doen, moet u het apparaat registratie software-pakket installeren op uw Windows 7-domein gekoppeld apparaten met behulp van een verdeling systeem voor software zoals System Center Configuration Manager. Zorg ervoor dat u via lezen en voert u de vereisten die wordt weergegeven in de automatische apparaatregistratie met apparaten die Azure Active Directory voor Windows-domein zijn gekoppeld.

>[AZURE.NOTE]
 Meest recente Zie voor instructies voor het instellen van automatische apparaatregistratie, [Automatische registratie van Windows-domein instellen die zijn gekoppeld apparaten met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

##<a name="installing-the-device-registration-software-package-on-windows-7-domain-joined-devices"></a>Het pakket apparaat registratie software installeren op Windows 7 samengevoegd domein apparaten

Apparaatregistratie voor Windows 7 is beschikbaar als een [downloadbare MSI-pakket](https://connect.microsoft.com/site1164). Het pakket moet worden geïnstalleerd op Windows 7-machines die zijn gekoppeld aan een Active Directory-domein. Het gebruik van een verdeling systeem voor software zoals System Center Configuration Manager-pakket, moet u distribueren. Het MSI-pakket ondersteunt de stille standaardinstallatie-opties met de/quiet parameter.
Het softwarepakket is gedownload van de [verbinding maken met Microsoft-website](https://connect.microsoft.com/site1164). Hier kunt u selecteren en vervolgens downloaden werkplek deelnemen aan voor Windows 7.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## <a name="workplace-join-with-azure-active-directory"></a>Deelnemen aan de werkplek met Azure Active Directory
Apparaatregistratie voor apparaten met Windows 7 domein toegevoegd niet vereisen of een gebruikersinterface opnemen. Nadat u op de computer zijn geïnstalleerd, wordt dat domein-gebruikers die zich in de computer automatisch en stilte geregistreerd met een apparaatobject in Azure AD. Er is één apparaatobject in Azure AD voor elke geregistreerde gebruiker van het apparaat.

Het installatieprogramma maakt u een taak gepland op het systeem dat wordt uitgevoerd in de context van de gebruiker en klik op aanmelding wordt geactiveerd. De taak registreert stilte de gebruiker en apparaat instellen met Azure AD nadat de gebruiker positief of negatief-in is voltooid.
De geplande taak vindt u in de bibliotheek voor Taakplanner onder **Microsoft** > **Bedrijf deelnemen aan**.
De taak wordt uitgevoerd en alle Active Directory-gebruikers die bij de aanmelding bij de computer registreren.
De volgende afbeelding bevat een stapsgewijze procedure voor automatische apparaatregistratie.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. Een gebruiker (IT-medewerker) zich aanmeldt bij een clientcomputer voor Windows 7 met referenties van Active Directory-domein.
1. De werkplek deelnemen aan geplande taak wordt uitgevoerd.
1. De gebruiker is stilte met AD FS met Windows-verificatie geverifieerd.
1. De PC met Windows 7 is geregistreerd voor de gebruiker in Azure AD.
1. Een apparaatobject en een certificaat is gemaakt in Azure AD. Hiermee geeft u het object het user@device.
1. Het certificaat werkplek deelnemen aan is opgeslagen op de computer.

## <a name="unregistering-your-windows-7-domain-joined-devices"></a>Afmelden van uw domein met Windows 7 apparaten die zijn gekoppeld

U kunt uw domein toegevoegd Windows 7-apparaten unregister door het volgende te doen: verwijdert de werkplek deelnemen aan softwarepakket vanaf uw Windows 7-domein die zijn gekoppeld apparaten met behulp van een verdeling systeem voor software zoals System Center Configuration Manager.

Vervolgens open een opdrachtprompt op de computer met Windows 7 en voer de volgende opdracht registratie van het apparaat:

    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>Deze opdracht moet worden uitgevoerd in de context van elke domeingebruiker handtekening in de computer.
Logboeken en fouten voor Windows 7 domein gekoppeld apparaten.

Het gebeurtenissenlogboek van Windows op de computer met Windows 7, worden berichten met betrekking tot het bedrijf deelnemen aan weergegeven. U kunt berichten vinden voor geslaagde en mislukte werkplek deelnemen aan gebeurtenissen. Het gebeurtenissenlogboek van vindt u in het logboek Viewer onder logboeken toepassingen en Services > Microsoft-werkplek deelnemen aan.

## <a name="additional-topics"></a>Aanvullende onderwerpen

- [Azure Active Directory-apparaatregistratie-overzicht](active-directory-conditional-access-device-registration-overview.md)
- [Automatische apparaatregistratie met Azure Active Directory for Windows Domain-Joined-apparaten](active-directory-conditional-access-automatic-device-registration.md)
- [Automatische apparaatregistratie voor apparaten met Windows 8.1 domein toegevoegd configureren](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automatische apparaatregistratie met een domein behoren Azure Active Directory voor Windows 10-apparaten](active-directory-azureadjoin-devices-group-policy.md)
