<properties
    pageTitle="Azure Active Directory identiteitsbeveiliging playbook | Microsoft Azure"
    description="Informatie over hoe de beveiliging van de Azure AD identiteit kan u de mogelijkheid van een hacker misbruik van een beschadigde identiteit of het apparaat en te beveiligen van een identiteit of een apparaat die eerder is verdachte of bekende gevaar lopen beperken."
    services="active-directory"
    keywords="beveiliging in de Azure active directory-identiteit, cloud-app discovery,-toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid beheren"
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory identiteitsbeveiliging playbook 

Deze playbook helpt u bij:

- Gegevens in de omgeving identiteitsbeveiliging vullen door simulating risico's en problemen
- Risico's gebaseerde voorwaardelijke clienttoegangsbeleid instellen en testen van de invloed van dit beleid


## <a name="simulating-risk-events"></a>Risico's simuleren

In deze sectie bevat stappen voor het typen van de volgende risico gebeurtenissen simuleren:

- Aanmeldingen van anonieme IP-adressen (eenvoudig)
- Aanmeldingen van onbekende locaties (gemiddeld)
- Waardoor reizen naar atypische locaties (moeilijk)

Andere risico's kunnen niet worden gesimuleerd op een veilige manier.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen van anonieme IP-adressen

Dit type van de gebeurtenis risico geeft aan wat gebruikers die zich heeft aangemeld vanaf een IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres. Deze proxy's worden gebruikt door personen die u wilt verbergen van hun apparaat IP-adres en kunnen worden gebruikt voor schadelijke bedoelingen.

**Deze de vorm van een aanmeldingsproblemen uit een anonieme IP, de volgende stappen uitvoeren**:

