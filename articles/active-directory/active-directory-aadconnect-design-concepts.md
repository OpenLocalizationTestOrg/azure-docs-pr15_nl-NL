<properties
   pageTitle="Azure AD Connect: Ontwerpen concepten | Microsoft Azure"
   description="In dit onderwerp details bepaalde gebieden van de ontwerpen implementatie"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Ontwerpconcepten
Het doel van dit onderwerp wordt beschreven gebieden die tot en met tijdens het implementatieontwerp van Azure AD Connect moeten worden beschouwd. Dit onderwerp is bedoeld voor een uitgebreide kennismaking op bepaalde gebieden en deze concepten worden kort beschreven in als u ook andere onderwerpen.

## <a name="sourceanchor"></a>sourceAnchor
Het kenmerk sourceAnchor wordt gedefinieerd als *een kenmerk onveranderlijke gedurende de levensduur van een object*. Een object dat het hetzelfde object on-premises implementatie en in Azure AD id heeft een unieke. Het kenmerk is een afkorting **immutableId** en de twee namen verwisselbaar worden gebruikt.

Het woord onveranderlijke, dat is "kan niet worden gewijzigd", moet u dit onderwerp. Aangezien de waarde van dit kenmerk kan niet worden gewijzigd nadat deze is ingesteld, is het is belangrijk om te kiezen een ontwerp die ondersteuning biedt voor uw scenario.

Het kenmerk wordt gebruikt voor de volgende scenario's:

- Als een nieuwe server van de synchronisatie-engine is gemaakt of opnieuw na het herstellen van fouten gemaakt, wordt in dit kenmerk bestaande objecten in Azure AD met lokale objecten gekoppeld.
- Als u van een identiteit alleen de cloud hebt verplaatst naar een identiteitsmodel gesynchroniseerde, kan dit kenmerk objecten "harde overeenkomst" bestaande objecten in Azure AD met on-premises implementatie objecten.
- Als u Federatie gebruikt, wordt wordt dit kenmerk samen met de **userPrincipalName** gebruikt in de claim ter identificatie van een gebruiker.

In dit onderwerp alleen moment spreekt over sourceAnchor met betrekking tot gebruikers. Dezelfde regels toepassen op alle objecttypen, maar deze is alleen bedoeld voor gebruikers dat meestal uitmaakt dat dit probleem.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Een goede sourceAnchor-kenmerk selecteren
De kenmerkwaarde moet volgen van de volgende regels:

- Minder dan 60 tekens lang
    - Tekens zijn niet op de a-z, A-Z of 0-9 worden gecodeerd en telt als 3 tekens
- Geen speciale tekens bevat: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ haken () '; : , [ ] " @ _
- Globaal uniek zijn
- Moet een tekenreeks, een geheel getal of een binair getal
- Niet moeten worden gebaseerd op de naam van de gebruiker, deze wijzigingen
- Niet mogen worden hoofdlettergevoelig en voorkomen dat waarden die per hoofdletters/kleine letters verschillen kunnen
- Als het object wordt gemaakt moeten worden toegewezen

Als de geselecteerde sourceAnchor is niet van het type tekenreeks, klikt u vervolgens Azure AD verbinding Base64Encode de kenmerkwaarde om ervoor te zorgen er geen speciale tekens worden weergegeven. Als u een andere federatieserver dan ADFS gebruikt, controleert u of uw server kan ook Base64Encode het kenmerk.

Het kenmerk sourceAnchor is hoofdlettergevoelig. Een waarde van "Jandevries" is niet hetzelfde als "jandevries". Maar u twee verschillende objecten met alleen een verschil in hoofdletters/kleine letters niet nodig hebt.

Als er één is on-premises implementatie, klikt u vervolgens het kenmerk dat u moet gebruiken **objectGUID**. Dit is ook het kenmerk gebruikt wanneer u express-instellingen in Azure AD Connect en ook het kenmerk door DirSync gebruikt.

Als u meerdere forests en gebruikers niet tussen forests en domeinen verplaatsen, is **objectGUID** te gebruiken in dit geval zelfs een goede kenmerk.

Als u gebruikers tussen forests en domeinen, verplaatsen en vervolgens moet u een kenmerk dat wordt niet gewijzigd of kan worden verplaatst met de gebruikers zoeken tijdens het verplaatsen. Er is een aanbevolen aanpak te bieden van een synthetische-kenmerk. Een kenmerk dat iets kan bevatten die op een GUID lijkt zou geschikt zijn. Tijdens het maken van het object, is een nieuwe GUID gemaakt en op de gebruiker een tijdstempel. Een regel aangepaste synchronisatie kan worden gemaakt in de synchronisatie-engine server deze waarde op basis van de **objectGUID** maken en bijwerken van het geselecteerde kenmerk in Hiermee. Wanneer u het object verplaatst, zorg er dan voor dat ook de inhoud van deze waarde te kopiëren.

