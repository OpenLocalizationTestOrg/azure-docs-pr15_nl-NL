<properties
pageTitle="De pagina aanmelden in de voorbeeldversie Azure Active Directory aanpassen | Microsoft Azure"
description="Informatie over het toevoegen van een huisstijl naar de pagina voor Azure aanmeldingsproblemen bedrijf"
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
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Naar de pagina aanmelden in de Azure Active Directory-voorbeeldversie met huisstijl bedrijf toevoegen

Om verwarring te voorkomen, wilt veel bedrijven toepassen op een consistent uiterlijk geven alle alle websites en services die ze beheren. Voorbeeld van Azure Active Directory biedt deze functionaliteit doordat u kunt het uiterlijk van de pagina aanmelden met uw bedrijfslogo en aangepaste kleurenschema's aanpassen. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) De aanmeldingspagina is de pagina die verschijnt wanneer u zich aanmeldt bij Office 365 of andere webtoepassingen dat Azure AD als de identiteitsprovider van uw gebruikt. U werken met deze pagina om uw referenties invoeren.

Als u de huisstijl van uw bedrijf, kleuren en andere aanpasbare elementen op deze pagina weergeven wilt, raadpleegt u de volgende afbeeldingen voor meer informatie over het verschil tussen de twee ervaringen.

De volgende schermafbeelding ziet en voorbeeld voor de Office 365-aanmelding pagina op een desktopcomputer **voordat u** een aanpassing:

![Pagina Office 365-aanmelding voor aanpassing](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

De volgende schermafbeelding ziet en voorbeeld voor de Office 365-aanmelding pagina op een desktopcomputer **nadat** een aanpassing:

![Office 365-aanmelding pagina die volgt na aanpassing](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>Aanpassen van de pagina aanmelden

Als u op basis van browser toegang tot uw cloud-apps en services die uw organisatie afsluit nodig hebt, gebruikt u meestal de aanmeldingspagina.

Als u wijzigingen hebt toegepast op de pagina aanmelden, duurt het maximaal een uur totdat de wijzigingen wilt weergeven.

Een huisstijl aanmeldingspagina wordt alleen weergegeven wanneer u een service met de URL van een tenant-specifieke zoals https://outlook.com/**contoso**.com of https://mail bezoekt. **Contoso**. com.

Wanneer u een service met specifieke URL's voor een niet-tenant bezoekt (bijvoorbeeld: https://mail.office365.com), een zonder huisstijl aanmeldingspagina wordt weergegeven. in dit geval de huisstijl wordt weergegeven nadat u uw gebruikers-ID hebt ingevoerd of u de gebruikerstegel van een hebt geselecteerd.

> [AZURE.NOTE]
>
- Uw domeinnaam moet worden weergegeven als 'Actief' in het gedeelte **domeinen** van de Azure portal waarin u hebt geconfigureerd huisstijl. Zie [aangepaste domeinnamen die toevoegen](active-directory-domains-add-azure-portal.md)voor meer informatie.
- Aanmeldingspagina huisstijl doorlopen niet op de pagina van Microsoft consumenten aanmelden. Als u zich met een Microsoft-account aanmelden, wordt er een huisstijl lijst van gebruiker tegels door Azure AD weergegeven, maar de huisstijl van uw organisatie niet van toepassing op de Microsoft-account aanmelden pagina.

Klik op de pagina aanmelden kan het selectievakje **aangemeld blijven** een gebruiker aangemeld blijven wanneer ze sluiten en opnieuw hun browser openen. 

   ![Laat me ondertekend in blijven](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Deze heeft geen invloed op de levensduur van de sessie. U kunt het selectievakje op de aanmeldingspagina van Azure Active Directory verbergen.
Of het selectievakje wordt weergegeven, hangt af van de instelling van **aangemeld uitgeschakeld blijven**.

   ![Laat me ondertekend in blijven](./media/active-directory-branding-custom-signon-azure-portal/02.png)


Als u wilt verbergen het selectievakje, configureert u deze instelling op **Ja**. 

> [AZURE.NOTE] Sommige functies van SharePoint Online en Office 2010, hangt af van gebruikers kunnen dit selectievakje inschakelen. Als u deze instelling verborgen configureert, ziet uw gebruikers mogelijk extra en vooral onverwacht aanwijzingen moeten aanmelden.




**Huisstijl naar het telefoonboek van uw bedrijf toevoegen:**

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2.  **Meer services**selecteren, voert u **gebruikers en groepen** in het tekstvak en druk vervolgens op **Enter**.

    ![Gebruikersbeheer openen](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. Selecteer de **huisstijl bedrijf**op het blad **gebruikers en groepen** .

4. Klik op de **gebruikers en groepen - bedrijfsbeheerder huisstijl** blade, selecteert u de opdracht **bewerken** .

    ![Aangepaste huisstijl bewerken](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. Wijzig de elementen die u wilt aanpassen. Alle elementen zijn optioneel.

6. Klik op **Opslaan**.

Het duurt maximaal een uur voor eventuele wijzigingen aan de huisstijl weergegeven pagina in het vak aanmelden.

## <a name="next-steps"></a>Volgende stappen

[Taalspecifieke bedrijf huisstijl toevoegen](active-directory-branding-localize-azure-portal.md)
