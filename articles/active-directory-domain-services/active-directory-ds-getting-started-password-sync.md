<properties
    pageTitle="Azure AD-domeinservices: Wachtwoordsynchronisatie inschakelen | Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Wachtwoordsynchronisatie Azure AD Domain Services inschakelen
In de voorafgaande taken, moet u Azure AD Domain Services voor uw Azure AD-tenant ingeschakeld. De volgende taak is vereist voor NTLM als Kerberos-verificatie voor synchronisatie met Azure AD-domeinservices referentie-hashes inschakelen. Zodra referentie synchronisatie is ingesteld, kunnen gebruikers zich kunnen aanmelden bij het beheerde domein met de referenties van hun bedrijf.

De benodigde stappen voor zijn verschillende op basis van of uw organisatie een Azure AD alleen de cloud heeft-tenant of om te synchroniseren met uw on-premises adreslijst met Azure AD Connect is ingesteld.

<br>

> [AZURE.SELECTOR]
- [Alleen de cloud Azure AD-tenant](active-directory-ds-getting-started-password-sync.md)
- [Azure AD-tenant gesynchroniseerd](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Taak 5: Inschakelen Wachtwoordsynchronisatie AAD Domain Services voor een alleen de cloud Azure AD-tenant
Azure AD-domeinservices moeten referenties hashes in een indeling die geschikt zijn voor NTLM als Kerberos-verificatie, om gebruikers in het beheerde domein te verifiëren. Tenzij u AAD Domain Services voor uw tenant inschakelt, Azure AD niet genereren of referentie hashes opslaan in de indeling voor NTLM of Kerberos-verificatie is vereist. Om beveiligingsredenen duidelijke Azure AD ook niet worden opgeslagen referenties in normale tekstindeling. Azure AD hoeft dus niet een manier om te genereren van deze NTLM of Kerberos referentie-hashes op basis van bestaande referenties van gebruikers.

> [AZURE.NOTE] Als uw organisatie een cloud-lezen heeft Azure AD-tenant, gebruikers die gebruikmaken van Azure AD-domeinservices moeten hun wachtwoord moet wijzigen.

Wachtwoord hierdoor wijzigen de referentie hashes vereist Azure AD-domeinservices voor Kerberos en NTLM-verificatie in Azure AD worden gegenereerd. U kunt een wachtwoorden opnieuw instellen voor alle gebruikers in de tenant die gebruikmaken van Azure AD-domeinservices of opgeven dat deze gebruikers hun wachtwoord kunnen wijzigen moeten zijn verlopen.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Inschakelen NTLM als Kerberos referentie hash generatie voor een alleen de cloud Azure AD-tenant
Hier vindt u instructies dat u nodig hebt om eindgebruikers te bieden, zodat zij hun wachtwoord kunnen wijzigen:

1. Navigeer naar de pagina Azure AD Access Configuratiescherm voor uw organisatie op [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Selecteer het tabblad **profiel** op deze pagina.

3. Klik op de tegel **wachtwoord wijzigen** op deze pagina.

    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Als u de optie **wachtwoord wijzigen** op de pagina toegang Panel niet ziet, controleert u dat uw organisatie [Wachtwoordbeheer in Azure AD](../active-directory/active-directory-passwords-getting-started.md)is geconfigureerd.

4. Typ uw wachtwoord voor bestaande (oude) op de pagina **wachtwoord wijzigen** en typ een nieuw wachtwoord en bevestig dit. Klik op **verzenden**.

    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Nadat u uw wachtwoord hebt gewijzigd, worden het nieuwe wachtwoord binnenkort gebruikt van Azure AD Domain Services. Na een paar minuten (meestal ongeveer 20 minuten), kunt u zich aanmelden bij computers toegevoegd aan het beheerde domein met de zojuist gewijzigde wachtwoord.

<br>

## <a name="related-content"></a>Verwante onderwerpen

- [Hoe u uw eigen wachtwoord ook bijwerken](../active-directory/active-directory-passwords-update-your-own-password.md)

- [Aan de slag met wachtwoordbeheer in Azure AD](../active-directory/active-directory-passwords-getting-started.md).

- [Inschakelen van Wachtwoordsynchronisatie AAD Domain Services voor een gesynchroniseerde Azure AD-tenant](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Een beheerde domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)

- [Deelnemen aan een virtuele Windows-computer naar een beheerde domeinservices van Azure AD-domein](active-directory-ds-admin-guide-join-windows-vm.md)

- [Een rood rol Enterprise Linux virtuele machine te koppelen aan een beheerde domeinservices van Azure AD-domein](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
