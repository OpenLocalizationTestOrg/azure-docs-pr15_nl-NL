<properties
    pageTitle="Azure Active Directory B2C: Meervoudige verificatie | Microsoft Azure"
    description="Meervoudige verificatie in consumenten-omlaag toepassingen die zijn beveiligd met Azure Active Directory B2C inschakelen"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory-B2C: Meervoudige verificatie inschakelen in uw toepassingen consumenten-omlaag

Azure Active Directory (Azure AD) B2C geïntegreerd rechtstreeks met [Azure meervoudige verificatie](../multi-factor-authentication/multi-factor-authentication.md) , zodat u een tweede laag van beveiliging aan aanmeldings- en aanmeldingsproblemen ervaringen in uw toepassingen consumenten-omlaag toevoegen kunt. En kunt u dit doen zonder een één regel met code te schrijven. Momenteel ondersteunen we telefoongesprek en tekst bericht-verificatie. Als u beleid voor het registreren en de aanmeldingsopties al hebt gemaakt, kunt u nog steeds meervoudige verificatie inschakelen.

> [AZURE.NOTE]
Meervoudige verificatie kan ook worden ingeschakeld wanneer u aanmelden bij en aanmeldingsproblemen beleid, niet alleen door het bewerken van bestaande beleid maakt.

Deze functie kunt afhandelen, scenario's zoals de volgende toepassingen:

- U kunt meervoudige verificatie voor toegang tot één toepassing niet vereist, maar u hoeft deze voor toegang tot een ander postvakbeleid. Bijvoorbeeld de consument kunt Meld u aan bij een automatische verzekeringen toepassing met een sociale of lokale account, maar het telefoonnummer dat u moet controleren voordat toegang krijgen tot de thuis verzekeringen toepassing geregistreerd in dezelfde map.
- U meervoudige verificatie voor een toepassing in het algemeen niet vereist, maar u hoeft deze voor toegang tot de gevoelige gedeelten erin. Bijvoorbeeld de consument kan aanmelden bij een toepassing voor de bank met een sociale of lokale-account en het selectievakje saldo, maar het telefoonnummer dat u moet controleren voordat u probeert een bedrag over te schrijven.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Meervoudige verificatie inschakelen in uw aanmelding beleid wijzigen

1. [Als volgt te werk om te navigeren naar het blad B2C functies op de Azure-portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **beleidsregels voor aanmelding**.
3. Klik op uw aanmelding beleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen.
4. Klik op **meervoudige verificatie** en schakelt u de **status** in **op**aan. Klik op **OK**.
5. Klik op **Opslaan** aan de bovenkant van het blad.

U kunt de functie 'Nu uitvoeren' op het beleid voor de verificatie van de consumenten-ervaring. Controleer het volgende:

Een account voor consumenten wordt gemaakt in uw adreslijst voordat de stap meervoudige verificatie plaatsvindt. Tijdens de stap, is de consument gevraagd naar zijn of haar telefoonnummer opgeven en bevestig dit. Als de verificatie is gelukt, wordt het telefoonnummer dat is gekoppeld aan het account voor consumenten voor later gebruik. Zelfs als de consument wordt geannuleerd of boven af, hij of zij vragen om te controleren of een telefoonnummer opnieuw tijdens de volgende aanmeldingsproblemen (met meervoudige verificatie is ingeschakeld).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Wijzigen van uw beleid aanmelden om in te schakelen meervoudige verificatie

1. [Als volgt te werk om te navigeren naar het blad B2C functies op de Azure-portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **aanmelden beleid**.
3. Klik op uw aanmeldingsproblemen beleid (bijvoorbeeld ' B2C_1_SiIn') om deze te openen. Klik op **bewerken** aan de bovenkant van het blad.
4. Klik op **meervoudige verificatie** en schakelt u de **status** in **op**aan. Klik op **OK**.
5. Klik op **Opslaan** aan de bovenkant van het blad.

U kunt de functie 'Nu uitvoeren' op het beleid voor de verificatie van de consumenten-ervaring. Controleer het volgende:

Wanneer de consument zich aanmeldt (met een account sociale of lokale), als een geverifieerde telefoonnummer dat is gekoppeld aan het account voor consumenten, hij of zij wordt gevraagd om te controleren of deze. Als er geen telefoonnummer dat is gekoppeld, wordt de consument gevraagd om te leveren een en controle. Op geminimaliseerd lint in Word, worden het telefoonnummer dat is gekoppeld aan het account voor consumenten voor later gebruik.

## <a name="multi-factor-authentication-on-other-policies"></a>Meervoudige verificatie op andere beleidsregels

Zoals wordt beschreven voor aanmelding & aanmeldingsproblemen beleidsregels bovenstaande, het is ook mogelijk om in te schakelen meervoudige verificatie op registreren of beleid voor het aanmelden en het wachtwoord opnieuw in beleid. Dit wordt binnenkort op profiel bewerken beleid beschikbaar zijn.
