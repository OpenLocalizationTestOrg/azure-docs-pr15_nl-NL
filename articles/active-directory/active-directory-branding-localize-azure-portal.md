<properties
pageTitle="Taalspecifieke bedrijf huisstijl naar de pagina aanmelden in de voorbeeldversie Azure Active Directory toevoegen | Microsoft Azure"
description="Informatie over het toevoegen van een specifiek bedrijf taal huisstijl afbeeldingen en tekst naar een Azure aanmeldingspagina"
services="active-directory"
documentationCenter=""
authors="curtand"
manager="femila"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/12/2016"
ms.author="curtand"/>

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Taalspecifieke bedrijf huisstijl naar de pagina aanmelden in de voorbeeldversie Azure Active Directory toevoegen

Om verwarring te voorkomen, wilt veel bedrijven toepassen op een consistent uiterlijk geven alle alle websites en services die ze beheren. Voorbeeld van Azure Active Directory biedt deze functionaliteit doordat u kunt het uiterlijk van de pagina aanmelden met uw bedrijfslogo en aangepaste kleurenschema's aanpassen. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) De aanmeldingspagina is de pagina die verschijnt wanneer u zich aanmeldt bij Office 365 of andere webtoepassingen dat Azure AD als de identiteitsprovider van uw gebruikt. U werken met deze pagina om uw referenties invoeren.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Aanpassen van de pagina aanmelden voor een andere taal

Alleen als u een aangepaste aanmeldingspagina als beschreven in [bedrijf huisstijl naar de pagina aanmelden toevoegen](active-directory-branding-custom-signon-azure-portal.md)al hebt gemaakt, kunt u taalspecifieke elementen toevoegen aan uw aangepaste aanmeldingspagina. U kunt een aanmeldingspagina per map configureren met een standaardset met aanpasbare elementen. Nadat u de standaardset met pagina-elementen hebt geconfigureerd, kunt u meer versies voor andere landinstellingen configureren. U kunt ook meng en overeenkomen met verschillende elementen. U kunt bijvoorbeeld het volgende doen:

- Maak een standaard **aanmeldingsproblemen pagina-afbeelding** die goed aansluit op alle culturen en maak vervolgens specifieke versies voor Engels en Frans. Wanneer u uw browsers op een van deze twee talen instellen, de afbeelding taalspecifieke wordt weergegeven, terwijl de standaard-afbeelding wordt weergegeven voor alle andere talen.

- Configureer verschillende logo's voor uw organisatie (bijvoorbeeld Japans of Hebreeuws versies).

Het is raadzaam dat u het aantal behouden taal variaties laag, voor onderhoud en prestaties redenen.

**Huisstijl naar het telefoonboek van uw bedrijf toevoegen:**

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2.  **Meer services**selecteren, voert u **gebruikers en groepen** in het tekstvak en druk vervolgens op **Enter**.

    ![Gebruikersbeheer openen](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. Selecteer de **huisstijl bedrijf**op het blad **gebruikers en groepen** .

4. Klik op de **gebruikers en groepen - bedrijfsbeheerder huisstijl** blade, selecteert u de opdracht **taal toevoegen** .

    ![Taalspecifieke huisstijl elementen toevoegen](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.

6. Klik op **Opslaan**.

Het duurt maximaal een uur voor eventuele wijzigingen aan de huisstijl weergegeven pagina in het vak aanmelden.

## <a name="next-steps"></a>Volgende stappen

[Naar de pagina aanmelden met huisstijl bedrijf toevoegen](active-directory-branding-custom-signon-azure-portal.md)
