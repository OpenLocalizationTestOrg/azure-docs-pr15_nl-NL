<properties
    pageTitle="Azure AD-domeinservices: Maak de beheerdersgroep AAD domeincontroller | Microsoft Azure"
    description="Aan de slag met Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="get-started-with-azure-ad-domain-services"></a>Aan de slag met Azure AD-domeinservices

In dit artikel worden doorlopen de configuratietaken uit te voeren vereist voor het inschakelen van Azure AD Domain Services voor uw Azure AD-tenant.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Taak 1: Maak de ' AAD domeincontroller ' beheerdersgroep
De eerste taak is een beheergroep maken in uw Azure Active Directory-tenant. Deze speciale beheergroep heet **AAD domeincontroller beheerders**. Leden van deze groep krijgen beheerdersbevoegdheden voor computers die domein-toegevoegd aan het beheerde domeinservices van Azure AD-domein zijn. Klik op een domein behoren machines, wordt deze groep toegevoegd aan de beheerdersgroep. Leden van deze groep kunt ook extern bureaublad extern verbinding maken met de computers domein behoren.  

> [AZURE.NOTE] U hebt niet de beheerder van het domein of Ondernemingsadministrator bevoegdheden in het beheerde domein gemaakt met behulp van Azure AD Domain Services. Klik op beheerde domeinen, wordt met deze bevoegdheden zijn gereserveerd door de service en zijn niet beschikbaar gemaakt voor gebruikers in de tenant. U kunt echter de speciale beheerdersgroep gemaakt in deze configuratietaak sommige bevoegdheden bewerkingen uit te voeren. Deze bewerkingen bevatten computers toevoegen aan het domein, van de beheerdersgroep op domein behoren computers, het configureren van de groep beleid, enzovoort.

In deze configuratietaak die u kunt de beheergroep maken en een of meer gebruikers in uw adreslijst toevoegen aan de groep. De volgende stappen om te maken van de beheergroep voor Azure AD-domeinservices uitvoeren:

1. Ga naar de **klassieke Azure-portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. Selecteer het knooppunt **Active Directory** in het linkerdeelvenster.

3. Selecteer de Azure AD-tenant (directory) waarvoor u wilt inschakelen Azure AD Domain Services. U kunt alleen een domein voor elke Azure AD-map maken.

    ![Selecteer Azure AD-map](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klik op het tabblad **groepen** .

5. Als u wilt een groep toevoegen aan uw Azure AD-tenant, klikt u op **Groep toevoegen** vanuit het taakvenster onder aan de pagina.

    ![Knop groep toevoegen](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. Maak een groep met de naam **AAD domeincontroller beheerders**. **GROEPSTYPE** ingesteld op **beveiliging**.

    > [AZURE.WARNING] Voor toegang binnen uw Azure AD-domeinservices beheerd domein, maakt u een groep met deze exacte naam.

    ![Beheerdersgroep maken](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Voeg een beschrijving voor deze groep zodat anderen begrijpen dat deze groep wordt gebruikt voor beheerdersbevoegdheden binnen Azure AD Domain Services.

8. Nadat u de groep hebt gemaakt, klikt u op de naam van de groep om de eigenschappen van deze groep weer te geven. Als gebruikers wilt toevoegen als leden van deze groep, klikt u op de knop **leden toevoegen** op de onderkant van de verpakking.

    ![De knop groeperen leden toevoegen](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. Selecteer de gebruikers die moeten worden leden van deze groep en schakel het selectievakje in als u klaar bent in het dialoogvenster **leden toevoegen** .

    ![Gebruikers toevoegen aan de beheerdersgroep](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Taak 2: Maken of een Azure virtuele netwerk selecteren
De volgende configuratietaak is [gemaakt](active-directory-ds-getting-started-vnet.md)of Selecteer een Azure virtuele netwerk.
