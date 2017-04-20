<properties 
    pageTitle="Extern bureaublad Gateway en Azure meervoudige verificatieserver RADIUS gebruiken"
    description="Dit is de pagina van de Azure meervoudige verificatie die helpt bij het implementeren van extern bureaublad (RD) Gateway en Azure meervoudige verificatieserver RADIUS gebruiken."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Extern bureaublad Gateway en Azure meervoudige verificatieserver RADIUS gebruiken

In veel gevallen wordt de lokale NPS in extern bureaublad-Gateway gebruikt om gebruikers te verifiëren. In dit document wordt beschreven hoe om te leiden RADIUS-verzoek om af van de extern bureaublad-Gateway (via de lokale NPS) op de meervoudige verificatie-Server.

De meervoudige verificatie-Server moeten worden geïnstalleerd op een afzonderlijke server, waarin wordt vervolgens proxy de RADIUS-aanvraag terug naar de NPS op het externe bureaublad Gateway-Server. Wanneer NPS is gevalideerd met de gebruikersnaam en wachtwoord, wordt een antwoord op de Server van meervoudige verificatie waarmee de tweede factor verificatie voordat u een resultaat terugkeren naar de gateway geretourneerd.





## <a name="configure-the-rd-gateway"></a>De RD-Gateway configureren

De RD-Gateway moet worden geconfigureerd RADIUS-verificatie verzenden naar een Server Azure meervoudige verificatie. Zodra de RD-Gateway is geïnstalleerd, geconfigureerd en werkt, Ga naar de RD-Gateway-eigenschappen. Ga naar het tabblad RD Initiaal archief en wijzigen, zodat een centrale server met NPS in plaats van de lokale server met NPS kan gebruiken. Voeg een of meer Azure meervoudige verificatieservers als RADIUS-servers en een gedeeld geheim voor elke server opgeven.





## <a name="configure-nps"></a>NPS configureren

De RD-Gateway wordt NPS gebruikt de RADIUS-aanvraag om naar te verzenden Azure meervoudige verificatie. Een time-out moet worden gewijzigd om te voorkomen dat de RD-Gateway van time-out voordat meervoudige verificatie is voltooid. Gebruik de volgende procedure laten.

1. In NPS uitvouwen van het menu RADIUS-Clients en Server in de linkerkolom en klik op externe RADIUS-Server-groepen. Ga naar de eigenschappen van de groep TS GATEWAY-SERVER. Bewerk de RADIUS-servers weergegeven en Ga naar het tabblad taakverdeling. Wijzig de "aantal seconden zonder reactie voordat aanvraag wordt beschouwd als tekst' en het 'aantal seconden tussen verzoeken als server wordt geïdentificeerd als niet beschikbaar' op 30-60 seconden. Klik op het tabblad verificatie-Account en zorg ervoor dat de RADIUS-poorten overeenkomen met de poorten die de meervoudige verificatie-Server wordt luisteren.
2. NPS moet ook worden geconfigureerd RADIUS-authenticatie ontvangen weer van de Azure meervoudige verificatie-Server. Klik op RADIUS-Clients in het linkermenu. De Server Azure meervoudige verificatie toevoegen als een RADIUS-client. Kies een beschrijvende naam en een gedeeld geheim opgeven.
3. Vouw de sectie beleidsregels in het linker navigatiegedeelte op en klik op verbinding beleid. Een verbinding aanvragen beleid TS GATEWAY AUTORISATIEBELEID dat is gemaakt tijdens de configuratie van RD Gateway moet bevatten. Dit beleid doorstuurt RADIUS-aanvragen naar de meervoudige verificatie-Server.
4. Kopieer dit beleid om een nieuwe record maken. In het nieuwe beleid toevoegen een voorwaarde die overeenkomt met de beschrijvende naam van de Client met de beschrijvende naam voor de Azure meervoudige verificatie Server RADIUS-client instellen in stap 2 hierboven. De verificatieprovider wijzigen in de lokale Computer. Dit beleid zorgt ervoor dat wanneer een aanvraag RADIUS is ontvangen van de Server Azure meervoudige verificatie, de verificatie doet zich lokaal in plaats van het verzenden van een RADIUS-verzoek terug naar de Server Azure meervoudige verificatie die in een lusvoorwaarde resulteert. Als u wilt voorkomen dat de lusvoorwaarde, moet dit nieuwe beleid worden besteld boven het oorspronkelijke beleid waarmee worden doorgestuurd naar de meervoudige verificatie-Server.

## <a name="configure-azure-multi-factor-authentication"></a>Azure meervoudige verificatie configureren


--------------------------------------------------------------------------------



De Azure meervoudige verificatie-Server is geconfigureerd als een RADIUS-proxy tussen RD-Gateway en NPS.  Deze moet worden geïnstalleerd op een domein behoren server die losstaat van de RD-Gateway-server. Gebruik de volgende procedure voor het configureren van de Server Azure meervoudige verificatie.

1. Open de Server Azure meervoudige verificatie en klik op het pictogram RADIUS-verificatie. Schakel het selectievakje in inschakelen RADIUS-verificatie.
2. Klik op het tabblad Clients controleert u de poorten overeenkomen met wat is geconfigureerd in NPS en klikt u op de toevoegen... knop. Het IP-adres van de RD-Gateway-server, de naam van de toepassing is (optioneel) en een gedeeld geheim toevoegen. Het gedeelde geheim moet gelijk zijn op de Server van Azure meervoudige verificatie en de RD-Gateway.
3. Klik op het tabblad doel en kies het keuzerondje van RADIUS-server (s).
4. Klik op de toevoegen... knop. Voer het IP-adres, gedeelde geheim en poorten van de NPS-server. Tenzij een centrale NPS gebruikt, wordt de RADIUS-client en de RADIUS doelsites hetzelfde zijn. Het gedeelde geheim moet overeenkomen met de één-instelling in het gedeelte van de client RADIUS van de NPS-server.

![RADIUS-verificatie](./media/multi-factor-authentication-get-started-server-rdg/radius.png)
