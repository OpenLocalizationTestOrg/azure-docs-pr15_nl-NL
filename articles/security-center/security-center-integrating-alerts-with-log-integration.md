<properties
   pageTitle="Meldingen van Azure Beveiligingscentrum integreren met Azure log integratie (Preview) | Microsoft Azure"
   description="In dit artikel vindt u aan de slag met Beveiligingscentrum waarschuwingen integreren met Azure log-integratie."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Meldingen van Beveiligingscentrum integreren met Azure log integratie (Preview)

Veel beveiligingsbewerkingen en incident antwoord teams afhankelijk van een oplossing beveiligingsinformatie en gebeurtenis Management (SIEM) als uitgangspunt voor gesorteerd en wordt onderzocht beveiligingsmeldingen. Met de integratie van de Azure log, kunnen klanten Azure Beveiligingscentrum waarschuwingen, samen met VM beveiligingsgebeurtenissen die zijn verzameld met Azure diagnostisch hulpprogramma en Azure controlelogboeken bijhouden, met hun log analytics of SIEM oplossing in near realtime synchroniseren.

Integratie van Azure log werkt met anderen HP ArcSight, Splunk en IBM QRadar.

## <a name="what-logs-can-i-integrate"></a>Welke logboeken kunt ik integreren?

Azure levert de uitgebreide logboekregistratie voor elke service. Deze logboeken zijn gecategoriseerd als:

- **Besturingselement/Management logboeken** waardoor inzicht in de bewerkingen Azure resourcemanager maken, bijwerken en verwijderen.
- **Gegevens vlak logboeken** waardoor inzicht in de gebeurtenissen die optreedt wanneer u gebruikt een Azure bron. Een voorbeeld is het gebeurtenislogboek Windows - beveiliging en toepassing Logboeken in een virtuele machine.

Integratie van Azure log ondersteunt momenteel de integratie van:

- Azure VM-Logboeken
- Azure controlelogboeken bijhouden
- Meldingen van Azure Beveiligingscentrum

## <a name="install-azure-log-integration"></a>Integratie van Azure log installeren

Download [Azure Meld integratie](https://www.microsoft.com/download/details.aspx?id=53324).

De service van de integratie van Azure log verzamelt telemetriegegevens van de computer waarop deze is geïnstalleerd.  Telemetriegegevens die worden verzameld luidt als volgt:

- Uitzonderingen die tijdens de uitvoering van Azure log-integratie optreden
- Aan de doelstellingen over het aantal query's en gebeurtenissen verwerkt
- Statistieken over welke Azlog.exe opdrachtregelopties worden gebruikt

> [AZURE.NOTE] U kunt de verzameling telemetriegegevens uitschakelen door deze optie uit te schakelen.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Azure controlelogboeken bijhouden en Beveiligingscentrum waarschuwingen integreren

1. Open de opdrachtprompt en de **cd** in **c:\Program Files\Microsoft Azure Log integratie**.

2. De opdracht **azlog createazureid** een [Azure Active Directory Service Principal](../active-directory/active-directory-application-objects.md) maken in de Azure Active Directory (AD)-tenants die als host van de Azure abonnementen uitvoeren.

    U wordt gevraagd voor uw Azure login.

    > [AZURE.NOTE] U moet de eigenaar van het abonnement of de beheerder van een collega van het abonnement.

    Verificatie van Azure vindt plaats via Azure AD.  Maken van een service principal voor integratie van Azure log maakt de Azure AD-identiteit die toegang tot het lezen van Azure-abonnement krijgt.

3. Voer de **azlog Autoriseer <SubscriptionID> ** opdracht Reader access het abonnement toewijzen aan de service-hoofdsom gemaakt in stap 2. Als u een **SubscriptionID**niet opgeeft, wordt de hoofdsom service toegewezen de rol van de lezer naar alle abonnementen waartoe u toegang hebt.

    > [AZURE.NOTE] U ziet mogelijk waarschuwingen als u de opdracht **Autoriseer** direct na de opdracht **createazureid** uitvoert, omdat er enkele latentie tussen wanneer het Azure AD-account is gemaakt en wanneer het account is beschikbaar voor gebruik. Als u ongeveer 10 seconden wacht na het uitvoeren van de opdracht **createazureid** de opdracht **Autoriseer** uitvoeren, moet u deze waarschuwingen niet weergegeven.

4. Controleer de volgende mappen om te bevestigen dat de controle-logboekbestanden JSON er zijn:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Controleer de volgende mappen om te bevestigen dat meldingen van Beveiligingscentrum bestaat niet in deze:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Wijs de standaard SIEM bestand doorstuurservers verbindingslijn naar de gewenste map naar de gegevens in het exemplaar van SIEM pipe. Raadpleeg [SIEM configuraties](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) van uw configuratie SIEM.

Stuur een e-mail naar als u vragen over Azure Log integratie hebt, [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure controlelogboeken bijhouden en eigenschapdefinities, raadpleegt u:

- [Bewerkingen met resourcemanager controleren](../resource-group-audit.md)
- [Lijst van de management gebeurtenissen in een abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx) - om op te halen controlegebeurtenissen.

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) , informatie over het beheren en reageren op beveiligingsmeldingen.
- [Veelgestelde vragen over Azure beveiliging beheercentrum](security-center-faq.md) , zoeken Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) , het laatste nieuws van Azure beveiliging en informatie.
