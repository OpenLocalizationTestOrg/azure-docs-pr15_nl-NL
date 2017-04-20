<properties
    pageTitle="Probleemoplossing: Azure AD wachtwoordbeheer | Microsoft Azure"
    description="Stappen algemene voor probleemoplossing voor Azure AD wachtwoordbeheer, inclusief opnieuw instellen, wijzigen, write-backs, registratie, en welke gegevens wilt opnemen bij zoekt u hulp."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Problemen met wachtwoordbeheer

> [AZURE.IMPORTANT] **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Als er zijn problemen met wachtwoordbeheer, we u hier graag bij. De meeste problemen die mogelijk optreden kunnen worden opgelost met een paar eenvoudige stappen die u lezen kunt over onder problemen met de implementatie:

* [**Gegevens wilt opnemen wanneer u hulp nodig hebt**](#information-to-include-when-you-need-help)
* [**Problemen met de configuratie van de wachtwoordbeheer in de beheerportal van Azure**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Problemen met wachtwoord Managment rapporten in de beheerportal van Azure**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Problemen met het wachtwoord opnieuw instellen voor registratie-Portal**](#troubleshoot-the-password-reset-registration-portal)
* [**Problemen met het wachtwoord opnieuw in Portal**](#troubleshoot-the-password-reset-portal)
* [**Problemen met wachtwoord write-backs**](#troubleshoot-password-writeback)
  - [Wachtwoord write-backs gebeurtenislogboek foutcodes](#password-writeback-event-log-error-codes)
  - [Problemen met wachtwoord write-backs connectivity](#troubleshoot-password-writeback-connectivity)

Als u de onderstaande stappen voor probleemoplossing hebt geprobeerd en nog steeds problemen optreden wordt uitgevoerd, kunt u contact opnemen met ondersteuning of stelt u een vraag in de [Azure AD-Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) en we wordt gaat u naar uw probleem zodra we kunt.

## <a name="information-to-include-when-you-need-help"></a>Gegevens wilt opnemen wanneer u hulp nodig hebt

Als u het probleem met de onderstaande richtlijnen niet kunt oplossen, kunt u contact opnemen met onze ondersteuningsmedewerkers. Wanneer u contact kunt opnemen, is het raadzaam om op te nemen van de volgende informatie:

 - **Algemene beschrijving van de fout** : welke exacte foutbericht heeft de gebruiker Zie?  Als er geen foutbericht wordt weergegeven, wordt de onverwacht gedrag dat u hebt gezien, tijdens detail beschreven.
 - **Pagina** – welke pagina u op wanneer u de fout hebt gezien waren (omvatten de URL)?
 - **Datum / tijd / Tijdzoneverschil** : Wat is de exacte datum en tijd dat u de fout hebt gezien (omvatten het tijdzoneverschil)?
 - **Code ondersteuning** : Wat is de ondersteuningscode gegenereerd wanneer de gebruiker hebt gezien de fout (naar dit vinden, Reproduceer de fout, en vervolgens klikt u op de koppeling Code ondersteunen onderaan in het scherm en verzenden van de ondersteuningstechnicus de GUID die het resultaat is).
   - Als u op een pagina zonder een ondersteuningscode onderaan, drukt u op F12 en beveiligings-id en CID zoeken en deze twee resultaten verzenden naar de engineeren ondersteuning.

    ![][001]

 - **Gebruikers-ID** : Wat is de ID van de gebruiker die de fout (bijvoorbeeld hebt gezienuser@contoso.com)?
 - **Informatie over de gebruiker** – is de gebruiker die zijn gekoppeld, wachtwoordhash hebt gesynchroniseerd, alleen de cloud?  Heeft de gebruiker een AAD Premium of AAD eenvoudige licentie is toegewezen?
 - **Gebeurtenislogboek toepassing** – als u gebruikmaakt van wachtwoord write-backs en de fout is in de infrastructuur van uw on-premises implementatie, neemt zip van een kopie van het gebeurtenislogboek van de toepassing van uw server Azure AD Connect en samen met uw verzoek verzenden.

Deze informatie inclusief helpt ons uw probleem zo snel mogelijk oplossen.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Problemen met de configuratie van de wachtwoord opnieuw instellen in de beheerportal van Azure
Als u een foutbericht krijgt wanneer wachtwoordherstel configureren, is het mogelijk dat u kunt dit oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout hoofdletters/kleine letters</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welke fout een gebruiker zien?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie de sectie <strong>Gebruiker opnieuw instellen wachtwoordbeleid </strong>onder het tabblad <strong>configureren</strong> in de portal Azure management niet</p>
            </td>
            <td>
              <p>De <strong>Gebruiker opnieuw instellen wachtwoordbeleid </strong>sectie is niet zichtbaar is op het tabblad <strong>configureren</strong> in de beheerportal Azure.</p>
            </td>
            <td>
              <p>Dit kan gebeuren als er geen een AAD Premium of eenvoudige AAD-licentie toegewezen aan de beheerder deze bewerking. </p>
              <p>Een licentie AAD Premium of AAD eenvoudige toewijzen aan de betreffende beheerdersaccount door te schuiven naar het tabblad <strong>licenties</strong> om te verhelpen dit, en probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie geen van de configuratieopties onder de sectie <strong>Gebruiker opnieuw instellen wachtwoordbeleid</strong> die worden beschreven in de documentatie.</p>
            </td>
            <td>
              <p>De <strong>Gebruiker opnieuw instellen wachtwoordbeleid </strong>sectie zichtbaar is, maar de enige vlag dat wordt weergegeven onder het is de vlag <strong>Gebruikers die zijn ingeschakeld voor het wachtwoord opnieuw instellen</strong> .</p>
            </td>
            <td>
              <p>De rest van de gebruikersinterface wordt weergegeven wanneer u de markering <strong>Gebruikers die zijn ingeschakeld voor het wachtwoord opnieuw instellen</strong> om te schakelen <strong>Ja.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie niet de configuratieoptie voor een bepaalde.</p>
            </td>
            <td>
              <p>Bijvoorbeeld Zie ik niet de optie <strong>aantal dagen voordat een gebruiker moet hun gegevens van contactpersonen bevestigen</strong> wanneer ik blader door de <strong>Gebruiker opnieuw instellen wachtwoordbeleid</strong> sectie (of andere voorbeelden van hetzelfde probleem).</p>
            </td>
            <td>
              <p>Aantal elementen van UI zijn verborgen totdat nodig zijn. Kunt u alle opties op de pagina inschakelen als u wilt zien.</p>
              <p>Zie <a href="active-directory-passwords-customize.md#password-management-behavior">Wachtwoordbeheer gedrag</a> voor meer informatie over alle besturingselementen die beschikbaar voor u zijn.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie niet de optie <strong>Schrijven terug om gebruikerswachtwoorden On-Premises</strong> configuratie</p>
            </td>
            <td>
              <p>De optie <strong>Schrijven terug om gebruikerswachtwoorden On-Premises</strong> is niet zichtbaar is op het tabblad <strong>configureren</strong> in de beheerportal van Azure</p>
            </td>
            <td>
              <p>Deze optie is alleen zichtbaar als u hebt gedownload van Azure AD Connect en geconfigureerd wachtwoord write-backs. Als u dit hebt gedaan, wordt deze optie wordt weergegeven en kunt u in- of uitschakelen van write-backs vanuit de cloud.</p>
              <p>Zie <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Inschakelen wachtwoord write-backs in Azure AD Connect</a> voor meer informatie over hoe u dit doet.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Problemen met wachtwoord management rapporten in de beheerportal van Azure
Als u een foutbericht krijgt wanneer u het wachtwoord management-rapporten gebruikt, is het mogelijk dat u kunt dit oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout hoofdletters/kleine letters</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welke fout een gebruiker zien?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik zie geen wachtwoord management rapporten</p>
            </td>
            <td>
              <p>De <strong>activiteit voor wachtwoordherstel</strong> en <strong>registratie activiteit voor wachtwoordherstel</strong> rapporten zijn niet zichtbaar zijn onder de <strong>Activiteit Log</strong> -rapporten op het tabblad <strong>rapporten</strong> .</p>
            </td>
            <td>
              <p>Dit kan gebeuren als er geen een AAD Premium of eenvoudige AAD-licentie toegewezen aan de beheerder deze bewerking. </p>
              <p>Een licentie AAD Premium of AAD eenvoudige toewijzen aan de betreffende beheerdersaccount door te schuiven naar het tabblad <strong>licenties</strong> om te verhelpen dit, en probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker registraties weergeven meerdere keren</p>
            </td>
            <td>
              <p>Wanneer een gebruiker alternatieve e, mobiele telefoon en vragen over beveiliging registreert, worden ze elke weergegeven als afzonderlijke regels in plaats van een ononderbroken lijn.</p>
            </td>
            <td>
              <p>Op dit moment wanneer een gebruiker registreert, we kunnen niet wordt ervan uitgegaan dat dat ze alles aanwezig op de registratiepagina wordt geregistreerd. Hierdoor wordt momenteel Meld u aan elke afzonderlijke gegevensitem dat is geregistreerd als een afzonderlijke gebeurtenis.</p>
              <p>Als u deze gegevens aggregeren wilt, kunt u deze kunt downloaden van het rapport en opent u de gegevens als een draaitabel in excel om meer flexibiliteit.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Problemen met de portal voor registratie van wachtwoord opnieuw instellen
Als u een foutbericht krijgt bij het registreren van een gebruiker voor wachtwoorden opnieuw instellen, is het mogelijk dat u kunt dit oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout hoofdletters/kleine letters</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welke fout een gebruiker zien?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is niet ingeschakeld voor wachtwoorden opnieuw instellen</p>
            </td>
            <td>
              <p>De beheerder heeft u voor deze functie niet ingeschakeld.</p>
            </td>
            <td>
              <p>Overschakelen van de vlag voor <strong>Gebruikers die zijn ingeschakeld voor het wachtwoord opnieuw instellen</strong> op <strong>Ja</strong> en klik op <strong>Opslaan</strong> in het tabblad Azure-beheerportal directory configuratie. U moet een Azure AD Premium of eenvoudige licentie is toegewezen aan de beheerder deze bewerking hebben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker beschikt niet over een AAD Premium of AAD eenvoudige licentie is toegewezen</p>
            </td>
            <td>
              <p>De beheerder heeft u voor deze functie niet ingeschakeld.</p>
            </td>
            <td>
              <p>Een licentie Azure AD Premium of Azure AD eenvoudige toewijzen aan de gebruiker, klik op het tabblad <strong>licenties</strong> in de beheerportal Azure. U moet een Azure AD Premium of eenvoudige licentie is toegewezen aan de beheerder deze bewerking hebben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fout verwerking van aanvraag</p>
            </td>
            <td>
              <p>Gebruiker ziet een foutbericht waarin wordt aangegeven:</p>
              <p>

              </p>
              <p>Fout verwerking van aanvraag </p>
              <p>Tijdens een poging om een wachtwoord opnieuw instellen.</p>
            </td>
            <td>
              <p>Dit kan worden veroorzaakt door problemen met veel, maar gewoonlijk deze fout wordt veroorzaakt door een van beide een service storing of configuratie-probleem dat niet kan opgelost worden. </p>
              <p>Als u deze fout wordt weergegeven en dit is die invloed hebben op uw bedrijf, neem contact op met ondersteuning en we helpt u ZSM. Zie <a href="#information-to-include-when-you-need-help">gegevens wilt opnemen wanneer u hulp nodig hebt</a> om te zien wat u moet leveren aan het engineeren ondersteuning als toelichting van een snelle resolutie.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>Problemen met de portal van wachtwoord opnieuw instellen
Als u een foutbericht krijgt wanneer u een wachtwoord van een gebruiker opnieuw instelt, is het mogelijk dat u kunt dit oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout hoofdletters/kleine letters</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welke fout een gebruiker zien?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is niet ingeschakeld voor wachtwoorden opnieuw instellen</p>
            </td>
            <td>
              <p>Uw account is niet ingeschakeld voor wachtwoorden opnieuw instellen</p>
              <p>Helaas, maar uw beheerder niet uw account voor gebruik met deze service heeft ingesteld. </p>
              <p>

              </p>
              <p>Als u wilt, kunt we contact opnemen met een beheerder in uw organisatie om uw wachtwoord opnieuw voor u.</p>
            </td>
            <td>
              <p>Overschakelen van de vlag voor <strong>Gebruikers die zijn ingeschakeld voor het wachtwoord opnieuw instellen</strong> op <strong>Ja</strong> en klik op <strong>Opslaan</strong> in het tabblad Azure-beheerportal directory configuratie. U moet een Azure AD Premium of eenvoudige licentie is toegewezen aan de beheerder deze bewerking hebben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker beschikt niet over een AAD Premium of AAD eenvoudige licentie is toegewezen</p>
            </td>
            <td>
              <p>Terwijl we kunnen geen niet-beheerders wachtwoorden automatisch opnieuw, kunt we contact opnemen met de beheerder van uw organisatie om het voor u betekenen.</p>
            </td>
            <td>
              <p>Een licentie Azure AD Premium of Azure AD eenvoudige toewijzen aan de gebruiker, klik op het tabblad <strong>licenties</strong> in de beheerportal Azure. U moet een Azure AD Premium of eenvoudige licentie is toegewezen aan de beheerder deze bewerking hebben.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is ingeschakeld voor het wachtwoord opnieuw instellen, maar de gebruiker heeft ontbrekende of ongeldige verificatiegegevens</p>
            </td>
            <td>
              <p>Uw account is niet ingeschakeld voor wachtwoorden opnieuw instellen</p>
              <p>Helaas, maar uw beheerder niet uw account voor gebruik met deze service heeft ingesteld. </p>
              <p>

              </p>
              <p>Als u wilt, kunt we contact opnemen met een beheerder in uw organisatie om uw wachtwoord opnieuw voor u.</p>
            </td>
            <td>
              <p>Zorgen dat die gebruiker heeft onjuist opgemaakte gegevens van contactpersonen op het bestand in de map voordat u verdergaat. Zie <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welke gegevens worden gebruikt door de wachtwoorden opnieuw instellen</a> voor informatie over het configureren van verificatie-informatie in de adreslijst, zodat gebruikers deze fout niet te zien.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is ingeschakeld voor het wachtwoord opnieuw instellen, maar een gebruiker heeft slechts een deel van de gegevens van contactpersonen op bestand wanneer het beleid is ingesteld op twee stappen voor verificatie vereisen</p>
            </td>
            <td>
              <p>Uw account is niet ingeschakeld voor wachtwoorden opnieuw instellen</p>
              <p>Helaas, maar uw beheerder niet uw account voor gebruik met deze service heeft ingesteld. </p>
              <p>

              </p>
              <p>Als u wilt, kunt we contact opnemen met een beheerder in uw organisatie om uw wachtwoord opnieuw voor u.</p>
            </td>
            <td>
              <p>Zorgen dat die gebruiker heeft ten minste twee correct geconfigureerde contactpersoonformulier verschillende (bijvoorbeeld zowel mobiele telefoon en telefoon op kantoor) voordat u verdergaat. Zie <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welke gegevens worden gebruikt door de wachtwoorden opnieuw instellen</a> voor informatie over het configureren van verificatie-informatie in de adreslijst, zodat gebruikers deze fout niet te zien.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is ingeschakeld voor het wachtwoord opnieuw instellen en gebruiker correct is geconfigureerd, maar kan contact met u opgenomen </p>
            </td>
            <td>
              <p>Bestaande!  We een onverwachte fout opgetreden contact met u opnemen.</p>
            </td>
            <td>
              <p>Dit is het resultaat van een tijdelijke servicefout of onjuist geconfigureerd gegevens van contactpersonen die we niet correct kan detecteren. Als de gebruiker 10 seconden, een probeer het opnieuw wacht en de koppeling 'contact op met uw beheerder' wordt weergegeven. Probeer u opnieuw op wordt het gesprek, het opnieuw verzenden dat te klikken op "contact op met uw beheerder" formulier per e-mail een gebruiker, wachtwoord of globale beheerders (in die voorrangsregels stuurt) aanvragen van een wachtwoord opnieuw instellen om te worden uitgevoerd voor die account.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker ontvangt nooit de wachtwoorden opnieuw instellen in SMS of telefoongesprek</p>
            </td>
            <td>
              <p>Gebruiker klikt op ' tekst mij ' of 'Bel mij' en vervolgens nooit ontvangt.</p>
            </td>
            <td>
              <p>Het resultaat van een ongeldige telefoonnummer in de adreslijst mogelijk. Controleer of het telefoonnummer dat is in de notatie 'en ccc xxxyyyzzzzXeeee'. Meer informatie over de opmaak van de telefoon Zie getallen voor gebruik met wachtwoorden opnieuw instellen in <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welke gegevens worden gebruikt door de wachtwoorden opnieuw instellen</a>.</p>
              <p>Als u een uitbreiding van worden doorgestuurd naar de desbetreffende gebruiker vereist, houd er rekening mee dat wachtwoorden opnieuw instellen in ondersteunt geen extensies, zelfs als u in de adreslijst opgeeft (ze worden verwijderd voordat het gesprek wordt verzonden). Gebruik van een getal zonder extensie, of de extensie integreren in het telefoonnummer in uw PBX.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker ontvangt nooit wachtwoord opnieuw instellen e-mail</p>
            </td>
            <td>
              <p>Gebruiker klikt op 'e-mail mij' en vervolgens nooit ontvangt.</p>
            </td>
            <td>
              <p>De meest voorkomende oorzaken van dit probleem is dat het bericht is geweigerd door een spamfilter. Controleer uw spam, de map Ongewenste e-mail of verwijderde items voor het e-mailbericht.</p>
              <p>Ook voor zorgen dat u het juiste e-mailbericht controleert voor het bericht... groot aantal personen vergelijkbaar e-mailadressen hebt en dit uiteindelijk controleren van het verkeerde Postvak in voor het bericht. Als geen van deze opties werken, is het ook mogelijk dat het e-mailadres in de adreslijst onjuist is, Controleer of dat het e-mailadres is het juiste account en probeer het opnieuw controleren. Meer informatie over het opmaken van e-mailbericht Zie adressen voor gebruik met wachtwoorden opnieuw instellen in <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">welke gegevens worden gebruikt door de wachtwoorden opnieuw instellen</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ik heb een beleid voor het opnieuw instellen van wachtwoorden ingesteld, maar als een beheerdersaccount gebruikmaakt van wachtwoorden opnieuw instellen, wordt dit beleid wordt niet toegepast</p>
            </td>
            <td>
              <p>Beheerdersaccounts hun wachtwoorden opnieuw instellen Zie dezelfde opties ingeschakeld voor het wachtwoord opnieuw instellen, e-mail en mobiele telefoon, ongeacht welk beleid is ingesteld in de sectie <strong>Gebruiker opnieuw instellen wachtwoordbeleid</strong> van het tabblad <strong>configureren</strong> .</p>
            </td>
            <td>
              <p>De opties onder de sectie <strong>Opnieuw gebruikersbeleid wachtwoord</strong> van het tabblad <strong>configureren</strong> geconfigureerd gelden alleen voor eindgebruikers in uw organisatie.</p>
              <p>Microsoft beheert en besturingselementen voor het beheerderswachtwoord opnieuw beleid om te zorgen dat het hoogste niveau van beveiliging</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Voorkomen dat u probeert wachtwoord opnieuw in te vaak in een dag gebruiker</p>
            </td>
            <td>
              <p>Gebruiker ziet een foutbericht dat aangeeft:</p>
              <p>

              </p>
              <p>Gebruik een andere optie.</p>
              <p>U hebt geprobeerd om te controleren of uw account te vaak in de laatste 1 uur. Veiligheidsoverwegingen moet u wacht u 24 uur voordat u kunt probeer het opnieuw. </p>
              <p>Als u wilt, kunt we contact opnemen met een beheerder in uw organisatie om uw wachtwoord opnieuw voor u.</p>
            </td>
            <td>
              <p>We implementeren een automatische bandbreedtebeperking voorkomen dat gebruikers van hun wachtwoorden opnieuw te vaak in een korte tijdsperiode wilt. Dit gebeurt wanneer:</p>
              <ol class="ordered">
                <li>
Gebruiker probeert te valideren een telefoonnummer 5 keer in één uur.<br\><br\></li>
                <li>
Gebruiker probeert te gebruiken van de beveiliging vragen heeft bereikt 5 keer in één uur.<br\><br\></li>
                <li>
Gebruiker probeert te een wachtwoord opnieuw instellen voor dezelfde gebruikersaccount 5 keer één uur.<br\><br\></li>
              </ol>
              <p>U lost dit op, moet de gebruiker wacht u 24 uur na de laatste poging en de gebruiker vervolgens kunnen zijn of haar wachtwoord opnieuw in.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Gebruiker ziet een fout bij het valideren van zijn of haar telefoonnummer</p>
            </td>
            <td>
              <p>Wanneer u probeert om te controleren of een telefoon als een verificatiemethode moet worden gebruikt, ziet de gebruiker een foutbericht dat aangeeft:</p>
              <p>

              </p>
              <p>Onjuiste telefoonnummer dat is opgegeven.</p>
            </td>
            <td>
              <p>Deze fout treedt op wanneer het telefoonnummer dat is ingevoerd niet overeenkomen met het telefoonnummer op het bestand.</p>
              <p>Controleer of dat het volledige telefoonnummer, inclusief gebied en land-code, wanneer u probeert te gebruiken van een methode telefoongebaseerde voor wachtwoorden opnieuw instellen door de gebruiker is ingevoerd.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fout verwerking van aanvraag</p>
            </td>
            <td>
              <p>Gebruiker ziet een foutbericht waarin wordt aangegeven:</p>
              <p>

              </p>
              <p>Fout verwerking van aanvraag </p>
              <p>Tijdens een poging om een wachtwoord opnieuw instellen.</p>
            </td>
            <td>
              <p>Dit kan worden veroorzaakt door problemen met veel, maar gewoonlijk deze fout wordt veroorzaakt door een van beide een service storing of configuratie-probleem dat niet kan opgelost worden. </p>
              <p>Als u deze fout wordt weergegeven en dit is die invloed hebben op uw bedrijf, neem contact op met ondersteuning en we helpt u ZSM. Zie <a href="#information-to-include-when-you-need-help">gegevens wilt opnemen wanneer u hulp nodig hebt</a> om te zien wat u moet leveren aan het engineeren ondersteuning als toelichting van een snelle resolutie.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Wachtwoord write-backs oplossen
Als u een fout aan bij inschakelen, uitschakelen of wachtwoord write-backs te gebruiken, is het mogelijk kunt oplossen door de onderstaande stappen voor probleemoplossing te volgen:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Fout hoofdletters/kleine letters</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Welke fout een gebruiker zien?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Oplossing</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Algemeen onboarding en opstarten fouten</p>
            </td>
            <td>
              <p>Wachtwoord opnieuw instellen service begint niet on-premises met fout 6800 in gebeurtenislogboek van de computer van de Azure AD Connect-toepassing.</p>
              <p>

              </p>
              <p>Na onboarding, federatieve of wachtwoordhash kunnen geen gesynchroniseerde gebruikers hun wachtwoord opnieuw.</p>
            </td>
            <td>
              <p>Wanneer wachtwoord write-backs is ingeschakeld, wordt de synchronisatie-engine de bibliotheek write-backs als u wilt uitvoeren van de configuratie (onboarding) bellen door de cloud-introductieservice spreekt. Eventuele fouten tijdens onboarding of tijdens het starten van het WCF-eindpunt voor wachtwoord write-backs treden fouten in het gebeurtenissenlogboek van het gebeurtenislogboek van de Azure AD Connect-computer.</p>
              <p>Tijdens opnieuw starten van ADSync service als write-backs is geconfigureerd, wordt het WCF-eindpunt gestart omhoog. Echter als het starten van het eindpunt mislukt, we gewoon gebeurtenis 6800 registreren en laat het starten van de synchronisatie-service. Aanwezigheid van deze gebeurtenis betekent dat het wachtwoord write-backs eindpunt niet is gestart. Gebeurtenislogboek gegevens van deze gebeurtenis (6800) samen met logboekvermeldingen genereren door PasswordResetService component wordt aangegeven waarom het eindpunt kan niet worden gestart omhoog. Deze gebeurtenislogboek-fouten te bekijken en probeer het opnieuw starten van de Azure AD Connect als wachtwoord write-backs nog steeds niet werkt. Als het probleem zich blijft voordoen, kunt u om te schakelen en opnieuw inschakelen wachtwoord write-backs.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Wanneer een gebruiker probeert een wachtwoord opnieuw instellen of ontgrendelen van een account met wachtwoord write-backs ingeschakeld, mislukt de bewerking. Daarnaast kunt u een gebeurtenis in de Azure AD Connect gebeurtenislogboek waarop u: "synchronisatie-Engine geretourneerd een fout hr = 800700CE, bericht = de bestandsnaam of extensie te lang is ' nadat de ontgrendelingsbewerking plaatsvindt.
                            </p>
            </td>
            <td>
              <p>Dit kan gebeuren als u oudere versies van Azure AD Connect of DirSync had bijgewerkt. Een upgrade naar oudere versies van Azure AD Connect een 254 teken wachtwoord instellen voor het Azure AD Management Agent-account (nieuwere versies wordt een wachtwoord van de lengte 127 tekens ingesteld). Deze lange wachtwoorden werken voor AD-Connector importeren en exporteren van gegevens, maar deze niet worden ondersteund door de bewerking ontgrendelen.
                            </p>
            </td>
            <td>
              <p>[Zoek het Active Directory-account](active-directory-aadconnect-accounts-permissions.md#active-directory-account) voor Azure AD Connect en opnieuw instellen van wachtwoorden om te niet meer dan 127 tekens bevatten. Open **Synchronisatieservice** in het menu Start. Ga naar de **verbindingslijnen** en de **Active Directory Connector**zoeken. Selecteer deze en klikt u op **Eigenschappen**. Ga naar de pagina **referenties** en voert u het nieuwe wachtwoord. Selecteer **OK** om de pagina te sluiten.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Fout bij het configureren van write-backs tijdens de installatie van de Azure AD Connect.</p>
            </td>
            <td>
              <p>De laatste stap van het installatieproces Azure AD Connect ziet u een foutbericht dat aangeeft dat wachtwoord write-backs niet kan worden geconfigureerd.</p>
              <p>

              </p>
              <p>Het gebeurtenissenlogboek van Azure AD verbinding toepassing bevat fout 32009 met tekst ' Fout bij ophalen auth token'.</p>
            </td>
            <td>
              <p>Deze fout treedt op in de volgende twee gevallen:</p>
              <ul>
                <li class="unordered">
U kunt een onjuist wachtwoord voor het opgegeven aan het begin van het installatieproces Azure AD Connect globale beheerder-account hebt opgegeven.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
U hebt geprobeerd een federatieve gebruiker gebruiken voor de opgegeven aan het begin van het installatieproces Azure AD Connect account voor globale beheerder.<br\><br\></li>
              </ul>
              <p>U lost deze fout door Zorg ervoor dat u niet met een federatieve account voor de globale beheerder die u aan het begin van het installatieproces Azure AD Connect hebt opgegeven, en of de opgegeven wachtwoord juist is.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fout bij het configureren van write-backs tijdens de installatie van de Azure AD Connect.</p>
            </td>
            <td>
              <p>Het gebeurtenissenlogboek van Azure AD Connect machine bevat fout veroorzaakt door de PasswordResetService 32002.</p>
              <p>

              </p>
              <p>Ontvanger van de fout: "fout verbinding maakt met ServiceBus, de token-provider is niet een beveiligingstoken..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>De oorzaak van deze fout is het wachtwoord opnieuw instellen service uitgevoerd in uw on-premises omgeving is geen verbinding maken met het eindpunt van de service bus in de cloud. Deze fout wordt normaal doorgaans veroorzaakt door een firewallregel blokkeren van een uitgaande verbinding met een bepaald poort of web-adres.</p>
              <p>

              </p>
              <p>Controleer of dat uw firewall uitgaande verbindingen voor het volgende mogelijk:</p>
              <ul>
                <li class="unordered">
Al het verkeer via TCP 443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Uitgaande verbindingen naar <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Wanneer u deze regels hebt bijgewerkt, de Azure AD Connect-computer opnieuw opstarten en wachtwoord write-backs moet gaan werken het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Wachtwoord write-backs eindpunt on-premises niet bereikbaar</p>
            </td>
            <td>
              <p>Nadat u voor bepaalde tijd, federatieve of wachtwoordhash kunnen geen gesynchroniseerde gebruikers hun wachtwoord opnieuw.</p>
            </td>
            <td>
              <p>In sommige gevallen het wachtwoord write-backs-service niet opnieuw gestart wanneer Azure AD Connect opnieuw is gestart. In dat geval Controleer eerst of wachtwoord write-backs niet ingeschakeld is op prem. U kunt dit doen met de wizard van de Azure AD Connect of powershell (Zie HowTos sectie hierboven). Als u de functie ziet worden ingeschakeld, probeer in- of uitschakelen van de functie opnieuw via de gebruikersinterface of PowerShell. Zie ' stap 2: wachtwoord write-backs op uw computer Directory-synchronisatie inschakelen &amp; firewallregels configureren ' in <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">het wachtwoord write-backs in-/ uitschakelen</a> voor meer informatie over hoe u dit doet.</p>
              <p>

              </p>
              <p>Als dit niet werkt, kunt u programma volledig te verwijderen en opnieuw installeren van Azure AD Connect.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Machtigingen fouten</p>
            </td>
            <td>
              <p>Federatieve of hash Wachtwoordsynchronisatie 'd gebruikers die proberen hun Zie wachtwoorden van een fout wanneer u het wachtwoord die aangeeft dat er is een serviceprobleem hebt verzonden opnieuw in te stellen.</p>
              <p>

              </p>
              <p>Daarnaast tijdens wachtwoord opnieuw instellen bewerkingen, ziet u mogelijk dat een fout met betrekking tot management agent is in uw aan gebeurtenislogboeken van lokale toegang geweigerd.</p>
            </td>
            <td>
              <p>Als u deze fouten in uw gebeurtenislogboek ziet, Controleer of het AD MA-account (dat is opgegeven in de wizard op het moment van configuratie) de benodigde machtigingen voor wachtwoord write-backs.</p>
              <p>

              </p>
              <p>Houd er rekening mee dat voor de machtigingen voor doorsijpelen omlaag via sdprop achtergrondtaak op de domeincontroller maximaal 1 uur duren kan nadat deze toestemming wordt verleend. </p>
              <p>Voor het wachtwoord opnieuw instellen als u wilt werken, moet de machtiging worden tijdstempel op de beveiligingsbeschrijving van het gebruikersobject waarvan het wachtwoord wordt hersteld. Totdat deze machtiging wordt weergegeven in het gebruikersobject, blijft wachtwoorden opnieuw instellen in mislukken met toegang geweigerd.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Fout bij het wachtwoord write-backs configureren van de configuratiewizard van Azure AD Connect </p>
            </td>
            <td>
              <p>"Kan niet vinden MA" fout in de Wizard tijdens het wachtwoord write-backs in/uitschakelen</p>
            </td>
            <td>
              <p>Er is een bekende fout in de uitgebrachte versie van Azure AD Connect die in de volgende omstandigheden:</p>
              <ol class="ordered">
                <li>
U configureren Azure AD Connect voor tenant abc.com (het domein is geverifieerd) referenties met. Hierdoor AAD verbindingslijn met de naam 'abc.com – AAD"wordt gemaakt.<br\><br\></li>
                <li>
U vervolgens wijzigen de AAD-referenties voor de verbindingslijn (via de oude synchronisatie UI) als u wilt (Let op dat dezelfde tenant maar andere domeinnaam is). <br\><br\></li>
                <li>
Nu probeert u te wachtwoord write-backs in-/ uitschakelen. De wizard wordt de naam van de verbindingslijn met de referenties, als "abc.onmicrosoft.com – AAD" maken en wordt doorgegeven aan de cmdlet wachtwoord write-backs. Dit is mislukt omdat er geen connector die zijn gemaakt met deze naam.<br\><br\></li>
              </ol>
              <p>Dit probleem is opgelost in onze meest recente versies. Als u een oudere versie hebt, is het een oplossing gebruik de powershell-cmdlet om de functie in-/ uitschakelen. Zie ' stap 2: wachtwoord write-backs op uw computer Directory-synchronisatie inschakelen &amp; firewallregels configureren ' in <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">het wachtwoord write-backs in-/ uitschakelen</a> voor meer informatie over hoe u dit doet.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kan geen wachtwoord opnieuw instellen voor gebruikers in speciale groepen zoals Domain Admins / Enterprise beheerders enzovoort.</p>
            </td>
            <td>
              <p>Federatieve of hash Wachtwoordsynchronisatie 'd gebruikers die deel uitmaken van een beveiligde groep en hun Zie wachtwoorden van een fout wanneer u het wachtwoord die aangeeft dat er is een serviceprobleem hebt verzonden opnieuw wilt instellen.</p>
            </td>
            <td>
              <p>Gebruikers met bevoegdheden in Active Directory zijn beveiligd met AdminSDHolder. Zie <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> voor meer informatie. </p>
              <p>

              </p>
              <p>Dit betekent dat de beveiligingsdescriptors op deze objecten regelmatig zodat deze overeenkomt met de ene opgegeven in AdminSDHolder worden gecontroleerd en worden opnieuw ingesteld als deze verschillen. De extra machtigingen die u nodig voor wachtwoord write-backs hebt dus niet doorsijpelen aan deze gebruikers. Dit kan resulteren in wachtwoord write-backs werken niet voor deze gebruikers. Hierdoor kunnen ondersteunen we geen wachtwoorden beheren voor gebruikers binnen deze groepen omdat het beveiligingsmodel AD worden verbroken.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Beginwaarden bewerkingen mislukt met Object kunnen niet worden gevonden</p>
            </td>
            <td>
              <p>Federatieve of hash Wachtwoordsynchronisatie 'd gebruikers die proberen hun Zie wachtwoorden van een fout wanneer u het wachtwoord die aangeeft dat er is een serviceprobleem hebt verzonden opnieuw in te stellen.</p>
              <p>

              </p>
              <p>Daarnaast tijdens wachtwoord opnieuw instellen bewerkingen, ziet u mogelijk een fout in uw gebeurtenislogboeken van de Azure AD Connect-service die aangeeft dat u een fout 'Object kan niet worden gevonden'.</p>
            </td>
            <td>
              <p>Deze fout wordt meestal aangegeven dat de synchronisatie-engine is kan het gebruikersobject in de ruimte AAD-connector of de gekoppelde MV of een AD-connector ruimte object niet vinden. </p>
              <p>

              </p>
              <p>Zorg ervoor dat de gebruiker wordt daadwerkelijk gesynchroniseerd vanuit on-premises met AAD via het huidige exemplaar van Azure AD Connect om dit probleem oplossen door en de status van de objecten in de verbindingslijn spaties en MV controleren. Controleer of het object AD CS connector aan het object MV via de regel 'Microsoft.InfromADUserAccountEnabled.xxx' is.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Beginwaarden bewerkingen mislukt met meerdere overeenkomsten zijn gevonden eror</p>
            </td>
            <td>
              <p>Federatieve of hash Wachtwoordsynchronisatie 'd gebruikers die proberen hun Zie wachtwoorden van een fout wanneer u het wachtwoord die aangeeft dat er is een serviceprobleem hebt verzonden opnieuw in te stellen.</p>
              <p>

              </p>
              <p>Daarnaast tijdens wachtwoord opnieuw instellen bewerkingen, ziet u mogelijk een fout in uw gebeurtenislogboeken van de Azure AD Connect-service die aangeeft dat u een fout 'Meerdere maches gevonden'.</p>
            </td>
            <td>
              <p>Hiermee wordt aangeduid dat de synchronisatie-engine gedetecteerd dat het object MV onderhouden met meer dan één AD CS objecten via de "Microsoft.InfromADUserAccountEnabled.xxx" is. Dit betekent dat de gebruiker een ingeschakelde account in meer dan één bos heeft. </p>
              <p>

              </p>
              <p>Dit scenario wordt momenteel niet ondersteund voor wachtwoord write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Wachtwoord bewerkingen mislukken met een configuratiefout.</p>
            </td>
            <td>
              <p>Wachtwoord bewerkingen mislukken met een configuratiefout. Het toepassingsgebeurtenislogboek Azure AD Connect fout 6329 met tekst bevat: 0x8023061f (de bewerking is mislukt omdat Wachtwoordsynchronisatie is niet ingeschakeld voor deze Agent Management).</p>
            </td>
            <td>
              <p>Dit gebeurt als de Azure AD Connect-configuratie wordt gewijzigd om toe te voegen&nbsp;een nieuwe AD-bos (of om te verwijderen en opnieuw toe te voegen een bestaande bos) <strong>nadat</strong> de functie wachtwoord write-backs is al ingeschakeld. Wachtwoord bewerkingen voor gebruikers in deze toegevoegde forests, mislukt. U kunt het probleem oplossen door uitschakelen en de functie wachtwoord write-backs opnieuw inschakelen nadat de configuratiewijzigingen bos zijn voltooid.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Terugschrijven van wachtwoorden die opnieuw zijn ingesteld door gebruikers werkt correct, maar wachtwoorden terugschrijven gewijzigd door een gebruiker of opnieuw instellen voor een gebruiker door een beheerder is mislukt.</p>
            </td>
            <td>
              <p>Wanneer u probeert een wachtwoord opnieuw instellen namens een gebruiker van de Azure-beheerportal, ziet u een bericht ontvangt: "het wachtwoord opnieuw instellen service uitvoert in uw on-premises omgeving biedt geen ondersteuning beheerders gebruikerswachtwoorden opnieuw instellen. Voer een upgrade uit naar de nieuwste versie van Azure AD Connect voor het oplossen van dit."</p>
            </td>
            <td>
              <p>Dit gebeurt wanneer de versie van de synchronisatie-engine biedt geen ondersteuning voor de speciale wachtwoord write-backs bewerking dat is gebruikt. Versies van Azure AD Connect later dan 1.0.0419.0911 ondersteuning voor alle wachtwoord management bewerkingen, inclusief wachtwoord opnieuw instellen van write-backs, wachtwoord wijzigen write-backs en door de beheerder geactiveerde wachtwoord opnieuw instellen write-backs van de Azure-beheerportal.&nbsp; DirSync versies ondersteunen hoger dan 1.0.6862 wachtwoord opnieuw instellen write-backs alleen. U lost dit probleem, wordt ten zeerste aangeraden dat u de nieuwste versie van Azure AD Connect of Azure Active Directory-verbinding installeren (Zie <a href="active-directory-aadconnect">Hulpprogramma´s voor Adreslijstintegratie</a>voor meer informatie) dit probleem op te lossen en om optimaal gebruikmaken van wachtwoord write-backs in uw organisatie te gaan.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Wachtwoord write-backs gebeurtenislogboek foutcodes
Er is een goede gewoonte bij het oplossen van problemen met het wachtwoord write-backs moet worden gecontroleerd die gebeurtenislogboek van toepassing op uw computer Azure AD Connect. Dit gebeurtenislogboek bevat gebeurtenissen uit twee bronnen van belang voor wachtwoord write-backs. De bron PasswordResetService beschrijft bewerkingen en met betrekking tot de werking van wachtwoord write-backs. De bron ADSync wordt beschrijven bewerkingen en problemen met het instellen van wachtwoorden in uw AD-omgeving.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Code</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Geef een naam / bericht</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Bron</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Beschrijving</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>PROBLEMEN: MMS(4924) 0x80230619 – "een beperking Hiermee voorkomt u dat het wachtwoord wordt gewijzigd in de huidige tabel die is opgegeven."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Deze gebeurtenis vindt plaats wanneer de service wachtwoord write-backs probeert een wachtwoord instellen op uw lokale map waarin het wachtwoord leeftijd, geschiedenis, complexiteit of filteren vereisten van het domein niet voldoet.</p>
              <ul>
                <li class="unordered">
Als u een leeftijd minimale wachtwoord, en het wachtwoord in dat venster tijd onlangs hebt gewijzigd, is het niet mogelijk om te wijzigen van het wachtwoord opnieuw totdat wordt de opgegeven leeftijd in uw domein bereikt. Voor testdoeleinden moet minimumleeftijd worden ingesteld op 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als er geschiedenis wachtwoordvereisten ingeschakeld en moet u een wachtwoord dat niet is gebruikt tijdens de laatste keer dat N, waarbij N de instelling van de geschiedenis wachtwoord. Als u een wachtwoord dat is gebruikt in de laatste N keer selecteert, klikt u vervolgens ziet u een fout in dit geval. Voor testdoeleinden moet geschiedenis worden ingesteld op 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als er complexiteit wachtwoordvereisten, worden alle labels afgedwongen wanneer de gebruiker probeert te wijzigen of een wachtwoord opnieuw instellen.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als u een wachtwoordfilters ingeschakeld hebt en een gebruiker selecteert een wachtwoord dat niet voldoet aan de filtercriteria, klikt u vervolgens het opnieuw instellen of wijzigen, bewerking mislukt.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Synchronisatie-Engine geretourneerd een fout hr = 80230402, bericht = een poging een object is mislukt omdat er dubbele items met het dezelfde anker zijn</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Deze gebeurtenis vindt plaats wanneer dezelfde gebruikersnaam is ingeschakeld in meerdere domeinen.  Deze fout kan bijvoorbeeld optreden als Account/Resource forests zijn gesynchroniseerd en dat u dezelfde gebruikersnaam aanwezig en ingeschakeld in elk.  </p>
              <p>Deze fout kan ook optreden als u een niet-unieke ankerkenmerk (zoals alias of UPN) gebruikt en twee gebruikers dat dezelfde ankerkenmerk delen.</p>
              <p>U lost dit probleem, zorg ervoor dat er geen individuele gebruikers in uw domeinen en dat u een unieke ankerkenmerk voor elke gebruiker gebruikt op te geven.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat de on-premises implementatie-service gedetecteerd een verzoek voor een federatieve voor wachtwoordherstel of hash Wachtwoordsynchronisatie 'd gebruiker die afkomstig zijn van de cloud. Deze gebeurtenis is dat de eerste gebeurtenis in elke wachtwoord opnieuw instellen voor bewerking write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat een gebruiker een nieuw wachtwoord geselecteerd tijdens het wachtwoord opnieuw instellen, wordt vastgesteld dat dit wachtwoord zakelijke wachtwoord voldoet, en dit wachtwoord is geschreven terug naar de lokale AD-omgeving.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat een gebruiker een wachtwoord ingeschakeld en dat wachtwoord aangekomen succes naar de on-premises omgeving, maar wanneer wordt geprobeerd het wachtwoord instellen in de lokale AD-omgeving, is een fout opgetreden. Dit kan gebeuren verschillende oorzaken hebben:</p>
              <ul>
                <li class="unordered">
Wachtwoord van de gebruiker niet voldoet aan de leeftijd, de geschiedenis, de complexiteit, of filteren vereisten voor het domein. Probeer een volledig nieuw wachtwoord u lost dit op.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
De serviceaccount MA heeft geen de juiste machtigingen voor het instellen van het nieuwe wachtwoord voor de betreffende gebruikersaccount.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Account van de gebruiker zich in een beveiligde groep, zoals domein of enterprise-beheerders neemt waarin set-bewerkingen voor wachtwoord is niet toegestaan.<br\><br\></li>
              </ul>
              <p>Zie <a href="#troubleshoot-password-writeback">Problemen met wachtwoord write-backs</a> voor meer informatie over meer informatie over welke andere situtions kunnen leiden tot deze fout.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis vindt plaats als u een wachtwoord write-backs met Azure AD Connect inschakelt en geeft aan dat we onboarding uw organisatie de webservice wachtwoord write-backs gestart.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat het onboarding-proces is geslaagd of wachtwoord write-backs videomogelijkheden gereed is voor gebruik.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat de on-premises implementatie-service een wijzigingsaanvraag wachtwoord voor een federatieve gedetecteerd of hash Wachtwoordsynchronisatie 'd gebruiker die afkomstig zijn van de cloud. Deze gebeurtenis is de eerste gebeurtenis in elke bewerking wachtwoord wijzigen write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat een gebruiker een nieuw wachtwoord tijdens een bewerking wachtwoord wijzigen geselecteerd, wordt vastgesteld dat dit wachtwoord zakelijke wachtwoord voldoet, en dit wachtwoord is geschreven terug naar de lokale AD-omgeving.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat een gebruiker een wachtwoord ingeschakeld en dat wachtwoord aangekomen succes naar de on-premises omgeving, maar wanneer wordt geprobeerd het wachtwoord instellen in de lokale AD-omgeving, is een fout opgetreden. Dit kan gebeuren verschillende oorzaken hebben:</p>
              <ul>
                <li class="unordered">
Wachtwoord van de gebruiker niet voldoet aan de leeftijd, de geschiedenis, de complexiteit, of filteren vereisten voor het domein. Probeer een volledig nieuw wachtwoord u lost dit op.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
De serviceaccount MA heeft geen de juiste machtigingen voor het instellen van het nieuwe wachtwoord voor de betreffende gebruikersaccount.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Account van de gebruiker zich in een beveiligde groep, zoals domein of enterprise-beheerders neemt waarin set-bewerkingen voor wachtwoord is niet toegestaan.<br\><br\></li>
              </ul>
              <p>Zie <a href="#troubleshoot-password-writeback">Problemen met wachtwoord write-backs</a> voor meer informatie over meer over welke andere situaties kunnen leiden tot deze fout.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>De on-premises implementatie-service gedetecteerd een verzoek voor een federatieve voor wachtwoordherstel of hash Wachtwoordsynchronisatie 'd gebruiker die afkomstig zijn van de beheerder namens een gebruiker. Deze gebeurtenis is de eerste gebeurtenis in elke bewerking door de beheerder geactiveerde wachtwoord opnieuw instellen write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>De beheerder een nieuw wachtwoord tijdens een door de beheerder geactiveerde wachtwoord opnieuw instellen bewerking is geselecteerd, wordt vastgesteld dat dit wachtwoord zakelijke wachtwoord voldoet, en dit wachtwoord is geschreven terug naar de lokale AD-omgeving.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>De beheerder een wachtwoord geselecteerd namens een gebruiker, en dit wachtwoord zijn aangekomen succes naar de on-premises omgeving, maar wanneer wordt geprobeerd het wachtwoord instellen in de lokale AD-omgeving, is een fout opgetreden. Dit kan gebeuren verschillende oorzaken hebben:</p>
              <ul>
                <li class="unordered">
Wachtwoord van de gebruiker niet voldoet aan de leeftijd, de geschiedenis, de complexiteit, of filteren vereisten voor het domein. Probeer een volledig nieuw wachtwoord u lost dit op.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
De serviceaccount MA heeft geen de juiste machtigingen voor het instellen van het nieuwe wachtwoord voor de betreffende gebruikersaccount.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Account van de gebruiker zich in een beveiligde groep, zoals domein of enterprise-beheerders neemt waarin set-bewerkingen voor wachtwoord is niet toegestaan.<br\><br\></li>
              </ul>
              <p>Zie <a href="#troubleshoot-password-writeback">Problemen met wachtwoord write-backs</a> voor meer informatie over meer informatie over welke andere situtions kunnen leiden tot deze fout.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis vindt plaats als u een wachtwoord write-backs met Azure AD Connect uitschakelt en geeft aan dat we offboarding uw organisatie de webservice wachtwoord write-backs gestart.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan het proces offboarding is geslaagd en dat wachtwoord write-backs videomogelijkheden succes is uitgeschakeld.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat het offboarding-proces is mislukt. Dit kan worden veroorzaakt door een foutbericht over machtigingen in de cloud of on-premises beheerdersaccount opgegeven tijdens het configureren van of omdat u probeert te gebruiken van de globale beheerder van een federatieve cloud tijdens het uitschakelen van wachtwoord write-backs. U lost dit probleem, Controleer uw administratieve machtigingen en dat u een gebruikt gekoppeld account tijdens het configureren van de mogelijkheid wachtwoord write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat het wachtwoord write-backs-service is gestart en gereed is voor het accepteren van wachtwoord management aanvragen vanuit de cloud.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat de wachtwoord write-backs-service is gestopt en dat een wachtwoord management aanvragen vanuit de cloud niet voltooid worden kunnen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat wordt opgehaald een Autorisatietoken voor de globale beheerder opgegeven tijdens de installatie van Azure AD Connect om te kunnen het offboarding of onboarding starten.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat we de wachtwoordversleutelingssleutel die wordt gebruikt gemaakt voor het coderen van wachtwoorden vanuit de cloud worden verzonden naar uw on-premises omgeving.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan een onbekende fout tijdens het beheer van een wachtwoord. Bekijk de Uitzonderingstekst in de gebeurtenis voor meer informatie. Als u problemen ondervindt, kunt u uitschakelen en opnieuw inschakelen wachtwoord write-backs. Als dit niet helpt, bevatten een kopie van het gebeurtenislogboek samen met de opgegeven id voor het bijhouden misbruik naar uw engineeren ondersteuning.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er is een fout bij verbinden met de cloud-wachtwoord opnieuw instellen van service en over het algemeen treedt op wanneer de on-premises implementatie-service is geen verbinding maken met de webservice voor wachtwoord opnieuw instellen. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er is een fout bij verbinden met uw tenant service bus exemplaar. Dit kan gebeuren omdat u uitgaande verbindingen in uw on-premises omgeving blokkeren. Controleer uw firewall om ervoor te zorgen u toestaan dat verbindingen via TCP 443 en <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>en probeer het opnieuw. Als u steeds problemen ondervindt nog, kunt u uitschakelen en opnieuw inschakelen wachtwoord write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat de invoer doorgegeven aan onze web service API ongeldig is. Probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er een fout decoderen het wachtwoord die zijn aangekomen vanuit de cloud. Dit kan zijn vanwege decoderen key niet overeen met de cloudservice uw on-premises omgeving. Als u wilt de oplossing uitschakelen en wachtwoord write-backs opnieuw inschakelen in uw on-premises omgeving.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tijdens het onboarding Sla we tenant-specifieke informatie in een configuratiebestand in uw on-premises omgeving. Deze gebeurtenis wordt aangegeven er is een fout opgetreden bij het opslaan van dit bestand of dat wanneer de service is gestart, er een fout van het bestand lezen. U lost dit probleem op door kunt uitschakelen en opnieuw inschakelen wachtwoord write-backs als u wilt dat een opnieuw schrijven van dit configuratiebestand. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AFGESCHAFT – deze gebeurtenis is niet aanwezig zijn in Azure AD Connect, alleen zeer eerdere versies van DirSync die worden ondersteund write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tijdens onboarding sturen we de gegevens uit de cloud het wachtwoord van de on-premises implementatie-service opnieuw instellen. Dat gegevens vervolgens naar een bestand in het geheugen geschreven worden voordat u deze gegevens veilig op schijf opslaan naar de synchronisatieservice is verzonden. Deze gebeurtenis geeft een probleem met schrijven of die gegevens in het geheugen bijwerken. U kunt dit probleem oplossen, kunt u proberen uitschakelen en opnieuw inschakelen wachtwoord write-backs als u wilt dat een opnieuw schrijven van deze configuratie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat we een ongeldig antwoord hebt ontvangen van de webservice voor wachtwoord opnieuw instellen. U kunt dit probleem oplossen, kunt u proberen uitschakelen en opnieuw inschakelen wachtwoord write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat we geen een vergunning token voor de opgegeven tijdens de installatie van Azure AD Connect account voor globale beheerder verkrijgen kan. Deze fout kan worden veroorzaakt door een ongeldige gebruikersnaam of wachtwoord voor de hoofdbeheerderaccount wordt aangegeven of omdat de hoofdbeheerderaccount opgegeven is gekoppeld. U lost dit probleem op door opnieuw uitvoeren configuratie heeft met de juiste gebruikersnaam en wachtwoord en controleer of de beheerder is een beheerde (alleen de cloud gebruikt of Wachtwoordsynchronisatie zou) account.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er is een fout opgetreden bij het genereren van het wachtwoord versleutelingssleutel of decoderen een wachtwoord van de cloudservice binnenkomt. Deze fout waarschijnlijk duidt op een probleem met uw omgeving. Bekijk de details van het gebeurtenislogboek voor meer informatie en kunt dit probleem oplossen. U kunt ook proberen uitschakelen en het wachtwoord write-backs-service voor het oplossen van deze opnieuw inschakelen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat de on-premises implementatie-service niet correct met de webservice voor wachtwoord opnieuw instellen communiceren kan naar het onboarding-proces te starten. Dit kan zijn vanwege een firewallregel of het probleem aan een token auth voor uw tenant. U lost dit probleem, zorg ervoor dat u niet worden geblokkeerd door uitgaande verbindingen via TCP 443 en TCP 9350-9354 of naar <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>en zodat de AAD-beheerdersaccount die u met ingebouwde niet is gekoppeld. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AFGESCHAFT – deze gebeurtenis is niet aanwezig zijn in Azure AD Connect, alleen zeer eerdere versies van DirSync die worden ondersteund write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat de on-premises implementatie-service niet correct kan communiceren met de webservice voor wachtwoord opnieuw instellen om te starten van het proces offboarding. Dit kan zijn vanwege een firewallregel of het probleem aan een Autorisatietoken voor uw tenant. U lost dit probleem, zorg ervoor dat u niet worden geblokkeerd door uitgaande verbindingen via 443 of naar <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>en dat het beheerdersaccount AAD u voor offboard gebruikt niet is gekoppeld. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat we hadden opnieuw als u wilt verbinden met uw tenant service bus exemplaar uit te voeren. Bij normaal, moet dit niet steeds een probleem, maar als u deze gebeurtenis in veel gevallen ziet kunt controleren van de netwerkverbinding met service bus, vooral als het een hoge latentie of de verbinding met lage bandbreedte.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Om te controleren van de status van uw wachtwoord write-backs-service, verzenden we onze wachtwoord heartbeat gegevens opnieuw webservice elke 5 minuten. Deze gebeurtenis wordt aangegeven dat er een fout opgetreden is bij het verzenden van deze informatie systeemstatus terug naar de cloud-webservice. Deze status informatie is niet inbegrepen bij een OII of PII gegevens en is zuiver een heartbeat en eenvoudige service statistieken worden aangepast zodat we bieden mogelijk service statusinformatie in de cloud.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er een onbekende fout die door AD, Controleer het gebeurtenislogboek van Azure AD Connect server voor gebeurtenissen uit de bron ADSync voor meer informatie over deze fout.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat de gebruiker die probeert te opnieuw instellen of wijzigen van een wachtwoord niet is gevonden in de on-premises adreslijst. Dit kan gebeuren wanneer de gebruiker verwijderde on-premises implementatie is, maar niet in de cloud, of als er een probleem bij het synchroniseren. Controleer uw synchronisatie-Logboeken, evenals de laatste paar synchronisatie uitvoeren details voor meer informatie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Wanneer een wachtwoord opnieuw instellen of aangevraagde wijziging is afkomstig van de cloud, gebruiken we de cloud-anker dat is opgegeven tijdens de installatie van Azure AD Connect om te bepalen hoe u dit verzoek koppeling terug naar een gebruiker in uw on-premises omgeving. Deze gebeurtenis wordt aangegeven dat we twee gebruikers in uw on-premises adreslijst met hetzelfde cloud ankerkenmerk gevonden. Controleer uw synchronisatie-Logboeken, evenals de laatste paar synchronisatie uitvoeren details voor meer informatie.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat de Agent van de Management service-account heeft geen de juiste machtigingen op het account betrokken voor het instellen van een nieuw wachtwoord. Controleer of het account MA in bos van de gebruiker opnieuw instellen en wijzigt u wachtwoord machtigingen zijn op alle objecten in de bos.  In deze, Zie uitvoeren voor meer informatie over het <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">stap 4: de juiste Active Directory-machtigingen instellen</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat we geprobeerd te opnieuw instellen of wijzigen van een wachtwoord voor een account dat on-premises is uitgeschakeld. Inschakelen van het account en probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Gebeurtenis geeft aan dat we geprobeerd te opnieuw instellen of wijzigen van een wachtwoord voor een account dat on-premises is vergrendeld. Uitsluitingen kunnen optreden wanneer een gebruiker heeft geprobeerd een wijziging of bewerking te vaak in een korte termijn wachtwoord opnieuw. Het account te ontgrendelen en probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis wordt aangegeven dat de gebruiker een onjuist wachtwoord voor de huidige opgegeven wanneer de bewerking uitvoeren van een wachtwoord wijzigen. Geef het juiste wachtwoord voor de huidige en probeer het opnieuw.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis vindt plaats wanneer de service wachtwoord write-backs probeert een wachtwoord instellen op uw lokale map waarin het wachtwoord leeftijd, geschiedenis, complexiteit of filteren vereisten van het domein niet voldoet.</p>
              <ul>
                <li class="unordered">
Als u een leeftijd minimale wachtwoord, en het wachtwoord in dat venster tijd onlangs hebt gewijzigd, is het niet mogelijk om te wijzigen van het wachtwoord opnieuw totdat wordt de opgegeven leeftijd in uw domein bereikt. Voor testdoeleinden moet minimumleeftijd worden ingesteld op 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als er geschiedenis wachtwoordvereisten ingeschakeld en moet u een wachtwoord dat niet is gebruikt tijdens de laatste keer dat N, waarbij N de instelling van de geschiedenis wachtwoord. Als u een wachtwoord dat is gebruikt in de laatste N keer selecteert, klikt u vervolgens ziet u een fout in dit geval. Voor testdoeleinden moet geschiedenis worden ingesteld op 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als er complexiteit wachtwoordvereisten, worden alle labels afgedwongen wanneer de gebruiker probeert te wijzigen of een wachtwoord opnieuw instellen.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Als u een wachtwoordfilters ingeschakeld hebt en een gebruiker selecteert een wachtwoord dat niet voldoet aan de filtercriteria, klikt u vervolgens het opnieuw instellen of wijzigen, bewerking mislukt.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Deze gebeurtenis geeft aan dat er is een probleem schrijven een wachtwoord terug naar uw on-premises adreslijst vanwege een probleem met Active Directory. Controleer van de computer van de Azure AD Connect-toepassing gebeurtenislogboek voor berichten van de service ADSync voor meer informatie over welke fout opgetreden. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AFGESCHAFT – deze gebeurtenis is niet aanwezig zijn in Azure AD Connect, alleen zeer eerdere versies van DirSync die worden ondersteund write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AFGESCHAFT – deze gebeurtenis is niet aanwezig zijn in Azure AD Connect, alleen zeer eerdere versies van DirSync die worden ondersteund write-backs.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AFGESCHAFT – deze gebeurtenis is niet aanwezig zijn in Azure AD Connect, alleen zeer eerdere versies van DirSync die worden ondersteund write-backs.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Problemen met wachtwoord write-backs connectivity

Als er serviceonderbrekingen met de component wachtwoord write-backs van Azure AD Connect, volgen hier enkele snelle stappen die u ondernemen kunt om dit probleem oplossen:

 - [Opnieuw starten van de Azure AD verbinden met synchronisatieservice](#restart-the-azure-AD-Connect-sync-service)
 - [Uitschakelen en de functie voor het wachtwoord write-backs opnieuw inschakelen](#disable-and-re-enable-the-password-writeback-feature)
 - [Installeer de meest recente versie van Azure AD Connect](#install-the-latest-azure-ad-connect-release)
 - [Wachtwoord write-backs oplossen](#troubleshoot-password-writeback)

In het algemeen, is het raadzaam dat u deze stappen uitvoeren in de bovenstaande volgorde om te kunnen herstellen van uw service op de snelste manier.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Opnieuw starten van de Azure AD verbinden met synchronisatieservice
Opnieuw starten van de Azure AD verbinden met synchronisatie-Service kan helpen oplossen van problemen met de serververbinding of andere tijdelijke problemen met de service.

 1. Klik op **Start** op de server met **Azure AD Connect**als beheerder.
 2. Typ **'services.msc'** in het zoekvak en druk op **Enter**.
 3. Zoek naar het **Microsoft Azure AD Connect** -fragment.
 4. Met de rechtermuisknop op de servicevermelding, klik op **Start opnieuw**en wacht totdat de bewerking is voltooid.

    ![][002]

Deze stappen wordt de verbinding met de cloudservice opnieuw tot stand te brengen en onderbrekingen kunt oplossen.  Als u de synchronisatie-Service opnieuw uw probleem niet is opgelost, wordt u aangeraden dat u wilt uitschakelen en de functie voor het wachtwoord write-backs een volgende stap opnieuw inschakelen.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Uitschakelen en de functie voor het wachtwoord write-backs opnieuw inschakelen
Uitschakelen en opnieuw inschakelen van de functie wachtwoord write-backs kunt connectivity problemen op te lossen.

 1. Als een beheerder van de **configuratiewizard van Azure AD Connect**te openen.
 2. Klik in het dialoogvenster **verbinding maken met Azure AD** voert u uw **referenties van Azure AD globale beheerder**
 3. Voer in het dialoogvenster **verbinding maken met AD DS** uw **beheerdersreferenties AD Domain Services**.
 4. Klik op de knop **volgende** in het dialoogvenster **een unieke aanduiding voor uw gebruikers** .
 5. Klik in het dialoogvenster **Extra opties** , schakel het selectievakje **wachtwoord terugschrijven** .

    ![][003]

 6. Klik op **volgende** in de overige dialoogvenster-pagina's zonder iets totdat u toegang hebt tot de pagina **Gereed om te configureren** .
 7. Zorg ervoor dat de pagina configureren ziet u het **wachtwoord terugschrijven optie als uitgeschakeld** en klik op de knop met groen **configureren** als uw wijzigingen wilt doorvoeren.
 8. Klik in het dialoogvenster **voltooid** , schakelt u de optie **Nu synchroniseren** en klik vervolgens op **Voltooien** om de wizard te sluiten.
 9. De **configuratiewizard van Azure AD Connect**opnieuw openen.
 10.    **Herhaal stappen 2-8**, tenzij u zorgen dat de **optionele onderdelen** of **Schakel het selectievakje wachtwoord terugschrijven** scherm naar de service opnieuw in te schakelen.

    ![][004]

Deze stappen wordt uw verbinding met onze cloudservice opnieuw tot stand te brengen en onderbrekingen kunt oplossen.

Als uitschakelen en opnieuw inschakelen van de functie wachtwoord write-backs wordt uw probleem niet is verholpen, wordt u aangeraden dat u probeert om Azure AD Connect een volgende stap opnieuw te installeren.

### <a name="install-the-latest-azure-ad-connect-release"></a>Installeer de meest recente versie van Azure AD Connect
Opnieuw installeren van de Azure AD Connect-pakket worden opgelost eventuele problemen met de configuratie die invloed is op de mogelijkheid om te een verbinding maken met onze cloudservices of wachtwoorden beheren in uw lokale AD-omgeving.
Het is raadzaam, u deze stap uitvoeren Probeer eerst de eerste twee stappen hierboven is beschreven.

 1. Download de meest recente versie van Azure AD Connect [hier](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Aangezien u Azure AD Connect al hebt geïnstalleerd, moet u alleen een in-place upgrade als u wilt uw Azure AD Connect-installatie bijwerken naar de nieuwste versie uitvoeren.
 3. Het gedownloade pakket uitvoeren en volg de aanwijzingen op het scherm instructies voor het bijwerken van uw computer Azure AD Connect.  Geen extra handmatige stappen zijn vereist, tenzij u het verouderd vak regels voor synchronisatie hebt aangepast zodat u moet **back-up maken voordat u verdergaat met upgraden en handmatig opnieuw deze implementeert nadat u klaar bent**.

Deze stappen wordt uw verbinding met onze cloudservice opnieuw tot stand te brengen en onderbrekingen kunt oplossen.

Als de installatie van de nieuwste versie van de Azure AD Connect-server uw probleem niet is opgelost, wordt u aangeraden dat u als laatste stap na de installatie van de meest recente synchronisatie QFE uitschakelen en opnieuw inschakelen wachtwoord write-backs probeert.

Als uw probleem niet is verholpen, klikt u vervolgens raadzaam dat u gaat u naar [Problemen met wachtwoord write-backs](#troubleshoot-password-writeback) en het [wachtwoord van Azure AD Management Veelgestelde vragen](active-directory-passwords-faq.md) om te zien als uw probleem er mogelijk worden besproken.


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw in documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD wachtwoorden opnieuw instellen:

* **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Hoe dit werkt**](active-directory-passwords-how-it-works.md) - meer informatie over de zes verschillende onderdelen van de service en wat elk bevat
* [**Aan de slag**](active-directory-passwords-getting-started.md) - Leer hoe u kunt u gebruikers opnieuw instellen en hun cloud of on-premises-wachtwoord wijzigen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van het uiterlijk en uiterlijk en gedrag van de service aan de behoeften van uw organisatie
* [**Aanbevolen procedures**](active-directory-passwords-best-practices.md) - Ontdek hoe u snel en effectief wachtwoorden in uw organisatie beheren
* [**Inzicht krijgen**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden
* [**Veelgestelde vragen**](active-directory-passwords-faq.md) - antwoorden op veelgestelde vragen
* [**Meer informatie**](active-directory-passwords-learn-more.md) - Ga diep op de technische details van de werking van de service



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
