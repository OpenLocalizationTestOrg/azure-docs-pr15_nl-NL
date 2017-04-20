<properties
    pageTitle="Azure Active Directory-B2C: Google + configuratie | Microsoft Azure"
    description="Geef aanmelding en aanmelden op consumenten met Google + accounts niet in de toepassingen die zijn beveiligd met Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory-B2C: Geef aanmelding en aanmelden op consumenten met Google + accounts

## <a name="create-a-google-application"></a>Een Google +-toepassing maken

Als u wilt gebruiken Google + als een identiteitsprovider in B2C Azure Active Directory (Azure AD), moet u een Google +-toepassing maken en deze met de juiste parameters opgeeft. Moet u een account Google + u dit wilt doen. Als u deze niet hebt, kunt u deze bij [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)verkrijgen.

1. Ga naar de [Google ontwikkelaars Console](https://console.developers.google.com/) en meld u aan met de referenties van uw Google + account.
2. Klik op **project maken**, voer een **naam van het Project**en klik vervolgens op **maken**.

    ![Google + - aan de slag](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google + - nieuw project](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Klik op **API Manager** en klik vervolgens in het linker navigatiegedeelte op **referenties** .
4. Klik op het tabblad **OAuth toestemming scherm** aan de bovenkant.

    ![Google + - referenties](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Selecteer of een geldig **e-mailadres**opgeven, Geef een **productnaam**en klik op **Opslaan**.

    ![Google + - OAuth toestemming scherm](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Klik op **nieuwe referenties** en kies vervolgens **OAuth cliënt-ID**.

    ![Google + - OAuth toestemming scherm](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. Selecteer onder **toepassingstype** **webtoepassing**.

    ![Google + - OAuth toestemming scherm](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Geef een **naam** voor uw toepassing, voert u `https://login.microsoftonline.com` in het veld **geautoriseerd JavaScript oorsprong** en `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld **geautoriseerde omleiden URI's** . **{Tenant}** vervangen door een van de tenant-naam (bijvoorbeeld contosob2c.onmicrosoft.com). De waarde **{tenant}** is hoofdlettergevoelig. Klik op **maken**.

    ![Google + - client-ID maken](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Kopieer de waarden van **Client-ID** en **geheim Client**. Moet u beide configureren Google + als een identiteitsprovider in uw tenant is ingeschakeld. **Geheim client** is een belangrijk beveiliging.

    ![Google + - geheim Client](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Configureer Google + als een identiteitsprovider in uw tenant

1. Volg deze stappen om te [Navigeren naar het blad B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure-portal.
2. Klik op het blad B2C-functies op **identiteitsprovider**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Geef een beschrijvende **naam** voor de configuratie van de provider identiteit. Voer bijvoorbeeld "G +".
5. Klik op **identiteit providertype**, selecteert u **Google**en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer de client-ID en geheim van de client van de Google +-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** om op te slaan uw Google +-configuratie.
