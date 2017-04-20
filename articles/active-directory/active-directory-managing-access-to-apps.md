<properties
  pageTitle="Toegang tot de apps met behulp van Azure AD beheren |  Microsoft Azure"
  description="Hierin wordt beschreven hoe Azure Active Directory kunnen organisaties om op te geven van de apps waartoe elke gebruiker toegang heeft."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Toegang tot de apps beheren

Doorlopende toegang management, gebruik evaluatie en rapportage blijven een uitdaging nadat een app is geïntegreerd in systeem van de identiteit van uw organisatie. In veel gevallen moet IT-beheerders of helpdesk uitvoeren een lopend actieve rol in de toegang tot uw apps beheren. Soms wordt toewijzing uitgevoerd door een algemene of divisie IT-team. Vaak de beschikking toewijzing is bedoeld om u te worden overgedragen naar de zakelijke beslissing maker, vereisen van hun goedkeuring IT heeft de toewijzing.  Andere organisaties investeren in integratie met een bestaand geautomatiseerde identiteits- en toegangsbeheer management systeem, zoals Rolgebaseerd toegangsbeheer RBAC () of toegangsbeheer op basis van het kenmerk (ABAC). De integratie en de ontwikkeling van de regel zijn meestal gespecialiseerde en duur. Cmdlets voor controle of op beide methode management reporting is een eigen afzonderlijke, duur en complexe investering.

