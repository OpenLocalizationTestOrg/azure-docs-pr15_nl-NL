<properties
    pageTitle="Het beheren van certificaten Federatie in Azure AD | Microsoft Azure"
    description="Leer hoe u de vervaldatum voor uw certificaten Federatie aanpassen en het vernieuwen van certificaten die binnenkort verloopt."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Certificaten voor federatieve eenmalige aanmelding in Azure Active Directory beheren

In dit artikel heeft betrekking op veelgestelde vragen met betrekking tot de certificaten die Azure Active Directory worden gemaakt om vast te stellen federatieve eenmalige aanmelding (SSO) naar uw SaaS-toepassingen.

In dit artikel is alleen relevant voor apps die zijn geconfigureerd voor het gebruik van **Azure AD eenmalige aanmelding**, zoals wordt weergegeven in het onderstaande voorbeeld:

![Azure AD voor eenmalige aanmelding](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>Het aanpassen van de vervaldatum voor uw certificaat Federatie

Certificaten zijn standaard ingesteld op verloopt na twee jaar. U kunt een andere vervaldatum voor uw certificaat door de onderstaande stappen. De opgenomen schermafbeeldingen gebruiken Salesforce voorbeeld, maar deze stappen kunnen toepassen op een federatieve SaaS-app.

1. Azure Active Directory, klik op de pagina snel aan de slag voor uw toepassing, klik op **eenmalige aanmelding configureren**.

    ![Open de wizard Eenmalige aanmelding configureren.](./media/active-directory-sso-certs/config-sso.png)

2. Selecteer **Azure AD eenmalige aanmelding**, en klik vervolgens op **volgende**.

3. Typ in de **URL voor eenmalige aanmelding** van uw toepassing en schakel het selectievakje in voor het **configureren van het certificaat gebruikt voor federatieve eenmalige aanmelding**. Klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. Op de volgende pagina, selecteert u **een nieuw certificaat genereren**en geef aan hoe lang u wilt het certificaat geldig zijn voor. Klik vervolgens op **volgende**.

    ![Een nieuw certificaat genereren](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Klik op **Download certificaat**. Als u wilt weten hoe u het certificaat uploaden naar uw bepaalde SaaS-app, klikt u op **Weergave configuratie-instructies**.

    ![Download en het certificaat uploaden](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. Het certificaat worden niet ingeschakeld totdat u het selectievakje bevestiging onderaan in het dialoogvenster selecteren en druk vervolgens op verzenden.

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>Een certificaat dat verloopt binnenkort verlengen

De verlenging stappen hieronder wordt weergegeven, moeten in het ideale geval resulteert in geen aanzienlijk downtime voor uw gebruikers. De schermafbeeldingen die in deze sectie functie Salesforce gebruikt als een voorbeeld, maar deze stappen kunt toepassen op een federatieve SaaS-app.

1. Azure Active Directory, klik op de pagina snel aan de slag voor uw toepassing, klik op **Configureren eenmalige aanmelding**.

    ![Open de wizard Eenmalige aanmelding configureren](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. Klik op de eerste pagina van het dialoogvenster **Azure AD eenmalige aanmelding** moet al zijn geselecteerd, dus klik op **volgende**.

3. Klik op de tweede pagina, schakelt u het selectievakje voor **het certificaat voor federatieve eenmalige aanmelding configureren**. Klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. Op de volgende pagina, selecteert u **een nieuw certificaat genereren**en geef aan hoe lang u wilt het nieuwe certificaat geldig zijn voor. Klik vervolgens op **volgende**.

    ![Een nieuw certificaat genereren](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Klik op het **certificaat downloaden**. Naar succes rewnew uw certificaat, moet u uitvoeren de volgende twee stappen:

    - Het nieuwe certificaat uploaden naar de SaaS-app configuratie voor eenmalige aanmelding scherm. Als u wilt weten hoe u deze stap herhalen voor uw bepaalde SaaS-app, klikt u op **Weergave configuratie-instructies**.

    - In Azure AD, schakel het selectievakje bevestiging onderaan in het dialoogvenster om in te schakelen van het nieuwe certificaat en klik vervolgens op **volgende** om in te dienen.

    > [AZURE.IMPORTANT] Eenmalige aanmelding bij de app wordt uitgeschakeld het moment dat u een van deze twee stappen is voltooid, maar deze opnieuw wordt ingeschakeld zodra de tweede stap is voltooid. Daarom om te beperken downtime, zorg ervoor dat voor het uitvoeren van beide stappen binnen een korte tijd van elkaar verschillen.

    ![Download en het certificaat uploaden](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Op SAML gebaseerde eenmalige aanmelding oplossen](active-directory-saml-debugging.md)
