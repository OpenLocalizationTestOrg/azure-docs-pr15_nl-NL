<properties
    pageTitle="Azure Active Directory voorwaardelijke toegang Veelgestelde vragen over | Microsoft Azure"
    description="Veelgestelde vragen over voorwaardelijke toegang "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Azure Active Directory voorwaardelijke toegang Veelgestelde vragen

## <a name="which-applications-work-with-conditional-access-policies"></a>Welke toepassingen werken met voorwaardelijke clienttoegangsbeleid?

**A:** Zie het onderwerp [voorwaardelijke access-What-toepassingen worden ondersteund](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Voorwaardelijke clienttoegangsbeleid, worden afgedwongen voor B2B samenwerken en gastgebruikers?

**A:** Beleidsregels worden voor B2B samenwerking gebruikers afgedwongen. Echter in sommige gevallen, een gebruiker niet mogelijk om te voldoen aan de voorwaarde als, bijvoorbeeld een organisatie niet ondersteunt meervoudige verificatie. 

Het beleid is momenteel niet voor gebruikers van SharePoint gasten afgedwongen. De relatie Gast behouden in SharePoint. Gastgebruikers accounts zijn niet onderhevig aan access beleid op de verificatieserver. Gasttoegang kan worden beheerd op SharePoint.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Is een SharePoint Online beleid ook van toepassing OneDrive voor bedrijven?

**A:** Ja.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Waarom kan niet ik geen beleid instellen voor de client-apps, zoals Word of Outlook?

**A:** Een voorwaardelijke-beleid Hiermee stelt u de vereisten voor toegang tot een service en wordt toegepast wanneer de verificatie gebeurt er met deze service. Het beleid is niet ingesteld rechtstreeks aan een clienttoepassing; in plaats daarvan wordt toegepast wanneer deze bij een service inbelt. Bijvoorbeeld een beleid is ingesteld op SharePoint is van toepassing op klanten bellen van SharePoint en een beleid is ingesteld op Exchange is van toepassing op Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Geldt er een voorwaardelijke-beleid voor serviceaccounts?

**A:** Voorwaardelijke-beleid van toepassing op alle gebruikersaccounts. Dit geldt ook voor gebruikersaccounts die worden gebruikt als serviceaccounts. In veel gevallen kan een serviceaccount die wordt uitgevoerd zonder toezicht niet voldoen aan een beleid. Dit is, bijvoorbeeld het geval is, wanneer MFA is vereist. In dat geval kunnen de serviceaccounts worden uitgesloten van een beleid, met voorwaardelijke management beleidsinstellingen. Meer informatie over het toepassen van een beleid voor gebruikers hier.
