<properties
    pageTitle="Invoegtoepassingen voor zich na meerdere fouten"
    description="Een rapport dat wordt aangegeven gebruikers die zich heeft aangemeld na meerdere achtereenvolgende aanmelding in pogingen mislukte."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-after-multiple-failures"></a>Aanmeldingen na meerdere fouten
Dit rapport geeft aan gebruikers die zich heeft aangemeld na meerdere opeenvolgende aanmelding bij pogingen mislukte. Oorzaken hebben:

- Gebruiker had hun wachtwoord vergeten</li><li>Gebruiker bevindt zich de slachtoffer van een succesvolle wachtwoord raden basis aanval afdwingen

Resultaten van dit rapport ziet u het aantal achtereenvolgende mislukte aanmeldingsproblemen pogingen vóór de succesvolle aanmelden en een tijdstempel dat is gekoppeld aan de eerste succesvolle aanmeldingsproblemen.

**Rapportinstellingen**: U kunt het minimum aantal achtereenvolgende mislukte aanmelding configureren in pogingen die moeten worden uitgevoerd voordat deze kan worden weergegeven in het rapport. Wanneer u wijzigingen in deze instelling aanbrengt is het belangrijk te weten dat deze wijzigingen worden niet toegepast op een bestaande mislukte aanmelding ins dat momenteel worden weergegeven in uw bestaand rapport. Ze worden echter worden toegepast op alle toekomstige aanmeldingen. Dit rapport kan alleen worden gewijzigd door gelicentieerde beheerders.


![Invoegtoepassingen voor zich na meerdere fouten](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)
