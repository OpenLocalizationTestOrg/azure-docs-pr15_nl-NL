<properties
    pageTitle="App-kenmerk gebaseerde inrichting met een bereik instellen filters | Microsoft Azure"
    description="Leer hoe u de scope filters gebruiken om te voorkomen dat de objecten in apps die ondersteuning bieden voor geautomatiseerde gebruiker inrichten daadwerkelijk wordt ingericht als een object niet voldoet aan uw vereisten voor bedrijven."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>App-kenmerk gebaseerde inrichting met een bereik van filters instellen

Het doel van deze sectie is wordt uitgelegd hoe u de scope filters gebruiken om te definiëren kenmerk gebaseerde regels waarmee u kunnen bepalen welke gebruikers ingericht met de toepassing.





## <a name="clauses-and-scope-groups"></a>Componenten en bereikgroepen


![Een bereik filteren instellen][1] 




Scope filters worden gedefinieerd door een of meer **bereikgroepen**, die elk houdt een of meer **componenten**. Als u wilt zien van de clausules voor een bepaald bereik-groep, door te klikken op de pijl links van de naam van de groep uitvouwen.

Een **component** bepaalt u welke gebruikers kunnen de scope filter passeert door de evaluatie van de kenmerken van elke gebruiker. Bijvoorbeeld, kunnen er één component die is vereist dat een gebruikerswachtwoord "staat" kenmerk gelijk is aan New York, wat betekent dat alleen de gebruikers van uw New York in de toepassing ingericht.

![Een bereik van de groepsnaam van de instellen][2] 



Elke **bereikgroep** begint met één verplicht **component**, zoals wordt weergegeven in de bovenstaande schermafbeelding. Deze component aanwezigheidsstatussen eenvoudigweg dat de gebruiker moet eerst worden toegewezen aan de toepassing voordat deze wordt geëvalueerd door uw scope filters. Deze component kan worden verwijderd of gewijzigd.

U kunt nieuwe componenten of nieuwe bereikgroepen toevoegen door op de gewenste knop te drukken. U kunt elke bereikgroep een naam geven door de **Naam van de groep bereik** eigenschap bewerken.





## <a name="how-scoping-filters-are-evaluated"></a>Hoe een bereik instellen Filters worden geëvalueerd

Tijdens het inrichten test we elke toegewezen gebruiker met uw scope filters om te bepalen als die gebruiker toegang tot de toepassing verdient. U kunt zien van elke component als een toets om de gebruiker om te voorkomen ophalen uitgefilterd worden doorgegeven. 

Als er meerdere bereikgroepen die zijn gedefinieerd, moet elke gebruiker ten minste één van deze doorgeven om toegang te krijgen tot de toepassing. Binnen elke bereikgroep echter moet de gebruiker doorgeven elke één-component om te kunnen het doorgeven van die groep specifiek bereik. 

Met andere woorden, u kunt zien van de bereikgroepen als of zou samen en u kunt zien van de clausules daarin als en zou samen. Houd rekening met het onderstaande scope filter bijvoorbeeld:


![Een bereik van de groepsnaam van de instellen][2]  


Op basis van deze scope filter, moeten de gebruikers de volgende criteria voldoet, voldoen om te worden ingericht:

1. Ze moeten worden toegewezen aan de toepassing.

2. Ze moeten werken in de afdeling voor technische?

3. Ze moeten werk zijn in San Francisco of Canada.


##<a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Gebruiker inrichting en Deprovisioning naar SaaS toepassingen automatiseren](active-directory-saas-app-provisioning.md)
- [Kenmerktoewijzingen voor het inrichten van de gebruiker aan te passen](active-directory-saas-customizing-attribute-mappings.md)
- [Expressies voor het toewijzen van kenmerken schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Account inrichting van meldingen](active-directory-saas-account-provisioning-notifications.md)
- [Gebruikmaken van SCIM waarmee automatisch inrichten van gebruikers en groepen van Azure Active Directory naar toepassingen](active-directory-scim-provisioning.md)
- [Lijst met zelfstudies over het integreren van SaaS Apps](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png
