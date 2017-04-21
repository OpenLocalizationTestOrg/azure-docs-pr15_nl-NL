<properties
pageTitle="Installeren en hulpprogramma's voor beschikbaar is in GitHub instellen"
description="Hulpprogramma's en procedure voor het ophalen voor het ontwerpen van Azure inhoud in GitHub ingesteld."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Installeren en hulpprogramma's voor beschikbaar is in GitHub instellen

Volg de stappen in dit artikel voor het instellen van hulpprogramma's voor die bijdragen aan de Azure technische documentatie. Incidentele en incidentele inzenders waarschijnlijk de beschikking over de GitHub UI beschreven in stap 2.

Als u niet bekend met cijfer bent, wilt u misschien moet worden gereviseerd terminologie cijfer: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Daarnaast deze thread StackOverflow bevat een lijst van cijfer termen die u in deze reeks stappen tegenkomt: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Inhoud

- [Maak een GitHub-account en uw profiel worden ingesteld](#create-a-github-account-and-set-up-your-profile)
- [Registreren voor Disqus](#sign-up-for-disqus)
- [Bepalen of u echt moet de rest van deze stappen volgen](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [Machtigingen in GitHub](#permissions-in-github)
- [Cijfer voor Windows installeren](#install-git-for-windows)
- [Tweeledige verificatie inschakelen](#enable-two-factor-authentication)
- [Een editor korting installeren](#install-a-markdown-editor)
- [Atom configureren](#configure-atom)
- [Zich de opslagplaats splitsen en deze kopiëren naar uw computer](#fork-the-repository-and-copy-it-to-your-computer)
- [Uw gebruikersnaam in te voeren en lokaal e-mail configureren](#configure-your-user-name-and-email-locally)
- [Volgende stappen](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Maak een GitHub-account en uw profiel worden ingesteld

Als u wilt bijdragen aan de inhoud van de Azure technische, moet u een [GitHub](http://www.github.com) -account.

Als u een Microsoft-Inzender, moet u uw account GitHub instellen zodat u duidelijk wordt herkend als een Microsoft-werknemer. Stel uw profiel als volgt in:

- **Profielfoto**: een afbeelding van u (vereist)
- **Naam**: de naam van de eerste en laatste (vereist)
- **E-mail**: uw Microsoft e-mailadres (optioneel)
- **Bedrijf**: Microsoft Corporation (vereist)
- **Locatie**: uw woonplaats (optioneel)

Uw profiel moet er dan ongeveer als dit profiel:

<p align="center">
 ![Voorbeeld van GitHub-profiel](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Registreren voor Disqus

Elke gepubliceerde Azure technische artikel heeft een opmerking stream verstrekt door de service Disqus.

 ![Dialoog-logo](./media/tools-and-setup/discus.png)

Als u een Microsoft-werknemer bent en als u de auteur van of een inzender naar een artikel, moet u zich aanmeldt voor Disqus, zodat u kunt de opmerking stream voor het artikel deelnemen.

1. Registreren voor een account voor [http://www.disqus.com/](http://www.disqus.com/)
2. Vul uw profiel als volgt in:

 - **Volledige naam**: uw volledige naam als worden weergegeven in uw Microsoft-mailadres adresboek aanbieding, plus de tussen haakjes info, dat wil zeggen de alias plus @MSFT. Indeling: *eerst laatst [alias@MSFT] *
 - **Locatie**: uw locatie
 - **Korte Bio**: uw titel

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Bepalen of u echt moet de rest van deze stappen volgen

Mogelijk moet u niet volgt u de stappen in dit artikel. Dit is afhankelijk van het sorteren van inhoud bijdrage die u wilt of moet maken.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Een wijziging alleen tekst aan een bestaand artikel indienen

Als u alleen nodig of u wilt aanbrengen tekstuele updates in een bestaand artikel, moet waarschijnlijk niet u de rest van de stappen te volgen. U kunt de GitHub korting op het web editor gebruiken om in te dienen van uw wijzigingen. Klik op de koppeling GitHub in het artikel die u wilt wijzigen:

 ![Voorbeeld van GitHub-profiel](./media/tools-and-setup/contributetogit.png)

 Klik vervolgens op het bewerkingspictogram in de GitHub-versie van het artikel

 ![Voorbeeld van GitHub-profiel](./media/tools-and-setup/editicon.PNG)

 Die wordt geopend met de eenvoudig-en-klare webeditor waarmee u gemakkelijk wijzigingen wilt indienen. U hoeft niet te volgen de overige stappen in dit artikel.

###<a name="all-other-changes"></a>Alle andere wijzigingen
De UI GitHub ondersteunt het maken van nieuwe bestanden slepen en neerzetten van afbeeldingen. Echter wanneer u in de gebruikersinterface werkt, zijn vertakkingen beheren duidelijker is meestal raden u de's zijn geïnstalleerd om te zien van de opdrachten voor het maken en beheren van artikelen. Als u gebruiken van de gebruikersinterface wilt, Zie:

- [Bestanden op Github maken](https://github.com/blog/1327-creating-files-on-github)
- [Bestanden uploaden naar uw opslagplaatsen](https://github.com/blog/2105-upload-files-to-your-repositories)

Voor de volgende soorten werk, wordt aangeraden u installeert en leer hoe u met de hulpmiddelen voor:

 - Belangrijkste wijzigingen aanbrengt in een artikel
 - Maken en publiceren van een nieuw artikel
 - Nieuwe afbeeldingen toevoegen of bijwerken van afbeeldingen
 - Bijwerken van een artikel over een periode van dagen zonder publicerende wijzigingen van deze dagen
 - Maken van inhoud voor een release die moet op een bepaalde dag op een bepaald tijdstip

##<a name="permissions-in-github"></a>Machtigingen in GitHub

Iedereen met een account GitHub kan bijdragen aan Azure technische inhoud via onze openbare opslagplaats bij [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content). Geen speciale machtigingen zijn vereist.

Als u een Microsoft-PM of schrijver die op Azure inhoud werkt, moet u werken in onze privé opslagplaats voor inhoud, azure-inhoud-prijs. Ga naar [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) om aan te vragen de leesmachtigingen waarmee bijdragen aan goede doelen tot en met de persoonlijke cessies‑retrocessies - te kunnen komen Meld u aan bij GitHub met de knop > Klik op Azure > klikt u op **deelnemen aan een team** of **deelnemen aan een ander team**, en zoek vervolgens naar en deelnemen aan de groep **azure-inhoud-gelezen** .

## <a name="install-git-for-windows"></a>Cijfer voor Windows installeren

Cijfer voor Windows installeren vanaf [http://git-scm.com/download/win](http://git-scm.com/download/win). Deze download installaties cijfer versiebeheer en het cijfer we vaker doen, de opdrachtregel app die u gebruiken wilt om te communiceren met uw lokale cijfer opslagplaats installeert.

U kunt de standaardinstellingen; accepteren Als u de opdrachten beschikbaar is binnen de opdrachtregel van Windows, schakelt u de optie die dit heeft ingeschakeld.

<p align="center">
 ![Voorbeeld van GitHub-profiel](./media/tools-and-setup/gitbashinstall.png)

(Notitie: dit is niet hetzelfde als "Github voor Windows". "Github voor Windows" is een ander gebruikersinterface gebaseerde hulpmiddel die ook werken als u wilt meer lezen over uzelf. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Tweeledige verificatie inschakelen

U hebt twee factor-verificatie (2FA) inschakelen op uw GitHub-account als u in de persoonlijke inhoud opslagplaats werkt. Dit vereist in de persoonlijke bibliotheek.

Volg de instructies in zowel de volgende GitHub help-onderwerpen om dit mogelijk:

- [Informatie over tweeledige verificatie](https://help.github.com/articles/about-two-factor-authentication/)
- [Maken van een toegangstoken voor de opdrachtregel gebruiken](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Wanneer u het token hebt gemaakt, selecteert u alle bereiken die beschikbaar zijn in de gebruikersinterface van tokens maken ([meer informatie over elk bereik](https://developer.github.com/v3/oauth/#scopes))

Nadat u 2FA inschakelt, moet u het toegangstoken in plaats van uw wachtwoord GitHub bij de opdrachtprompt invoeren wanneer u probeert voor toegang tot een bibliotheek GitHub vanaf de opdrachtregel. Het toegangstoken is niet de verificatiecode die u in een SMS-bericht wanneer u 2FA instelt. Dit is een lange tekenreeks die er ongeveer zo uitziet: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Een paar opmerkingen hierover:

- Wanneer u uw toegangstoken maakt, het opslaan in een tekstbestand zodat u gemakkelijk toegankelijk wanneer u deze nodig hebt.

- Wanneer u nodig hebt voor het plakken van het token, moet u later weet er zijn twee manieren om te plakken in de opdrachtregel:

 - Klik op het pictogram in de linkerbovenhoek van het venster opdrachtregel > Bewerken > Plakken.
 - Met de rechtermuisknop op het pictogram in de linkerbovenhoek van het venster en klik op Eigenschappen > Opties > modus Snel bewerken. Hiermee wordt de opdrachtregel zodat u plakken kunt door te klikken in het venster opdrachtregel.

## <a name="install-a-markdown-editor"></a>Een editor korting installeren

We auteur van inhoud met behulp van eenvoudige "korting" notatie in de bestanden liever dan complex "markeringen" (HTML, XML, enzovoort). Zo is, moet u een korting-editor installeren.

- **Atom**: meest van ons van GitHub Atom korting editor gebruiken: [http://atom.io](http://atom.io). Deze hoeft niet een licentie voor bedrijven gebruiken. Spellingcontrole heeft.

- **Kladblok**: U kunt Kladblok gebruiken voor een zeer licht model.

- **Prose**: dit is een eenvoudige, elegante online en openen korting Broneditor die een voorbeeld biedt. Ga naar [http://prose.io](http://prose.io) en Autoriseer Prose in uw bibliotheek.

- **[Visual Studio-Code](https://www.visualstudio.com/products/code-vs.aspx)** - Microsoft vermelding in deze ruimte.

## <a name="configure-atom"></a>Atom configureren

Als u Atom gebruikt, moet u enkele zaken instellen.

- Atom standaard met behulp van 2 spaties voor tabs, maar korting verwacht 4 spaties. Als u deze op de standaardwaarde van twee verlaten, uw artikel wordt er professioneel uitzien in lokale voorbeeld, maar niet wanneer deze wordt geïmporteerd in Azure. Ja, configureren Atom als u wilt gebruiken 4 spaties - u kunt deze instelling vinden via Bestand > Instellingen > Editor-instellingen > tabblad lengte.
- U waarschijnlijk wordt ook wilt inschakelen vloeiende laten teruglopen in deze sectie, welke is gelijk aan "tekstterugloop" in Kladblok.
- Als u wilt de voorbeeldversie korting inschakelen, klikt u op pakketten > korting Preview > wisselknop Preview. U kunt de Ctrl-Shift-M gebruiken om te schakelen van de voorbeeldversie HTML-weergave.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>Zich de opslagplaats splitsen en deze kopiëren naar uw computer

1. Maken van een zich splitsen van de bibliotheek in GitHub: Ga naar de rechterbovenhoek van de pagina en klik op de knop zich splitsen. Als u wordt gevraagd, selecteert u uw account als de locatie waar de zich splitsen moet worden gemaakt. Hiermee maakt u een kopie van de bibliotheek in uw account cijfer Hub. In het algemeen technisch schrijvers en programma managers moeten zich splitsen azure-inhoud-prijs, de privé cessies‑retrocessies. Community inzenders moeten zich splitsen azure-inhoud, de openbare cessies‑retrocessies. U moet alleen zich één keer; splitsen na de eerste configuratie, als u wilt uw zich splitsen kopiëren naar een andere computer, hebt alleen u de opdrachten die in deze sectie volgt kopiëren van de cessies‑retrocessies naar uw computer kunt uitvoeren.  Als u beide opslagplaatsen in alle andere domeinen maakt, moet u maken een zich splitsen voor elke opslagplaats.

2. Kopieer de persoonlijke Access Token dat u hebt ontvangen van [https://github.com/settings/tokens](https://github.com/settings/tokens). U kunt de standaardmachtigingen voor de token accepteren.  Sla de persoonlijke Access Token in een tekstbestand voor later opnieuw kunt gebruiken.

3. Kopieer vervolgens de bibliotheek met uw computer met uw referenties die zijn ingesloten in de tekenreeks.  Klik hiertoe open cijfer we vaker doen en als administrator worden uitgevoerd. Voer de volgende opdracht in bij de opdrachtprompt.  Deze opdracht maakt een azure-content(-pr)-map op uw computer.  Als u de standaardlocatie gebruikt, moeten worden bij c:\users<your Windows user name>\azure-content(-pr).

Openbare cessies‑retrocessies:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Privé cessies‑retrocessies:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Bijvoorbeeld: deze opdracht klonen kan er ongeveer zo uitzien:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Externe opslagplaats verbinding instellen en configureren van referenties

Een verwijzing naar de hoofdsite bibliotheek maken door in te voeren van deze opdrachten. Dit verbindingen naar de bibliotheek in GitHub ingesteld zodat u kunt de meest recente wijzigingen op uw lokale computer en uw wijzigingen weer push naar GitHub. Deze opdracht configureert ook uw token lokaal zodat u niet hoeft te Voer uw gebruikersnaam en wachtwoord telkens wanneer die u probeert voor toegang tot het boven cessies‑retrocessies en uw zich splitsen op GitHub.

Openbare cessies‑retrocessies:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Privé cessies‑retrocessies:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Hiermee gaat meestal tijd. Nadat u dit doet, hoeft u niet te zich opnieuw splitsen of Voer uw referenties opnieuw. U moet alleen de gewenste splitsingen wordt kopiëren naar een lokale computer opnieuw als u de hulpmiddelen op een andere computer ingesteld.


## <a name="configure-your-user-name-and-email-locally"></a>Uw gebruikersnaam in te voeren en lokaal e-mail configureren

Om ervoor te zorgen dat u correct als een inzender worden vermeld, moet u uw gebruikersnaam in te voeren en e-mail op een lokaal ter cijfer configureren.

1. Start cijfer we vaker doen en overschakelen naar azure-inhoud of azure-inhoud-prijs:

   ````
   cd azure-content
   ````

 of

   ````
   cd azure-content-pr
   ````

2. Configureer uw gebruikersnaam in te voeren zodat deze overeenkomt met uw naam zoals u deze in uw profiel GitHub instellen:

    ````
    git config --global user.name "John Doe"
    ````
3. Uw e-mail configureren zodat dit overeenkomt met het primaire e-mailbericht aangewezen in uw profiel GitHub; Als u een werknemer MSFT, moet uw e-mailadres MSFT:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Type `git config -l` en controleer de map local settings om ervoor te zorgen de gebruikersnaam en e-mailbericht in de configuratie in orde zijn.

##<a name="next-steps"></a>Volgende stappen

- Meer informatie over het type inhoud dat in de technische inhoud cessies‑retrocessies hoort en weet wat geen deel uitmaakt. Zie de [richtlijnen van inhoud kanaal](./content-channel-guidance.md)!
- Volg [deze stappen om te maken of wijzigen van een artikel en vervolgens dient voor publicatie](./git-commands-for-master.md).
- Kopieer [de korting-sjabloon](../markdown templates/markdown-template-for-new-articles.md) als basis voor een nieuw artikel.
- Gebruik [deze controlelijst voor de verificatie van uw aanvraag halen gekoppeld is voldoet aan de kwaliteitscriteria](./contributor-guide-pr-criteria.md) voor het samenvoegen.


###<a name="contributors-guide-navigation"></a>Inzenders de handleiding voor navigatie

- [Van overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
