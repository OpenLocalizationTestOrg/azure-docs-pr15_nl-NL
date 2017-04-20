<properties
    pageTitle="Gebruikers toewijzen aan een aangepast domein in Azure Active Directory | Microsoft Azure"
    description="Hoe u een aangepast domein in Azure Active Directory met gebruikersaccounts vullen."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>Gebruikers toewijzen aan een aangepast domein

Nadat u uw aangepaste domein hebt toegevoegd met Azure Active Directory, moet u de gebruikersaccounts voor dit domein toevoegen, zodat u kunt beginnen met het verifiëren van deze.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Gebruikers die zijn gesynchroniseerd uit een map op uw bedrijfsnetwerk

Als u hebt al een verbinding tussen uw on-premises Active Directory en Azure Active Directory instellen, kan de accounts de synchronisatie invullen. Zie de [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md)voor meer informatie over het Azure Active Directory synchroniseren met uw lokale Active Directory.

## <a name="users-added-and-managed-in-the-cloud"></a>Gebruikers toegevoegd en wordt beheerd in de cloud

Het domein dat voor een bestaand gebruikersaccount wijzigen:

1.  Open de Azure klassieke portal met een account dat is een globale beheerder of een gebruiker.

2.  Open de map.

3.  Selecteer het tabblad **gebruikers** .

4.  Selecteer de gebruiker in de lijst.

5.  Wijzig het domein dat voor de gebruiker en selecteer vervolgens **Opslaan**.

U kunt ook hiervoor via [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) of de [Grafiek-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Een aangepast domein selecteren als u een nieuwe gebruiker

1.  Open de Azure klassieke portal met een account dat is een globale beheerder of een gebruiker.

2.  Open de map.

3.  Selecteer het tabblad **gebruikers** .

4.  Selecteer in de opdrachtenbalk **toevoegen**.

5.  Wanneer u de naam van de gebruiker toevoegt, kiest u het aangepaste domein in de domeinlijst.

6.  Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

-   [Aangepaste domeinnamen gebruiken om u te vereenvoudigen de aanmeldervaring voor uw gebruikers](active-directory-add-domain.md)

-   [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md)

-   [Meer informatie over de basisbeginselen over het beheer van domein in Azure AD](active-directory-add-domain-concepts.md)
