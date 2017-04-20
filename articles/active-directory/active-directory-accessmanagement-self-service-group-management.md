<properties
    pageTitle="Azure Active Directory instellen voor selfservice access Toepassingsbeheer | Microsoft Azure"
    description="Selfservice-groepsbeheer kan gebruikers maken en beheren van beveiligingsgroepen of Office 365-groepen in Azure Active Directory en biedt gebruikers de mogelijkheid op aanvraag-beveiligingsgroep of lidmaatschap van de Office 365-groepen"
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
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Azure Active Directory in te stellen voor selfservice groepsbeheer

Selfservice-groepsbeheer kan gebruikers beveiligingsgroepen of Office 365-groepen in Azure Active Directory (Azure AD) maken en beheren. Gebruikers kunnen ook aanvragen beveiligingsgroep of Office 365 groepslidmaatschappen en klikt u vervolgens de eigenaar van de groep kunt goedkeuren of weigeren lidmaatschap. Op deze manier kunt dagelijkse controle over groepslidmaatschap worden overgedragen aan mensen die de context van de bedrijven voor lidmaatschap begrijpen. Selfservice-groep management functies beschikbaar zijn alleen voor beveiligingsgroepen en Office 365-groepen, maar niet voor beveiligingsgroepen met e-mail groepen of distributielijsten.

Selfservice-groepsbeheer momenteel bestaat uit twee essentiële scenario's: gedelegeerde groepsbeheer- en groepsbeheer zelf.

- **Groepsbeheer van de gedelegeerd** 
   een voorbeeld is een beheerder die toegang tot een SaaS-toepassing die van het bedrijf gebruikmaakt beheert. Beheer van de volgende toegangsrechten wordt omslachtig, zodat deze beheerder wordt gevraagd om de eigenaar van het bedrijf een nieuwe groep maken. De beheerder wordt toegewezen toegang voor de toepassing naar de nieuwe groep en toegevoegd aan de groep alle personen die al toegang tot de toepassing. Eigenaar van het bedrijf vervolgens meer gebruikers kunt toevoegen en deze gebruikers automatisch met de toepassing is ingericht. Eigenaar van het bedrijf hoeft niet te wachten om door de beheerder voor het beheren van toegang voor gebruikers. Als de beheerder worden dezelfde machtiging aan een manager in een andere business-groep, toegewezen en vervolgens die persoon toegang voor hun eigen gebruikers ook kan beheren. Eigenaar van het bedrijf noch de manager kunt weergeven of elkaars gebruikers beheren. De beheerder kan nog steeds zien voor alle gebruikers die toegang tot de toepassing en blokkeren toegangsrechten hebben indien nodig.

- **Selfservice-groepsbeheer** 
   een voorbeeld van dit scenario is twee gebruikers die beide hebben van SharePoint Online-sites die ze zich afzonderlijk instellen. Ze willen elkaars teams toegang geven tot de sites. Ze kunnen hiervoor een groep maken in Azure AD en in SharePoint Online elk van deze groep voor toegang tot de sites selecteren. Wanneer iemand toegang wil, ze dit uit het deelvenster toegang aanvraagt en na goedkeuring ze toegang krijgen tot beide SharePoint Online-sites automatisch. Een van deze later besluit dat alle personen die toegang hebben tot de site ook toegang tot een bepaalde SaaS-toepassing krijgen moeten. De beheerder van de toepassing SaaS kunt toegangsrechten voor de toepassing op de SharePoint Online-site toevoegen. Verzoeken die krijgen goedgekeurd biedt toegang daarna de twee SharePoint Online-sites en ook in deze SaaS-toepassing.

## <a name="making-a-group-available-for-end-user-self-service"></a>Een groep maken beschikbaar voor eindgebruikers Self-service

1. Open het telefoonboek van uw Azure AD in de [klassieke Azure-portal](https://manage.windowsazure.com).

2. Klik op het tabblad **configureren** stelt u **gedelegeerd groepsbeheer** op ingeschakeld.

3. Stelt **gebruikers beveiligingsgroepen kunnen maken** of **Office-groepen of gebruikers mogen maken** op ingeschakeld.

Wanneer **dat gebruikers beveiligingsgroepen kunnen maken** is ingeschakeld, worden alle gebruikers in uw adreslijst zijn toegestaan maken van nieuwe beveiligingsgroepen en leden toevoegen aan deze groepen. Deze nieuwe groepen zou ook weergegeven in het deelvenster Access voor alle andere gebruikers. Als de instelling is ingeschakeld op de groep dit toestaat, kunnen andere gebruikers aanvragen voor deelname aan deze groepen maken. Als **gebruikers beveiligingsgroepen kunnen maken** is uitgeschakeld, kunnen gebruikers momenteel geen groepen maken en bestaande groepen waarvoor ze een eigenaar zijn niet wijzigen. Ze kunnen echter nog steeds de lidmaatschappen van deze groepen beheren en goedkeuren aanmeldingsaanvragen van andere gebruikers te nemen aan de groepen.

U kunt ook **gebruikers wie kunnen zelf te kunnen voor beveiligingsgroepen gebruiken** om een meer fijnmazige toegangscontrole over Self-service groepsbeheer voor uw gebruikers. Wanneer **dat groepen of gebruikers mogen maken** is ingeschakeld, worden alle gebruikers in uw adreslijst zijn toegestaan maken van nieuwe groepen en leden toevoegen aan deze groepen. Door in te stellen **wie kunnen zelf te kunnen voor beveiligingsgroepen gebruiken gebruikers** ook deel, u bent beperken management om slechts een beperkt aantal gebruikers te groeperen. Wanneer deze schakeloptie is ingesteld op enkele, moet u gebruikers toevoegen aan de groep SSGMSecurityGroupsUsers voordat ze kunnen maken van nieuwe groepen en leden aan deze toevoegen. Door in te stellen **wie kunnen zelf te kunnen voor beveiligingsgroepen gebruiken gebruikers** op alle, kunt u alle gebruikers in uw adreslijst te maken van nieuwe groepen inschakelen.

U kunt ook het vak **groep die de beschikking over self-service voor beveiligingsgroepen** gebruiken om op te geven van een aangepaste naam voor een groep waarvan de leden kunnen zelf te kunnen gebruiken.

## <a name="additional-information"></a>Aanvullende informatie

Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Toegang tot resources met Azure Active Directory-groepen beheren](active-directory-manage-groups.md)

* [Azure Active Directory-cmdlets voor het configureren van de groepsinstellingen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)

* [Wat is Azure Active Directory?](active-directory-whatis.md)

* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
