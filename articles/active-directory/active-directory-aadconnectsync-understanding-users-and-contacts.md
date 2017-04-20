<properties
    pageTitle="Azure AD Connect synchroniseren: wat gebruikers en contactpersonen | Microsoft Azure"
    description="Dit artikel wordt uitgelegd gebruikers en contactpersonen in Azure AD Connect synchroniseren."
    services="active-directory"
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
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD Connect synchroniseren: wat gebruikers en contactpersonen

Er zijn verschillende verschillende redenen waarom u meerdere Active Directory-forests wilt hebben, en er zijn verschillende implementatietopologieën voor verschillende. Algemene modellen bevatten een account-resource-implementatie en GAL sync'ed forests na een fusie & acquisition. Maar zelfs als er puur modellen, hybride modellen, ook worden gebruikt. De standaard-configuratie in Azure AD Connect synchroniseren wordt niet wordt ervan uitgegaan dat een bepaald model, maar afhankelijk van hoe gebruiker overeenkomende is geselecteerd in de installatiehandleiding, verschillende gedrag kunnen worden waargenomen.

In dit onderwerp gaan we wordt door de werking van de standaardconfiguratie in bepaalde topologieën. We doorloopt naar de configuratie en de synchronisatie-regeleditor kunnen worden gebruikt om de configuratie bekijkt.

Er zijn een paar algemene regels voor die de configuratie wordt ervan uitgegaan:

- Ongeacht welke volgorde we uit de bron actieve mappen importeren, moet het eindresultaat altijd hetzelfde zijn.
- Een actieve account wordt altijd aanmeldingsproblemen informatie, inclusief **userPrincipalName** en **sourceAnchor**bijdragen.
- Een uitgeschakelde account bijdragen userPrincipalName en sourceAnchor, tenzij er een gekoppelde postvak, als er geen actieve account bevinden.
- Een account met een gekoppelde postvak wordt nooit worden gebruikt voor userPrincipalName en sourceAnchor. Ervan wordt uitgegaan dat een actieve account later te vinden.
- Een contact-object kan worden ingericht naar Azure AD als een contactpersoon of als een gebruiker. U weet u niet echt totdat alle bron Active Directory-forests zijn verwerkt.

## <a name="contacts"></a>Contactpersonen

Met contactpersonen dat staat voor een gebruiker in een ander domein is algemene na een fusie & acquisition waar een oplossing GALSync twee of meer Exchange forests is bruggen. Het contact-object is altijd uit de verbindingslijn ruimte toevoegen aan de metaverse met het e-kenmerk. Als er bestaat al een contact-object of gebruikersobject met hetzelfde adres e-mail door zijn de objecten samengevoegd. Dit is geconfigureerd in de regel **In uit Active Directory-contactpersoon deelnemen aan**. Er is ook een regel met de naam **In uit Active Directory-contactpersoon algemene** met een bijvoeglijke-mailstroom naar de metaverse kenmerk **Bronobjecttype** met de constante **contactpersoon**. Deze regel zeer lage prioriteit heeft dus als u een willekeurig gebruikersobject is gekoppeld aan hetzelfde object metaverse, wordt de regel **In uit Active Directory-gebruiker algemene** wordt de waarde van gebruiker aan dit kenmerk bijdragen. Met deze regel heeft dit kenmerk de waarde contactpersoon als er geen gebruiker is toegevoegd en de waarde van gebruiker als ten minste één gebruiker is gevonden.

Voor het inrichten van een object naar Azure AD, wordt de uitgaande regel **Out naar AAD: Neem contact op met deelnemen aan** een contact-object maken als de metaverse kenmerk **Bronobjecttype** is ingesteld op **contact opnemen met**. Als dit kenmerk is ingesteld op **gebruiker**, de regel **Out naar AAD-gebruiker deelnemen aan** maakt een gebruikersobject in plaats daarvan.
Het is mogelijk dat een object wordt verschoven van contactpersoon aan gebruiker als meer bron actieve mappen worden geïmporteerd en gesynchroniseerd.

Bijvoorbeeld in een topologie GALSync vindt we contactpersonen objecten voor iedereen in de tweede bos als we de eerste bos importeert. Hiermee wordt nieuwe contactpersonen objecten in de verbindingslijn AAD fase. Als we later importeren en synchroniseren van de tweede bos, we de reële gebruikers zoeken en voeg ze toe aan de bestaande metaverse-objecten. We vervolgens de contactpersonen object in AAD verwijderen en een nieuwe gebruikersobject in plaats daarvan maken.

Als er een topologie waar gebruikers en weergegeven als contactpersonen, Controleer of u selecteert om aan te passen gebruikers op het kenmerk e-mail in de installatiehandleiding. Als u een andere optie selecteert, wordt u een afhankelijke configuratie volgorde hebt. Contactpersonen objecten wordt altijd deelnemen aan op het e-kenmerk, maar gebruikersobjecten wordt alleen deelnemen aan op het e-kenmerk als deze optie is geselecteerd in de installatiehandleiding. U kunt vervolgens uiteindelijk twee verschillende objecten in de metaverse met het kenmerk dat dezelfde e als het contact-object is geïmporteerd voordat het gebruikersobject. Tijdens het exporteren naar Azure AD wordt een fout worden gegenereerd. Dit probleem is inherent aan het ontwerp en betekent ongeldige gegevens of dat de topologie niet correct tijdens de installatie aangetroffen.

## <a name="disabled-accounts"></a>Uitgeschakelde accounts

Uitgeschakelde accounts worden ook gesynchroniseerd met Azure AD. Uitgeschakelde accounts gelden voor resources in Exchange, bijvoorbeeld vergaderruimten vertegenwoordigen. Geldt niet voor gebruikers met een gekoppelde Postvak; zoals eerder vermeld, wordt deze nooit een account aan Azure AD inrichten.

Aangenomen is die als een uitgeschakelde gebruikersaccount wordt gevonden, en vervolgens we niet u een andere actieve account later vindt en het object naar Azure AD met de userPrincipalName en sourceAnchor gevonden is ingericht. Geval een andere actieve account wordt deelnemen aan op hetzelfde metaverse object, worden klikt u vervolgens de userPrincipalName en sourceAnchor gebruikt.

## <a name="changing-sourceanchor"></a>SourceAnchor wijzigen

Wanneer een object naar Azure AD is geëxporteerd en vervolgens het niet is toegestaan om de sourceAnchor meer wijzigen. Wanneer het object is geëxporteerd wordt de metaverse kenmerk **cloudSourceAnchor** instellen met de waarde **sourceAnchor** is geaccepteerd door Azure AD. Als **sourceAnchor** is gewijzigd en niet overeenkomen met **cloudSourceAnchor**, de regel **Out naar AAD-gebruiker toevoegen** in de weergave van de fout **sourceAnchor kenmerk is gewijzigd**. In dit geval moeten de configuratie of de gegevens worden gecorrigeerd zodat de dezelfde sourceAnchor aanwezig in de metaverse opnieuw is voordat het object opnieuw kan worden gesynchroniseerd.

## <a name="additional-resources"></a>Aanvullende informatie

* [Azure AD verbinden met synchronisatie: Opties voor het aanpassen van synchronisatie](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
