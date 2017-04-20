<properties
   pageTitle="Internet van zaken aanbevolen procedures voor beveiliging | Microsoft Azure"
   description="Het artikel vindt u een curated lijst van Microsoft Internet van zaken beveiliging aanbevolen procedures en algemene aanbevelingen."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Internet van zaken aanbevolen procedures voor beveiliging

Beveiligen van de infrastructuur van Internet van dingen (IoT) is een kritieke onderneming voor iedereen die nodig zijn voor het IoT oplossingen. Vanwege het aantal betrokken apparaten en de gedistribueerde aard van deze apparaten, de gevolgen voor de gebeurtenis voor een waardepapier gerelateerd aan het risico opleveren voor uw miljoenen IoT apparaten is niet eenvoudig en algemene invloed kan hebben.

Daarom moet IoT beveiliging een aanpak beveiliging in diepteas. Gegevens moeten worden beveiligde in de cloud en wordt persoonlijke en openbare netwerken. Methoden moeten op hun plaats staan voor het inrichten van veilig de IoT apparaten zelf. Elke laag apparaat bij het netwerk, naar cloud back-enddatabase moet garanties van krachtige beveiliging.

Aanbevolen procedures IoT kunnen worden ingedeeld in de volgende manier:

- IoT hardwarefabrikant of integrator
- IoT oplossing ontwikkelaars
- Implementatie van IoT-oplossing
- IoT oplossing operator

In dit artikel bevat een overzicht van [Internet van zaken aanbevolen procedures voor beveiliging](../iot-suite/iot-security-best-practices.md). Zie dit artikel voor meer informatie.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IoT hardwarefabrikant of integrator

Volg de onderstaande aanbevolen procedures als u een hardwarefabrikant IoT of een integrator hardware:

- **Bereik hardware aan de minimale vereisten**: het hardwareontwerp behoren minimale functies die zijn vereist voor de werking van de hardware en niets meer. 
- **Hardware knoeivrij zorg**: samen in regelingen om te bepalen fysiek wordt geknoeid van hardware, zoals het openen van de omslag apparaat verwijderen van een deel van het apparaat, enzovoort. 
- **Bouwen rond secure hardware**: als [omzet](https://en.wikipedia.org/wiki/Cost_of_goods_sold) toestaan, beveiligingsfuncties zoals beveiligde en versleutelde opslag en Trusted Platform Module vertrouwde-opstart functionaliteit maken.
- **Secure opwaarderen**: firmware upgraden tijdens de levensduur van het apparaat is onvermijdelijke.

## <a name="iot-solution-developer"></a>IoT oplossing ontwikkelaars

Volg de onderstaande aanbevolen procedures als u een ontwikkelaar van de oplossing IoT:

- **Opvolgen secure software development methodologie**: a t/m na te denken over beveiliging van het begin van het project helemaal naar de implementatie, testen en implementatie ontwikkel secure software vereist.
- **Kies software bron openen noodzakelijk**: bron openen software biedt een mogelijkheid om snel oplossingen ontwikkelen.
- **Integratie met noodzakelijk**: veel van de software beveiligingsfouten bij de begrenzing van bibliotheken en API's bestaat. 

## <a name="iot-solution-deployer"></a>Implementatie van IoT-oplossing

Voer de onderstaande aanbevolen procedures als u een IoT oplossing-implementatie:

- **Hardware veilig implementeren**: IoT implementaties vereisen hardware onbeveiligde plaatsen, zoals in openbare spaties of werkende landinstellingen worden geïmplementeerd.
- **Verificatie toetsen veilig te houden**: tijdens de implementatie elk apparaat vereist apparaat-id's en bijbehorende verificatiesleutels die zijn gegenereerd door de cloudservice. Beveilig deze toetsen fysiek zelfs na de implementatie. Een beschadigde toets kan worden gebruikt door een schadelijke apparaat naar zich voordoen als een bestaand apparaat.

## <a name="iot-solution-operator"></a>IoT oplossing operator

Voer de onderstaande aanbevolen procedures als u een IoT oplossing operator:

- **Systemen up-to-date te houden**: zorgen dat apparaat-besturingssystemen en alle stuurprogramma worden bijgewerkt naar de meest recente versies. 
- **Beveiligen tegen schadelijke activiteit**: als het besturingssysteem toestaat, plaatst u de meest recente anti-virus uitvoert en anti-malware-functies op elk apparaat-besturingssysteem. 
- **Vaak controle**: controle IoT infrastructuur voor beveiliging problemen is heel belangrijk verwante wanneer reageren op beveiligingsincidenten.
- **Fysiek beveiligen de infrastructuur IoT**: de slechtste beveiligingsaanvallen op IoT infrastructuur worden gestart met fysieke toegang tot apparaten.
- **Beveiligen cloud referenties**: cloud verificatiereferenties gebruikt voor het configureren en gebruiken van een IoT-implementatie zijn mogelijk de eenvoudigste manier toegang te krijgen een IoT-systeem. 
