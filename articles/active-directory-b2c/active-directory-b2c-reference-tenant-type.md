<properties
    pageTitle="Azure Active Directory-B2C: Productie-schaal versus preview B2C-tenants | Microsoft Azure"
    description="Een onderwerp van het type Azure Active Directory B2C tenants"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory-B2C: Productie-schaal versus preview B2C-tenants

Als u van plan bent een productie-app om op te schrijven B2C Azure Active Directory (Azure AD), moet u er zeker van te zijn dat u hebt de juiste tenant "type" om te gaan live op. Om te zien wat u hebt, als volgt te werk om te [Navigeren naar het blad B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure-portal en kijk onder **type van de Tenant**.

## <a name="summary"></a>Overzicht

Azure AD B2C ondersteunt alleen aan **productie-schaal** B2C-tenants in Noord-Amerika productie-apps.

| Type van de tenant | Landen/regio 's | Algemeen-beschikbaar? |
| ----------- | -------------- | --------------------- |
| **Productie-schaal tenant** | Noord-Amerikaanse landen/regio 's | Ja |
| **Productie-schaal tenant** | Alle landen/regio's, behalve van Noord-Amerika | Nee |
| **Voorbeeld tenant** | Alle landen/regio 's | Nee |

> [AZURE.NOTE]
Azure AD B2C tenants (voor consumenten) zijn momenteel niet beschikbaar in een paar landen of regio's waarin Azure AD-tenants (voor werknemers) beschikbaar zijn. Lees de volgende secties voor meer informatie.

## <a name="production-scale-b2c-tenant-in-north-america"></a>Productie-schaal B2C tenant in Noord-Amerika

Als u [uw tenant B2C gemaakt](active-directory-b2c-get-started.md) in Noord-Amerika, dat wil zeggen op een van de volgende landen of regio's: Verenigde Staten, Canada, Costa Rica, Dominicaanse Republiek, El Salvador, Guatemala, Mexico, Panama, Puerto Rico en Trinidad en Tobago, en het **type van de Tenant** in de gebruikersinterface van de beheerder B2C wordt gemeld dat **productie-schaal**, uw tenant kan worden gebruikt voor productie-apps.

> [AZURE.NOTE]
Productie-schaal tenants zijn kunnen schaalbaarheid naar 100s miljoenen consumenten identiteiten per pachter.

![Schermafbeelding van een tenant productie-schaal](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Voorbeeld B2C tenant in een land/regio

Als u een tenant B2C hebt gemaakt tijdens de proefperiode Azure AD B2C, is het waarschijnlijk dat uw **Tenant Typ** **voorbeeld van de tenant zegt**. Als dit het geval is, moet u uw tenant alleen voor ontwikkelen en testen en niet voor productie apps is ingesteld.

> [AZURE.IMPORTANT]
Er is geen migratiepad van een voorbeeld B2C tenant naar een productie-schaal B2C-tenant. Merk op dat er zijn bekende problemen wanneer u een tenant preview B2C verwijderen en opnieuw een productie-schaal B2C-tenant met dezelfde domeinnaam maken. U hoeft te maken van een productie-schaal B2C-tenant met een andere domeinnaam.

![Schermafbeelding van een tenant preview](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>Productie-schaal B2C tenant buiten Noord-Amerika

Azure AD B2C is momenteel niet in het algemeen-beschikbaar buiten Noord-Amerika. U kunt echter maken en gebruiken van productie-schaal tenants, voor het ontwikkelen en testen doeleinden, in een van de volgende landen of regio's: Algerije, Oostenrijk, Azerbeidzjan, Bahrein, Belarus, België, Bulgarije, Kroatië, Cyprus, Tsjechische Republiek, Denemarken, Egypte, Estland, Finland, Frankrijk, Duitsland, Griekenland, Hongarije, IJsland, Ierland, Israël, Italië, Jordanië, Kazachstan, Kenia, Koeweit, Letland, Libanon, Liechtenstein, NNNNNN, Luxemburg, Voormalige Joegoslavische Republiek Macedonië, Malta, Montenegro, Marokko, Nederland, Nigeria, Noorwegen , Oman, Pakistan, Polen, Portugal, Qatar, Roemenië, Rusland, Saudi-Arabië, Servië, Slowakije, Slovenië, Zuid-Afrika, Spanje, Zweden, Zwitserland, Tunesië, Turkije, Oekraïne, Verenigde Arabische Emiraten en Verenigd Koninkrijk.

Verzamelt Azure AD B2C wordt gemeld die algemeen beschikbaar is in boven landen of regio's, kunt u bestaande gegevens bij gebruik van deze tenants productie-schaal en ga live met uw apps productie zonder dat er gegevens verloren.

## <a name="availability-of-b2c-tenants"></a>Beschikbaarheid van B2C tenants

B2C tenants zijn momenteel niet beschikbaar in de volgende landen of regio's: Afghanistan, Argentinië, Australië, Brazilië, Chili, Colombia, Ecuador, Hong Kong SAR, India, Indonesië, Irak, Japan, Korea, Maleisië, Nieuw-Zeeland, Paraguay, Peru, Filipijnen, Singapore, Srilankaanse, Taiwan, Thailand, Uruguay en Venezuela. We plannen deze wilt opnemen in de toekomst.
