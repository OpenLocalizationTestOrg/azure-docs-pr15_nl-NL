<properties
    pageTitle="Azure Active Directory-B2C: Configuratie Amazon | Microsoft Azure"
    description="Geef aanmelding en aanmelden op consumenten met Amazon-accounts in de toepassingen die zijn beveiligd met Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory-B2C: Geef aanmelding en aanmelden op consumenten met Amazon-accounts

## <a name="create-an-amazon-application"></a>Een Amazon-toepassing maken

Als u wilt gebruiken Amazon als een identiteitsprovider in B2C Azure Active Directory (Azure AD), moet u een Amazon-toepassing maken en deze met de juiste parameters opgeeft. Moet u een account Amazon u dit wilt doen. Als u deze niet hebt, kunt u deze bij [http://www.amazon.com/](http://www.amazon.com/)verkrijgen.

1. Ga naar het [Amazon Developer Center](https://login.amazon.com/) en meld u aan met de referenties van uw Amazon-account.
2. Als u dit nog niet hebt gedaan, klikt u op **Registreren**, volgt u de stappen van de registratie ontwikkelaars en het beleid te accepteren.
3. Klik op **nieuwe toepassing registreren**.

    ![Een nieuwe toepassing op de website van Amazon registreren](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. Informatie over toepassingen (**naam**, **Beschrijving**en **Privacy kennisgeving URL**) en klik op **Opslaan**.

    ![Informatie over toepassingen voor het registreren van een nieuwe toepassing op Amazon leveren](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. Kopieer de waarden van **Client-ID** en **Geheim Client**in de sectie **Web-instellingen** . (U moet u op de knop **Geheim weergeven** om dit te zien.) U moet beide Amazon configureren als een identiteitsprovider in uw tenant is ingeschakeld. Klik op **bewerken** onderaan in de sectie. **Geheim client** is een belangrijk beveiliging.

    ![Client-ID en geheim Client leveren voor uw nieuwe toepassing op Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. Voer `https://login.microsoftonline.com` in het veld **Toegestane JavaScript oorsprong** en `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in het veld **URL's voor retourneren toegestaan** . **{Tenant}** vervangen door een van de tenant-naam (bijvoorbeeld contoso.onmicrosoft.com). Klik op **Opslaan**. De waarde **{tenant}** is hoofdlettergevoelig.

    ![JavaScript-oorsprong en retourneren URL's leveren voor uw nieuwe toepassing op Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Amazon configureren als een identiteitsprovider in uw tenant

1. Volg deze stappen om te [Navigeren naar het blad B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure-portal.
2. Klik op het blad B2C-functies op **identiteitsprovider**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Geef een beschrijvende **naam** voor de configuratie van de provider identiteit. Voer bijvoorbeeld "Amzn".
5. Klik op **identiteit providertype**, selecteert u **Amazon**en klik op **OK**.
6. Klik op **deze identiteitsprovider instellen** en voer de client-ID en geheim van de client van de Amazon-toepassing die u eerder hebt gemaakt.
7. Klik op **OK** en klik vervolgens op **maken** om op te slaan uw Amazon-configuratie.
