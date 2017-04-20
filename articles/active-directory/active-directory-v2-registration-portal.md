<properties
    pageTitle="App registratie Portal Help-onderwerpen | Microsoft Azure"
    description="Een beschrijving van de verschillende functies in de portal van de registratie van de Microsoft-app."
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

# <a name="app-registration-reference"></a>App registratie verwijzing
Dit document bevat context en beschrijvingen van diverse functies die zijn gevonden in de Portal voor registratie van Microsoft-App [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Mijn toepassingen
Deze lijst bevat al uw toepassingen geregistreerd voor gebruik met het eindpunt van de v2.0 Azure AD.  Deze toepassingen hebt de mogelijkheid om aan te melden gebruikers met beide persoonlijke accounts van Microsoft-account en werk-, school-accounts van Azure Active Directory.  Zie meer informatie over het eindpunt van Azure AD v2.0 onze [v2.0 overzicht](active-directory-appmodel-v2-overview.md).  Deze toepassingen kunnen ook worden gebruikt om integreren met het eindpunt van de verificatie in de Microsoft-account, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK toepassingen
Deze lijst bevat al uw toepassingen geregistreerd voor gebruik uitsluitend met Microsoft-account.  Ze zijn niet ingeschakeld voor gebruik met Azure Active Directory dan ook.  Dit is waar u vindt alle toepassingen die u eerder had is geregistreerd met de MSA developer portal op `https://account.live.com/developers/applications`.  Alle functies die u eerder hebt uitgevoerd in `https://account.live.com/developers/applications` kan nu worden uitgevoerd in deze nieuwe portal `https://apps.dev.microsoft.com`.  Als u meer vragen over de toepassingen van uw Microsoft-account hebt, neem dan contact met ons opnemen.

## <a name="application-secrets"></a>Geheimen van toepassing
Geheimen van toepassing zijn referenties waarmee uw toepassing betrouwbare [clientverificatie](http://tools.ietf.org/html/rfc6749#section-2.3) met Azure AD uitvoeren.  In het OAuth & OpenID verbinding, een toepassing geheimen is meestal genoemd een `client_secret`.  In het v2.0 protocol, een toepassing die u een beveiligingstoken op een weblocatie gebruikt ontvangt (met een `https` kleurenschema) moet een toepassing geheim gebruiken om te Azure AD na aflossingsprijs van die beveiligingstoken wordt herkend.  Bovendien elke native client die krijgen tokens op een apparaat wordt verboden gebruiken uit een toepassing geheim te voeren clients verifiëren, te ontmoedigen de opslag van geheimen in onveilige omgevingen.

Elke app bevatten twee geldige toepassing geheimen op elk gewenst moment in tijd.  Door twee geheimen, hebt u de ablilty periodiek sleutelrollover uitvoert voor de hele omgeving van uw toepassing.  Zodra u hebt het geheel van uw toepassing naar een nieuwe geheim gemigreerd, kunt u deze kunt verwijderen van het oude geheim en inrichten van een nieuwe record.

Slechts twee soorten geheimen van toepassing zijn op dit moment kan toegestaan in de portal van de registratie van de app.  **Een nieuw wachtwoord genereren** kiezen genereren en een gedeeld geheim opslaan in de desbetreffende gegevens store, waarin u in uw toepassing kunt.  **Nieuwe sleutel-paar genereren** kiezen maakt een nieuwe/combinatie openbare persoonlijke sleutel die kan worden gedownload en die wordt gebruikt voor clientverificatie naar Azure AD.

## <a name="profile"></a>Profiel
Het gedeelte van het profiel van de portal van de registratie van de app kan worden gebruikt om aan te passen van de aanmeldingspagina voor uw toepassing.  U kunt op dit moment de aanmelden van pagina-toepassing logo, gebruiksvoorwaarden service URL en privacyverklaring wijzigen.  Het logo moet een transparante 48 x 48 of 50 x 50 pixels afbeelding in een GIF-, PNG- of JPEG-bestand dat is 15 KB of kleiner.  Probeer het wijzigen van de waarden en het bekijken van de resulterende sign in pagina!

## <a name="live-sdk-support"></a>Live SDK ondersteuning
Wanneer u "Live SDK Support" inschakelt, geen toepassing geheimen die u maakt in zowel de Azure AD ingericht en gegevensopslag van Microsoft-Account.  Hierdoor wordt de toepassing rechtstreeks te integreren met de service Microsoft-Account (login.live.com).  Als u wilt maken van een app met Microsoft-Account rechtstreeks (in plaats van het eindpunt van de v2.0 Azure AD), moet u ervoor zorgen dat Live SDK ondersteuning is ingeschakeld.

Live SDK ondersteuning uitschakelen zorgt ervoor dat dat het geheim toepassing alleen is geschreven in de Azure AD-gegevens opslaan.  De gegevens van Azure AD store bedrijfstoepassingen regelgeving die toestaan om te voldoen aan bepaalde standaarden, zoals FISMA naleving implementeren.  Als u Live SDK ondersteuning inschakelt, krijgen de toepassing niet voldoen aan bepaalde van deze normen.

Als u alleen ooit van plan bent het eindpunt van Azure AD v2.0 gebruiken, kunt u Live SDK ondersteuning veilig uitschakelen.

