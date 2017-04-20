<properties 
    pageTitle="Azure meervoudige verificatie - hoe het werkt"
    description="Azure meervoudige verificatie helpt beschermen toegang tot gegevens en toepassingen tijdens de vergadering gebruiker aanvraag voor een eenvoudig proces aanmelden. Deze extra beveiliging biedt doordat een tweede vorm van verificatie en sterke verificatie via een bereik van eenvoudig controleopties biedt."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

#<a name="how-azure-multi-factor-authentication-works"></a>De werking van Azure meervoudige verificatie

De beveiliging van meervoudige verificatie wordt veroorzaakt door de gelaagde benadering. Verlies van meerdere verificatie factoren worden in een belangrijke uitdaging voor hackers weergegeven. Zelfs als onbevoegden beheert voor meer informatie over het wachtwoord van de gebruiker, worden deze onbruikbaar bezit van het apparaat dat vertrouwde ook wordt zonder. Moet de gebruiker het apparaat kwijtraakt, de persoon die vindt u deze niet mogelijk te gebruiken, tenzij hij of zij ook van de gebruiker wachtwoord kent.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure meervoudige verificatie helpt beschermen toegang tot gegevens en toepassingen tijdens de vergadering gebruiker aanvraag voor een eenvoudig proces aanmelden.  Deze extra beveiliging biedt doordat een tweede vorm van verificatie en biedt sterke verificatie via een bereik van eenvoudig controleopties:

- telefoongesprek
- SMS-bericht
- melding van de mobiele app, zodat gebruikers kunnen kiezen hoe ze liever
- de verificatiecode mobiele app
- 3e partijen EED tokens

Zie voor meer informatie vliegt hoe dit werkt de volgende video.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

##<a name="methods-available-for-multi-factor-authentication"></a>Methoden die beschikbaar zijn voor meervoudige verificatie
Als een gebruiker zich aanmeldt, wordt een extra verificatie verzonden naar de gebruiker.  Hier volgen een lijst met methoden die kunnen worden gebruikt voor deze keuzes.

Verification Method  | Beschrijving
------------- | ------------- |
Telefoongesprek | Een oproep wordt voor Smartphone van een gebruiker te vragen of ze zich wilt aanmelden door op het teken # te drukken.  Hiermee wordt de verificatie voltooien.  Deze optie kan worden geconfigureerd en kan worden gewijzigd in een code die u opgeeft.
SMS-bericht | Een SMS-bericht ontvangt van een gebruiker Smartphone met een code van 6 cijfers.  Voer deze code in om de verificatieproces te voltooien.
Melding van de mobiele App | Een verzoek om controle ontvangt van een gebruiker Smartphone om hen te vragen voltooid de verificatie door te verifiëren selecteren in de mobiele app. Dit gebeurt als u als uw primaire verificatiemethode app melding hebt geselecteerd.  Als ze dit ontvangen wanneer ze niet zich wilt aanmelden, kunnen deze kiezen om dit te melden als fraude.
Verificatiecode in die met Mobile-App | Een verificatiecode in die wordt verzonden naar de mobiele app die wordt uitgevoerd op een gebruikerswachtwoord Smartphone.  Dit gebeurt als u een verificatiecode als uw primaire verificatiemethode hebt geselecteerd.


##<a name="available-versions-of-azure-multi-factor-authentication"></a>Beschikbare versies van Azure meervoudige verificatie
Azure meervoudige verificatie is beschikbaar in drie verschillende versies.  De onderstaande tabel worden alle van de volgende uitgebreider beschreven.

