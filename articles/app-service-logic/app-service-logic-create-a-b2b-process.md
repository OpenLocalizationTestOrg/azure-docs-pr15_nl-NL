<properties 
   pageTitle="Maken van een proces B2B in Azure App Service | Microsoft Azure" 
   description="Overzicht van het maken van een Business-to-Business-proces" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>Maken van een proces B2B

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Bedrijfsscenario 
Contoso en Noordenwind zijn in twee zakenpartners. Contoso (de winkel) stuurt inkooporders naar Noordenwind (leverancier) via een industriële niveau transport zoals AS2. Alle binnenkomende orders Noordenwind opgeslagen in de Cloud-opslag. De inkooporders zijn XML-berichten tussen deze twee partners. Zodra het bericht is opgeslagen in de Noordenwind-cloudopslag verwerken van Northwind interne processen de volgorde vanaf dat punt op.
 
Het doel van deze zelfstudie is tot stand brengen van hoe Noordenwind een bedrijfsproces waarlangs deze kunt berichten (inkooporders in XML-bestand) van de partner Contoso via AS2 en deze vervolgens nog steeds in de Cloud-opslag kunt definiëren.


## <a name="capabilities-demonstrated"></a>Mogelijkheden gedemonstreerd 
Deze zelfstudie kunt presenteren van de volgende mogelijkheden: 

- **Bericht transport**: de winkel en de leverancier kunnen zijn op andere platforms, maar ze kunnen nog steeds communicatie tussen de twee bereiken. In deze zelfstudie ze communiceren via AS2 (toepassing instructie 2). AS2 is een populaire manier om transport gegevens tussen werkbladen partners in business-to-business communicatie.
- **Gegevens permanente**: zodra het bericht is ontvangen via AS2 en vervolgens Noordenwind wil persistent deze voordat u nadere verwerking. Deze kunt een verbindingslijn gebruiken om berichten in de Cloud-opslag. In deze zelfstudie is Azure BLOB's wordt gebruikt als de cloudopslag voor Northwind.
- **Maken van een bedrijfsproces**: In een stroom, meerdere API-apps samen kunnen worden stitched als u wilt bereiken van een uitkomst bedrijven, zoals hier wordt gedemonstreerd.


## <a name="before-you-begin"></a>Voordat u begint
Deze zelfstudie wordt ervan uitgegaan dat u hebt een basiskennis van Azure App Services, weten hoe u API-apps maken en u kunt een stroom samen.


## <a name="steps-to-achieve-the-business-scenario"></a>Stappen voor het bedrijfsscenario bereiken
**Maken en configureren van de vereiste API-apps**

1. Maak een exemplaar van de **Azure opslag Blob verbindingslijn**. Dit is de referenties met een opslag van Azure-account vereist. Zorg ervoor dat deze gereed is voordat u dit maken.
2. Maak een exemplaar van de **BizTalk handelsdag Partner Management**. U moet hiervoor een lege SQL-Database-functie. Zorg ervoor dat deze gereed is voordat u dit maken.
3. Maak een exemplaar van de **AS2 verbindingslijn**. Hiervoor wordt ook een lege SQL-Database-functie. Zorg ervoor dat deze gereed is voordat u dit maken. Ook als u archiveren van berichten als onderdeel van AS2 processing wilt, kunt u desgewenst referenties naar een Azure Blob tijdens het maken.
4. Configureer de TPM (handelsdag Partner Management)-service die is gemaakt:  
    1. Blader naar het exemplaar van de TPM-service gemaakt als onderdeel van de bovenstaande stappen.
    2. De optie **Partners** onder *onderdelen* **toevoegen** een nieuwe partner map **Contoso** en klik in het profiel toevoegen de vereiste AS2-identiteit.
    3. De optie **Partners** onder *onderdelen* **toevoegen** een nieuwe partner **Noordenwind** met de naam en klik in het profiel toevoegen de vereiste AS2-identiteit.
    4. Gebruik de optie **overeenkomsten** onder *onderdelen* **toevoegen** een nieuwe AS2 overeenkomst tussen Noordenwind en Contoso. Northwind worden hier de gehoste partner en Contoso worden de partner Gast. Uw besturingssysteem handtekeningen, codering, compressie en bevestigingen tijdens het maken van deze overeenkomst configureren. Geval certificaten worden gebruikt moeten, kunnen ze worden geüpload via de optie **certificaten** tijdens het bladeren door de TPM-service die u maakt.


## <a name="create-a-flow--business-process"></a>Maken van een stroom / bedrijven verwerken
1. Maak een nieuwe stroom waarin de eerste stap AS2 is. Slepen en neerzetten de **AS2 verbindingslijn** en kiest u de sessie al hebt gemaakt. Kies trigger als de functionaliteit voor:  
    ![][1]  
2. Volgende slepen en neerzetten van **Azure opslag Blob verbindingslijn** en kies het exemplaar dat al hebt gemaakt. Kies actie als de functionaliteit en in die, selecteert u **Uploaden Blob** als de gewenste functionaliteit. Configureer uw besturingssysteem.
3. Nu maken/implementeren de stroom.


## <a name="message-processing--troubleshooting"></a>Verwerking van berichten en probleemoplossing
1. Het is tijd om te testen om de stroom die we hebben geïmplementeerd. Stuur dat XML berichten u met terugloop in AS2 (aan de hand van de AS2 overeenkomst hiervoor hebt gemaakt) naar het eindpunt AS2 is opgehaald door het exemplaar AS2Connector die u hebt gemaakt. Mogelijk moet u de verificatie voor het eindpunt zodanig configureren dat deze openbaar toegankelijk is.
2. Uitvoering van informatie over de stroom zichtbaar is door te bladeren naar de stroom en klikt u vervolgens stap voor stap in de stroom-exemplaar die hebt uitgevoerd
3. Blader naar het exemplaar AS2Connector betrokken voor AS2 verwerking informatie en volg te doorlopen in het gedeelte bijhouden. U kunt de filters die nodig zijn voor de weergave beperken tot de informatie dat nodig is.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
