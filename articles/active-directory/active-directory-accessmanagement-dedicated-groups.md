<properties
    pageTitle="Groepen in Azure Active Directory specifiek | Microsoft Azure"
    description="Overzicht van hoe speciale groepen werken in Azure Active Directory en hoe ze worden gemaakt."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="dedicated-groups-in-azure-active-directory"></a>Speciale groepen in Azure Active Directory

In Azure Active Directory (Azure AD), de functie speciale groepen automatisch gemaakt en wordt gevuld lidmaatschap voor Azure AD vooraf gedefinieerde groepen. Leden van speciale groepen kan niet worden toegevoegd of verwijderd via de portal van Azure klassieke, Windows PowerShell-cmdlets, of via een programma.

>[AZURE.NOTE] Speciale groepen vereisen dat een Azure AD Premium-licentie is toegewezen aan
>- de beheerder die worden beheerd in de regel op een groep
>- alle gebruikers die zijn geselecteerd door de regel moet lid zijn van de groep

**Speciale groepen inschakelen**

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com)Selecteer **Active Directory**en open vervolgens de adreslijst van uw organisatie.

2. Selecteer het tabblad **groepen** en open vervolgens de groep die u wilt bewerken.

3. Selecteer het tabblad **configureren** en stel vervolgens **Specifiek groepen inschakelen** op **Ja**.

Als de schakeloptie speciale groepen inschakelen is ingesteld op **Ja**, verder kunt u de map automatisch de speciale groep alle gebruikers maken door in te stellen de **inschakelen 'Iedereen' groep** overschakelen op **Ja**. U kunt vervolgens ook bewerken de naam van deze speciale groep door te typen in het **weergavenaam voor 'Iedereen' groep** veld.

De groep alle gebruikers kan worden gebruikt voor dezelfde machtigingen toewijzen aan alle gebruikers in uw adreslijst. Bijvoorbeeld, kunt u alle gebruikers in uw adreslijst toegang tot een toepassing voor SaaS verlenen toegang voor de speciale groep alle gebruikers toewijzen aan deze toepassing.

De speciale groep alle gebruikers bevat alle gebruikers in de map, inclusief gasten en externe gebruikers. Als u een groep nodig hebt die worden genegeerd externe gebruikers en kunt u dit doen door te maken van een groep met een kenmerk gebaseerde dynamische regel zoals het volgende:

                (user.userPrincipalName -notContains "#EXT#@")

Voor een groep die alle gasten sluit, moet u een regel zoals de volgende gebruiken:

                (user.userType -ne "Guest")

Zie voor meer informatie over het maken van *Geavanceerde* regels (regels die meerdere vergelijkingen kunnen bevatten) voor dynamische groepslidmaatschap, [werken met kenmerken om geavanceerde regels te maken](active-directory-accessmanagement-groups-with-advanced-rules.md).


Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Toegang tot resources met Azure Active Directory-groepen beheren](active-directory-manage-groups.md)
* [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
* [Wat is Azure Active Directory?](active-directory-whatis.md)
* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
