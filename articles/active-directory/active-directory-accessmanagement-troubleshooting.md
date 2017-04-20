
<properties
    pageTitle="Problemen met dynamische lidmaatschap voor groepen | Microsoft Azure"
    description="Tips voor probleemoplossing voor dynamische lidmaatschap voor groepen in Azure AD."
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


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Problemen met dynamische lidmaatschappen voor groepen

**Ik een regel voor een groep hebt geconfigureerd, maar geen lidmaatschappen bijgewerkt in de groep**<br/>Controleer of de instelling **inschakelen gedelegeerd groepsbeheer van de** is ingesteld op **Ja** in het tabblad **configureren** . U kunt deze instelling wilt zien, alleen als u bent aangemeld als een gebruiker aan wie een Azure Active Directory Premium-licentie is toegewezen. Controleer de waarden voor de kenmerken van de gebruiker op de regel: zijn er gebruikers die voldoen aan de regel?

**Ik een regel hebt geconfigureerd, maar nu de bestaande leden van de regel worden verwijderd**<br/>Dit is normaal. Bestaande leden van de groep worden verwijderd wanneer een regel is ingeschakeld of gewijzigd. De gebruikers die zijn geretourneerd uit evaluatie van de regel worden toegevoegd als lid aan de groep.     

**Ik zie niet met het lidmaatschap wijzigingen direct als ik toevoegen of wijzigen van een regel waarom niet?**<br/>Speciale lidmaatschapsevaluatie klaar is geregeld in een achtergrondproces asynchroon. Hoe lang duurt het proces wordt bepaald door het aantal gebruikers in uw adreslijst en de grootte van de groep gemaakt als gevolg van de regel. Mappen met een klein aantal gebruikers ziet meestal de groepslidmaatschap wijzigingen in minder dan een paar minuten. Mappen met een groot aantal gebruikers kunnen 30 minuten duren voordat of langer om te vullen.

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Toegang tot resources met Azure Active Directory-groepen beheren](active-directory-manage-groups.md)
* [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
* [Wat is Azure Active Directory?](active-directory-whatis.md)
* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
