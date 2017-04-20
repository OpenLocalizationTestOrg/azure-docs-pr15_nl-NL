<properties
    pageTitle="Meld u invoegtoepassingen uit verschillende gebieden"
    description="Een rapport dat wordt aangegeven waar twee Meld u invoegtoepassingen voor gebruikers weergegeven om te zijn afkomstig uit verschillende regio's en de tijd tussen het teken die ins maakt het kan de gebruiker hebt afgelegde tussen regio's niet meer."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-from-multiple-geographies"></a>Aanmeldingen uit verschillende gebieden

Dit rapport bevat succesvolle aanmeldingen van een gebruiker waar twee aanmeldingen leken te zijn afkomstig uit verschillende regio's en de tijd tussen de aanmeldingen maakt het kan de gebruiker hebt afgelegde tussen regio's niet meer. Oorzaken hebben:

- Gebruikers deelt hun wachtwoord met andere gebruikers

- Gebruiker is via een extern bureaublad starten een webbrowser voor aanmelden

- Een hacker heeft aangemeld bij het account van een gebruiker van een ander land

- Gebruiker is via een VPN- of -proxy

- Gebruiker is aangemeld op meerdere apparaten op hetzelfde moment, zoals een bureaublad en een mobiele telefoon, en het IP-adres van de mobiele telefoon is ongebruikelijk.

Resultaten van dit rapport ziet u de geslaagde aanmeldingsproblemen gebeurtenissen, samen met de tijd tussen de aanmeldingen de regio's waar de aanmeldingen afkomstig van te zijn weergegeven en de tijd geschatte reizen tussen regio's. De tijd reizen is alleen een schatting en kan afwijken van de tijd werkelijke reizen tussen de locaties.


![Meld u invoegtoepassingen uit verschillende gebieden](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)
