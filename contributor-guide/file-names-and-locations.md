<properties title="" pageTitle="Namen van bestanden en locaties voor Azure technische artikelen" description="Dit artikel wordt uitgelegd de bestandsstructuur voor artikelen en de naamgevingsconventies die u volgen moet wanneer u een nieuw artikel maken." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Namen van bestanden en locaties voor Azure technische artikelen

In onze technische opslagplaats voor inhoud gebruiken we een aparte map maken (de map met de **artikelen** ) voor alle artikelen. Er is geen maphiërarchie: alle artikelen die in de structuur plat bestand wonen. Als u mappen met artikelen in deze maakt, kan uw artikelen kunnen niet worden gepubliceerd.

In plaats van de bestandsstructuur van een als een organiseren principe, gebruiken we een strikte bestand naamgevingsconventie die duidelijk onderwerpen aangeeft en die bijdraagt zichtbaarheid op het web.

Hier volgt wat u moet weten:

+ [Regels]
+ [Patroon]
+ [Standaard voorbeelden]
+ [Marketplace-inhoud]
+ [Bestand naam goedkeuring]

##<a name="rules"></a>Regels

- Bestandsnamen kunnen alleen kleine letters, cijfers en afbreekstreepjes bevatten. 
- Geen spaties of interpunctie. Gebruik de afbreekstreepjes te scheiden van woorden en getallen in de bestandsnaam.
- Niet meer dan 80 tekens - dit is de systeemlimiet van een publicerende
- Gebruik actie woorden die specifiek, zoals zijn ontwikkelen, kopen, maken, oplossen. Geen - den woorden.
- Geen kleine woorden - niet bevatten een en, de, in of, enzovoort.
- Alle bestanden moeten worden in korting en de bestandsextensie .md.
- Spelling van de woorden niet-goedgekeurde of onnodige afkortingen in bestandsnamen vermijden

Afkortingen en initialisms in bestandsnamen - specifieke richtlijnen:

- Gebruik geen Azure servicenamen afkortingen: de eerste woorden van de bestandsnaam de standaard, moeten zijn gespeld Azure-service of technologie naam. 
-   Staan geen rm of arm als afkortingen ergens in een bestandsnaam
- De afkortingen van andere gestandaardiseerde zijn aanvaardbaar zo nodig in bestandsnamen. 

##<a name="pattern"></a>Patroon

Hier ziet u de grote lijnen:

 **Service-platform-Language-Content-product-Version.MD**

De onderdelen van het patroon dat toepassen en bekijk de lijst met artikelen in de bibliotheek om een idee van bestaande namen gebruiken. Namen die niet met een ontwikkelaar platform beginnen of de naam van een service zijn waarschijnlijk verdachte en GKGW tot en met.

##<a name="standard-examples"></a>Standaard voorbeelden

Hier volgen een paar voorbeelden van geldige namen die het patroon volgen. :

- cloud-services-dotnet-continue-delivery.md
- Mobile-services-ios-get-started.md
- documentdb-beheren-account.md
- Mobile-services-DotNet-backend-Get-Started-Settings-Sync.MD
- Active-Directory-Java-Authenticate-Users-Access-Control-eclipse.MD
- Virtual-machines-Install-Windows-Server-2008r2.MD
- cache-aspnet-sessie-staat-provider
- Azure-SDK-DotNet-Release-Notes-2-8
- storsimple-Disaster-Recovery-Using-Azure-site-Recovery

##<a name="marketplace-content"></a>Marketplace-inhoud

Om te onderscheiden van inhoud die is gericht op partner bijdragen aan de Azure marketplace, start u de bestandsnamen met "marketplace". Dit inhoudstype zijn mag geen te vaak, zoals de meeste partner-inhoud op de websites partners eigen moet worden gemaakt.

- Marketplace-mongodb-Virtual-machines-Install-Windows-Server-2008r2.MD

##<a name="file-name-approval"></a>Bestand naam goedkeuring

Dit is de taak van onze groep halen verzoek revisoren moet worden gereviseerd bestandsnamen wanneer een nieuw bestand voor de eerste keer wordt verstuurd naar de bibliotheek. Halen verzoek revisoren Controleer de bestandsnaam en feedback geven via de stream halen verzoek Opmerking Als u wijzigingen wilt aanbrengen. De bestandsnaam moet worden verholpen voordat het verzoek halen wordt geaccepteerd. Inzenders kunnen eenvoudig push-de update naar de aanvraag in behandeling halen.

##<a name="folder-names-in-the-repo"></a>Mapnamen in de cessies‑retrocessies

Mappen alleen voor services moeten worden gemaakt en de bestandsnaam moet overeenkomen met de service-witruimte. Gebruik alleen letters en afbreekstreepjes, en alle kleine letters. Voordat u een nieuwe map die is niet van een uitgebrachte service maakt, moet u goedkeuring krijgen van de beheerder van de bibliotheek.

##<a name="changing-case-in-file-names"></a>Hoofdletters in bestandsnamen wijzigen

Windows-besturingssystemen zijn hoofdlettergevoelig, dus als u de bestandsnaam van een om op te lossen behuizing wijzigen moet, is het beter om te maken van een belangrijke omslag, tenzij u kunnen maken van de wijziging op een Linux of Mac. Bijvoorbeeld:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

De volgende opdracht uit om de naam van een bestand te gebruiken:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Inzenders de handleiding voor koppelingen

- [Van overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)


<!--Anchors-->
[Regels]: #rules
[Patroon]: #pattern
[Standaard voorbeelden]: #standard-examples
[Marketplace-inhoud]: #marketplace-content
[Bestand naam goedkeuring]: #file-name-approval
