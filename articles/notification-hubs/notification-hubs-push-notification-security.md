<properties
    pageTitle="Beveiliging voor melding Hubs"
    description="In dit onderwerp wordt uitgelegd beveiliging voor Azure melding hubs."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Beveiliging

##<a name="overview"></a>Overzicht

In dit onderwerp wordt het beveiligingsmodel van Azure melding Hubs beschreven. Aangezien melding Hubs een Service Bus entiteit, implementeert ze de dezelfde beveiligingsmodel als Service Bus. Zie de [Service Bus verificatie](https://msdn.microsoft.com/library/azure/dn155925.aspx) -onderwerpen voor meer informatie.

##<a name="shared-access-signature-security-sas"></a>Beveiliging in gedeelde Access handtekening (SA's) 

Melding Hubs implementeert een kleurenschema beveiliging op gebruikersniveau entiteit genoemd SA's (gedeeld Access handtekening). Dit schema kunt SMS entiteiten maximaal 12 autorisatieregels in de beschrijving die op die entiteit machtigen declareren.

Elke regel bevat een naam, de waarde van een sleutel (gedeelde geheim) en een reeks rechten, zoals beschreven in de sectie "Beveiliging Claims." Wanneer u een melding-Hub maakt, twee regels worden gemaakt: een met luisteren rechten (die de client-app wordt gebruikt) en een met alle rechten (met de app-end).

Bij het uitvoeren van het beheer van de registratie van client-apps, als de gegevens die zijn verzonden meldingen is niet gevoelige (bijvoorbeeld weer updates), een gebruikelijke manier voor toegang tot een Hub melding wordt de waarde van de sleutel van de regel beluisteren van alleen-lezen toegang geven tot de client-app, en worden de sleutelwaarde van de regel volledige toegang geven tot de app-end.

Het wordt niet aanbevolen dat u de sleutelwaarde in de Windows Store-clienttoepassingen insluiten. Er is een manier om te voorkomen dat de sleutelwaarde insluiten dat de client-app opgehaald van de app-end bij het opstarten.

Het is belangrijk om te begrijpen dat een client-app om u te registreren voor een label in de toets met luisteren toegang is toegestaan. Als uw app moet registraties beperken tot specifieke labels aan specifieke clients (bijvoorbeeld wanneer tags vertegenwoordigen gebruikers-id's), moet de registraties uitvoeren op uw app backend. Zie beheer van de registratie voor meer informatie. Houd er rekening mee dat op deze manier de client-app niet direct toegang tot melding Hubs hebben wordt.

##<a name="security-claims"></a>Beveiliging op claims

Net als andere entiteiten, melding Hub bewerkingen zijn toegestaan voor drie beveiliging claims: beluisteren, verzenden en beheren.

| Claimen | Beschrijving | Bewerkingen die zijn toegestaan |
|-------|-------------|--------------------|
| Luisteren | Maken of bijwerken, lezen en verwijderen van één registraties | Registratie maken of bijwerken<br><br>Registratie lezen<br><br>Alle registraties voor grip lezen<br><br>Registratie verwijderen |
| Verzenden | Berichten verzenden op de melding-hub | Bericht verzenden |
| Beheren | CRUDs op melding Hubs (inclusief bijwerken PNS referenties en beveiliging toetsen), en lees registraties op basis van labels | Maken, bijwerken of gelezen/verwijderen melding hubs<br><br>Lezen van registraties per label |


Melding Hubs accepteren claims verleend door Microsoft Azure-toegangsbeheer tokens en handtekening tokens die zijn gegenereerd met gedeelde sleutels rechtstreeks voor de Hub melding hebt geconfigureerd.