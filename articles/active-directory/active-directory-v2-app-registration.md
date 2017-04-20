<properties
    pageTitle="v2.0 app registratie | Microsoft Azure"
    description="Het registreren van een app bij Microsoft voor het aanmelden inschakelen en toegang krijgen tot het Microsoft-services met het eindpunt v2.0"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Het registreren van een app bij het eindpunt v2.0

Als u wilt maken van een app die zowel MSA & Azure AD accepteert aanmeldingsproblemen, u moet eerst een app hebt geregistreerd bij Microsoft.  Op dit moment niet mogelijk moet u met Azure AD wellicht bestaande apps gebruiken of MSA - moet u een nieuw document maken.

> [AZURE.NOTE]
    Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="visit-the-microsoft-app-registration-portal"></a>Ga naar de Microsoft-app registratie-portal
Eerste dingen Ga eerst - naar [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Dit is een nieuwe app-portal Registratie van waar u uw Microsoft-apps kunt beheren.

Meld u aan met een een persoonlijke of werk- of Microsoft-account voor school.  Als u geen al hebt, moet u zich registreren voor een nieuwe persoonlijk account. Verdergaan of het won't lang duurt - we hier moet wachten.

Klaar? U moet nu kijken aan uw lijst met apps van Microsoft, die waarschijnlijk leeg is.  Laten we wijzigen.

Klik op **een app toevoegen**en geef deze een naam.  De portal toewijzen uw app een globaal unieke Id-toepassing die u later in uw code gebruikt.  Als uw app een onderdeel van de servers bevat die toegangstokens moet voor bellen API's (denken: Office, Azure of uw eigen web API), wilt u een **Toepassing geheim** hier ook maken.
<!-- TODO: Link for app secrets -->

Voeg nu de Platforms voor dat uw app wilt gebruiken.

- Geef op het web-apps voor een **Omleiden URI** waar aanmeldingsproblemen berichten kunnen worden verzonden.
- Voor mobiele apps, kopieert u omlaag de standaard-redirect-uri automatisch voor u gemaakt.

Desgewenst kunt u het uiterlijk van uw aanmeldingspagina in de sectie profiel aanpassen.  Zorg ervoor dat Klik op **Opslaan** voordat u verdergaat.

> [AZURE.NOTE] Wanneer u een toepassing via [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)maakt, wordt de toepassing worden geregistreerd in de tenant start van het account waarmee u zich aan te melden bij de portal.  Dit betekent dat u niet een toepassing kunt registreren in uw Azure AD-tenant met een persoonlijk Microsoft-account.  Als u expliciet registreren van een toepassing in een bepaalde tenant wilt, meld u aan met een account dat is gemaakt in die tenant.

## <a name="build-a-quick-start-app"></a>Een snel aan de slag-app maken
Nu dat u een Microsoft-app hebt, kunt u een van onze v2.0 snel aan de slag-zelfstudies voltooien.  Hier volgen een paar aanbevelingen:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
