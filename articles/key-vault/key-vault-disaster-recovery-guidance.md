<properties
    pageTitle="Wat moet u doen in het geval van een Azure service-onderbreking die van invloed is op Azure-toets kluis | Microsoft Azure"
    description="Lees wat u moet doen in het geval van een Azure service-onderbreking die van invloed is op Azure-toets kluis."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure-toets kluis beschikbaarheid en redundantie

Azure-toets kluis functies van meerdere lagen redundantie om ervoor te zorgen dat uw toetsen en geheimen beschikbaar is in uw toepassing blijven zelfs als afzonderlijke onderdelen van de service is mislukt.

De inhoud van uw belangrijkste kluis worden gerepliceerd in het gebied en naar een secundaire gebied ten minste 150 mijl afwezig maar binnen de dezelfde Geografie. Op deze manier hoog levensduur van uw toetsen en geheimen gehandhaafd.

Als u afzonderlijke onderdelen binnen de belangrijkste kluis-service is mislukt, stap alternatieve onderdelen in het gebied in moet fungeren van uw aanvraag kunt invullen om ervoor te zorgen dat er geen afname van de functionaliteit is. U hoeft niet te doen om te dit te activeren. Er worden automatisch en transparant voor u.

In het geval van het gegevenstype niet vaak voorkomen dat een gehele Azure gebied niet beschikbaar is, zijn de aanvragen die u van Azure-toets kluis in dat gebied aanbrengt automatisch routering (*overgenomen*) naar een secundaire gebied. Wanneer de primaire regio weer beschikbaar is, aanvragen worden gerouteerd terug (*is mislukt terug*) naar de primaire regio. Nogmaals, hoeft u niet geen actie te ondernemen omdat dit automatisch gebeurt.

Er gelden enkele beperkingen letten:

* Bij een failover regio duurt een paar minuten voor de service worden uitgevoerd. Aanvragen die zijn aangebracht tijdens deze periode kunnen mislukken totdat de overname is voltooid.
* Wanneer een failover voltooid is, wordt uw belangrijkste kluis in de modus alleen-lezen is. Aanvragen die worden ondersteund in deze modus zijn:
 * Lijst met belangrijke kluizen
 * Eigenschappen van belangrijke kluizen ophalen
 * Lijst geheimen
 * Geheimen ophalen
 * Lijst toetsen
 * (Eigenschappen van) sleutels ophalen
 * Versleutelen
 * Ontsleutelen
 * Tekstterugloop
 * Pak
 * Controleer of
 * Aanmelden
 * Back-up maken
* Nadat een failover terug is mislukt, zijn alle typen (inclusief lezen *en* schrijven aanvragen) beschikbaar.
