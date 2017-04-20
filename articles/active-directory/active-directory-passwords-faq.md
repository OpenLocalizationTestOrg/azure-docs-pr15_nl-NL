<properties
    pageTitle="Veelgestelde vragen over: Azure AD wachtwoordbeheer | Microsoft Azure"
    description="Veelgestelde vragen over wachtwoordbeheer in Azure AD, inclusief wachtwoord opnieuw instellen, registratie, rapporten en write-backs met lokale Active Directory."
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

# <a name="password-management-frequently-asked-questions"></a>Wachtwoordbeheer Veelgestelde vragen

> [AZURE.IMPORTANT] **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Hier volgen enkele veelgestelde vragen voor alle mogelijke die betrekking hebben op wachtwoordbeheer.

Als u merkt dat met een vraag die u niet weet wie het antwoord op, of met bepaalde problemen die u zijn tegenover elkaar liggen zoekt, kunt u lezen op hieronder om te zien als we hebben besproken deze al.  Als we nog niet is gedaan, geen probleem. Je mag rustig vraag er waarop geen hier op de [Azure AD-Forums](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) en we gaat u terug wilt naar u zodra we kunt.

Deze Veelgestelde vragen over opgedeeld in de volgende secties:

- [**Vragen over het wachtwoord opnieuw instellen registreren**](#password-reset-registration)
- [**Vragen over wachtwoorden opnieuw instellen**](#password-reset)
- [**Vragen over wachtwoord Management rapporten**](#password-management-reports)
- [**Vragen over wachtwoord write-backs**](#password-writeback)

## <a name="password-reset-registration"></a>Registratie voor wachtwoordherstel
 - **V: kunnen mijn gebruikers hun eigen wachtwoord opnieuw instellen gegevens registreren?**

 > **A:** Ja, zo lang maken als wachtwoorden kunnen opnieuw is ingeschakeld en ze worden in licentie gegeven, ze gaat u naar het wachtwoord opnieuw instellen registratie-portal op http://aka.ms/ssprsetup voor het registreren hun verificatie-informatie voor gebruik met wachtwoorden opnieuw instellen. Gebruikers kunnen ook registreren door te gaan naar het deelvenster access op http://myapps.microsoft.com, op het profieltabblad te klikken en te klikken op het Register voor de optie wachtwoord opnieuw instellen. Meer informatie over hoe u uw gebruikers geconfigureerd voor het wachtwoord opnieuw instellen door te lezen hoe u gebruikers geconfigureerd voor het wachtwoord opnieuw instellen.

 - **V: kan ik gegevens van wachtwoord opnieuw instellen namens mijn gebruikers definiëren?**

 > **A:** Ja, u kunt doen met DirSync of PowerShell of via de portal [Azure-beheerportal](https://manage.windowsazure.com) of beheerder van Office. Meer informatie over deze functie op het blogbericht verbeterde Privacy voor Azure AD MFA en wachtwoord opnieuw instellen telefoonnummers en door te lezen informatie over hoe gegevens worden gebruikt door wachtwoord opnieuw instellen.

 - **V: kan ik gegevens voor vragen van de beveiliging van on-premises synchroniseren?**

 > **A:** Nee, dit is vandaag nog niet mogelijk, maar we dit overwegen.

 - **V: kunnen mijn gebruikers gegevens registreren zodanig dat andere gebruikers deze gegevens niet zien?**

 > **A:** Ja, wanneer gebruikers registreert gegevens met behulp van de Portal registratie van wachtwoord opnieuw in deze wordt opgeslagen in privé verificatie-velden die alleen zichtbaar zijn door globale beheerders en de gebruiker zelf. Meer informatie over deze functie op het blogbericht verbeterde Privacy voor Azure AD MFA en wachtwoord opnieuw in telefoonnummers en door te lezen informatie over hoe gegevens worden gebruikt door wachtwoord opnieuw instellen.

 - **V: Mijn gebruikers moet worden geregistreerd voordat zij wachtwoorden opnieuw instellen gebruiken kunnen?**

 > **A:** Nee, als u weinig verificatiegegevens namens hen definieert, gebruikers hebben geen registreren. Wachtwoorden kunnen opnieuw werkt prima zo lang maken als u gegevens die zijn opgeslagen in de juiste velden in de adreslijst correct hebt opgemaakt. Meer informatie over door te lezen informatie over hoe gegevens worden gebruikt door de wachtwoorden opnieuw instellen.

 - **V: kan ik synchroniseren of instellen van de verificatie-e-mailbericht of alternatieve verificatie telefoon velden, verificatie telefoon namens mijn gebruikers?**

 > **A:** Momenteel niet, maar we bezig zijn met het inschakelen van deze mogelijkheid.

 - **V: hoe weet de registratie-portal welke opties voor het weergeven van mijn gebruikers?**

 > **A:** Het wachtwoord opnieuw instellen registratie-portal bevat alleen de opties die u hebt ingeschakeld voor uw gebruikers onder de sectie gebruiker opnieuw instellen wachtwoordbeleid van uw map configureren tabblad. Dit betekent dat als u niet, bijvoorbeeld beveiliging vragen inschakelt, klikt u vervolgens gebruikers kunnen geen kunnen registreren voor die optie.

 - **V: wanneer een gebruiker het volgende wordt geregistreerd?**

 > **A:** Een gebruiker wordt beschouwd als geregistreerd wanneer hij of zij ten minste heeft N stukken verificatie info gedefinieerd, waarbij N staat voor het nummer van verificatie methoden nodig die u hebt ingesteld in de [Beheerportal van Azure](https://manage.windowsazure.com). Zie meer informatie aan te passen gebruiker opnieuw instellen wachtwoordbeleid.


## <a name="password-reset"></a>Wachtwoorden opnieuw instellen

 - **V: hoe lang moet ik wachten moet ontvangen een e-mail, SMS of telefoongesprek van wachtwoorden opnieuw instellen?**

 > **A:** E-mail, SMS-berichten en telefoongesprekken moet komen terecht in onder 1 minuut, met het normale hoofdlettergebruik wordt 5-20 seconden. Als u niet de melding in deze periode ontvangt, controleert u uw map Ongewenste e-mail, die het nummer / waarmee contact wordt gemaakt van e-mail is de presentatie die u verwacht en of de verificatiegegevens in de map juist is ingedeeld. Meer informatie over het opmaken van telefoonnummers en e-mailbericht Zie adressen voor gebruik met wachtwoorden opnieuw instellen in Leer hoe gegevens worden gebruikt door de wachtwoorden opnieuw instellen.

 - **V: welke talen worden ondersteund door de wachtwoorden opnieuw instellen?**

 > **A:** Het wachtwoord opnieuw instellen UI, SMS-berichten en gesprekken in dezelfde 40 talen die worden ondersteund in Office 365 zijn gelokaliseerd. Dit zijn: Arabisch, Bulgaars, vereenvoudigd Chinees, Chinees, traditioneel Chinees, Kroatisch, Tsjechisch, Deens, Nederlands, Engels, Ests, Fins, Frans, Duits, Grieks, Hebreeuws, Hindi, Hongaars, Indonesisch, Italiaans, Japans, Kazachstaans, Koreaans, Lets, Litouws, Maleis (Maleisië), Noors (Bokmål), Pools, Portugees (Brazilië), Portugees (Portugal), Roemeens, Russisch, Servisch (Latijns), Slowaaks, Sloveens, Spaans, Zweeds, Thais, Turks, Oekraïens, en Vietnamees.

 - **V: welke delen van het wachtwoord opnieuw instellen ervaring krijgen huisstijl wanneer ik instel organisatie-huisstijl in mijn map bevindt zich tabblad configureren?**

 > **A:** Het wachtwoord opnieuw instellen portal uw organisatie-logo wordt weergegeven en kunt u voor het configureren van de contactpersoon die uw beheerder koppeling zodat deze verwijzen naar een aangepaste e-mail of URL. Een e-mailbericht dat wordt verzonden door de wachtwoorden opnieuw instellen van uw organisatie logo, kleuren (in dit geval rood), naam in de hoofdtekst van het e-mailbericht, bevat en aangepast van naam. Zie een voorbeeld waarbij alle onderstaande huisstijl elementen. Lees meer informatie het uiterlijk aanpassen wachtwoorden opnieuw instellen.

  ![][001]

 - **V: hoe kan ik mijn gebruikers waar u het wachtwoord opnieuw in te stellen trainen?**

 > **A:** U kunt uw gebruikers rechtstreeks naar https://passwordreset.microsoftonline.com verzenden of u kunt opgeven dat ze op de geen toegang tot uw accountkoppeling gevonden op elke School of werk-ID aanmeldingsscherm. U kunt je mag rustig deze koppelingen publiceren (of URL omleidingen ze maken) op welke plaats die gemakkelijk toegankelijk is voor uw gebruikers.

 - **V: kan ik deze pagina vanaf een mobiel apparaat gebruiken?**

 > **A:** Ja, werkt deze pagina op mobiele apparaten.

 - **V: ondersteund ontgrendelen lokale active directory-accounts wanneer gebruikers hun wachtwoorden opnieuw instellen?**

 > **A:** Ja, wanneer een gebruiker opnieuw instelt zijn of haar wachtwoord en wachtwoord write-backs is geïmplementeerd met alle versies van Azure AD Connect of versies van Azure AD-synchronisatie 1.0.0485.0222 of hoger, van die gebruiker account automatisch ontgrendelde is dan wanneer die gebruiker zijn of haar wachtwoord opnieuw instelt.

 - **V: hoe kan ik wachtwoord opnieuw in rechtstreeks aan op mijn bureaublad aanmeldingsproblemen gebruikerservaring integreren?**

 > **A:** Dit is niet mogelijk vandaag. Echter als u absoluut nodig deze mogelijkheid een Azure AD Premium-klant bent, kunt u Microsoft identiteit Manager gratis installeren en implementeren de on-premises implementatie wachtwoord opnieuw instellen oplossing gevonden daarin voor het oplossen van deze vereiste.

 - **V: kan ik verschillende beveiliging vragen voor andere landinstellingen instellen?**

 > **A:** Nee, dit is vandaag nog niet mogelijk, maar we dit overwegen.

 - **V: hoe veel vragen kunnen we configureren voor de optie beveiliging vragen verificatie?**

 > **A:** U kunt maximaal 20 aangepaste beveiliging vragen in de [Beheerportal van Azure](https://manage.windowsazure.com)configureren.

 - **V: hoe lang mogelijk beveiliging vragen?**

 > **A:** Vragen over beveiliging mogelijk tussen 3 en 200 tekens bevatten.

 - **V: hoe lang mogelijk vindt u antwoorden op vragen over beveiliging?**

 > **A:** Antwoorden mogelijk 3 tot en met 40 tekens lang zijn.

 - **V: zijn dubbele vindt u antwoorden op vragen over beveiliging afgewezen?**

 > **A:** Ja, we dubbele vindt u antwoorden op vragen over beveiliging negeren.

 - **V: mei een gebruiker registreren meer dan één van de dezelfde beveiligingsvraag?**

 > **A:** Nee, wanneer een gebruiker een bepaald criterium registreert, hij of zij mogelijk niet registreren voor deze vraag een tweede maal.

 - **V: is het mogelijk om een minimale limiet van beveiliging vragen voor registratie instellen en opnieuw instellen?**

 > **A:** Ja, kan een limiet worden ingesteld voor registratie en een andere voor opnieuw instellen. vragen over de beveiliging van 3 tot en met 5 mogelijk zijn vereist voor de registratie en 3 tot en met 5 mogelijk zijn vereist voor de opnieuw instellen.

 - **V: als een gebruiker heeft meer dan het maximum aantal vragen die zijn vereist voor het opnieuw geregistreerd, hoe worden vragen over beveiliging geselecteerd tijdens opnieuw instellen?**

 > **A:** Vragen over de beveiliging van N worden willekeurig geselecteerd uit het totale aantal vragen die een gebruiker heeft geregistreerd voor, waarbij N staat voor het minimum aantal vragen die zijn vereist voor het wachtwoord opnieuw instellen. Bijvoorbeeld als een gebruiker 5 beveiliging vragen geregistreerd heeft, maar alleen 3 zijn vereist voor het opnieuw instellen, 3 van deze 5 geselecteerd willekeurig en gepresenteerd aan de gebruiker op het moment van opnieuw instellen. Als de gebruiker krijgt de antwoorden op vragen gegaan, wordt het selectieproces opnieuw optreedt om te voorkomen dat vraag hammering.

 - **V: worden u voorkomen dat gebruikers poging wachtwoord opnieuw in vaak in een korte periode?**

 > **A:** Ja, er zijn verschillende beveiligingsvoorzieningen over wachtwoorden opnieuw instellen. Gebruikers kunnen alleen proberen 5 wachtwoord opnieuw instellen pogingen binnen een uur voordat u 24 uur wordt uitgesloten. Gebruikers kunnen alleen proberen om deze te valideren een telefoonnummer 5 keer binnen een uur voordat u 24 uur worden vergrendeld. Gebruikers kunnen alleen een enkele verificatiemethode proberen 5 keer binnen een uur voordat u 24 uur worden vergrendeld.

 - **V: voor hoe lang zijn de e-mail- en SMS eenmalige toegangscode geldig?**

 > **A:** De sessie levensduur van wachtwoorden opnieuw instellen is 105 minuten. Dit betekent dat vanaf het begin van het wachtwoord opnieuw instellen van betrekking heeft, heeft de gebruiker 105 minuten zijn of haar wachtwoord opnieuw in te stellen. De e-mailbericht en SMS eenmalige toegangscode zijn ongeldig als deze periode is verlopen.


## <a name="password-management-reports"></a>Wachtwoordbeheer-rapporten

 - **V: hoe lang duurt het duren voordat gegevens worden weergegeven op de wachtwoord management rapporten?**

 > **A:** Gegevens moet worden weergegeven in de wachtwoord management rapporten binnen 5-10 minuten. Het aantal exemplaren het op een uur duren kan moet worden weergegeven.

 - **V: hoe kan ik de wachtwoord management rapporten filteren?**

 > **A:** U kunt het wachtwoord management rapporten filteren door te klikken op het kleine Vergrootglas extreme rechts van de kolomlabels bovenaan in het rapport (Zie schermafbeelding). Als u doen rijkere filteren wilt, kunt u het rapport naar excel en maak een draaitabel downloaden.

  ![][002]

 - **V: Wat is het maximum aantal gebeurtenissen worden opgeslagen in de wachtwoord management rapporten?**

 > **A:** Maximaal 1000 wachtwoord opnieuw instellen of wachtwoord opnieuw instellen registratie gebeurtenissen opgeslagen in de wachtwoord management-rapporten.  We werken om uit te vouwen dit nummer als u wilt meer gebeurtenissen opnemen.

 - **V: hoe ver terug de wachtwoordbeheer rapporten gaan doen?**

 > **A:** Het wachtwoord management rapporten met bewerkingen die in de afgelopen 30 dagen. Wij onderzoeken momenteel hoe u dit een langere periode. Voorlopig als u wilt archiveren van deze gegevens kunt u regelmatig de rapporten downloaden en deze opslaan in een andere locatie.

 - **V: is er een maximum aantal rijen dat kan worden weergegeven op de wachtwoord management rapporten?**

 > **A:** Ja, een maximum van 1000 rijen wordt mogelijk weergegeven op een van de rapporten wachtwoordbeheer, ongeacht of ze worden weergegeven in de gebruikersinterface of worden gedownload. Wij onderzoeken momenteel hoe u deze limiet verhoogt.

 - **V: is er een API voor toegang tot de wachtwoorden opnieuw instellen of de registratie rapportagegegevens?**

 > **A:** Ja, raadpleegt u de volgende documentatie voor meer informatie over hoe u toegang hebt tot de gegevensstroom rapportage wachtwoorden opnieuw instellen.  [Leer hoe u toegang tot wachtwoord opnieuw in rapportage gebeurtenissen via programmacode](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Wachtwoord write-backs
 - **V: hoe werkt wachtwoord write-backs achter de schermen?**

 > **A:** Zie [dat hoe wachtwoord write-backs werkt](active-directory-passwords-learn-more.md#how-password-writeback-works) voor een gedetailleerde uitleg van wat er gebeurt wanneer u wachtwoord write-backs, ook inschakelt hoe gegevens in het systeem terug in uw on-premises omgeving doorloopt. Zie [wachtwoord write-backs beveiligingsmodel](active-directory-passwords-learn-more.md#password-writeback-security-model) in hoe wachtwoord write-backs werkt als u wilt weten hoe we zorgen dat wachtwoord write-backs is een zeer veilige service.

 - **V: hoe lang duurt wachtwoord write-backs voordat werken?  Is er een vertraging van de synchronisatie zoals met hash Wachtwoordsynchronisatie?**

 > **A:** Wachtwoord write-backs is direct. Het is een synchroon pijplijn die fundamenteel anders dan hash Wachtwoordsynchronisatie werkt. Wachtwoord write-backs kan gebruikers realtime feedback vragen over het succes van hun wachtwoorden opnieuw instellen of wijzigen van de bewerking. De gemiddelde tijd voor een succesvolle write-backs van een wachtwoord is onder 500 ms.

 - **V: welke typen accounts werkt wachtwoord write-backs voor?**

 > **A:** Wachtwoord write-backs werkt voor federatieve en Hash Wachtwoordsynchronisatie 'd gebruikers.

 - **V: wachtwoord write-backs afdwingen wachtwoordbeleidsregels van mijn domein?**

 > **A:** Ja, wachtwoord write-backs leeftijd wachtwoord, geschiedenis, complexiteit, filters en eventuele andere beperkingen die u in het gedeelte van wachtwoorden in uw lokale domein plaatsen mogelijk afgedwongen.

 - **V: is wachtwoord write-backs veilig?  Hoe kan ik ervoor dat ik won't krijgen hacked zijn?**

 > **A:** Ja, wachtwoord write-backs erg veilig is. Meer informatie over de 4 lagen van beveiliging geïmplementeerd door de service wachtwoord write-backs, raadpleegt u het [wachtwoord write-backs beveiligingsmodel](active-directory-passwords-learn-more.md#password-writeback-security-model) in hoe wachtwoord write-backs werkt.




## <a name="links-to-password-reset-documentation"></a>Koppelingen naar wachtwoord opnieuw in documentatie
Hieronder vindt u koppelingen naar alle pagina's documentatie Azure AD wachtwoorden opnieuw instellen:

* **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [**Hoe dit werkt**](active-directory-passwords-how-it-works.md) - meer informatie over de zes verschillende onderdelen van de service en wat elk bevat
* [**Aan de slag**](active-directory-passwords-getting-started.md) - Leer hoe u kunt u gebruikers opnieuw instellen en hun cloud of on-premises-wachtwoord wijzigen
* [**Aanpassen**](active-directory-passwords-customize.md) - informatie over het aanpassen van het uiterlijk en uiterlijk en gedrag van de service aan de behoeften van uw organisatie
* [**Aanbevolen procedures**](active-directory-passwords-best-practices.md) - Ontdek hoe u snel en effectief wachtwoorden in uw organisatie beheren
* [**Inzicht krijgen**](active-directory-passwords-get-insights.md) - meer informatie over onze geïntegreerde rapportagemogelijkheden
* [**Problemen oplossen**](active-directory-passwords-troubleshoot.md) - Leer hoe u snel problemen oplossen met de service
* [**Meer informatie**](active-directory-passwords-learn-more.md) - Ga diep op de technische details van de werking van de service


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