1.  Download de [Referentiekader Browser](https://www.torproject.org/projects/torbrowser.html.en).
2.  Met de Tor-Browser, Ga naar [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Voer de referenties van het account dat u wilt weergeven in het rapport **aanmeldingen van anonieme IP-adressen** .

De aanmeldingsproblemen wordt weergegeven op het dashboard identiteitsbeveiliging binnen 5 minuten. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Aanmeldingen van onbekende locaties

Dit risico onbekende locaties is een realtime aanmeldingsproblemen evaluatie om acht eerdere aanmeldingsproblemen locaties (IP, breedtegraad / lengtegraad en ASN) om te bepalen van nieuwe / onbekende locaties. Slaat het systeem vorige IP-adressen, de breedtegraad / lengtegraad en ASN's van een gebruiker en wordt deze naar de vertrouwde locaties worden beschouwd. Een locatie aanmeldingsproblemen wordt beschouwd als onbekende als de locatie aanmelden niet overeenkomen met een van de bestaande vertrouwde locaties.

Beveiliging van Azure Active Directory-identiteit:  

 - heeft een eerste learning periode van 14 dagen waarin deze nieuwe locaties als onbekende locaties niet wordt gemarkeerd.
 - aanmeldingen vanuit vertrouwde apparaten en locaties die geografisch dicht bij een bestaande vertrouwde locatie zijn, worden genegeerd.

U moet zich een locatie en een apparaat dat het account is niet aangemeld uit voordat u deze de vorm van onbekende locaties. 


**Deze de vorm van een aanmeldingsproblemen vanaf een onbekende locatie, de volgende stappen uitvoeren**:

1.  Kies een account met ten minste een 14 dagen aanmeldingsproblemen geschiedenis. 

2.  Als u dit doen:
    
    een. Bij het gebruik van een VPN-verbinding, Ga naar [https://myapps.microsoft.com](https://myapps.microsoft.com) en voer de referenties van het account dat u wilt de onvoorziene gebeurtenis voor simuleren.

    b. Vraag een koppelen in een andere locatie aan te melden met van het account referenties (niet aanbevolen).

De aanmeldingsproblemen wordt weergegeven op het dashboard identiteitsbeveiliging binnen 5 minuten.
 
### <a name="impossible-travel-to-atypical-location"></a>Waardoor reizen naar atypische locatie
De voorwaarde waardoor reizen simuleren is moeilijk omdat de algoritme machine learning gebruikt-te selecteren uit ONWAAR-positieve zoals waardoor reizen vanaf vertrouwde apparaten of aanmeldingen van VPN's die worden gebruikt door andere gebruikers in de adreslijst. De algoritme van de bovendien vereist een geschiedenis aanmeldingsproblemen van 3 tot en met 14 dagen voor de gebruiker voordat deze begint met het genereren van de risico's.

**Deze de vorm van een waardoor reizen op atypische locatie, de volgende stappen uitvoeren**:

1.  Met de standaard-browser, Ga naar [https://myapps.microsoft.com](https://myapps.microsoft.com).  

2.  Voer de referenties van het account dat u wilt een waardoor reizen onvoorziene gebeurtenis voor genereren.

3.  Wijzig uw gebruikersagent. U kunt gebruikersagent in Internet Explorer wijzigen van speciale tools voor ontwikkelaars of wijzig uw gebruikersagent in Firefox of Chrome met een gebruikersagent overschakeling-invoegtoepassing.

4.  Uw IP-adres wijzigen. U kunt uw IP-adres wijzigen via een VPN-verbinding, een invoegtoepassing referentiekader, of draaiende van een nieuwe computer in Azure wordt aangegeven in een ander datacenter.

5.  Aanmeldingsproblemen [https://myapps.microsoft.com](https://myapps.microsoft.com) met dezelfde referenties als voorheen en binnen een paar minuten na de vorige aanmeldingsproblemen.

De aanmeldingsproblemen wordt weergegeven in het dashboard identiteitsbeveiliging binnen 2-4 uur.<br>
Vanwege de complexe machine learning-modellen die nodig zijn, zijn er is een kans dat deze wordt niet worden opgenomen.<br> U mogelijk wilt repliceren van de volgende stappen door voor meerdere Azure AD-accounts.


## <a name="simulating-vulnerabilities"></a>Problemen simuleren 
Problemen zijn weaknesses in een Azure AD-omgeving die kan worden gebruikt door een acteur bad. 3 soorten problemen zijn momenteel opgehaald in Azure AD identiteit beveiliging die gebruikmaken van andere functies van Azure AD. Deze problemen op het dashboard identiteitsbeveiliging automatisch weergegeven nadat deze functies zijn ingesteld.

-   Azure AD [meervoudige verificatie?](../multi-factor-authentication/multi-factor-authentication.md)
-   Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
-   Azure AD [bevoegdheden identiteitsbeheer](active-directory-privileged-identity-management-configure.md). 



##<a name="user-compromise-risk"></a>Gebruiker inbreuk op het risico

**Als u wilt testen gebruiker inbreuk op het risico, de volgende stappen uitvoeren**:

1.  Meld u aan bij [https://portal.azure.com](https://portal.azure.com) met referenties van de globale beheerder voor uw tenant.

2.  Navigeer naar de **identiteitsbeveiliging**. 

3.  Klik op het belangrijkste **Azure AD identiteit beveiliging** blad, klikt u op **Instellingen**. 

4.  Klik op het blad **Portal-instellingen** onder **Beveiligingsregels**op **gebruiker manipuleren risico**. 

5.  Klik op het blad **aanmelden risico** **regel inschakelen** uitschakelen en klik op instellingen **Opslaan** .

6.  Simuleren voor een gebruikersaccount, een onbekende locaties of anonieme IP-onvoorziene gebeurtenis. Hiermee wordt het niveau van de gebruiker risico voor die gebruiker worden omgezet op **Normaal**.

7.  Wacht enkele minuten en controleer vervolgens of gebruikersniveau voor uw gebruikers **Gemiddeld**.

8.  Ga naar het blad **Instellingen van Videoportal** .

9.  Klik op het blad **Gebruiker inbreuk op het risico** , klikt u onder **regel inschakelen**, selecteert u **op** . 

10. Selecteer een van de volgende opties:

    een. Als u wilt blokkeren, selecteert u **Normaal** onder **blokkeren aanmelden**.

    b. Als u wilt afdwingen beveiligd-wachtwoordverificatie wijzigen, selecteert u **Gemiddeld** onder **meervoudige verificatie vereisen**.

13. Klik op **Opslaan**.

14. U kunt nu voorwaardelijke toegang risico gebaseerde testen door te melden bij het gebruik van een gebruiker met een niveau verhoogde risico. Als het risico gebruiker normaal is, afhankelijk van de configuratie van het beleid, uw aanmeldingsproblemen is worden geblokkeerd of u gedwongen uw wachtwoord wijzigen. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>Aanmeldingsproblemen risico

 
**Als u wilt testen een teken in risico, moet u de volgende stappen uitvoeren:**

1.  Meld u aan bij [https://portal.azure.com](https://portal.azure.com) met referenties van de globale beheerder voor uw tenant.

2.  Navigeer naar de **identiteitsbeveiliging**.

3.  Klik op het belangrijkste **Azure AD identiteit beveiliging** blad, klikt u op **Instellingen**. 

4.  Klik op het blad **Portal-instellingen** onder **Beveiligingsregels**op **risico aanmelden**.

5.  Selecteer op het blad **risico aanmelden ** **op** onder **regel inschakelen**. 

7.  Selecteer een van de volgende opties:

    een. Als u wilt blokkeren, selecteert u **Normaal** onder **blokkeren aanmelden**

    b. Als u wilt afdwingen beveiligd-wachtwoordverificatie wijzigen, selecteert u **Gemiddeld** onder **meervoudige verificatie vereisen**.

8.  Als u wilt blokkeren, selecteert u normaal Klik onder blokkeren aanmelden.

9.  Als wilt meervoudige verificatie afdwingen, schakelt u **Normaal** onder **meervoudige verificatie vereisen**.

10. Klik op **Opslaan**.

11. Nu kunt u voorwaardelijke toegang risico gebaseerde testen door de onbekende locaties of gebeurtenissen voor anonieme IP-risico simuleren omdat ze beide **Gemiddeld** risico's.

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Zie ook

 - [Beveiliging van Azure Active Directory-identiteit](active-directory-identityprotection.md)
