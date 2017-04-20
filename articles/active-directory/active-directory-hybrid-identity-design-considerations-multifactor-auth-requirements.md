<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - bepalen vereisten voor meervoudige verificatie"
    description="Met voorwaardelijke toegangsbeheer controles Azure Active Directory de specifieke voorwaarden die u bij het verifiëren van de gebruiker en vóór het toestaan van toegang tot de toepassing kiezen. Zodra deze voorwaarden is voldaan, wordt de gebruiker is geverifieerd, en toegang hebben tot de toepassing."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Meervoudige verificatievereisten voor uw hybride identiteit oplossing bepalen

In deze wereld van mobiliteit, met gebruikers toegang hebben tot gegevens en toepassingen in de cloud en vanaf elk apparaat, deze gegevens beveiligen geworden grootste.  Elke dag wordt er een nieuwe kop over een beveiligingsprobleem is.  Hoewel er is geen waarborg tegen dergelijke inbreuk op, biedt meervoudige verificatie, een extra beveiliging om u te helpen voorkomen dat deze inbreuk op.
Eerst de organisaties vereisten voor meervoudige verificatie. Dat wil zeggen, wat is de organisatie wilt beveiligen.  Deze evaluatie is belangrijk dat het definiëren van de technische vereisten voor het instellen en inschakelen van de gebruikers organisaties voor meervoudige verificatie.

>[AZURE.NOTE]
Als u niet bekend met MFA en wat het doet bent, het is raadzaam dat u het artikel lezen [Wat Azure meervoudige verificatie is?](../multi-factor-authentication/multi-factor-authentication.md) voorafgaande kunt doorgaan met lezen in deze sectie.

Zorg ervoor dat u beantwoorden van de volgende handelingen uit:

- Is uw bedrijf bij het beveiligen van Microsoft-apps? 
- Hoe deze apps worden gepubliceerd?
- Biedt uw bedrijf RAS medewerkers voor toegang tot on-premises implementatie apps toe te staan?

Indien Ja: welk type RAS? U moet ook evalueren waar de gebruikers van wie toegang krijgt deze toepassingen tot zich bevindt. Deze evaluatie is een andere belangrijke stap bij het bepalen van de juiste meervoudige verificatie. Zorg ervoor dat de volgende vragen beantwoorden:

- Waar de gebruikers gaat bevinden zich?
- Kunnen ze zich bevinden overal?
- Uw bedrijf tot stand wilt brengen beperkingen op basis van de locatie van de gebruiker?

Als u meer informatie over deze vereisten is voldaan, is het belangrijk is ook van de gebruiker vereisten voor meervoudige verificatie wordt berekend. Deze evaluatie is belangrijk omdat deze de vereisten voor het implementeren van meervoudige verificatie wordt gedefinieerd. Zorg ervoor dat de volgende vragen beantwoorden:

- De gebruikers bekend bent met meervoudige verificatie?
- Enkele toepassingen moeten worden uitgevoerd voor aanvullende verificatie?  
 - Indien Ja: altijd, wanneer die afkomstig zijn van externe netwerken of toegang tot specifieke toepassingen, of andere voorwaarden?
- De gebruikers training over het installeren en implementeren van meervoudige verificatie vereist?
- Wat zijn de belangrijkste scenario's die uw bedrijf wil meervoudige verificatie inschakelen voor hun gebruikers?

Na de vorige vragen te beantwoorden, is mogelijk om te begrijpen als er meervoudige verificatie al geïmplementeerd on-premises implementatie. Deze evaluatie is belangrijk dat het definiëren van de technische vereisten voor het instellen en inschakelen van de gebruikers organisaties voor meervoudige verificatie. Zorg ervoor dat de volgende vragen beantwoorden:

- Heeft uw bedrijf om te beveiligen bevoegdheden accounts met MFA nodig?
- Heeft uw bedrijf om te schakelen voor bepaalde toepassing voor naleving redenen MFA nodig?
- Heeft uw bedrijf MFA inschakelen voor alle in aanmerking komend gebruikers van deze toepassing of alleen beheerders nodig?
- U nodig hebt MFA altijd ingeschakeld of alleen wanneer de gebruikers die zijn aangemeld buiten uw bedrijfsnetwerk?


## <a name="next-steps"></a>Volgende stappen
[Een hybride identiteit aanpassingsstrategie voor definiëren](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)