Een andere oplossing is om te kiezen van een bestaand kenmerk dat u weet niet wordt gewijzigd. Veelgebruikte aanwezigheidskenmerken zijn de **werknemer-id**. Als u rekening houden met een kenmerk met letters, zorg er is dat geen kans de hoofdletters/kleine letters (hoofdletter of kleine letters) voor de waarde van het kenmerk kunt wijzigen. Ongeldige kenmerken die niet mogen worden gebruikt opnemen die kenmerken met de naam van de gebruiker. Klik in een huwelijk of echtscheiding, de naam naar verwachting wijzigen, die niet is toegestaan voor dit kenmerk. Dit is ook een reden waarom kenmerken zoals **userPrincipalName**, **e-mail**en **targetAddress** niet zelfs mogelijk om te selecteren in de installatiewizard Azure AD Connect zijn. Deze kenmerken ook bevatten de @-character, die niet is toegestaan in de sourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>Het kenmerk sourceAnchor wijzigen
De kenmerkwaarde sourceAnchor worden niet gewijzigd nadat het object in Azure AD is gemaakt en de identiteit wordt gesynchroniseerd.

Daarom zijn de volgende beperkingen van toepassing op Azure AD Connect:

- Het kenmerk sourceAnchor kan alleen worden ingesteld tijdens de installatie van de eerste. Als u de installatiewizard opnieuw uitvoeren, wordt deze optie is alleen-lezen. Als u nodig hebt om deze instelling te wijzigen, moet u verwijderen en opnieuw te installeren.
- Als u een andere Azure AD Connect-server hebt geïnstalleerd, selecteert u hetzelfde sourceAnchor kenmerk als eerder hebt gebruikt. Als u eerder DirSync gebruikten en naar Azure AD Connect verplaatsen, moet u **objectGUID** gebruiken omdat dit het kenmerk door DirSync gebruikt.
- Als de waarde voor sourceAnchor is gewijzigd na is het object naar Azure AD, klikt u vervolgens Azure AD Connect synchroniseren genereert een fout en mag niet meer wijzigingen op dat object voordat het probleem is opgelost en de sourceAnchor terug in de adreslijst bron wordt gewijzigd geëxporteerd.

## <a name="azure-ad-sign-in"></a>Azure AD aanmelden
Terwijl uw on-premises adreslijst integreren met Azure AD, is het belangrijk om te begrijpen hoe de synchronisatie-instellingen van invloed kunnen zijn op de gebruiker manier verifieert. Azure AD worden userPrincipalName (User Principal Name) gebruikt om de gebruiker te verifiëren. Wanneer u uw gebruikers synchroniseert, moet u het kenmerk moet worden gebruikt voor de waarde van userPrincipalName zorgvuldig kiezen.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Het kenmerk voor userPrincipalName kiezen
Wanneer u het kenmerk voor het leveren selecteert moet de waarde van UPN moet worden gebruikt in Azure één

- Waarden van het kenmerk voldoen aan de UPN-syntaxis (RFC 822), die is moet van de indelingusername@domain
- Het achtervoegsel in de waarden resultaten die overeenkomen met een van de geverifieerde aangepaste domeinen in Azure AD

Express-instellingen is de aangenomen dat keuze voor het kenmerk userPrincipalName. Als het kenmerk userPrincipalName niet de waarde bevat u wilt dat uw gebruikers te melden bij een Azure en vervolgens moet u **Aangepaste installatie**.

### <a name="custom-domain-state-and-upn"></a>Status van het aangepaste domein en UPN
Het is belangrijk om ervoor te zorgen dat er een gecontroleerde domein voor de UPN-achtervoegsel.

John is een gebruiker op contoso.com. Gewenste John gebruik van de on-premises UPN john@contoso.com te melden bij een Azure nadat u gebruikers hebt gesynchroniseerd met uw Azure AD directory contoso.onmicrosoft.com. Klik hiervoor die u wilt toevoegen en contoso.com als een aangepast domein verifiëren in Azure AD voordat u kunt beginnen met het synchroniseren van de gebruikers. Als u de UPN-achtervoegsel van Jan, bijvoorbeeld contoso.com, niet overeenkomen met een geverifieerd-domein in Azure AD, klikt u vervolgens vervangen Azure AD door de UPN-achtervoegsel contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Niet-omgeleid on-premises domeinen en UPN voor Azure AD
Sommige organisaties hebben niet geschikt domeinen, zoals contoso.local of eenvoudige Eén etiket domeinen zoals contoso. U bent niet mogen een niet-omgeleid domein verifiëren in Azure AD. Azure AD Connect kan synchroniseren met alleen een geverifieerd-domein in Azure AD. Wanneer u een Azure AD-directory maakt, wordt een geschikt domein dat standaarddomein voor uw Azure AD bijvoorbeeld contoso.onmicrosoft.com wordt. Het wordt daarom nodig is om te controleren of een ander domein in een dergelijke situatie geschikt voor het geval u niet wilt synchroniseren met het standaard onmicrosoft.com-domein.

Lees [de naam van uw aangepaste domein met Azure Active Directory toevoegen](active-directory-add-domain.md) voor meer informatie over het toevoegen en verifiëren van domeinen.

Azure AD Connect vastgesteld als u deze in een niet-omgeleid domeinomgeving uitvoert en u correct waarschuwen zou uit wilt doorgaan met express-instellingen. Als u in een niet-omgeleid domein werkt, klik er waarschijnlijk dat de UPN van de gebruikers die niet geschikt achtervoegsels te hebben. Bijvoorbeeld als u onder contoso.local uitvoert, zijn stelt Azure AD Connect u de aangepaste instellingen in plaats van met express instellingen gebruiken. Met aangepaste instellingen, bent u kunnen het kenmerk dat moet worden gebruikt als UPN te melden bij een Azure nadat de gebruikers die zijn gesynchroniseerd met Azure AD opgeven.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
