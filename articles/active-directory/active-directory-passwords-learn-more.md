<properties
    pageTitle="Meer informatie: Azure AD wachtwoordbeheer | Microsoft Azure"
    description="Geavanceerde onderwerpen op Azure AD wachtwoordbeheer, inclusief hoe wachtwoord write-backs werkt, write-backs wachtwoordbeveiliging, hoe het wachtwoord opnieuw in werkt en welke gegevens worden gebruikt door de wachtwoorden opnieuw instellen."
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
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="learn-more-about-password-management"></a>Meer informatie over wachtwoordbeheer

> [AZURE.IMPORTANT] **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Als u al wachtwoordbeheer hebt geïmplementeerd of slechts gastgebruikers zijn voor meer informatie over de technische mailindelingen dat ingaan hoe dit werkt voordat u implementeert, in dit gedeelte krijgt u een handig overzicht van de technische concepten achter de service. Besproken het volgende:

* [**Wachtwoord write-backs overzicht**](#password-writeback-overview)
  - [De werking van wachtwoord write-backs](#how-password-writeback-works)
  - [Scenario's die worden ondersteund voor wachtwoord write-backs](#scenarios-supported-for-password-writeback)
  - [Wachtwoord write-backs beveiligingsmodel](#password-writeback-security-model)
* [**Hoe het wachtwoord opnieuw in portal werk?**](#how-does-the-password-reset-portal-work)
  - [Welke gegevens worden gebruikt door de wachtwoorden opnieuw instellen?](#what-data-is-used-by-password-reset)
  - [Toegang tot wachtwoord opnieuw instellen van gegevens voor uw gebruikers](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Wachtwoord write-backs overzicht
Wachtwoord write-backs is een onderdeel van [Azure Active Directory verbinding maken](active-directory-aadconnect.md) die kan worden ingeschakeld en kan worden gebruikt door de huidige abonnees van Azure Active Directory Premium. Zie [Azure Active Directory-edities](active-directory-editions.md)voor meer informatie.

Wachtwoord write-backs kunt u uw tenant cloud u schrijft wachtwoorden terug naar u lokale Active Directory configureren.  U hoeft te instellen en beheren van een oplossing ingewikkeld on-premises implementatie selfservice-wachtwoord opnieuw instellen overbodig maakt en biedt een handige manier cloudgebaseerde voor uw gebruikers kunnen hun on-premises implementatie wachtwoorden opnieuw instellen waar ze ook zijn.  Lees verder voor enkele van de belangrijkste functies van wachtwoord write-backs:

- **Nul feedback vertraging.**  Wachtwoord write-backs is een synchrone bewerking.  Uw gebruikers wordt onmiddellijk steeds gewaarschuwd als hun wachtwoord niet voldoet aan beleid of is niet mogen worden opnieuw instellen of wijzigen voor welke reden dan ook.
- **Ondersteunt het opnieuw instellen van wachtwoorden voor gebruikers met AD FS of andere technologieën Federatie.**  Met wachtwoord write-backs, zolang de federatieve gebruikersaccounts worden gesynchroniseerd naar uw Azure AD-tenant, ze kunnen voor het beheren van hun lokale AD wachtwoorden vanuit de cloud.
- **Ondersteunt het opnieuw instellen van wachtwoorden voor gebruikers met hash Wachtwoordsynchronisatie.** Wanneer het wachtwoord opnieuw instellen service wordt gedetecteerd dat een gesynchroniseerde gebruikersaccount is ingeschakeld voor Wachtwoordsynchronisatie hash, opnieuw wordt ingesteld zowel van dit account on-premises en cloud wachtwoord tegelijk.
- **Ondersteunt het wijzigen van wachtwoorden uit het deelvenster toegang en Office 365.**  Wanneer federatieve of Wachtwoordsynchronisatie zou gebruikers bij het wijzigen van hun wachtwoorden verlopen of niet-verlopen komt, we wordt terugschrijven deze wachtwoorden naar uw lokale AD-omgeving.
- **Ondersteunt terugschrijven van wachtwoorden als een beheerder opnieuw hij of zij de** [**Azure-beheerportal**](https://manage.windowsazure.com).  Wanneer een beheerder opnieuw instellen van het wachtwoord van een gebruiker in de [Beheerportal van Azure](https://manage.windowsazure.com), als deze gebruiker wordt gekoppeld of Wachtwoordsynchronisatie zou wordt we het wachtwoord dat Hiermee selecteert u de beheerder op uw lokale AD, ook instellen.  Dit is momenteel niet ondersteund in de beheerportal van Office.
- **Uw on-premises afdwingt AD wachtwoordbeleidsregels.**  Wanneer een gebruiker opnieuw zijn of haar wachtwoord instelt, we controleren of het voldoet aan uw on-premises Active Directory-beleid voordat u deze in die map vastlegt.  Dit geldt ook voor geschiedenis, complexiteit leeftijd, wachtwoordfilters en eventuele andere wachtwoordbeperkingen die u hebt gedefinieerd in uw lokale AD.
- **Alle inkomende firewallregels, geen vereist.**  Een relay Azure Service Bus wachtwoord write-backs gebruikt als een onderliggende communicatiekanaal, wat betekent dat u niet hoeft geen binnenkomende poorten in de firewall voor deze functie kan werken te openen.
- **Wordt niet ondersteund voor gebruikersaccounts die zijn opgeslagen in de beveiligde groepen in uw lokale Active Directory.** Zie voor meer informatie over de beveiligde groepen, [beveiligde Accounts en groepen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>De werking van wachtwoord write-backs
Wachtwoord write-backs heeft drie hoofdonderdelen:

- Wachtwoord opnieuw instellen-cloudservice (dit is ook geïntegreerd met pagina's van Azure AD wachtwoord wijzigen)
- Tenant / regiospecifieke Azure Service Bus relay
- On-premises wachtwoordherstel eindpunt

Deze passen samen zoals is beschreven in de onderstaande diagram:

  ![][001]

Wanneer een federatieve of wachtwoord hash synchroniseert, zou gebruiker wordt geleverd opnieuw instellen of wijzigen van zijn of haar wachtwoord in de cloud, gebeurt het volgende:

1.  We controleren om te zien welk type wachtwoord de gebruiker heeft.  Als we het wachtwoord wordt beheerd on-premises wordt weergegeven, klikt u vervolgens we Controleer of dat de service write-backs actief is.  Als dat zo is, we dat de gebruiker die doorgaan, als dit nog niet het geval is, wordt de gebruiker melden dat hun wachtwoord kan geen hier worden opnieuw.
2.  Vervolgens wordt de gebruiker de juiste verificatiemethode gates worden doorgegeven en bereikt van het scherm van de wachtwoord opnieuw instellen.
3.  De gebruiker selecteert een nieuw wachtwoord en bevestigt dit.
4.  Na het op verzenden klikt, versleutelt we het leesbare wachtwoord met een symmetric sleutel die is gemaakt tijdens de installatie write-backs.
5.  Na het wachtwoord versleutelt, opnemen we in een pakket dat wordt verzonden via een kanaal HTTPS naar uw tenant specifieke service bus relay (die we ook voor u instellen tijdens de installatie write-backs).  Deze relay is beveiligd met een willekeurig wordt gegenereerd wachtwoord dat alleen uw on-premises implementatie-installatie weet.
6.  Wanneer het bericht service bus bereikt, wordt het wachtwoord opnieuw instellen eindpunt automatisch kunt u snel weer en blijkt dat er een aanvraag opnieuw instellen in behandeling.
7.  De service vervolgens Hiermee wordt gezocht naar de gebruiker betrokken met behulp van de cloud anker-kenmerk.  Voor deze zoekactie alleen mogelijk als het gebruikersobject moet bestaan in de ruimte AD-connector, moet het worden gekoppeld aan de bijbehorende MV-object en deze moet worden gekoppeld aan het bijbehorende AAD verbindingslijn object. Ten slotte, in de volgorde voor synchronisatie met deze gebruikersaccount zoeken, de koppeling van AD verbindingslijn object naar MV moet hebben de synchronisatie-regel `Microsoft.InfromADUserAccountEnabled.xxx` op de koppeling.  Dit is nodig omdat wanneer u de oproep ontvangt vanuit de cloud, de synchronisatie-engine het kenmerk cloudAnchor gebruikt om het object AAD verbindingslijn ruimte te zoeken en volgt de koppeling terug naar het object MV en volgt de koppeling naar het object AD. Omdat het is mogelijk dat er meerdere AD-objecten (met meerdere bos) voor dezelfde gebruiker, de synchronisatie-engine is afhankelijk van de `Microsoft.InfromADUserAccountEnabled.xxx` koppeling naar de juiste datumnotatie kiezen.
8.  Als het gebruikersaccount wordt gevonden, proberen we het wachtwoord rechtstreeks in de juiste AD-bos opnieuw instellen.
9.  Als het wachtwoord set-bewerking geslaagd is, zien we de gebruiker het wachtwoord is gewijzigd en die ze op hun prettige manier kunnen gaan.
10. Als het wachtwoord mislukt instelt, wordt de fout weer voor de gebruiker en laten probeer het opnieuw.  De bewerking mislukt mogelijk omdat de service geselecteerd is, omdat het wachtwoord dat ze geselecteerd voldoet niet aan het beleid van organisatie omdat we de gebruiker niet in de lokale AD vinden kan, of een aantal redenen.  We hebben van een specifiek bericht voor veel van deze gevallen en de gebruiker vertellen wat zij kunnen doen om het probleem te verhelpen.

### <a name="scenarios-supported-for-password-writeback"></a>Scenario's die worden ondersteund voor wachtwoord write-backs
De onderstaande tabel wordt beschreven welke scenario's worden ondersteund voor welke versies van onze mogelijkheden synchroniseren.  Het wordt in het algemeen wordt ten zeerste aanbevolen dat u de nieuwste versie van [Azure AD Connect](active-directory-aadconnect.md#install-azure-ad-connect) installeren als u wilt gebruiken wachtwoord write-backs.

  ![][002]

### <a name="password-writeback-security-model"></a>Wachtwoord write-backs beveiligingsmodel
Wachtwoord write-backs is een krachtige en zeer veilige-service.  Om te zorgen dat uw gegevens is beveiligd, inschakelen we een beveiligingsmodel 4 lagen die hieronder wordt beschreven.

- **Tenant specifieke service-bus relay** – bij het instellen van de service, die we instellen van een tenant-specifieke service bus-relay dat is beveiligd met een willekeurig wordt gegenereerd sterk wachtwoord dat Microsoft nooit toegang tot heeft.
- **Vergrendeld, versleutelen sterk wachtwoord versleuteling-toets ingedrukt** – nadat de service bus relay is gemaakt, maak een sterk symmetric sleutel die we gebruiken om te versleutelen van het wachtwoord in zoals ze over het netwerk komen.  Deze toets bevindt zich alleen in geheime winkel van uw bedrijf in de cloud, die sterk is vergrendeld en gecontroleerd, net als een wachtwoord in de adreslijst.
- **Industriële standaard TLS** – wanneer u een wachtwoord opnieuw instellen of wijzigen van de bewerking wordt weergegeven in de cloud, we nemen het leesbare wachtwoord coderen met uw openbare sleutel.  We vervolgens plop die in een HTTPS-mailbericht die via een versleuteld kanaal met Microsoft SSL-certificaten aan uw service bus relay is verzonden.  Nadat u dat bericht binnenkomt op Service Bus uw on-premises-agent kunt u snel weer, wordt geverifieerd door het wachtwoord die eerder zijn gegenereerd met bus voor Service, het versleutelde bericht wordt opgehaald, ontsleutelt deze het wachtwoord via de AD DS SetPassword API instellen met de persoonlijke sleutel die wordt gegenereerd, waarna pogingen.  Deze stap is wat kunnen wij afdwingen van uw AD on-premises wachtwoordbeleid (complexiteit, leeftijd, geschiedenis, filters, enzovoort) in de cloud.
- **Bericht verloopbeleid** – ten slotte, als om een bepaalde reden het bericht wordt weergegeven in de Service Bus omdat uw on-premises-service niet beschikbaar is, deze timed-out en verwijderd na een paar minuten om te kunnen nog verder beveiligen.

## <a name="how-does-the-password-reset-portal-work"></a>Hoe het wachtwoord opnieuw in portal werk?
Wanneer een gebruiker navigeert naar de portal voor wachtwoordherstel, een werkstroom wordt gestart om te bepalen of die account geldig is, welke organisatie die gebruikers behoort, waar die gebruikerswachtwoord wordt beheerd, en of de gebruiker is licentie de functie te gebruiken.  Lees de onderstaande stappen voor meer informatie over de logica achter de pagina wachtwoord opnieuw instellen.

1.  Gebruiker klikt op de geen toegang tot uw accountkoppeling of Hiermee gaat u rechtstreeks naar [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2.  Gebruiker voert een gebruikers-id en een captcha worden doorgegeven.
3.  Azure AD wordt gecontroleerd of als de gebruiker kan deze functie gebruiken door het volgende te doen:
    - Hiermee wordt gecontroleerd op dat de gebruiker heeft deze functie is ingeschakeld en een Azure AD-licentie toegewezen.
        - Als de gebruiker beschikt niet over deze functie is ingeschakeld of een licentie is toegewezen, wordt de gebruiker gevraagd contact opnemen met zijn of haar beheerder om zijn of haar wachtwoord opnieuw te.
    - Controles voor dat de gebruiker het recht heeft uitdaging gegevens die zijn gedefinieerd op zijn of haar account beheerder beleid.
        - Als het beleid vereist is voor slechts één uitdaging, is er vervolgens gezorgd dat de gebruiker de juiste gegevens die zijn gedefinieerd voor ten minste één van de uitdagingen ingeschakeld door het beheerdersbeleid heeft.
          - Als de gebruiker niet is geconfigureerd, klikt u vervolgens de gebruiker voor het contact op met zijn of haar beheerder om zijn of haar wachtwoord opnieuw te aanbevolen.
        - Als het beleid twee uitdagingen vereist, is er vervolgens gezorgd dat de gebruiker de juiste gegevens die zijn gedefinieerd voor ten minste twee van de uitdagingen ingeschakeld door het beheerdersbeleid heeft.
          - Als de gebruiker niet is geconfigureerd, is we de gebruiker contact op met zijn of haar-beheerder om zijn of haar wachtwoord opnieuw instellen.
    - Hiermee wordt gecontroleerd of wachtwoord van de gebruiker wordt beheerd on-premises (federatieve of hash Wachtwoordsynchronisatie had).
       - Als write-backs wordt geïmplementeerd en wachtwoord van de gebruiker wordt beheerd on-premises, is klikt u vervolgens de gebruiker toegestaan om verder te verifiëren en zijn of haar wachtwoord opnieuw instellen.
       - Als write-backs niet is geïmplementeerd en wachtwoord van de gebruiker wordt beheerd on-premises, klikt u vervolgens de gebruiker gevraagd contact opnemen met zijn of haar beheerder om zijn of haar wachtwoord opnieuw te.
4.  Als blijkt dat de gebruiker kan zijn of haar wachtwoord opnieuw ingesteld, klikt u vervolgens de gebruiker geholpen bij het opnieuw instellen.

Meer informatie over het implementeren van wachtwoord write-backs bij [aan de slag: Azure AD-wachtwoordbeheer](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Welke gegevens worden gebruikt door de wachtwoorden opnieuw instellen?
Het overzichten van de volgende tabel waar en hoe deze gegevens wordt gebruikt tijdens wachtwoorden opnieuw instellen en is ontworpen om te bepalen welke verificatie beschikbaar zijn voor uw organisatie. Deze tabel ziet u ook alle opmaak vereisten voor situaties waarin u gegevens hebt opgenomen namens gebruikers vanaf invoer paden die deze gegevens niet worden gevalideerd.

> [AZURE.NOTE] Telefoon op kantoor niet wordt weergegeven in de portal Registratie omdat gebruikers zijn momenteel niet kunnen deze eigenschap in de adreslijst bewerken.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>De naam van de contactmethode</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Azure Active Directory gegevenselement</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Gebruikte / worden ingesteld waar?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Vereisten voor opmaak</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefoon op kantoor</p>
            </td>
            <td>
              <p>Telefoonnummer</p>
              <p>bijvoorbeeld Set-MsolUser - UserPrincipalName JWarner@contoso.com - PhoneNumber '+ 1 1234567890 x 1234'</p>
            </td>
            <td>
              <p>Gebruikt in:</p>
              <p>Wachtwoord opnieuw instellen Portal</p>
              <p>Instelbaar uit:</p>
              <p>PhoneNumber kan worden ingesteld via PowerShell, DirSync, Azure Management Portal en de beheerportal van Office</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (bijvoorbeeld + 1 1234567890)</p>
              <ul>
                <li class="unordered">
Een landcode moeten opgeven<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Een netnummer moet opgeven (indien van toepassing)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Een + vóór van het land code moet hebben opgeven<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Een spatie tussen landnummer en de rest van het getal moet hebben<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Extensies worden niet ondersteund, hebt u een extensies die zijn opgegeven, wordt we dit verwijderen uit het getal voordat het telefoongesprek verzenden.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobiele telefoon</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>OF-BEWERKING</p>
              <p>MobilePhone</p>
              <p>(Verificatie die telefoon wordt gebruikt als er gegevens aanwezig zijn, anders dit valt terug naar het veld mobiele telefoon).</p>
              <p>bijvoorbeeld Set-MsolUser - UserPrincipalName JWarner@contoso.com - MobilePhone '+ 1 1234567890 x 1234'</p>
            </td>
            <td>
              <p>Gebruikt in:</p>
              <p>Wachtwoord opnieuw instellen Portal</p>
              <p>Registratie-Portal</p>
              <p>Instelbaar uit: </p>
              <p>AuthenticationPhone kan worden ingesteld van de portal voor de registratie van wachtwoord opnieuw instellen of de portal voor registratie van MFA.</p>
              <p>MobilePhone kan worden ingesteld via PowerShell, DirSync, Azure Management Portal en de beheerportal van Office</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (bijvoorbeeld + 1 1234567890)</p>
              <ul>
                <li class="unordered">
Moet ondersteuning bieden voor een land in.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Geef een netnummer (indien van toepassing).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Moet hebben een + vóór van het land code opgeven.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Moet u een spatie tussen landnummer en de rest van het getal.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Extensies worden niet ondersteund, hebt u een extensies die zijn opgegeven, wordt genegeerd wanneer het telefoongesprek verzenden.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Alternatieve e</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>OF-BEWERKING</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Verificatie die e-mail wordt gebruikt als er gegevens aanwezig zijn, anders dit valt terug naar het veld alternatieve e).</p>
              <p>Opmerking: het alternatieve e-veld is opgegeven als een matrix van tekenreeksen in de adreslijst.  We gebruiken het eerste item in de volgende matrix.</p>
              <p>bijvoorbeeld Set-MsolUser - UserPrincipalName JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>Gebruikt in:</p>
              <p>Wachtwoord opnieuw instellen Portal</p>
              <p>Registratie-Portal</p>
              <p>Instelbaar uit: </p>
              <p>AuthenticationEmail kan worden ingesteld van de portal voor de registratie van wachtwoord opnieuw instellen of de portal voor registratie van MFA.</p>
              <p>AlternateEmail kan worden ingesteld in PowerShell de beheerportal Azure en de Office-beheerportal</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>of甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
E-mailberichten moeten volgen als per standaardopmaak.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Unicode-e-mailberichten worden ondersteund.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Beveiliging vragen en antwoorden</p>
            </td>
            <td>
              <p>Niet beschikbaar voor het wijzigen van rechtstreeks in de adreslijst.</p>
            </td>
            <td>
              <p>Gebruikt in:</p>
              <p>Wachtwoord opnieuw instellen Portal</p>
              <p>Registratie-Portal </p>
              <p>Instelbaar uit: </p>
              <p>De enige manier om in te stellen vragen over beveiliging is via de Portal van Azure Management.</p>
              <p>De enige manier om in te stellen vindt u antwoorden op vragen over de beveiliging voor een bepaalde gebruiker is via de registratie-Portal.</p>
            </td>
            <td>
              <p>Beveiliging vragen hebben een max 200 tekens en een min van 3 tekens</p>
              <p>Antwoorden hebben een max 40 tekens en een min van 3 tekens</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Toegang tot wachtwoord opnieuw instellen van gegevens voor uw gebruikers
####<a name="data-settable-via-synchronization"></a>Gegevens worden ingesteld via synchronisatie
De volgende velden kunnen worden gesynchroniseerd vanuit on-premises:

* Mobiele telefoon
* Telefoon op kantoor

####<a name="data-settable-with-azure-ad-powershell"></a>Gegevens worden ingesteld met Azure AD PowerShell
De volgende velden zijn toegankelijk met Azure AD PowerShell & de API Graph:

* Alternatieve e
* Mobiele telefoon
* Telefoon op kantoor
* Verificatie-telefoon
* Verificatie-e-mailbericht

####<a name="data-settable-with-registration-ui-only"></a>Gegevens worden ingesteld met alleen registratie UI
De volgende velden zijn alleen toegankelijk via de registratie SSPR UI (https://aka.ms/ssprsetup):

* Beveiliging vragen en antwoorden

####<a name="what-happens-when-a-user-registers"></a>Wat gebeurt er wanneer een gebruiker wordt geregistreerd?
Wanneer een gebruiker wordt geregistreerd, wordt de registratiepagina **altijd** instellen de volgende velden:

* Verificatie-telefoon
* Verificatie-e-mailbericht
* Beveiliging vragen en antwoorden

Als u een waarde voor **mobiele telefoon** of een **Alternatief e**hebt opgegeven, kunnen gebruikers onmiddellijk gebruiken die opnieuw instellen van hun wachtwoorden vermeld, zelfs als ze nog niet hebt geregistreerd voor de service.  Bovendien gebruikers raadpleegt u deze waarden bij het registreren voor de eerste keer, en deze desgewenst wijzigen.  Echter nadat deze is geregistreerd, deze waarden bewaard in de velden **Verificatie telefoon** en **Verificatie-e** , respectievelijk.

Dit is een handige manier om de blokkering van grote aantallen gebruikers SSPR gebruiken en gebruikers voor het valideren van deze gegevens het registratieproces toch te.

####<a name="setting-password-reset-data-with-powershell"></a>Instelling voor wachtwoordherstel gegevens met PowerShell
U kunt waarden voor de volgende velden met Azure AD PowerShell instellen.

* Alternatieve e
* Mobiele telefoon
* Telefoon op kantoor

Als u wilt beginnen, moet u eerst [downloaden](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)en installeren van de Azure AD PowerShell-module.  Nadat u geïnstalleerd hebt, kunt u de onderstaande stappen voor het configureren van elk veld volgen.

#####<a name="alternate-email"></a>Alternatieve e
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Mobiele telefoon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Telefoon op kantoor
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Gegevens met PowerShell voor wachtwoordherstel lezen
U kunt waarden voor de volgende velden met Azure AD PowerShell lezen.

* Alternatieve e
* Mobiele telefoon
* Telefoon op kantoor
* Verificatie-telefoon
* Verificatie-e-mailbericht

Als u wilt beginnen, moet u eerst [downloaden](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)en installeren van de Azure AD PowerShell-module.  Nadat u geïnstalleerd hebt, kunt u de onderstaande stappen voor het configureren van elk veld volgen.

#####<a name="alternate-email"></a>Alternatieve e
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Mobiele telefoon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Telefoon op kantoor
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Verificatie-telefoon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>Verificatie-e-mailbericht
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw in documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD wachtwoorden opnieuw instellen:

* **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Hoe dit werkt**](active-directory-passwords-how-it-works.md) - meer informatie over de zes verschillende onderdelen van de service en wat elk bevat
* [**Aan de slag**](active-directory-passwords-getting-started.md) - Leer hoe u kunt u gebruikers opnieuw instellen en hun cloud of on-premises-wachtwoord wijzigen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van het uiterlijk en uiterlijk en gedrag van de service aan de behoeften van uw organisatie
* [**Aanbevolen procedures**](active-directory-passwords-best-practices.md) - Ontdek hoe u snel en effectief wachtwoorden in uw organisatie beheren
* [**Inzicht krijgen**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden
* [**Veelgestelde vragen**](active-directory-passwords-faq.md) - antwoorden op veelgestelde vragen
* [**Problemen oplossen**](active-directory-passwords-troubleshoot.md) - Leer hoe u snel problemen oplossen met de service



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
