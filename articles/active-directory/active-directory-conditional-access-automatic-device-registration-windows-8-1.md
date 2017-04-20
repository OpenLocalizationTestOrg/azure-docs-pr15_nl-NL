<properties
    pageTitle="Automatische apparaatregistratie voor apparaten met Windows 8.1 domein toegevoegd configureren | Microsoft Azure"
    description=" Stappen voor het configureren van Groepsbeleid voor Windows 8.1 domein behoren apparaten automatisch met Azure AD kunnen registreren. "
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
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Automatische apparaatregistratie voor apparaten met Windows 8.1 domein toegevoegd configureren

U kunt een Active Directory-groepsbeleid Windows 8.1 domein toegevoegd apparaten automatisch registreren met Azure AD configureren. Als u wilt het Groepsbeleid configureren, moet u ten minste één domein gekoppelde Windows Server 2012 R2 of Windows 8.1 computer hebben met de functie Groepsbeleid beheren is geïnstalleerd. Als dit groepsbeleid is ingeschakeld voor uw domein, wordt dat domein-gebruikers die zich in de computer automatisch en stilte geregistreerd met een apparaatobject in Azure AD. Er is één apparaatobject in Azure AD voor elke geregistreerde gebruiker van het apparaat. Zorg ervoor dat u lees en de vereisten van de automatische apparaatregistratie met Azure Active Directory for Windows Domain-Joined apparaten te voltooien.

>[AZURE.NOTE]
 Meest recente Zie voor instructies voor het instellen van automatische apparaatregistratie, [Automatische registratie van Windows-domein instellen die zijn gekoppeld apparaten met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Het groepsbeleid voor uw domein toegevoegd apparaten met Windows 8.1 configureren

1. Open Serverbeheer en Ga naar **Extra** > **Groepsbeleid beheren**.
2. Ga naar het knooppunt van het domein dat overeenkomt met het domein waarin u wilt **Deelnemen aan automatische bedrijf**inschakelen uit Groepsbeleid beheren.
3. Met de rechtermuisknop op het **Beleid-objecten groeperen** en selecteer **Nieuw**. Geef het Groepsbeleid-object een naam, bijvoorbeeld **Automatische werkplek deelnemen**. Klik op **OK**.
4. Met de rechtermuisknop op het nieuwe Groepsbeleid-object en selecteer vervolgens **bewerken**.
5. Navigeer naar de **Computerconfiguratie** > **beleidsregels** > **administratieve sjablonen** > **Windows-onderdelen** > **bedrijf deelnemen aan**.
6. Met de rechtermuisknop op automatisch werkplek join-clientcomputers en selecteer vervolgens **bewerken**.
7. Selecteer het keuzerondje ingeschakeld en klik vervolgens op toepassen. Klik op **OK**.
8. U kunt nu het Groepsbeleid object koppelingen naar een locatie van uw keuze. Samengevoegd zodat dit beleid voor alle van het domein Windows 8.1 apparaten in uw organisatie, het groepsbeleid koppelen aan het domein.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>Afmelden van uw domein Windows 8.1 apparaten die zijn gekoppeld

U kunt uw domein toegevoegd Windows 8.1 apparaten unregister door het volgende te doen: Wijzig de werkplek deelnemen aan Groepsbeleid-instellingen in de vorige sectie hebt gemaakt. Stel de automatisch werkplek join computers clientbeleid op uitgeschakeld. Hiermee wordt voorkomen dat nieuwe apparaten automatisch deelnemen aan het bedrijf.

De bestaande domein toegevoegd Windows 8.1 machines unregister op volgende een van de volgende twee opties:

* Optie 1: **een Windows 8.1 Unregister domein die zijn gekoppeld apparaat met PC-instellingen**
  1. Ga naar **PC-instellingen**op het apparaat met Windows 8.1 > **netwerk** > **bedrijf**
  2. Selecteer **verlaten**.
Dit proces moet worden herhaald voor elk domeingebruiker die is aangemeld bij de computer en is automatisch bedrijf die zijn gekoppeld.

* Optie 2: Een Windows 8.1-domein gekoppelde apparaat met een script Unregister
    1. Open een opdrachtprompt op de computer met Windows 8.1 en voer de volgende opdracht uit:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
Deze opdracht moet worden uitgevoerd in de context van elke domeingebruiker handtekening in de computer.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Logboeken en fouten voor Windows 8.1 domein toegevoegd-apparaten

Het gebeurtenissenlogboek van Windows op een computer met Windows 8.1 wordt berichten met betrekking tot apparaatregistratie weergegeven. U vindt u berichten voor geslaagde en mislukte gebeurtenissen. 

Het gebeurtenissenlogboek van vindt u in het logboek Viewer onder toepassingen en Services **Logboeken** > **Microsoft** > **Windows > werkplek deelnemen aan**.

##<a name="additional-details"></a>Meer informatie

Het groepsbeleid kunt een geplande taak op het systeem dat wordt uitgevoerd in de context van de gebruiker en klik op aanmelding wordt geactiveerd. De taak wordt stilte de gebruiker en apparaat registreren met Azure AD nadat u de aanmeldingsproblemen is voltooid. De geplande taak kan worden gevonden op Windows 8.1-apparaten in de bibliotheek voor Taakplanner onder **Microsoft** > **Windows** > **Bedrijf deelnemen aan**. De taak wordt uitgevoerd en alle Active Directory-gebruikers die aanmelding in registreren. 

## <a name="additional-topics"></a>Aanvullende onderwerpen
- [Azure Active Directory-apparaatregistratie-overzicht](active-directory-conditional-access-device-registration-overview.md)
- [Automatische apparaatregistratie met een domein behoren Azure Active Directory voor Windows 10-apparaten](active-directory-conditional-access-automatic-device-registration.md)
- [Automatische apparaatregistratie voor apparaten met Windows 7-domein gekoppeld configureren](active-directory-conditional-access-automatic-device-registration-windows7.md)

