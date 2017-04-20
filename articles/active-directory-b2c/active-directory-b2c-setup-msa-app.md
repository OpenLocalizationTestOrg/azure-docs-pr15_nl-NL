<properties
    pageTitle="Azure Active Directory-B2C: Configuratie Microsoft-account | Microsoft Azure"
    description="Geef aanmelding en aanmelden op consumenten met Microsoft-accounts in de toepassingen die zijn beveiligd met Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory-B2C: Geef aanmelding en aanmelden op consumenten met Microsoft-accounts

## <a name="create-a-microsoft-account-application"></a>Maken van een toepassing voor Microsoft-account.

Als u wilt gebruiken Microsoft-account als een identiteitsprovider in B2C Azure Active Directory (Azure AD), moet u een Microsoft-account-toepassing maken en deze met de juiste parameters opgeeft. Moet u een Microsoft-account u dit wilt doen. Als u deze niet hebt, kunt u deze bij [https://www.live.com/](https://www.live.com/)verkrijgen.

1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) en meld u aan met de referenties van uw Microsoft-account.
2. Klik op **een app toevoegen**.

    ![Microsoft-account: Voeg een nieuwe app](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. Geef een **naam** voor uw toepassing en klik op **de toepassing maken**.

    ![Microsoft-account - App-naam](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. Kopieer de waarde van **Toepassings-Id**. Moet u deze naar het Microsoft-account configureren als een identiteitsprovider in uw tenant is ingeschakeld.

    ![Microsoft-account - Id van de toepassing](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. Klik op **toevoegen platform** en kies **Web**.

    ![Microsoft-account: Voeg platform](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Microsoft-account - Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld **Omleiden URI's** . **{Tenant}** vervangen door een van de tenant-naam (bijvoorbeeld contosob2c.onmicrosoft.com).

    ![Microsoft-account - Omleidings-URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. Klik op **Nieuw wachtwoord genereren** onder de sectie **Toepassing geheimen** . Kopieer het nieuwe wachtwoord op scherm weergegeven. Moet u deze naar het Microsoft-account configureren als een identiteitsprovider in uw tenant is ingeschakeld. Dit wachtwoord is een belangrijk beveiliging.

    ![Microsoft-account: een nieuw wachtwoord genereren](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Microsoft-account - nieuwe wachtwoord](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. Schakel het selectievakje met de mededeling **Live SDK ondersteunen** onder de sectie **Geavanceerde opties dat** . Klik op **Opslaan**.

    ![Microsoft-account - Live SDK ondersteuning](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Microsoft-account configureren als een identiteitsprovider in uw tenant

1. Volg deze stappen om te [Navigeren naar het blad B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure-portal.
2. Klik op het blad B2C-functies op **identiteitsprovider**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Geef een beschrijvende **naam** voor de configuratie van de provider identiteit. Voer bijvoorbeeld "MSA".
5. Klik op **identiteit providertype**, selecteert u **Microsoft-account**en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer het toepassings-Id en wachtwoord van de Microsoft-account-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** om op te slaan de configuratie van de Microsoft-account.
