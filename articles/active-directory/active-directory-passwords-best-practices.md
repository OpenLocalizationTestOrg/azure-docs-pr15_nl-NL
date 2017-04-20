<properties
    pageTitle="Aanbevolen procedures: Azure AD wachtwoordbeheer | Microsoft Azure"
    description="Implementatie en het gebruik van de aanbevolen procedures, voorbeeld eindgebruikers documentatie en training handleidingen voor wachtwoordbeheer in Azure Active Directory."
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

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Wachtwoordbeheer implementeert en trainen van gebruikers gebruiken

> [AZURE.IMPORTANT] **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Wanneer wachtwoordherstel inschakelen, is de volgende stap die u moet uitvoeren om te zorgen dat gebruikers met de service in uw organisatie. Hiervoor hebt u nodig om ervoor te zorgen dat uw gebruikers zijn geconfigureerd voor gebruik van de service correct en ook dat uw gebruikers de training die ze nodig hebben bij het beheren van eigen wachtwoorden lukt hebben. In dit artikel wordt uitgelegd u de volgende begrippen:

* [**Hoe kom ik aan uw gebruikers geconfigureerd voor wachtwoordbeheer**](#how-to-get-users-configured-for-password-reset)
  * [Wat is een account dat is geconfigureerd voor het wachtwoord opnieuw instellen](#what-makes-an-account-configured)
  * [Manieren waarop u kunt in te vullen verificatiegegevens uzelf](#ways-to-populate-authentication-data)
* [**De beste manieren om te implementeren wachtwoord opnieuw instellen voor uw organisatie**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [Op basis van een e-uitrol + steekproef e-communicatie](#email-based-rollout)
  * [Uw eigen aangepaste wachtwoord management portal voor uw gebruikers maken](#creating-your-own-password-portal)
  * [Het gebruik van afgedwongen registratie gebruiker registreren op aanmelden](#using-enforced-registration)
  * [Het uploaden van verificatiegegevens voor gebruikersaccounts](#uploading-data-yourself)
* [**Steekproef-gebruiker en ondersteuning trainingsmateriaal (binnenkort)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>Hoe kom ik aan gebruikers die is geconfigureerd voor wachtwoorden opnieuw instellen
Dit onderwerp vindt u verschillende methoden waarmee u ervoor zorgen dat elke gebruiker in uw organisatie de beschikking over Self-service wachtwoord opnieuw in effectief dat kunt geval ze hun wachtwoord vergeten.

### <a name="what-makes-an-account-configured"></a>Wat is een account dat is geconfigureerd
Voordat u een gebruiker de beschikking over wachtwoorden opnieuw instellen, moet **alle** volgende voorwaarden worden voldaan:

1.  Wachtwoorden kunnen opnieuw moet zijn ingeschakeld in de adreslijst.  Leer hoe u het wachtwoord opnieuw instellen door te lezen [dat gebruikers kunnen hun Azure AD-wachtwoorden opnieuw instellen](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) of [gebruikers opnieuw instellen of wijzigen van hun AD-wachtwoorden kunnen](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) inschakelen
2.  De gebruiker moet beschikken.
 - Voor gebruikers van de cloud moet de gebruiker **geen betaald Office 365-licentie**, of een **Eenvoudige AAD** of **AAD Premium-licentie** is toegewezen.
 - Voor on-premises gebruikers (federatieve of hash gesynchroniseerd), de gebruiker **moet beschikken over een AAD Premium-licentie toegewezen**.
3.  Een gebruiker beschikken over de **minimale set verificatiegegevens gedefinieerd** basis van het huidige wachtwoord opnieuw instellen beleid.
 - Verificatiegegevens wordt beschouwd als gedefinieerd als het bijbehorende veld in de adreslijst juist opgemaakte gegevens bevat.
 - Een minimale set verificatiegegevens is gedefinieerd op **minimaal één** van de verificatieopties ingeschakeld als een beleid één heeft bereikt is geconfigureerd, of **minimaal twee** van de verificatieopties ingeschakeld als een beleid twee heeft bereikt is geconfigureerd.
4.  Als de gebruiker een on-premises implementatie-account gebruikt is, moet klikt u vervolgens [Wachtwoord write-backs](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) worden ingeschakeld en ingeschakeld

### <a name="ways-to-populate-authentication-data"></a>Manieren om te vullen verificatiegegevens
U hebt verschillende opties voor het opgeven van gegevens voor gebruikers in uw organisatie moet worden gebruikt voor het wachtwoord opnieuw instellen.

- Gebruikers in de [Beheerportal van Azure](https://manage.windowsazure.com) of de [Office 365-beheerportal](https://portal.microsoftonline.com) bewerken
- Azure AD-synchronisatie gebruiken voor het synchroniseren van gebruikerseigenschappen in Azure AD vanuit een on-premises Active Directory-domein
- Windows PowerShell voor het bewerken van gebruikerseigenschappen van de aan de hand van [de volgende stappen proberen](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)gebruiken.
- Gebruikers toestaan zich te registreren van hun eigen gegevens door deze naar de registratie-portal op [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
- Vereisen dat gebruikers om te registreren voor het wachtwoord opnieuw instellen wanneer ze zich aanmeldt bij hun Azure AD-account door in te stellen de [**vereisen dat gebruikers om te registreren als u zich aanmeldt?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) configuratieoptie op **Ja**.

Gebruikers hoeft niet te registreren voor het wachtwoord opnieuw instellen voor het systeem om te werken.  Bijvoorbeeld als u bestaande Mobiel of telefoonnummers van office in uw lokale adreslijst hebt, kunt u ze in Azure AD synchroniseren en we gebruiken deze voor het wachtwoord opnieuw in automatisch.

U kunt ook meer informatie over het [hoe gegevens worden gebruikt door wachtwoord opnieuw instellen](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) en [Hoe kunt u afzonderlijke verificatie-velden met PowerShell vullen](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)lezen.

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Wat is de beste manier om te implementeren wachtwoord opnieuw instellen voor gebruikers?
Hier volgen de algemene uitrol stappen voor het wachtwoord opnieuw instellen:

1.  Wachtwoord opnieuw in uw adreslijst door te gaan naar het tabblad **configureren** in de [Beheerportal van Azure](https://manage.windowsazure.com) en **Ja** te selecteren voor de optie **Gebruikers zijn ingeschakeld voor het wachtwoord opnieuw instellen** inschakelen.
2.  De juiste licenties toewijzen aan elke gebruiker aan wie u bieden wachtwoord opnieuw wilt de door te gaan naar het tabblad **licenties** in de [Beheerportal van Azure](https://manage.windowsazure.com).
3.  (Optioneel) beperken wachtwoord opnieuw instellen op een groep gebruikers naar het implementeren van de functie traag na verloop van tijd door de wisselknop **Beperkte toegang wachtwoord opnieuw in te** stellen op **Ja** en een beveiligingsgroep moet worden ingeschakeld voor wachtwoordherstel (Let op deze gebruikers alle licenties moeten zijn toegewezen) te selecteren.
4.  Moeten uw gebruikers gebruik wachtwoord opnieuw in beide door ze te sturen een e-mailbericht dat ze hebt geregistreerd, zodat registratie in het deelvenster access of door de juiste verificatiegegevens voor deze gebruikers zelf uploaden via DirSync, PowerShell of de [Beheerportal van Azure](https://manage.windowsazure.com)afgedwongen.  Hieronder vindt u meer informatie hierover.
5.  Na verloop van tijd gebruikers door te navigeren naar het tabblad rapporten en het [**Wachtwoord opnieuw instellen registratie activiteit**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) rapport bekijkt registreren herzien
6.  Nadat u een goede aantal gebruikers hebt geregistreerd, bekijkt u deze gebruik wachtwoord opnieuw instellen door te navigeren naar het tabblad rapporten en het [**Wachtwoord opnieuw instellen activiteit**](active-directory-passwords-get-insights.md#view-password-reset-activity) rapport bekijkt.

Er zijn verschillende manieren aan de gebruikers informeren dat ze kunnen registreren voor en gebruik wachtwoord opnieuw in uw organisatie.  Ze worden hieronder beschreven.

### <a name="email-based-rollout"></a>Op basis van een e-uitrol
Misschien is de eenvoudigste manier om te de gebruikers informeren over aanmelden voor of gebruik wachtwoord opnieuw in door ze te sturen een e-mailbericht dat ze kunt doen.  Hieronder ziet u een sjabloon die kunt u dit wilt doen.  Je mag rustig voor het vervangen van de kleuren / logo's met die van uw eigen kiezen om aan te passen deze aan uw behoeften voldoet.

  ![][001]

U kunt [de e-mailsjabloon downloaden](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>De portal van uw eigen wachtwoord maken
Een strategie die geschikt voor grotere klanten implementeert beheermogelijkheden wachtwoord is het opzetten van één 'wachtwoord portal"die uw gebruikers gebruiken kunnen voor het beheren van alles met betrekking tot hun wachtwoorden op één locatie.  

Veel van onze grootste klanten wilt maken van een basiscertificeringsinstantie DNS-vermelding, zoals https://passwords.contoso.com met koppelingen naar het Azure AD-wachtwoord opnieuw in portal, wachtwoordherstel registratie-portal en wachtwoord wijzigen pagina's.  Op deze manier in een e-mailberichten of Folders die u verstuurt, kunt u ook een enkel, gemakkelijk te onthouden, URL die gebruikers gaat u kunnen naar wanneer ze een tweede aan de slag met de service.

Als u wilt aan de slag hier we een eenvoudige pagina met de meest recente heeft gereageerd UI ontwerp paradigma's hebt gemaakt en op alle browsers en mobiele apparaten werken.

  ![][007]

U kunt [downloaden van de website van de sjabloon hier](https://github.com/kenhoff/password-reset-page).  Het is raadzaam om het logo en de kleuren aan de behoeften van uw organisatie aan te passen.

### <a name="using-enforced-registration"></a>Afgedwongen registratie gebruiken
Als u wilt dat uw gebruikers kunnen registreren voor zelf opnieuw instellen, kunt u ook om te registreren als ze zich bij het deelvenster access op [http://myapps.microsoft.com aanmelden](http://myapps.microsoft.com)afdwingen.  Doordat de optie **Gebruikers vereisen te registreren wanneer u zich aanmeldt het Configuratiescherm Access** kunt u deze optie uit de tabblad **configureren** van uw map inschakelen.  

U kunt desgewenst ook definiëren al dan niet wordt gevraagd dat u opnieuw wilt registreren na een configureerbare tijdsperiode doordat de optie **aantal dagen voordat gebruikers moeten hun gegevens van contactpersonen bevestigen** moeten een waarde dan nul. Zie [Gebruiker wachtwoord Management gedrag aanpassen](active-directory-passwords-customize.md#password-management-behavior) voor meer informatie.

  ![][002]

Nadat u deze optie wordt ingeschakeld als gebruikers zich bij het deelvenster access aanmelden, zien ze een pop-up dat deze wordt gemeld dat de beheerder deze om te controleren of de contactgegevens is vereist. Zij kunnen deze gebruiken voor het wachtwoord opnieuw wordt ingesteld als ze ooit geen toegang meer tot hun-account.

  ![][003]

Te klikken op **Nu controleren** brengt ze naar de **portal Registratie voor wachtwoordherstel** bij [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) en is vereist om te registreren.  Registratie via deze methode kan worden gesloten door te klikken op de knop **Annuleren** of u het venster sluit, maar gebruikers een herinnering telkens wanneer ze zich aanmelden als ze niet registreert.

  ![][004]

### <a name="uploading-data-yourself"></a>Uploaden van gegevens
Als u uploaden verificatiegegevens uzelf wilt, klikt u vervolgens gebruikers hoeft niet te registreren voor het wachtwoord opnieuw in te kunnen hun wachtwoorden opnieuw instellen.  Zo lang maken als gebruikers hebben beleid die u hebt gedefinieerd door de verificatiegegevens gedefinieerd op hun account die voldoet aan het wachtwoord opnieuw instellen en klik vervolgens die gebruikers kunnen hun wachtwoorden opnieuw instellen.

Meer informatie over welke eigenschappen die u via AAD verbinding maken of Windows PowerShell instellen kunt, ziet u [welke gegevens worden gebruikt door de wachtwoorden opnieuw instellen](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

U kunt de verificatiegegevens via de [Beheerportal van Azure](https://manage.windowsazure.com) uploaden door de onderstaande stappen:

1.  Navigeer naar de map in de **Active Directory-extensie** in de [Beheerportal van Azure](https://manage.windowsazure.com).
2.  Klik op het tabblad **gebruikers** .
3.  Selecteer de gebruiker die u geïnteresseerd in de lijst bent.
4.  Klik op het eerste tabblad vindt u **Alternatieve e**, die kunnen worden gebruikt als een eigenschap waarmee wachtwoorden opnieuw instellen.

    ![][005]

5.  Klik op het tabblad **Info werk** .
6.  Klik op deze pagina vindt u **Telefoon op kantoor**, **mobiele telefoon**, **Verificatie telefoon**en **Verificatie-e-mailbericht**.  Deze eigenschappen kunnen ook worden ingesteld zodat een gebruiker zijn of haar wachtwoord opnieuw in te stellen.

    ![][006]

Zie [welke gegevens worden gebruikt door de wachtwoorden opnieuw instellen](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) om te zien hoe elk van deze eigenschappen kan worden gebruikt.

Zie [hoe u toegang tot wachtwoord opnieuw instellen van gegevens voor gebruikers van PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) om te zien hoe u kunt lezen en deze gegevens met PowerShell instellen.

## <a name="sample-training-materials"></a>Voorbeeld trainingsmateriaal
We werken aan steekproef trainingsmateriaal die u gebruiken kunt om uw IT-organisatie en uw gebruikers snel aan de slag snel over de implementatie en gebruik van wachtwoorden opnieuw instellen.  Blijf lopende!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw in documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD wachtwoorden opnieuw instellen:

* **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Hoe dit werkt**](active-directory-passwords-how-it-works.md) - meer informatie over de zes verschillende onderdelen van de service en wat elk bevat
* [**Aan de slag**](active-directory-passwords-getting-started.md) - Leer hoe u kunt u gebruikers opnieuw instellen en hun cloud of on-premises-wachtwoord wijzigen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van het uiterlijk en uiterlijk en gedrag van de service aan de behoeften van uw organisatie
* [**Inzicht krijgen**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden
* [**Veelgestelde vragen**](active-directory-passwords-faq.md) - antwoorden op veelgestelde vragen
* [**Problemen oplossen**](active-directory-passwords-troubleshoot.md) - Leer hoe u snel problemen oplossen met de service
* [**Meer informatie**](active-directory-passwords-learn-more.md) - Ga diep op de technische details van de werking van de service



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
