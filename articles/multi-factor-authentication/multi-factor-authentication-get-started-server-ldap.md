<properties 
    pageTitle="LDAP-verificatie en Azure meervoudige verificatie-Server"
    description="Dit is de pagina van de Azure meervoudige verificatie die helpt bij het implementeren van de LDAP-verificatie en Azure meervoudige verificatie-Server."
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
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP-verificatie en Azure meervoudige verificatie-Server


Standaard worden de Azure meervoudige verificatie-Server is geconfigureerd als u wilt importeren of gebruikers uit Active Directory synchroniseren. Echter kan dit worden geconfigureerd binding maken met verschillende LDAP-mappen, zoals een ADAM directory of specifieke Active Directory-domeincontroller. Wanneer geconfigureerd voor verbinding maken met een directory via LDAP, kan de Server Azure meervoudige verificatie op te treden als een LDAP-proxy om uit te voeren authenticatie worden geconfigureerd. Daarnaast kunt u voor het gebruik van de LDAP-binding als een doel voor RADIUS is opgegeven voor de verificatie van gebruikers bij gebruik van IIS-verificatie vooraf of voor primaire verificatie in de Portal met Azure meervoudige verificatie-gebruiker.

Wanneer u Azure meervoudige verificatie als een LDAP-proxy bevindt, wordt de Server Azure meervoudige verificatie tussen de LDAP-client (bijvoorbeeld VPN toestel, toepassing) en de LDAP-directoryserver om te kunnen toevoegen meervoudige verificatie ingevoegd. Voor Azure meervoudige verificatie-functie, moet de Azure meervoudige verificatieserver worden geconfigureerd om te communiceren met zowel de client-servers als de LDAP-diagram. In deze configuratie wordt met de Server Azure meervoudige verificatie accepteert LDAP-verzoeken van client-servers en toepassingen en doorsturen naar de doelsite LDAP directory-server de primaire referenties te valideren. Als de reactie van de LDAP-diagram wordt vastgesteld dat ze primaire referenties geldig zijn, wordt Azure meervoudige verificatie tweede-factor-verificatie wordt uitgevoerd en stuurt een reactie terug naar de LDAP-client. De hele verificatie slagen alleen als u zowel de verificatie voor de LDAP-server en de meervoudige verificatie is gelukt.





## <a name="ldap-authentication-configuration"></a>Configuratie van de LDAP-verificatie


Als u wilt configureren LDAP-verificatie, installeert u de Server Azure meervoudige verificatie op een Windows server. Gebruik de volgende procedure:

