<properties
    pageTitle="Automatische SaaS App-gebruiker inrichting in Azure AD | Microsoft Azure"
    description="Een inleiding over hoe u Azure AD kunt gebruiken om automatisch te stellen, maak inrichten en werkt u continu gebruikersaccounts in verschillende SaaS toepassingen van derden."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Gebruiker inrichting en Deprovisioning naar SaaS toepassingen met Azure Active Directory automatiseren

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Wat is de inrichting van gebruiker wordt automatisch voor SaaS-Apps?

Azure Active Directory (Azure AD) kunt u de maken, onderhoud en verwijderen van de identiteit van de gebruikers in de cloud ([SaaS](https://azure.microsoft.com/overview/what-is-saas/))-toepassingen zoals Dropbox, Salesforce, ServiceNow en meer automatiseren.

**Hieronder vindt u enkele voorbeelden van wat deze functie u kunt doen:**

- Automatisch nieuwe accounts maken in de juiste SaaS-apps voor nieuwe personen wanneer u deelneemt aan uw team.
- Accounts van SaaS apps automatisch deactiveren wanneer personen onvermijdelijk het team verlaat.
- Zorg ervoor dat de identiteiten in uw apps SaaS zijn bijgewerkt op basis van wijzigingen in de adreslijst.
- Niet-gebruikers-objecten, zoals groepen aan SaaS-apps die ondersteuning bieden voor deze inrichten.

**De inrichting van geautomatiseerde gebruiker bevat ook de volgende functionaliteit:**

- De mogelijkheid om te voldoen aan bestaande identiteiten tussen Azure AD en SaaS apps.
- Opties voor aanpassing van Azure AD-Help aanpassen aan de huidige configuraties van de SaaS-apps die voor uw organisatie is momenteel gebruikt.
- Optionele e-mailwaarschuwing voor het inrichten van fouten.
- Rapporteren en activiteit logboeken kunnen helpen met het bewaken en probleemoplossing.

##<a name="why-use-automated-provisioning"></a>Het nut van geautomatiseerde inrichting?

Enkele veelvoorkomende motivaties voor het gebruik van deze functie zijn:

- Om te voorkomen dat de kosten, efficiëntie en menselijke fouten die zijn gekoppeld aan de inrichting van processen handmatig.
- Beveiligen van uw organisatie doordat direct gebruikers identiteiten van belangrijke SaaS apps wanneer ze de organisatie verlaat.
- Eenvoudig importeren van een groot aantal aantal gebruikers in een bepaalde SaaS-toepassing.
- Als u wilt profiteren van hebt u de inrichting van oplossing Speel mensen uit de dezelfde clienttoegangsbeleid app die u hebt gedefinieerd voor Azure AD eenmalige aanmelding.

##<a name="frequently-asked-questions"></a>Veelgestelde vragen

**Hoe vaak Azure AD directorywijzigingen bij de app SaaS schrijven?**

Azure AD Hiermee wordt gecontroleerd op wijzigingen elke vijf tot tien minuten. Als de app SaaS als resultaat verschillende fouten (zoals in het geval van ongeldige beheerdersreferenties), klikt u vervolgens in Azure AD geleidelijk de frequentie naar tot één keer per dag totdat de fouten worden opgelost vertragen.

**Hoe lang duurt het voor het inrichten van mijn gebruikers?**

Incrementele wijzigingen optreden vrijwel direct, maar als u probeert voor het inrichten van grootste deel van uw adreslijst, klikt u vervolgens dit hangt af van het aantal gebruikers en groepen die u hebt. Kleine mappen slechts een paar minuten duren, middelgrote mappen kunnen enkele minuten duren en zeer grote mappen kunnen enkele uren duren.

**Hoe kan ik de voortgang van de huidige inrichten taak bijhouden**

Het rapport inrichting onder het gedeelte rapporten van uw adreslijst, kunt u bekijken. Er is een andere optie te bezoeken van het tabblad Dashboard voor de SaaS-toepassing die u naar zijn inrichting en zoek in het gedeelte "integratiestatus" aan de onderkant van de pagina.

**Hoe weet ik wanneer als gebruikers niet correct krijgen ingericht?**

Aan het einde van de inrichten configuratie is de wizard er een optie voor het abonneren op meldingen per e-mail voor het inrichten van fouten. U kunt ook het rapport fouten inrichting om te zien welke gebruikers kunnen niet worden ingericht controleren en waarom.

**Kan Azure AD schrijven wijzigingen uit de app SaaS terug naar de map?**

De meeste SaaS-apps voor is inrichting alleen uitgaande, wat betekent dat gebruikers naar de toepassing uit de adreslijst geschreven worden en wijzigingen van de toepassing niet terug naar de map kunnen worden geschreven. Voor [werkdag](https://msdn.microsoft.com/library/azure/dn762434.aspx)is inrichting echter inkomende alleen-lezen, wat inhoudt dat die gebruikers in de adreslijst van werkdag zijn geïmporteerd, en ook wijzigingen in de map kunnen niet ophalen terug naar geschreven werkdag.

**Hoe kan ik feedback geven aan het team voor technische?**

Neem contact met ons opnemen via het [forum van Azure Active Directory-feedback](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="how-does-automated-provisioning-work"></a>Hoe werkt automatische inrichten?

Azure AD bepalingen gebruikers tot SaaS apps door te verbinden met inrichting eindpunten verstrekt door elke leverancier. Deze eindpunten toestaan Azure AD via programmacode maken, bijwerken en verwijderen van gebruikers. Hieronder vindt u een beknopt overzicht van de verschillende stappen die Azure AD gaat naar de inrichting van automatiseren.

1. Wanneer u voor een toepassing voor de eerste keer is geïnstalleerd inschakelt, worden de volgende acties uitgevoerd:
 - Azure AD wordt geprobeerd om aan te passen elk bestaande gebruikers in de app SaaS met hun bijbehorende identiteiten in de adreslijst. Een gebruiker is gekoppeld, worden deze *niet* automatisch worden ingeschakeld voor eenmalige aanmelding. In de volgorde van een gebruiker toegang heeft tot de toepassing, moeten ze expliciet toegewezen aan de app in Azure AD rechtstreeks of via groepslidmaatschap.
 - Als u al hebt opgegeven welke gebruikers moeten worden toegewezen aan de toepassing en als Azure AD mislukt om bestaande accounts voor deze gebruikers, wordt nieuwe accounts voor hen door Azure AD in de toepassing inrichten.
2. Nadat de eerste synchronisatie is voltooid zoals hierboven is beschreven, kan Azure AD 10 minuten voor de volgende wijzigingen moeten worden gecontroleerd:
 - Als u nieuwe gebruikers zijn toegewezen aan de toepassing ze zijn (rechtstreeks of via groepslidmaatschap), kan deze is ingericht een nieuw account in de app SaaS.
 - Als de toegang van een gebruiker is verwijderd en hun account in de app SaaS wordt gemarkeerd als uitgeschakeld (gebruikers worden nooit volledig verwijderd, waarin, voorkomt u gegevensverlies in het geval van een onjuiste configuratie).
 - Als een gebruiker onlangs is ingesteld voor de toepassing en ze al een account in de app SaaS, dat account wordt gemarkeerd als ingeschakeld en bepaalde gebruikerseigenschappen kunnen worden bijgewerkt als ze verouderde vergeleken met de adreslijst zijn.
 - Als een gebruikersgegevens (bijvoorbeeld telefoonnummer, kantoorlocatie, enzovoort) is gewijzigd in de adreslijst, wordt worden deze informatie ook bijgewerkt in de toepassing SaaS.

Voor meer informatie over hoe kenmerken tussen Azure AD worden toegewezen en de app SaaS: Zie het artikel over het [Toewijzen van kenmerken aan te passen](active-directory-saas-customizing-attribute-mappings.md).

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Lijst met Apps die ondersteuning bieden voor de inrichting van geautomatiseerde gebruiker

Klik op een app om te zien van een zelfstudie over het configureren van geautomatiseerde voor deze is geïnstalleerd:

- [Vak](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Instemming](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropbox voor bedrijven](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Google Apps](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [SalesForce](http://go.microsoft.com/fwlink/?LinkId=286017)
- [SalesForce-Sandbox](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [Werkdag](http://go.microsoft.com/fwlink/?LinkId=690250) (inkomende inrichting)

In de volgorde voor een toepassing voor de ondersteuning van de inrichting van geautomatiseerde gebruiker, moet deze eerst de benodigde eindpunten die staan voor externe programma's voor het automatiseren van het maken, onderhoud en verwijderen van gebruikers opgeven. Niet alle SaaS apps zijn er daarom compatibel met deze functie. Voor apps die dit ondersteunen, wordt het technische Azure AD-team vervolgens kunnen maken van een inrichten connector aan deze apps en deze werk voorrang krijgt door de behoeften van huidige en potentiële klanten.

Als u wilt contact opnemen met het technische Azure AD-team om aan te vragen inrichten ondersteuning voor extra toepassingen, een bericht door het [forum van Azure Active Directory-feedback](https://feedback.azure.com/forums/169401-azure-active-directory/)te verzenden.

##<a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Kenmerktoewijzingen voor het inrichten van de gebruiker aan te passen](active-directory-saas-customizing-attribute-mappings.md)
- [Expressies voor het toewijzen van kenmerken schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Een bereik van Filters voor het inrichten van de gebruiker instellen](active-directory-saas-scoping-filters.md)
- [Gebruikmaken van SCIM waarmee automatisch inrichten van gebruikers en groepen van Azure Active Directory naar toepassingen](active-directory-scim-provisioning.md)
- [Account inrichting van meldingen](active-directory-saas-account-provisioning-notifications.md)
- [Lijst met zelfstudies over het integreren van SaaS Apps](active-directory-saas-tutorial-list.md)