Versie  | Beschrijving
------------- | ------------- |
Meervoudige verificatie voor Office 365 | Deze versie werkt uitsluitend met Office 365-toepassingen en wordt beheerd met de Office 365-portal. Zodat beheerders kunnen nu helpen beveiligen hun Office 365-informatiebronnen met meervoudige verificatie.  In deze versie wordt geleverd met een Office 365-abonnement.
Meervoudige verificatie voor beheerders van Azure | De dezelfde subset van mogelijkheden voor meervoudige verificatie voor Office 365 zijn beschikbaar zonder bijkomende kosten voor alle Azure beheerders. Elke beheerdersaccount van een Azure abonnement krijgt nu extra beveiliging doordat deze core meervoudige verificatie-functionaliteit. Zodat een beheerder die toegang tot Azure-portal als u wilt maken van een VM, een website, wil opslag beheren, kunt mobiele services of een andere Azure-Service meervoudige verificatie toevoegen aan zijn beheerdersaccount.
Azure meervoudige verificatie | Azure meervoudige verificatie biedt de rijkste functieset. <br><br>Deze aanvullende configuratieopties via de portal Azure Management, geavanceerde rapportage en ondersteuning biedt voor een bereik on-premises en cloud toepassingen. Azure meervoudige verificatie kan worden aangeschaft als een zelfstandig licentie en is binnen Azure Active Directory Premium en Enterprise mobiliteit Suite is opgenomen. <br><br>Dit kan ook worden aangeschaft op basis van de verbruik door te maken van een Provider van Azure meervoudige verificatie in een Azure-abonnement.
##<a name="feature-comparison-of-versions"></a>Vergelijking van functies van versies
De volgende tabel hieronder vindt u een lijst van de functies die beschikbaar in verschillende versies van Azure meervoudige verificatie zijn.


Functie  | Meervoudige verificatie voor Office 365 (opgenomen in Office 365-SKU's)|Meervoudige verificatie voor beheerders van Azure (inbegrepen in Azure abonnement) | Azure meervoudige verificatie (opgenomen in Azure AD Premium en Enterprise mobiliteit Suite)
------------- | :-------------: |:-------------: |:-------------: |
Beheerders kunnen accounts met MFA beveiligen| * | * (Alleen beschikbaar voor Azure Administrator-accounts)|*
De mobiele app als een tweede factor|* | * | *
Telefoongesprek als een tweede factor|* | * | *
SMS als een tweede factor|* | * | *
Appwachtwoorden voor clients die geen ondersteuning MFA bieden|* | * | *
Beheerder controle over verificatiemethoden| *| *| *
PINCODE-modus| | | *
Waarschuwing| | | *
MFA rapporten| | | *
Eenmalige overslaan| | | *
Aangepaste begroeting voor telefoongesprekken| | | *
Aanpassing van de beller-ID voor telefoongesprekken| | | *
Bevestiging van de gebeurtenis| | | *
Vertrouwde IP-adressen| | | *
MFA schorten voor onthouden apparaten (Public Preview)| | | *
MFA SDK| | | *
MFA voor on-premises implementatie-toepassingen MFA-server gebruiken| | | *


##<a name="how-to-get-azure-multi-factor-authentication"></a>Hoe kom ik aan Azure meervoudige verificatie

Als u de volledige functionaliteit van Azure meervoudige verificatie in plaats van alleen bedoeld voor Office 365-gebruikers en beheerders van Azure dat wilt, zijn er verschillende opties kunt u deze ophalen:

1.  Azure meervoudige verificatie licenties aanschaffen en toewijzen aan uw gebruikers.
2.  Kopen van licenties waarvoor Azure meervoudige verificatie gebundelde daarin zoals Azure Active Directory Premium of Enterprise mobiliteit Suite en toewijzen aan uw gebruikers.
3.  Maak een Azure meervoudige verificatie-Provider in een Azure-abonnement. Als u niet al een Azure-abonnement hebt, kunt u zich kunt aanmelden voor een proefabonnement op Azure. Proefabonnementen moet worden geconverteerd naar gewone abonnementen voor de proefversie vervaldatum.

Bij gebruik van een Azure meervoudige verificatie-Provider er zijn twee gebruikersmodellen beschikbaar die worden gefactureerd door uw Azure-abonnement:


- **Per gebruiker**. In het algemeen voor ondernemingen die wilt meervoudige verificatie inschakelen voor een vast aantal werknemers die regelmatig verificatie.
- **Per verificatie**. In het algemeen voor ondernemingen die wilt meervoudige verificatie inschakelen voor een grote groep externe gebruikers die niet vaak nodig verificatie.

Zie voor meer informatie prijzen [prijzen van Azure MFA.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

Kies de per locatie of verbruik gebaseerde model dat geschikt is voor uw organisatie.   Typt u vervolgens aan de slag te gaan Zie [Aan de slag](multi-factor-authentication-get-started.md)
