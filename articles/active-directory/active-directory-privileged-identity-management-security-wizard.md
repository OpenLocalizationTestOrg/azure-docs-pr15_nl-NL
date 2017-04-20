<properties
   pageTitle="De wizard Azure AD bevoegdheden identiteitsbeheer beveiliging"
   description="De eerste keer dat u de extensie Azure Active Directory bevoegdheden identiteitsbeheer, gebruikt u krijgt een beveiligingswizard. In dit artikel worden de stappen beschreven voor het gebruik van de wizard."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>De wizard Azure AD bevoegdheden identiteitsbeheer beveiliging

Als u de eerste persoon om uit te voeren Azure bevoegdheden identiteit Management (PIM) voor uw organisatie, u krijgt een wizard. De wizard kunt u het beveiligingsrisico van bevoegdheden identiteiten en hoe u deze risico's verminderen met PIM kent. U hoeft te Breng eventuele wijzigingen aan bestaande roltoewijzingen in de wizard als u liever later doen.

## <a name="what-to-expect"></a>Wat u kunt verwachten

Voordat uw organisatie kunt u met PIM starten, alle roltoewijzingen zijn permanent: de gebruikers hebben altijd een deze rollen zelfs als ze hun bevoegdheden heb momenteel niet nodig.  De eerste stap van de wizard ziet u een lijst met veel rechten rollen en hoeveel gebruikers momenteel beschikbaar zijn in deze functies. U kunt inzoomen naar een bepaalde rol voor meer informatie over gebruikers als er een of meer tabelstijlen niet bekend bent.

De tweede stap van de wizard kunt u een van de beheerder roltoewijzingen wijzigen.  

> [AZURE.WARNING]Het is belangrijk dat u ten minste één globale beheerder en meer dan één bevoegdheden rol beheerder zijn met een organisatieaccount (niet een Microsoft-account hebt). Als er slechts één bevoegdheden rol beheerder, is de organisatie niet mogelijk om te beheren PIM als dat account wordt verwijderd.
> Ook, houd roltoewijzingen permanente als een gebruiker een Microsoft-account (een account dat zij gebruiken heeft voor het aanmelden bij Microsoft-services, zoals Skype en Outlook.com). Als u van plan bent MFA vereisen voor activering voor die rol, wordt die gebruiker toegang heeft.


Nadat u wijzigingen hebt aangebracht, wordt de wizard niet meer weergegeven. De volgende keer dat u of een andere bevoegdheden rol beheerder gebruiken PIM, ziet u het dashboard PIM.  

- Als u wilt toevoegen of verwijderen van gebruikers rol of toewijzingen van permanente naar in aanmerking komen, gelezen meer bij [het toevoegen of verwijderen van de functie van een gebruiker](active-directory-privileged-identity-management-how-to-add-role-to-user.md)wijzigen.
- Zie voor meer informatie als u meer gebruikers toegang geven wilt tot het beheren van PIM, hoe [u toegang geven tot beheren in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
