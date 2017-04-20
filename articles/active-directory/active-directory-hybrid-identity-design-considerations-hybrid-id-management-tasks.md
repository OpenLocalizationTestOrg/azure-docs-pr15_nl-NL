<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - beheertaken voor hybride identiteit bepalen | Microsoft Azure"
    description="Met voorwaardelijke toegangsbeheer controles Azure Active Directory de specifieke voorwaarden die u bij het verifiëren van de gebruiker en vóór het toestaan van toegang tot de toepassing kiezen. Zodra deze voorwaarden is voldaan, wordt de gebruiker is geverifieerd, en toegang hebben tot de toepassing."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="plan-for-hybrid-identity-lifecycle"></a>Plan voor de levenscyclus van de hybride-identiteit 

Identiteit is een van de basis van uw strategie voor enterprise mobiliteit en -toepassing. Of u naar uw mobiele apparaat of SaaS-app op ondertekent, is uw identiteit de toets om toegang tot alles. Op het hoogste niveau omsluit een oplossing voor identiteitsbeheer verenigen en synchroniseren tussen uw identiteit opslagplaatsen waaronder automatiseren en centraal opslaan van het proces voor het inrichten van resources. De identiteit-oplossing moet een gecentraliseerde identiteit over on-premises en cloud en ook enkele vorm van identiteitsfederatie gebruiken voor het behoud van gecentraliseerde verificatie en veilig delen en samenwerken met externe gebruikers en bedrijven. Resources variëren van besturingssystemen en toepassingen tot mensen binnen of verbonden met een organisatie. Organisatiestructuur kan worden gewijzigd zodat het inrichten beleid en procedures.

Het is ook van belang dat u over een oplossing afgestemd op uw gebruikers door te voorzien selfservice ervaringen om ze te houden productief. Uw identiteit-oplossing is krachtiger als kunt eenmalige aanmelding voor gebruikers over alle resources die zij nodig hebben toegang beheerders op alle niveaus gestandaardiseerde procedures voor het beheer van gebruikersreferenties kunnen gebruiken. Beheer op bepaalde niveaus kunnen worden beperkte of is verwijderd, afhankelijk van de breedte van de beheeroplossing voor het inrichten. Bovendien kunt u veilig distribueren beheermogelijkheden, handmatig of automatisch, tussen verschillende organisaties. De domeinbeheerder van een kan bijvoorbeeld dienen alleen de personen en bronnen in dat domein. Deze gebruiker inrichten en administratieve taken kunt uitvoeren, maar het is niet geautoriseerd moet configuratietaken uit te voeren, zoals het maken van werkstromen.


## <a name="determine-hybrid-identity-management-tasks"></a>Beheertaken voor hybride identiteit bepalen
Beheertaken in uw organisatie distribueren verbetert de nauwkeurigheid en de effectiviteit van beheer en verbetert de saldo van de werklast van een organisatie. Hier volgen de draaitabellen die een robuuste identiteitsbeheersysteem definiëren.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


Als u wilt definiëren beheertaken voor hybride identiteit, moet u enkele hoofdkenmerken van de organisatie die vast te hybride identiteit stellen begrijpen. Het is belangrijk is voor meer informatie over de huidige opslagplaatsen wordt gebruikt voor identiteit bronnen. Als u weet die core-elementen, wordt de moeten beschikken over vereisten en op basis van dat u moet meer gedetailleerde vragen die u tot een betere functionaliteit voor uw identiteit-oplossing leiden.  

Terwijl u definieert die vereisten, zorg ervoor dat ten minste de volgende vragen worden beantwoord.

- Configuratieopties: 
 - Ondersteunt de hybride identiteit oplossing een access-robuuste accountbeheer en inrichtingssysteem?
 - Hoe zijn gebruikers, groepen en wachtwoorden, gaat u naar de inhoud beheerd?
 - Is de levenscyclus van identiteitsbeheer heeft gereageerd? 
      - Hoe lang duurt wachtwoord updates account wordt geschorst?
      
- Licentiebeheer: 
 - De hybride oplossing grepen licentie identiteitsbeheer betekent?
     - Indien Ja: is welke mogelijkheden zijn beschikbaar?
- Wordt de oplossing op basis van een groep Licentiebeheer verwerkt? 
      -Indien Ja, is het mogelijk een beveiligingsgroep toewijzen aan deze? 
       -Indien Ja, wordt de map cloud automatisch licenties toewijzen aan alle leden van de groep? 
        -Wat gebeurt er als een gebruiker is vervolgens toegevoegd aan of uit de groep verwijderen, wordt een licentie automatisch toegewezen of uw besturingssysteem verwijderd? 

- Integratie met andere identiteitsprovider van derden:
- Kan deze hybride oplossing worden geïntegreerd met identiteitsprovider van derden implementeren eenmalige aanmelding?
- Is het mogelijk alle andere identiteitsproviders consistent in een systeem samenhangende identiteit?
- Indien Ja: hoe en welke zijn ze en welke mogelijkheden zijn beschikbaar?

## <a name="synchronization-management"></a>Beheer van de synchronisatie
Een van de doelstellingen van de manager van een identiteit, kunnen alle identiteitsprovider brengen en kunt houden gesynchroniseerd. U de gegevens die zijn gesynchroniseerd houden op basis van een gezaghebbende basispagina identiteitsprovider. In een hybride identiteit scenario, met een gesynchroniseerde beheermodel, die u kunt alle gebruiker en apparaat identiteiten beheren in een on-premises implementatie-server en synchroniseert de accounts en eventueel wachtwoorden in de cloud. De gebruiker de hetzelfde wachtwoord on-premises implementatie invoert als hij of zij heeft in de cloud en bij het aanmelden, het wachtwoord door de oplossing identiteit is geverifieerd. Dit model gebruikt een hulpprogramma directory-synchronisatie.
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)BEGINLETTERS ontwerp de synchronisatie van uw identiteit hybride oplossing Zorg ervoor dat de volgende vragen worden beantwoord: • wat zijn de synchronisatie-oplossingen die beschikbaar zijn voor de identiteit hybride oplossing?
• Wat de eenmalige aanmelding mogelijkheden die beschikbaar zijn?
• Wat zijn de opties voor identiteitsfederatie tussen B2B en B2C?

## <a name="next-steps"></a>Volgende stappen
[Hybride identiteit management aanpassingsstrategie voor bepalen](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)

