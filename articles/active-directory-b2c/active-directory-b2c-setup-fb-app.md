<properties
    pageTitle="Azure Active Directory-B2C: Configuratie Facebook | Microsoft Azure"
    description="Geef aanmelding en aanmelden op consumenten met Facebook-accounts in de toepassingen die zijn beveiligd met Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory-B2C: Geef aanmelding en aanmelden op consumenten met Facebook-accounts

## <a name="create-a-facebook-application"></a>Een Facebook-toepassing maken

Als u wilt gebruiken Facebook als een identiteitsprovider in B2C Azure Active Directory (Azure AD), moet u een Facebook-toepassing bouwen en verstrekt haar de juiste parameters. U moet een Facebook-account u dit wilt doen. Als u deze niet hebt, kunt u deze bij [https://www.facebook.com/](https://www.facebook.com/)verkrijgen.

1. Ga naar de website van [Facebook voor ontwikkelaars](https://developers.facebook.com/) en meld u aan met de referenties van uw Facebook-account.
2. Als u dit nog niet hebt gedaan, moet u registreren als een ontwikkelaar Facebook. Klik hiertoe klikt u op **registreren** (in de rechterbovenhoek van de pagina), van Facebook-beleid accepteert en voert u de registratie-stap.
3. Klik op **Mijn Apps** en klik op **een nieuwe App toevoegen**. Kies **Website** als het platform en klik vervolgens op **overslaan en App-ID maken**.

    ![Facebook - een nieuwe App toevoegen](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebook - Website voor een nieuwe App - toevoegen](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebook - App-ID maken](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. Klik op het formulier een **Weergavenaam**, een geldig **E-mailadres van contactpersoon**, een **categorie**, en klik op **App-ID maken**. Dit moet u Facebook platform beleid accepteert en een online controle te voltooien.

    ![Facebook - maken van een nieuwe App-ID](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. Klik op **Instellingen** op het linker navigatiegedeelte op.
6. Klik op **+ Platform toevoegen** en selecteer vervolgens **Website**.

    ![Facebook - instellingen](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![Facebook - instellingen - Website](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. Voer [https://login.microsoftonline.com/](https://login.microsoftonline.com/) in het veld **URL van de Site** en klik vervolgens op **Wijzigingen opslaan**.

    ![Facebook - URL van de Site](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. Kopieer de waarde van **App-ID**. Klik op **weergeven** en kopieer de waarde van de **App geheim**. Moet u beide Facebook configureren als een identiteitsprovider in uw tenant is ingeschakeld. **App geheim** is een belangrijk beveiliging.

    ![Facebook - App-ID en App geheim](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. Klik op **+ Product toevoegen** op het linker navigatiegedeelte op en klik vervolgens op de knop **Aan de slag** naast **Facebook Login**.

    ![Facebook - Facebook Login](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld **geldig OAuth omleiden URI's** in de sectie **OAuth clientinstellingen** . **{Tenant}** vervangen door een van de tenant-naam (bijvoorbeeld contosob2c.onmicrosoft.com). Klik op **Wijzigingen opslaan** onder aan de pagina.

    ![Facebook - OAuth Redirect URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. Als u uw Facebook-toepassing kan worden gebruikt door Azure AD B2C, moet u deze openbaar beschikbaar te maken. U kunt dit doen door te klikken op **App controleren** op het linker navigatiegedeelte en in te schakelen van de schakeloptie boven aan de pagina op **Ja** en **Bevestig**klikken.

    ![Facebook - App openbare](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Facebook configureren als een identiteitsprovider in uw tenant

1. Volg deze stappen om te [Navigeren naar het blad B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure-portal.
2. Klik op het blad B2C-functies op **identiteitsprovider**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Geef een beschrijvende **naam** voor de configuratie van de provider identiteit. Voer bijvoorbeeld "FB".
5. Klik op **identiteit providertype**, selecteer **Facebook**en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer de app-ID en de app geheim (van de Facebook-toepassing die u eerder hebt gemaakt) in de **Client-ID** en **geheim Client** velden respectievelijk.
7. Klik op **OK**en klik vervolgens op **maken** om op te slaan uw Facebook-configuratie.