## <a name="how-does-azure-active-directory-help"></a>Hoe kan Azure Active Directory beter?

 Azure AD ondersteunt de uitgebreide toegangsbeheer voor geconfigureerde toepassingen, zodat organisaties gemakkelijk de juiste clienttoegangsbeleid die variëren van automatische, op basis van het kenmerk toewijzing (ABAC of RBAC scenario's) tot en met de overdracht en met inbegrip van beheer door de systeembeheerder. Met Azure AD u kunt eenvoudig complexe beleid bereiken combineren van meerdere management modellen voor één toepassing en management regels opnieuw zelfs in toepassingen met de dezelfde doelgroepen kunt gebruiken.

 - [Nieuwe toevoegen of bestaande toepassingen](active-directory-sso-integrate-saas-apps.md)


 Azure AD-toepassing toewijzing ligt de nadruk op twee primaire toewijzing modi:

- **Afzonderlijke toewijzing** Een IT-beheerder met hoofdbeheerdersmachtigingen directory kan afzonderlijke gebruikersaccounts te selecteren en ze toegang verlenen tot de toepassing.
- **Op basis van een groep toewijzing (Azure AD alleen betaald)** Een IT-beheerder met mapmachtigingen globale beheerder kunt u een groep toewijzen aan de toepassing. Specifieke gebruikers toegang wordt bepaald door de vraag of ze lid zijn van de groep op het moment dat zij proberen toegang tot de toepassing. Beheerders kunnen met andere woorden, effectief maken voor een toewijzingsregel met de melding "alle leden van de toegewezen groep heeft toegang tot de toepassing". Met deze optie toewijzing kunnen beheerders profiteren van Azure AD groep management opties, waaronder [dynamische groepen kenmerk gebaseerde](active-directory-accessmanagement-manage-groups.md), extern systeemgroepen (bijvoorbeeld lokale Active Directory of werkdag) of beheerder wordt beheerd of zelfstandige-verkeer beheerd groepen. Een groep kan worden eenvoudig toegewezen aan meerdere apps, ervoor te zorgen dat toepassingen met toewijzing affiniteit toewijzingsregels voor kunnen delen minder complex algehele beheer. Houd er rekening mee dat geneste groep lidmaatschappen worden niet ondersteund voor de toewijzing tot aan de toepassingen op basis van een groep op dit moment.

Deze twee toewijzing modi gebruikt, kunnen beheerders van een benadering van de management wenselijk toewijzing bereiken.

Met Azure AD is gebruik en rapportage van de toewijzing volledig geïntegreerd, om beheerders eenvoudig rapporteren van toewijzing staat, toewijzingsfouten en zelfs gebruik.

## <a name="complex-application-assignment-with-azure-ad"></a>Toewijzing van complexe toepassingen met Azure AD

Houd rekening met een toepassing zoals Salesforce. In veel organisaties Salesforce hoofdzakelijk wordt gebruikt door de marketing- en -organisaties. Leden van de marketingactiviteit team hebben toegang tot Salesforce, vaak veel bevoegdheden terwijl leden van het verkoopteam beperkte toegang hebben. In veel gevallen een brede populatie van informatiewerkers toegangsbeperkingen voor de toepassing. Uitzonderingen op deze regels bemoeilijken zaken. Dit is het meestal het recht van de marketing of verkoop leiderschap teams gebruikerstoegang geven of te wijzigen van hun rol afzonderlijk van deze algemene regels.

Azure AD-toepassingen zoals Salesforce worden vooraf geconfigureerde voor eenmalige aanmelding (SSO) en de inrichting van geautomatiseerde. Wanneer de toepassing is geconfigureerd, kan een beheerder de eenmalige actie maken en toewijzen van de juiste groepen duren. In dit voorbeeld, kan een beheerder van de volgende toewijzingen uitvoeren:

- [Dynamische groepen](active-directory-accessmanagement-manage-groups.md) kan bevatten automatisch alle leden van de marketing- en teams met kenmerken zoals rol of afdeling te worden gedefinieerd:

    - Alle leden van marketing groepen wordt toegewezen aan de 'marketing' rol in Salesforce

    - Alle leden van verkoopteam groepen wordt toegewezen aan de "verkoop" rol in Salesforce. Een verdere verfijning kan meerdere groepen die overeenkomen met regionale verkoop teams die zijn toegewezen aan verschillende Salesforce-rollen gebruiken.

- Als u wilt dat de uitzondering om, kan een groep selfservice worden gemaakt voor elke rol. De groep "Salesforce marketing uitzondering" kan bijvoorbeeld worden gemaakt als een groep zelf. De groep kan worden toegewezen aan de marketingactiviteit Salesforce-rol en de marketingactiviteit leiderschap-team eigenaren kan worden aangebracht. Leden van de marketingactiviteit leiderschap-team kunnen toevoegen of verwijderen van gebruikers, instellen van een join-beleid, of zelfs goedkeuren of weigeren individuele gebruikers aanvragen voor deelname aan. Dit wordt ondersteund door een informatie werknemer juiste ervaring waarvoor geen speciale training voor eigenaren of leden vereist.

In dit geval alle toegewezen gebruikers zou worden automatisch deze is ingericht naar Salesforce, zoals ze zijn toegevoegd aan verschillende groepen die hun roltoewijzing zou zijn bijgewerkt in Salesforce. Gebruikers zou kunnen opsporen en Salesforce openen via de toepassing toegang Configuratiescherm van Microsoft Office web-clients, of zelfs door te gaan naar de aanmeldingspagina van hun organisatie-Salesforce. Beheerders zou kunnen gemakkelijk weergeven en toewijzing de status met Azure AD rapportage.

Beheerders kunnen gebruikmaken van [voorwaardelijke toegang Azure AD](active-directory-conditional-access.md) -beleid voor bepaalde rollen instellen. Dit beleid kunnen opnemen of toegang is toegestaan buiten de bedrijfsomgeving en zelfs meervoudige verificatie of apparaat vereisten voor het bereiken van access in verschillende gevallen.

## <a name="how-can-i-get-started"></a>Hoe kan ik aan de slag?

Eerste, als u geen Azure AD al gebruikt en u een IT-beheerder bent:

 - [Probeer het zelf!](https://azure.microsoft.com/trial/get-started-active-directory/) -kunt u zich registreren voor een gratis proefabonnement 30 dagen op vandaag en implementeren van uw eerste cloud-oplossing in onder 5 minuten via deze koppeling

Azure AD-functies waarmee account delen zijn:

- [Toewijzing aan een groep](active-directory-accessmanagement-self-service-group-management.md)
- Azure AD-toepassingen toevoegen
- Aan de slag met de toewijzing
- Toepassing toewijzing Veelgestelde vragen
- [App dashboard/gebruiksrapporten](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Waar vind ik meer informatie?

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Apps gebruiken met voorwaardelijke toegang beveiligen](active-directory-conditional-access.md)
- [Selfservice-groep management/SSAA](active-directory-accessmanagement-self-service-group-management.md)
