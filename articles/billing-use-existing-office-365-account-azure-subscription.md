<properties
    pageTitle="Delen van één Azure AD-tenant in Office 365 en Azure-abonnementen | Microsoft Azure"
    description="Meer informatie over het delen van uw Office 365 Azure AD-tenant en de gebruikers met uw Azure-abonnement of omgekeerd"
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Een bestaand Office 365-account aan uw Azure-abonnement of vice versa gebruiken
Scenario: U al een Office 365-abonnement hebt en gereed zijn voor een Azure-abonnement, maar u wilt de bestaande Office 365-gebruikersaccounts gebruiken voor uw Azure-abonnement. U kunt ook u Azure abonnee bent en wilt ontvangen van een Office 365-abonnement voor de gebruikers in uw bestaande Azure Active Directory. In dit artikel leest u hoe makkelijk het is om te bereiken beide.

> [AZURE.NOTE] In dit artikel niet van toepassing op klanten Enterprise Agreement (EA). Als u meer hulp op een willekeurige plaats in dit artikel, [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) nodig om het probleem snel opgelost.


## <a name="quick-guidance"></a>Snelle hulp

- Als u al een Office 365-abonnement hebt en u wilt registreren voor Azure, gebruikt u de optie **aanmelden met uw organisatie-account** . Vervolgens verdergaan met de procedure voor het Azure aanmelden met uw Office 365-account. Zie [gedetailleerde stappen verderop in dit artikel](#s1).

- Als u al een Azure-abonnement hebt en u wilt ontvangen van een Office 365-abonnement, meld u aan bij Office 365 met uw Azure-account. Ga vervolgens verder met de aanmeldingsstappen uitvoeren. Nadat u de registratie hebt voltooid, wordt de Office 365-abonnement wordt toegevoegd aan dezelfde Azure Active Directory-sessie waartoe uw Azure abonnement behoort. Zie de sectie [gedetailleerde stappen verderop in dit artikel](#s2)voor meer informatie.

>[AZURE.NOTE] Als u een Office 365-abonnement, het account dat u gebruikt voor aanmelding moet lid zijn van de globale beheerder of beheerder facturering directory rol in uw Azure Active Directory-tenant. [Meer informatie over het bepalen van de rol in Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

Als u wilt weten wat er gebeurt wanneer u een abonnement aan een account toevoegen, raadpleegt u de [achtergrondinformatie later in dit artikel](#background-information).

## <a name="detailed-steps"></a>Gedetailleerde stappen
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Scenario 1: Office 365-gebruikers die u wilt kopen Azure
In dit scenario we wordt ervan uitgegaan dat Kelley Wall is een gebruiker aan wie Office 365-abonnement heeft en is van plan om u te abonneren op Azure. Er zijn twee aanvullende actieve gebruikers, Jane en Tricia. Kelley van account admin@contoso.onmicrosoft.com.

![Beheercentrum van Office 365-gebruiker](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Om u te registreren voor Azure, als volgt te werk:

1. Registreren voor Azure op [Azure.com](https://azure.microsoft.com/). Klik op **gratis proberen**. Klik op de volgende pagina op **nu starten**.

    ![Azure gratis proberen.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Klik op **aanmelden met uw organisatie-account**.

    ![Meld u aan bij Azure.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Aanmelden met uw Office 365-account. In dit geval is Kelley van Office 365-account.

    ![Aanmelden met uw Office 365-account.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Vul de gegevens en het aanmeldingsproces voltooien.

    ![Gegevens in te vullen en de registratie hebt voltooid.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Klik op Start het beheren van mijn service.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Nu u verder te doen. Klik in de portal Azure ziet u dezelfde gebruikers weergegeven. U kunt dit controleren, als volgt te werk:

1. Klik op **begin mijn service beheren** in het scherm eerder weergegeven.
2. Klik op **Bladeren**en klik vervolgens op **Active Directory**.

    ![Klik op Bladeren en klik vervolgens op Active Directory.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klik op **gebruikers**.

    ![Het tabblad gebruikers](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Alle gebruikers, waaronder Kelley, worden weergegeven zoals verwacht.

    ![Lijst met gebruikers](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>Scenario 2: Azure gebruikers die Office 365 te kopen plannen

In dit scenario Kelley Wall een gebruiker met een Azure-abonnement onder het account is admin@contoso.onmicrosoft.com. Kelley wil abonnement op Office 365 en gebruiken van dezelfde map als die ze al met Azure.

>[AZURE.NOTE] Als u een Office 365-abonnement, moet het account dat u voor aanmelden gebruikt lid zijn van de globale beheerder of beheerder facturering directory rol in uw Azure Active Directory-tenant. [Leer hoe u de rol in Azure Active Directory weten](#how-to-know-your-role-in-your-azure-active-directory).

![Instellingen voor Azure portal abonnement](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Portal Azure Active Directory-gebruikers](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Abonneren op Office 365, als volgt te werk:

1. Ga naar de [pagina Office 365-product](https://products.office.com/business)en selecteer vervolgens een abonnement die geschikt is voor u.
2. Nadat u het abonnement hebt geselecteerd, wordt de volgende pagina wordt weergegeven. Vul het formulier. Klik op **aanmelden** in de rechterbovenhoek van de pagina.

    ![Pagina van Office 365-proefabonnement](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Meld u aan met referenties van uw account. In dit voorbeeld is het Kelley van account.

    ![Office 365-aanmelding](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Klik op **nu proberen**.

    ![Controleer uw bestelling voor Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Klik op de pagina bestelling ontvangstbevestiging op **Continue**.

    ![Office 365 ontvangst](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Nu u verder te doen. In het Office 365-beheercentrum ziet u gebruikers in de directory van Contoso weergegeven als actieve gebruikers. U kunt dit controleren, als volgt te werk:

1. Open de Office 365-beheercentrum.
2. Vouw **gebruikers**en klik vervolgens op **Actieve gebruikers**.

    ![Office 365 admin center-gebruikers](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Hoe weet ik uw rol in uw Azure Active Directory

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **Bladeren**en klik vervolgens op **Active Directory**.

    ![Active Directory in de portal van Azure](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klik op **gebruikers**.

    ![Azure portal standaard Active Directory-gebruikers](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Klik op de gebruiker. In dit voorbeeld wordt de gebruiker Kelley Wall.

    Zoals u ziet het veld van de **Rol binnen de organisatie**.

    ![Azure portal gebruikers-id](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Achtergrondinformatie over Azure en Office 365-abonnementen
Office 365 en Azure gebruik van de Azure Active Directory-service voor het beheren van gebruikers en abonnementen. Houd rekening met een Azure-directory als een container waarin u gebruikers en abonnementen kunt groeperen. Als u wilt dezelfde gebruikersaccount gebruiken voor uw Azure en Office 365-abonnementen, moet u zorgen dat de abonnementen in dezelfde map worden gemaakt. Houd er rekening mee met de volgende punten:

- Een abonnement wordt gemaakt onder een map en de andere leren kennen.
- Gebruikers deel uitmaakt van mappen, niet andersom.
- Een abonnement terechtkomt in de adreslijst van de gebruiker die het abonnement maakt. Hierdoor worden uw Office 365-abonnement is gekoppeld aan hetzelfde account als uw Azure abonnement wanneer u dat account gebruikt voor het maken van de Office 365-abonnement.

![Achtergrondinformatie](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Zie [hoe Azure-abonnementen zijn gekoppeld aan Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)voor meer informatie.

>[AZURE.NOTE] Azure abonnementen zijn eigendom van individuele gebruikers in de adreslijst.

>[AZURE.NOTE] Office 365-abonnementen zijn eigendom van de map zelf. Als gebruikers binnen de adreslijst de vereiste machtigingen hebt, kunnen ze worden toegepast op de volgende abonnementen.

## <a name="next-steps"></a>Volgende stappen
Als u uw Azure en de Office 365-abonnementen apart hebt aangeschaft in het verleden en u kunnen wilt voor toegang tot de Office 365-tenant vanaf het Azure abonnement, raadpleegt u het [koppelen van een Office 365-tenant met een Azure-abonnement](billing-add-office-365-tenant-to-azure-subscription.md).

> [AZURE.NOTE] Als u nog vragen, [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel opgelost.
