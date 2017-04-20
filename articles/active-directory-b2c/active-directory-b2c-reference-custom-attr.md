<properties
    pageTitle="Azure Active Directory-B2C: Kenmerken van aangepast veld | Microsoft Azure"
    description="Het gebruik van aangepaste kenmerken in Azure Active Directory B2C voor het verzamelen van informatie over uw consumenten"
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
    ms.date="07/24/2016"
    ms.author="swkrish"/>

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory-B2C: Aangepaste kenmerken gebruiken voor het verzamelen van informatie over uw consumenten

Het telefoonboek van uw Azure Active Directory (Azure AD) B2C wordt geleverd met een ingebouwde reeks gegevens (kenmerken): voornaam, achternaam, City, postcode en andere kenmerken. Elke toepassing consumenten gerichte heeft echter unieke vereisten op welke kenmerken voor het verzamelen van consumenten. Met Azure AD B2C, kunt u de reeks kenmerken die zijn opgeslagen op elk account voor consumenten uitbreiden. U kunt maken van aangepaste kenmerken van de [Azure-portal](https://portal.azure.com/) en deze gebruiken in uw aanmelding beleidsregels, zoals hieronder wordt weergegeven. U kunt ook lezen en schrijven van deze kenmerken met behulp van de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [AZURE.NOTE]
Aangepaste kenmerken [Azure AD Graph API Directory Schema extensies](https://msdn.microsoft.com/library/azure/dn720459.aspx)gebruiken.

## <a name="create-a-custom-attribute"></a>Een aangepaste kenmerk maken

1. [Als volgt te werk om te navigeren naar het blad B2C functies op de Azure-portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **gebruikerskenmerken**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Geef een **naam** voor het aangepaste kenmerk (bijvoorbeeld "ShoeSize") en desgewenst een **Beschrijving**. Klik op **maken**.

    > [AZURE.NOTE]
    Alleen het 'Tekenreeks'- **Gegevenstype** is momenteel beschikbaar.

Het aangepaste kenmerk is nu beschikbaar in de lijst van **kenmerken van de gebruiker**en voor gebruik in uw aanmelding beleid.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Een aangepaste kenmerk gebruiken in uw aanmelding beleid

1. [Als volgt te werk om te navigeren naar het blad B2C functies op de Azure-portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **beleidsregels voor aanmelding**.
3. Klik op uw aanmelding beleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen. Klik op **bewerken** aan de bovenkant van het blad.
4. Klik op **aanmelden bij kenmerken** en selecteert u het aangepaste kenmerk (bijvoorbeeld "ShoeSize"). Klik op **OK**.
5. Klik op van **toepassing op claims** en selecteert u het aangepaste kenmerk. Klik op **OK**.
6. Klik op **Opslaan** aan de bovenkant van het blad.

U kunt de functie 'Nu uitvoeren' op het beleid voor de verificatie van de consumenten-ervaring. U moet nu Zie 'ShoeSize' in de lijst met de kenmerken die worden verzameld tijdens consumenten aanmelding, en deze in het token verzonden terug naar de toepassing.

## <a name="notes"></a>Notities

- Samen met de aanmelding beleidsregels, kunnen aangepaste kenmerken ook worden gebruikt in beleid voor het registreren of aanmelden en uw profiel bewerken van beleid.
- Er is een bekende beperking van aangepaste kenmerken. Dit is alleen de eerste keer dat deze wordt gebruikt in een beleid en niet wanneer u deze toevoegen aan de lijst met **kenmerken van de gebruiker**gemaakt.
