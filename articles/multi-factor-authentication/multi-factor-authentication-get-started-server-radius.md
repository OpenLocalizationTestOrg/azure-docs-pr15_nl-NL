<properties 
    pageTitle="RADIUS-verificatie en Azure meervoudige verificatie-Server"
    description="Dit is de pagina van de Azure meervoudige verificatie die helpt bij het implementeren van RADIUS-verificatie en Azure meervoudige verificatie-Server."
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



# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>RADIUS-verificatie en Azure meervoudige verificatie-Server

De sectie RADIUS-verificatie kunt u inschakelen en configureren van RADIUS-verificatie voor de Server Azure meervoudige verificatie. RADIUS is een standaard protocol verificatieaanvragen worden geaccepteerd en deze aanvragen verwerken. De Server Azure meervoudige verificatie fungeert als een RADIUS-server en tussen de RADIUS-client (bijvoorbeeld VPN toestel) en uw doel voor verificatie, die AD (Active Directory), een LDAP-diagram of een andere RADIUS-server, zijn kan om te kunnen toevoegen Azure meervoudige verificatie wordt ingevoegd. Voor Azure meervoudige verificatie-functie, moet u de Server Azure meervoudige verificatie configureren zodat deze met zowel de client-servers als doel voor verificatie communiceren kan. De Server Azure meervoudige verificatie accepteert verzoeken van een RADIUS-client, referenties vergeleken met het verificatiedoel is gevalideerd, wordt toegevoegd Azure meervoudige verificatie en stuurt een reactie terug naar de RADIUS-client. De hele verificatie slagen alleen als de zowel de primaire verificatie en de Azure meervoudige verificatie is gelukt.

>[AZURE.NOTE]
>De Server MFA ondersteunt alleen PAP (wachtwoord verificatieprotocol) en MSCHAPv2 (Microsofts uitdaging-handshakeverificatieprotocol) RADIUS protocollen als fungeert als een RADIUS-server.  Andere protocollen zoals EAP (extensible verificatieprotocol) kunnen worden gebruikt wanneer de server MFA worden verwerkt als een RADIUS-proxy naar een andere RADIUS-server die ondersteuning biedt voor dat protocol zoals Microsoft NPS.
></br>
>Wanneer u met andere protocollen in deze configuratie, werkt één richting SMS en EED tokens niet omdat de Server MFA niet een positief RADIUS uitdaging antwoord dat protocol met initiëren.


![RADIUS-verificatie](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="radius-authentication-configuration"></a>Configuratie van RADIUS-verificatie

Als u wilt configureren RADIUS-verificatie, installeert u de Server Azure meervoudige verificatie op een Windows server. Als er een Active Directory-omgeving, moet de server worden toegevoegd aan het domein binnen het netwerk. Gebruik de volgende procedure voor het configureren van de Server Azure meervoudige verificatie:

1. Klik op het pictogram RADIUS-verificatie in het linkermenu binnen de Server Azure meervoudige verificatie.
2. Schakel het selectievakje in inschakelen RADIUS-verificatie.
3. Klik op het tabblad Clients de verificatie-poorten en wijzigen financieel poorten als de service Azure meervoudige verificatie RADIUS met niet-standaard poorten verbindt moet te luisteren naar RADIUS-aanvragen van de clients die worden geconfigureerd.
4. Klik op toevoegen... knop.
5. Voer in het dialoogvenster toevoegen RADIUS-Client het IP-adres van de toestel/de server die wordt geverifieerd naar de Server Azure meervoudige verificatie, (optioneel) op de naam van een toepassing en een gedeeld geheim. Het gedeelde geheim moet niet hetzelfde zijn op de Server van Azure meervoudige verificatie en toestel/server. De naam van de toepassing wordt weergegeven in Azure meervoudige verificatie-rapporten en kan worden weergegeven in SMS of Mobile-App verificatie-berichten.
6. Schakel het selectievakje meervoudige verificatie vereisen gebruiker overeenkomst in als u alle gebruikers zijn of worden geïmporteerd naar de Server en kosten voor meervoudige verificatie. Als een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of worden vrijgesteld van meervoudige verificatie, laat u het vak uitgeschakeld. Zie het help-bestand voor meer informatie over deze functie.
7. Schakel het selectievakje inschakelen fallback EED token in als gebruikers de verificatie van de mobiele app Azure meervoudige verificatie wordt gebruikt en u wilt gebruiken EDE wachtwoorden als een fallback verificatie op de melding van gesprek, SMS of push out-van-band-telefoon.
8. Klik op de knop OK.
9. Stap 4 tot en met 8 extra RADIUS-clients toevoegen, kunt u herhalen.
10. Klik op het tabblad doel.
11. Als de Server Azure meervoudige verificatie is geïnstalleerd op een server die een domein zijn gekoppeld in een omgeving voor Active Directory, selecteert u de Windows-domein.
12. Als gebruikers moeten worden geverifieerd ten opzichte van een LDAP-diagram, selecteert u LDAP-binding. Wanneer u met de LDAP-binding, moet u op het pictogram Directory-integratie en de LDAP-configuratie op het tabblad Instellingen bewerken, zodat de Server met uw adreslijst verbinden kunt. Instructies voor het configureren van LDAP kunt u vinden in de LDAP-Proxy-configuratie-handleiding.
13. Als gebruikers moeten worden geverifieerd ten opzichte van een andere RADIUS-server, selecteert u RADIUS-server (s).
14. De server dat de Server worden proxy de RADIUS-aanvragen naar door te klikken op de toevoegen configureren... knop.
15. Voer het IP-adres van de RADIUS-server en een geheim gedeeld in het dialoogvenster RADIUS-Server toevoegen. Het gedeelde geheim moet niet hetzelfde zijn op de Server van Azure meervoudige verificatie en de RADIUS-server. De verificatiepoort en financieel poort wijzigen als u verschillende poorten worden gebruikt door de RADIUS-server.
16. Klik op de knop OK.
17. U moet de Server Azure meervoudige verificatie toevoegen als een RADIUS-client in de andere RADIUS-server zodat toegangsaanvragen afkomstig van de Server Azure meervoudige verificatie worden verwerkt. U moet hetzelfde gedeelde geheim geconfigureerd in de Server Azure meervoudige verificatie gebruiken.
18. U kunt deze stap als u aanvullende RADIUS-servers toevoegen en configureren van de volgorde waarin de Server ze met de knoppen omhoog en omlaag bellen moet wilt herhalen. Hiermee de serverconfiguratie Azure meervoudige verificatie is voltooid. De Server is nu beschikbaar op de geconfigureerde poorten voor RADIUS-toegangsaanvragen van de geconfigureerde-clients.   


## <a name="radius-client-configuration"></a>RADIUS-clientconfiguratie

Gebruik de richtlijnen om de RADIUS-client configureren:

- Configureer uw toestel/server om te verifiëren via RADIUS naar Azure meervoudige verificatie van de Server IP-adres, dat als de RADIUS-server fungeert.
- Gebruik hetzelfde gedeelde geheim dat hierboven is geconfigureerd.
- De RADIUS is opgegeven time-out op 30-60 seconden zodanig configureren dat er tijd valideren referenties van de gebruiker is, het uitvoeren van de meervoudige verificatie, het ontvangen van de reactie en klikt u vervolgens reageren op de toegangsaanvraag RADIUS is opgegeven.
