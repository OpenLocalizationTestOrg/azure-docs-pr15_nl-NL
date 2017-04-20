<properties
    pageTitle="Azure Active Directory Domain Services: Handleiding voor het beheer | Microsoft Azure"
    description="Een organisatie-eenheid op Azure AD-domeinservices beheerde domeinen maken"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Maken van een organisatie-eenheid op een beheerde domeinservices van Azure AD-domein
Azure AD-domeinservices beheerde domeinen bevatten twee ingebouwde containers respectievelijk 'AADDC Computers' en 'AADDC Users' genoemd. De container 'AADDC Computers' heeft computerobjecten voor alle computers die zijn gekoppeld aan de beheerde domein. De container 'AADDC Users' bevat gebruikers en groepen in de Azure AD-tenant. Soms deze mogelijk moet u serviceaccounts maken in het beheerde domein werkbelasting implementeren. U kunt hiertoe maken van een aangepaste organisatie-eenheid in het beheerde domein en serviceaccounts binnen de organisatie-eenheid maken. In dit artikel leest u het maken van een organisatie-eenheid in uw beheerde domein.


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>AD beheerprogramma's installeren op een domein behoren virtuele machine voor extern beheer
Azure AD-domeinservices beheerde domeinen kunnen extern worden beheerd via vertrouwde Active Directory beheerprogramma's zoals Active Directory administratieve Center (ADAC) of AD PowerShell. Beheerders van de tenant hebben geen bevoegdheden verbinding maken met domeincontrollers in het beheerde domein via Extern bureaublad. Als u wilt de beheerde domein beheert, de functie AD beheer van de hulpmiddelen voor op een virtuele machine toegevoegd aan het beheerde domein te installeren. Raadpleeg het artikel [een Azure AD-domeinservices beheerde domein beheert](active-directory-ds-admin-guide-administer-domain.md) voor instructies.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Maak een organisatie-eenheid in de beheerde domein
Nu dat de AD beheerprogramma's zijn geïnstalleerd op het domein VM die zijn gekoppeld, we kunnen deze hulpmiddelen gebruiken om te maken van een organisatie-eenheid op de beheerde domein. De volgende stappen uitvoeren:

> [AZURE.NOTE] Alleen leden van de ' AAD domeincontroller ' beheerdersgroep hebben de vereiste machtigingen om te maken van een aangepaste organisatie-eenheid. Zorg ervoor dat u de volgende stappen uitvoeren als een gebruiker die deel uitmaakt van deze groep.

1. Klik op **Systeembeheer**vanuit het startscherm. Hier ziet u de AD beheerprogramma's geïnstalleerd op de virtuele machine.

    ![Beheerprogramma's op de server is geïnstalleerd](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klik op **Beheerderscentrum voor Active Directory**.

    ![Active Directory-Beheerderscentrum](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Als u wilt bekijken van het domein, klikt u op de domeinnaam in het linkerdeelvenster (bijvoorbeeld ' contoso100.com').

    ![ADAC - weergave domein](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. Klik op **Nieuw** onder het knooppunt van de naam in het deelvenster rechts **taken** . In dit voorbeeld we klikt u op **Nieuw** onder het knooppunt 'contoso100(local)' in het deelvenster rechts **taken** .

    ![ADAC - nieuwe organisatie-eenheid](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. Hier ziet u de optie voor het maken van een organisatie-eenheid. Klik op **Organisatie-eenheid** om te starten van het dialoogvenster **Organisatie-eenheid maken** .

6. Geef in het dialoogvenster **Organisatie-eenheid maken** een **naam** voor de nieuwe OU. Geef een korte beschrijving voor de organisatie-eenheid. U kunt ook het veld **Beheerd door** instellen voor de organisatie-eenheid. Klik op **OK**om de aangepaste organisatie-eenheid.

    ![ADAC - OU dialoogvenster maken](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. De zojuist gemaakte OU moet nu worden weergegeven in de AD administratieve Center (ADAC).

    ![ADAC - OU gemaakt](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>Machtigingen/beveiliging voor nieuwe organisatie-eenheden
Standaard wordt de gebruiker (lid van de ' AAD domeincontroller ' beheerdersgroep) die gemaakt van de aangepaste OU beheerdersbevoegdheden (volledig beheer) verleend via de organisatie-eenheid. De gebruiker kan vervolgens verdergaan en bevoegdheden verlenen aan andere gebruikers of aan de ' AAD domeincontroller ' beheerdersgroep naar wens. Zoals gezien in de volgende schermafbeelding, de gebruiker 'bob@domainservicespreview.onmicrosoft.com' die de nieuwe 'MyCustomOU' organisatie-eenheid hebt gemaakt, wordt de volledige controle over deze worden verleend.

 ![ADAC - nieuwe OU-beveiliging](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>Notities over het beheren van aangepaste organisatie-eenheden
U kunt nu dat u een aangepaste OU hebt gemaakt, gaat u verder en maken van gebruikers, groepen, computers en serviceaccounts in deze organisatie-eenheid. U kunt gebruikers of groepen van de 'AAD domeincontroller gebruikers' OU niet verplaatsen naar de aangepaste organisatie-eenheden.

> [AZURE.WARNING] Gebruikersaccounts, groepen, serviceaccounts en computerobjecten die u onder aangepaste OU's maakt zijn niet beschikbaar in uw Azure AD-tenant. Met andere woorden, weergeven deze objecten niet met behulp van de Azure AD Graph-API of in de gebruikersinterface van Azure AD. Deze objecten zijn alleen beschikbaar in uw beheerde domeinservices van Azure AD-domein.


## <a name="related-content"></a>Verwante onderwerpen

- [Een beheerde domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)

- [Active Directory-Beheerderscentrum: Aan de slag](https://technet.microsoft.com/library/dd560651.aspx)

- [Serviceaccounts, stapsgewijze handleiding](https://technet.microsoft.com/library/dd548356.aspx)