1. Klik op het pictogram LDAP-verificatie in het linkermenu binnen de Server Azure meervoudige verificatie.
2. Schakel het selectievakje in de LDAP-verificatie inschakelen.![LDAP-verificatie](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Wijzigen op het tabblad Clients de TCP-poorten en SSL-poort als de Azure meervoudige verificatie LDAP-service met niet-standaard poorten verbindt moet moeten beluisteren voor LDAP-aanvragen van de clients die worden geconfigureerd.
4. Als u van plan bent leesbaar TEKSTFORMAAT gebruik van de client naar de Server Azure meervoudige verificatie, moet een SSL-certificaat op de server waarop de Server wordt uitgevoerd in zijn geïnstalleerd. Klik op de bladeren... knop naast het vak voor SSL-certificaat en selecteer het geïnstalleerde certificaat dat wordt gebruikt voor de beveiligde verbinding.
5. Klik op toevoegen... knop.
6. Voer in het dialoogvenster toevoegen LDAP-Client het IP-adres van de toestel, de server of de toepassing die u verifieert op de Server en (optioneel) op de naam van een toepassing. De naam van de toepassing wordt weergegeven in Azure meervoudige verificatie-rapporten en kan worden weergegeven in SMS of Mobile-App verificatie-berichten.
7. Schakel het selectievakje van Azure meervoudige verificatie vereisen gebruiker vergelijken als alle gebruikers zijn of worden geïmporteerd naar de Server en onderhevig aan mutli-factor verificatie. Als een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of worden vrijgesteld van mutli-factor verificatie, laat u het vak uitgeschakeld. Zie het help-bestand voor meer informatie over deze functie.
8. Stap 5 tot en met 7 om toe te voegen extra LDAP-clients, kunt u herhalen.
9. Als u de Azure meervoudige verificatie is geconfigureerd voor het ontvangen van de LDAP-authenticatie, moet deze proxy die authenticatie naar de LDAP-diagram. Het tabblad doel toont alleen een enkel, is er daarom grijs van optie voor het gebruik van een LDAP-doel. Als u wilt de LDAP-directory-verbinding configureert, klikt u op het pictogram Directory-integratie.
10. Selecteer op het tabblad instellingen voor het gebruik specifieke LDAP configuratie-keuzerondje.
11. Klik op de bewerken... knop.
12. Klik in het dialoogvenster bewerken LDAP-configuratie door de velden te vullen met de informatie die nodig is om te verbinden met de LDAP-diagram. Beschrijvingen van de velden zijn opgenomen in de onderstaande tabel. Opmerking: Deze informatie is ook opgenomen in het help-bestand van Azure meervoudige verificatie-Server.![Directory-integratie](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. De LDAP-verbinding testen door te klikken op de knop testen.
14. Als de LDAP-verbindingstest geslaagd is, klikt u op de knop OK.
15. Klik op het tabblad Filters. De Server is vooraf geconfigureerde containers, beveiligingsgroepen en gebruikers laden uit Active Directory. Als u naar een andere map voor de LDAP binden en moet u waarschijnlijk bewerken van de filters weergegeven. Klik op de koppeling Help voor meer informatie over filters.
16. Klik op tabblad kenmerken. De Server is vooraf geconfigureerde toewijzen van kenmerken in Active Directory.
17. Als een andere LDAP-map of wijzigen van de vooraf geconfigureerde kenmerktoewijzingen binden en klikt u op de bewerken... knop.
18. Wijzig de LDAP-kenmerktoewijzingen voor het telefoonboek van uw in het dialoogvenster kenmerken bewerken. Kenmerknamen kunnen worden getypt- of geselecteerd door te klikken op de... knop naast elk veld.
19. Klik op de koppeling Help voor meer informatie over kenmerken.
20. Klik op de knop OK.
21. Klik op het pictogram instellingen van het bedrijf en selecteer het tabblad gebruikersnaam resolutie.
22. Als verbinding maakt met Active Directory van een domein behoren-server, moet u mogelijk zijn te verlaten de gebruik Windows beveiliging-id's voor overeenkomende gebruikersnamen keuzerondje geselecteerd. Selecteer anders het kenmerk Gebruik LDAP unieke id voor overeenkomende gebruikersnamen keuzerondje. Wanneer u selecteert, probeert de Server Azure meervoudige verificatie voor het oplossen van elke gebruikersnaam op een unieke id in de LDAP-diagram. Een LDAP-zoekopdracht wordt uitgevoerd op de gebruikersnaam kenmerken die zijn gedefinieerd in de Directory-integratie -> tabblad kenmerken. Wanneer een gebruiker wordt geverifieerd, de gebruikersnaam worden opgelost aan de unieke id in de LDAP-diagram en de unieke id wordt gebruikt voor het afstemmen van de gebruiker in het gegevensbestand Azure meervoudige verificatie. In deze sectie geeft voor hoofdlettergevoelige vergelijkingen, als u ook als lange en korte gebruikersnaam opmaak dit voltooit de serverconfiguratie Azure meervoudige verificatie. De Server luistert nu op de geconfigureerde poorten voor LDAP-aanvragen van de geconfigureerde clients en stel naar proxy die aanvragen voor de LDAP-diagram voor verificatie.


## <a name="ldap-client-configuration"></a>LDAP-clientconfiguratie

Gebruik de richtlijnen om configureren voor de LDAP-client:

- Configureer uw toestel, een server of een toepassing om te verifiëren via LDAP op de Server Azure meervoudige verificatie alsof het uw LDAP-diagram. U moet dezelfde instellingen die u normaal gebruiken zou om rechtstreeks naar uw LDAP-diagram, behalve voor de servernaam of IP-adres dat van de Server Azure meervoudige verificatie verbinding te gebruiken.
- De LDAP-out op 30-60 seconden zodanig configureren dat er tijd valideren van de gebruiker referenties met de LDAP-diagram is, het uitvoeren van de tweede-factor-verificatie, het ontvangen van de reactie en vervolgens de LDAP-toegangsaanvraag beantwoorden.
- Als leesbaar TEKSTFORMAAT gebruikt, moet de toestel of de server de LDAP-query's maken het SSL-certificaat dat is geïnstalleerd op de Server Azure meervoudige verificatie vertrouwen.
