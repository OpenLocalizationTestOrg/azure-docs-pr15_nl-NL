<properties 
   pageTitle="Maken van een handelen partnerovereenkomst in Azure App Service | Microsoft Azure" 
   description="Partner overeenkomsten handelsdag maken" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>Maken van een handelen partnerovereenkomst   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Werkbladen partners worden de entiteiten voor het B2B (Business-to-Business) communicatie. Wanneer u twee partners een relatie definiëren, is dit genoemd een *overeenkomst*. De overeenkomst gedefinieerd op basis van de communicatie de twee partners wilt bereiken en is protocol of transport specifieke. De verschillende B2B protocollen en transport worden ondersteund door Azure App Service bevatten:

- AS2 (toepassing overzicht 2)
- EDIFACT (United Nations/elektronische Data Interchange voor beheer, Commerce en Transport (UN/EDIFACT))
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>BizTalk-API-Apps die ondersteuning bieden voor B2B scenario 's
De volgende API-Apps inschakelen deze mogelijkheden voor een uitgebreide en intuïtieve-ervaring in de Portal Azure gebruikt:


## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk handelsdag Partner Management (Trusted)
- Maken en beheren van Partners, profielen en identiteiten
- Opslag en het beheer van schema's bewerken
- Opslag en het beheer van certificaten (gebruikt in AS2 protocol)
- Maken en beheren van AS2 overeenkomsten
- Maken en beheren van EDIFACT overeenkomsten (inclusief batchen aan de kant verzenden)
- Maken en beheren van X12 overeenkomsten (inclusief batchen aan de kant verzenden)

![][1]


## <a name="as2-connector"></a>AS2 verbindingslijn
- AS2 overeenkomsten wordt uitgevoerd, zoals gedefinieerd in de gerelateerde TPM API App-exemplaar
- Itemweergavesjabloon geeft AS2 verwerking/bijhouden informatie voor probleemoplossing


## <a name="biztalk-edifact"></a>BizTalk EDIFACT
- EDIFACT overeenkomsten wordt uitgevoerd, zoals gedefinieerd in de gerelateerde TPM API App-exemplaar
- Itemweergavesjabloon geeft EDIFACT verwerking/bijhouden informatie voor probleemoplossing
- Statusbeheer van batches (start en beëindiging) biedt voorzien, zoals gedefinieerd in EDIFACT overeenkomst(en) in de gerelateerde TPM API App-exemplaar


## <a name="biztalk-x12"></a>BizTalk X12
- Wordt uitgevoerd X12 overeenkomsten zoals gedefinieerd in de gerelateerde TPM API App-exemplaar 
- Oppervlakken X12 verwerking/bijhouden informatie voor probleemoplossing
- Statusbeheer van batches (start en beëindiging) biedt voorzien, zoals gedefinieerd in X12 overeenkomst(en) in de gerelateerde TPM API App-exemplaar

De AS2, X 12 en EDIFACT API Apps vereist een App TPM API-functie als bovengenoemde, zoals verwacht.


## <a name="getting-started"></a>Aan de slag
Werkbladen partner overeenkomsten maken:

1. Maak een exemplaar van de verbindingslijn **BizTalk handelsdag Partner Management** . U moet hiervoor een lege SQL-Database-functie. Voordat u begint Zorg ervoor dat u hebt een lege database beschikbaar en gereed voor gebruik.
2. Schema's en certificaten zoals vereist door de overeenkomsten uploaden. Dit doen door het exemplaar TPM gemaakt en in het gedeelte 'Schema's ' en/of 'Certificaten' zit bladeren
3. Blader naar het TPM exemplaar gemaakt en stap in het gedeelte **Partners**
4. Maak partners naar wens. Ook de profiel(en) desgewenst bewerken en de vereiste identiteiten toevoegen
5. Nu gebruiken het deel achter de **overeenkomsten** overeenkomsten maken. Wanneer u een overeenkomst maakt, moet u het protocol dat wordt gebruikt. De resterende configuratieopties op basis van het protocol dat u hebt geselecteerd.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
