<properties
   pageTitle="Azure Active Directory-B2B samenwerking | Microsoft Azure"
   description="Azure Active Directory-B2B samenwerking kan bedrijven partners toegang tot uw zakelijke toepassingen, met elk van hun gebruikers dat wordt aangeduid met een enkel Azure AD-account"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory-B2B samenwerking

Azure Active Directory (Azure AD) B2B samenwerking kunt u toegang tot uw zakelijke toepassingen inschakelen uit identiteiten partner worden beheerd. U kunt intern relaties maken door nodigen en machtiging van gebruikers van partnerbedrijven voor toegang tot uw resources. Complexiteit minder omdat elk bedrijf federates eenmaal met Azure Active Directory en elke gebruiker wordt aangeduid met één Azure AD-account. De beveiliging is vergroot als uw zakenpartners hun accounts in Azure AD beheert omdat toegang is ingetrokken wanneer gebruikers van de partner van hun organisaties worden beëindigd en onbedoelde toegang via lidmaatschap van interne adreslijsten wordt verhinderd. Voor zakelijke partners die geen Azure AD al hebben, heeft B2B samenwerking via een gestroomlijnde aanmelding ervaring te leveren van Azure AD-accounts zakenpartners.

-   Uw zakenpartners gebruik hun eigen aanmeldingsproblemen-referenties, wat hoeft u uit een externe partner directory beheren en uit de hoeft te verwijderen van access wanneer gebruikers de partnerorganisatie verlaten.

-   U de toegang tot uw apps afzonderlijk van levenscyclus van uw zakelijke-partner-account beheren. Dit betekent, bijvoorbeeld: u kunt toegang intrekken zonder om te vragen de IT-afdeling van uw zakelijke partner te doen.

## <a name="capabilities"></a>Mogelijkheden

B2B samenwerking vereenvoudigd beheer en verbetert de beveiliging van de partnertoegang tot bedrijfsresources inclusief SaaS-apps zoals Office 365, Salesforce Azure Services en elke mobile, cloud en on-premises implementatie claims-toepassing. B2B samenwerking kunt partners hun eigen account beheren en ondernemingen beveiligingsbeleid voor apparaten met partner access kunnen toepassen.

Azure Active Directory-B2B samenwerking eenvoudig is te configureren met vereenvoudigd aanmelden voor partners allerlei, zelfs als ze hun eigen Azure Active Directory via een e-geverifieerd-proces niet hebben. Het is ook gemakkelijk te onderhouden met geen externe mappen of per partner Federatie configuraties.

## <a name="b2b-collaboration-process"></a>B2B samenwerkingsproces

1. Azure AD B2B samenwerking kan de bedrijfsbeheerder van een uitnodigen en een verzameling externe gebruikers in een bestand met door komma's gescheiden waarden (CSV) niet meer dan 2000 regels uploaden naar de portal voor samenwerking B2B toestaan.

  ![Dialoogvenster voor CSV-bestand uploaden](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. De portal worden verzonden e-mailuitnodigingen voor deze externe gebruikers.

3. De uitgenodigde gebruiker wordt u aanmelden bij een bestaand werkaccount met Microsoft (beheerd in Azure AD) of een nieuw werkaccount in Azure AD ophalen.

4. Zodra aangemeld, wordt de gebruiker omgeleid naar de app die met hen is gedeeld.

Uitnodigingen voor consumenten e-mailadressen (zoals Gmail of [*comcast.net*](http://comcast.net/)) worden momenteel niet ondersteund.

Bekijk [deze video](http://aka.ms/aadshowb2b)voor meer informatie over de werking van B2B samenwerking.

## <a name="next-steps"></a>Volgende stappen
Onze andere artikelen op Azure AD B2B samenwerking bladeren.

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Werkwijze](active-directory-b2b-how-it-works.md)
- [Gedetailleerd overzicht](active-directory-b2b-detailed-walkthrough.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Externe gebruiker token opmaken](active-directory-b2b-references-external-user-token-format.md)
- [Wijzigingen van de externe gebruiker object kenmerken](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Huidige preview beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
