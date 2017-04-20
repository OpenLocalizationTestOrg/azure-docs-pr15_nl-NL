<properties
   pageTitle="Inleiding tot Microsoft Azure log integratie | Microsoft Azure"
   description="Meer informatie over de integratie van Azure log, de belangrijkste mogelijkheden en hoe dit werkt."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Inleiding tot Microsoft Azure log integratie (Preview)

Meer informatie over de integratie van Azure log, de belangrijkste mogelijkheden en hoe dit werkt.

## <a name="overview"></a>Overzicht

Platform als een Service (PaaS) en de infrastructuur als een Service (IaaS) die wordt gehost in Azure Genereer een grote hoeveelheid gegevens in beveiligingslogboeken aan de. Deze logboeken bevatten cruciale informatie dat intelligence en krachtige inzichten beleid schending, interne en externe threats regelgeving problemen en afwijkingen in netwerk, host en gebruikers actief leveren kan.

Integratie van Azure log kunt u onbewerkte logboeken van uw Azure resources integreren in uw on-premises implementatie-systemen voor beveiligingsinformatie en gebeurtenis Management (SIEM). Integratie van Azure log verzamelt Azure diagnostische gegevens uit uw Windows *(af)* virtuele machines, evenals de hulpprogramma's voor diagnose van partneroplossingen zoals een Web Application Firewall (WAF). Deze integratie biedt een geïntegreerd dashboard voor alle activa, on-premises implementatie of in de cloud, zodat u kunt aggregeren, relateren analyseren en Waarschuw voor beveiligingsgebeurtenissen.

![Azure log-integratie][1]

## <a name="what-logs-can-i-integrate"></a>Welke logboeken kunt ik integreren?

Azure levert de uitgebreide logboekregistratie voor elke Azure-service. Deze logboeken zijn met twee typen gecategoriseerd:

- **Logboeken/beheren**, waardoor inzicht in de bewerkingen Azure resourcemanager maken, bijwerken en verwijderen. Azure controlelogboeken bijhouden is een voorbeeld van dit type log.
- **Gegevens vlak logboeken**, waardoor inzicht in de gebeurtenissen die als onderdeel van het gebruik van een Azure resource verheven. Voorbeelden van dit type log zijn de Windows-gebeurtenis systeem, beveiliging en toepassing Logboeken in een virtuele machine.

Integratie van Azure log ondersteunt momenteel integratie van Azure controlelogboeken bijhouden, VM logboeken en meldingen van Azure Beveiligingscentrum.

Stuur een e-mail naar als u vragen over Azure Log integratie hebt, [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Volgende stappen

In dit document, zijn u kennis met Azure log-integratie. Zie de volgende onderwerpen voor meer informatie over de integratie van Azure log en de soorten logboeken ondersteund:

- [Integratie van Microsoft Azure Log Azure logboekbestanden (Preview)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center voor meer informatie, systeemvereisten en installatie-instructies over de integratie van Azure log.
- [Aan de slag met Azure log integratie](security-azure-log-integration-get-started.md) - deze zelfstudie leert u installatie van Azure log integratie en logboeken van Azure opslag, Azure controlelogboeken bijhouden en waarschuwingen van Beveiligingscentrum integreren.
- [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dit blogbericht ziet u hoe u het logboek met Azure-integratie voor gebruik met partneroplossingen Splunk, HP ArcSight en IBM QRadar configureren.
- [Azure log integratie vaak gevraagd Veelgestelde vragen](security-azure-log-integration-faq.md) - deze Veelgestelde vragen over vindt u antwoorden op vragen over het logboek met Azure-integratie.
- [Integratie Beveiligingscentrum waarschuwingen met Azure Meld integratie](../security-center/security-center-integrating-alerts-with-log-integration.md) – dit document, ziet u hoe u het synchroniseren van meldingen van Beveiligingscentrum, samen met VM beveiligingsgebeurtenissen die zijn verzameld met Azure diagnostisch hulpprogramma en Azure controlelogboeken bijhouden, met uw log analytics of SIEM oplossing.
- [Nieuwe functies voor Azure diagnostisch hulpprogramma en Azure controlelogboeken bijhouden](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – dit blogbericht maakt u kennis met Azure controlelogboeken bijhouden en andere functies waarmee u inzicht in de bewerkingen van uw Azure bronnen krijgen.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
