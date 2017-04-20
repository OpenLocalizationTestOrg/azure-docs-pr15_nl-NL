<properties
    pageTitle="Probleemoplossing voor Azure Active Directory-toegangsproblemen | Microsoft Azure"
    description="Lees de stappen beschreven die u uitvoeren kunt voor het oplossen van problemen met online-informatiebronnen van uw organisatie."
    services="active-directory"
    keywords="voorwaardelijke toegang op basis van het apparaat, apparaatregistratie, apparaatregistratie, apparaatregistratie en MDM inschakelen"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Probleemoplossing voor toegangsproblemen Azure Active Directory

U probeert voor toegang tot SharePoint Online intranet van uw organisatie en krijgt u een foutbericht 'toegang geweigerd' wordt weergegeven. Wat voor werk doe je?

In dit artikel behandelt remediation stappen waarmee u oplossen van problemen van access met online-informatiebronnen van uw organisatie kunt.

Toegang tot problemen voor hulp bij het oplossen van Azure Active Directory (Azure AD), gaat u naar de sectie in het artikel die betrekking heeft op uw apparaatplatform:

-   Windows-apparaat
-   iOS-apparaat (controleren terug binnenkort voor hulp bij iPhones en iPads.)
-   Android-apparaat (Kijk hier binnenkort voor hulp met Android-telefoons en tablets.)

## <a name="access-from-a-windows-device"></a>Toegang vanaf een Windows-apparaat

Als uw apparaat wordt uitgevoerd een van de volgende platforms, kijkt u in de volgende secties voor het foutbericht dat wordt weergegeven wanneer u probeert voor toegang tot een toepassing of service:

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Apparaat is niet geregistreerd

Als uw apparaat niet is geregistreerd bij Azure AD en de toepassing is beveiligd met een beleid op basis van het apparaat, ziet u mogelijk een pagina waarop u een van deze foutberichten:

!["U kunt geen gekomen vanaf hier" berichten voor niet-geregistreerde apparaten] (./media/active-directory-conditional-access-device-remediation/01.png "Scenario")

Als uw apparaat een domein behoren met Active Directory in uw organisatie is, probeer dit:

1.  Zorg ervoor dat u zich aanmeldt bij Windows met behulp van uw werkaccount (uw Active Directory-account).
2.  Verbinding maken met uw bedrijfsnetwerk via een VPN (VPN) of DirectAccess.
3.  Nadat u verbonden bent, drukt u op de Windows-logotoets + de toets L vergrendelen van uw Windows-sessie.
4.  Voer de referenties van uw werk-account als u wilt ontgrendelen van uw Windows-sessie.
5.  Wacht totdat minuten en probeer het opnieuw voor toegang tot de toepassing of service.
6.  Als u dezelfde pagina ziet, klikt u op de koppeling **meer details** en klikt u vervolgens contact op met uw beheerder met de details.

Als uw apparaat niet is voorzien van een domein behoren en wordt uitgevoerd van Windows 10, hebt u twee opties:

- Azure AD-Join uitvoeren
- Uw werk of school-account toevoegen in Windows

Zie voor informatie over hoe deze opties verschillende zijn, [gebruik van Windows 10-apparaten in uw bedrijf](active-directory-azureadjoin-windows10-devices.md).

Deelnemen aan Azure AD, voert u de volgende stappen voor het platform dat op uw apparaat wordt uitgevoerd. (Azure AD-Join is niet beschikbaar op Windows-telefoons.)

**Update voor Windows 10 jubileum**

1.  Open de app- **Instellingen** .
2.  Klik op **Accounts** > **Access werk of school**.
3.  Klik op **verbinding maken**.
4.  Klik op **Dit apparaat Azure AD deelnemen**.
5.  Uw organisatie wordt geverifieerd, meervoudige verificatie bevatten als u hierom wordt gevraagd en volg de stappen die worden weergegeven.
6.  Meld u af en meld u aan met uw werkaccount.
7.  Probeer het opnieuw voor toegang tot de toepassing.


**Update voor Windows 10 November 2015**

1.  Open de app- **Instellingen** .
2.  Klik op **systeem** > **over**.
3.  Klik op **deelnemen aan Azure AD**.
4.  Uw organisatie wordt geverifieerd, meervoudige verificatie bevatten als u hierom wordt gevraagd en volg de stappen die worden weergegeven.
5.  Meld u af en meld u aan met uw werkaccount (uw Azure AD-account).
6.  Probeer het opnieuw voor toegang tot de toepassing.

Als u wilt toevoegen van uw account voor werk of school, volgt u de volgende stappen uit:

**Update voor Windows 10 jubileum**

1.  Open de app- **Instellingen** .
2.  Klik op **Accounts** > **Access werk of school**.
3.  Klik op **verbinding maken**.
4.  Uw organisatie wordt geverifieerd, meervoudige verificatie bevatten als u hierom wordt gevraagd en volg de stappen die worden weergegeven.
5.  Probeer het opnieuw voor toegang tot de toepassing.


**Update voor Windows 10 November 2015**

1.  Open de app- **Instellingen** .
2.  Klik op **Accounts** > **uw accounts**.
3.  Klik op **Voeg werk of school account**.
4.  Uw organisatie wordt geverifieerd, meervoudige verificatie bevatten als u hierom wordt gevraagd en volg de stappen die worden weergegeven.
5.  Probeer het opnieuw voor toegang tot de toepassing.

Als uw apparaat niet is voorzien van een domein behoren en wordt uitgevoerd van Windows 8.1, als u een bedrijf Join en registreren in Microsoft Intune, volgt u de volgende stappen uit:

1.  Open **PC-instellingen**.
2.  Klik op **netwerk** > **bedrijf**.
3.  Klik op **deelnemen**.
4.  Uw organisatie wordt geverifieerd, meervoudige verificatie bevatten als u hierom wordt gevraagd en volg de stappen die worden weergegeven.
5.  Klik op **inschakelen**.
6.  Probeer het opnieuw voor toegang tot de toepassing.


### <a name="browser-is-not-supported"></a>Browser niet wordt ondersteund

U kunt geen toegang als u probeert voor toegang tot een toepassing of service met een van de volgende browsers:

- Chrome, Firefox of een andere browser dan Microsoft Edge- of Microsoft Internet Explorer in Windows 10 of Windows Server 2016
- Firefox in Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2

Hier ziet u een foutpagina die er zo uitziet:

![Bericht "U kunt geen gekomen vanaf hier" voor niet-ondersteunde browsers] (./media/active-directory-conditional-access-device-remediation/02.png "Scenario")

De enige remediation is via een browser die de toepassing worden ondersteund voor uw apparaatplatform.

## <a name="next-steps"></a>Volgende stappen

[Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access.md)
