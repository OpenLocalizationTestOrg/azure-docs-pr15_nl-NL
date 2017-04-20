<properties
    pageTitle="Azure Active Directory-B2C: Configuratie LinkedIn | Microsoft Azure"
    description="Aanmelden en aanmelden op consumenten met LinkedIn-accounts in de toepassingen die zijn beveiligd met Azure Active Directory B2C opgeven"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory-B2C: Geef aanmelding en aanmelden op consumenten met LinkedIn-accounts

## <a name="create-a-linkedin-application"></a>Een LinkedIn-servicetoepassing maken

Als u wilt gebruiken LinkedIn als een identiteitsprovider in B2C Azure Active Directory (Azure AD), moet u een LinkedIn-servicetoepassing maken en deze met de juiste parameters opgeeft. U moet een LinkedIn-account u dit wilt doen. Als u deze niet hebt, kunt u deze bij [https://www.linkedin.com/](https://www.linkedin.com/)verkrijgen.

1. Ga naar de [website van LinkedIn-ontwikkelaars](https://www.developer.linkedin.com/) en meld u aan met de referenties van uw LinkedIn-account.
2. Klik op **Mijn Apps** in de bovenste menubalk en klik vervolgens op **Toepassing maken**.

    ![LinkedIn - nieuwe app](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. Vul het formulier voor het **maken van een nieuwe toepassing** in de relevante informatie (**Bedrijfsnaam**, **naam**, **Beschrijving**, **De URL van de toepassing-Logo**, **Gebruik van de toepassing**, **De Website-URL**, **E-mailadres** en **Telefoon (werk)**).
4. Akkoord met de **LinkedIn API gebruiksvoorwaarden** en klik op **verzenden**.

    ![LinkedIn - Register-app](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. Kopieer de waarden van **Client-ID** en **Geheim Client**. (U vindt deze onder **Verificatiesleutels**.) Moet u beide LinkedIn configureren als een identiteitsprovider in uw tenant is ingeschakeld.

    >[AZURE.NOTE] **Geheim client** is een belangrijk beveiliging.

6. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld **URL's voor omleiden geautoriseerd** (onder **OAuth 2.0**). **{Tenant}** vervangen door een van de tenant-naam (bijvoorbeeld contoso.onmicrosoft.com). Klik op **toevoegen**en klik vervolgens op **bijwerken**. De waarde **{tenant}** is hoofdlettergevoelig.

    ![LinkedIn - Setup-app](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>LinkedIn configureren als een identiteitsprovider in uw tenant

1. Volg deze stappen om te [Navigeren naar het blad B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure-portal.
2. Klik op het blad B2C-functies op **identiteitsprovider**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Geef een beschrijvende **naam** voor de configuratie van de provider identiteit. Voer bijvoorbeeld "LI".
5. Klik op **identiteit providertype**, selecteert u **LinkedIn**en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer de client-ID en geheim van de client van de LinkedIn-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** om op te slaan uw LinkedIn-configuratie.
