<properties
   pageTitle="Bewerkingen Management Suite beveiligings- en controle oplossing gegevensbeveiliging | Microsoft Azure"
   description="In dit document wordt uitgelegd hoe gegevens wordt beheerd en gewaarborgd in bewerkingen Management Suite beveiligings- en controle oplossing."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Bewerkingen Management Suite beveiligings- en controle oplossing beveiliging van gegevens

Zodat klanten voorkomen, detecteren en reageren op threats [bewerkingen Management Suite (OMS) beveiliging en controle oplossing](operations-management-suite-overview.md) verzamelt en verwerkt van gegevens over uw bronnen, waaronder:

- Gebeurtenislogboek beveiliging
- Gebeurtenis Aanwijzen voor Windows (ETW) gebeurtenissen
- Controle van gebeurtenissen in AppLocker
- Windows Firewall log
- Geavanceerde bedreiging Analytics-gebeurtenissen
- Resultaten van de basislijn assessment
- Resultaten van antimalware beoordeling
- Resultaten van de update/patch assessment
- Syslogs streams die expliciet zijn ingeschakeld op de agent

We geven sterke verplichtingen de privacy en beveiliging van deze gegevens te beschermen. Microsoft volgt strikte richtlijnen voor naleving en beveiliging, uit codering aan een dienst.
In dit artikel wordt uitgelegd hoe gegevens wordt beheerd en gewaarborgd in OMS beveiligings- en controle oplossing.

## <a name="data-sources"></a>Gegevensbronnen

OMS beveiligings- en controle oplossing analyseren gegevens uit uw virtuele Machines en fysieke computers waarop de OMS-Agent is geïnstalleerd. OMS beveiligings- en oplossing controle kan configuratiegegevens verzamelen over beveiligingsgebeurtenissen, zoals Windows gebeurtenis, controlelogboeken bijhouden, IIS-logboeken en syslog berichten. Voorbeelden van dergelijke gegevens zijn: besturingssysteemtype en versie, processen, de computernaam van de, IP-adressen, geregistreerd in de gebruiker en tenant-ID.  

## <a name="data-protection"></a>Gegevensbescherming

**Scheiding van gegevens**: gegevens worden bijgehouden logisch afzonderlijk op elk onderdeel overal in de service. Alle gegevens is gelabeld per organisatie. Deze labelen zich blijft voordoen gedurende de levenscyclus van de gegevens en deze op elke laag die u van de service wordt afgedwongen. 

**Toegang tot gegevens**: beveiligingsaanbevelingen en mogelijke beveiligingsrisico's onderzoeken, personeel van Microsoft mogelijk gegevens weer te geven die zijn verzameld of geanalyseerd door services. We ons houden aan de [Microsoft Online Services-voorwaarden](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), welke staat dat Microsoft niet klantgegevens gebruiken of gegevens uit dit voor commerciële doeleinden reclame of soortgelijke worden afgeleid. Aanbevelingen voor beveiliging en mogelijke beveiligingsrisico's onderzoeken, personeel van Microsoft mogelijk gegevens weer te geven die zijn verzameld of geanalyseerd door services. We alleen gebruiken klantgegevens zo nodig kunt u opgeven met Azure services, met inbegrip van de toepassing die compatibel is met het leveren van deze services. U bewaren alle rechten om uw eigen gegevens te.

**Gebruik van gegevens**: Microsoft gebruikt patronen en bedrijfsinformatie van bedreiging gezien over meerdere tenants ter verbetering van de mogelijkheden van onze voorkomen en detectie; dit doen we overeenkomstig de privacyafspraken in onze [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx)beschreven.

> [AZURE.NOTE] Gegevenslocatie is geconfigureerd op het niveau van de werkruimte OMS tijdens het maken van de werkruimte, die deel van de eerste configuratieproces OMS beveiligings- en controle uitmaakt.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe gegevens wordt beheerd en gewaarborgd in OMS. Meer informatie over OMS beveiligings- en controle oplossing, raadpleegt u:

- [Overzicht van de Management Suite Kantoorbeheersysteem bewerkingen](operations-management-suite-overview.md)
- [Cmdlets voor controle en reageren op beveiligingsmeldingen in bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-responding-alerts.md)
- [Resources voor controle in bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-monitoring-resources.md)

