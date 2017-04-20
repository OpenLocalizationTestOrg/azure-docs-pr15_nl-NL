<properties 
    pageTitle="Overzicht van de Enterprise-integratie | App-Service van Microsoft Azure | Microsoft Azure" 
    description="De functies van Enterprise-integratie gebruiken om het proces en integratie scenario's voor bedrijven met logica apps inschakelen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>Overzicht van het taalpakket Enterprise-integratie

## <a name="what-is-the-enterprise-integration-pack"></a>Wat is de Enterprise-integratie Pack?
Het taalpakket Enterprise-integratie is van Microsoft cloud-oplossing voor het inschakelen van naadloos business-to-business (B2B) communicatie. Het taalpakket gebruikt industriële standaard protocollen, inclusief [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md)en [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) uitwisselen van berichten tussen zakelijke partners. Berichten kunnen desgewenst worden beveiligd met behulp van digitale handtekeningen en versleuteling. 

Het pakket kan organisaties die gebruikmaken van verschillende protocollen en indelingen om berichten door de verschillende indelingen omzetten in een indeling die de beide organisaties systemen kunnen interpreteren en actie ondernemen uitwisselt. 

Als u bekend met BizTalk Server of Microsoft Azure BizTalk-Services bent, vindt u deze eenvoudig te gebruiken dat de Enterprise-integratie omdat de meeste van de begrippen vergelijkbare zijn functies. Een belangrijk verschil is dat Enterprise-integratie integratie accounts gebruikt om de opslag en beheer van onderdelen die worden gebruikt in B2B communicatie te vereenvoudigen. 

Conceptueel, is de Enterprise-integratie Pack gebaseerd op **integratie accounts** die alle onderdelen die kunnen worden gebruikt om te ontwerpen, implementeren en onderhouden van uw apps B2B opslaan. Een account integratie is in principe een cloudgebaseerde container waarin u onderdelen zoals schema's, partners, certificaten, kaarten en overeenkomsten wilt opslaan. Deze onderdelen kunnen vervolgens worden gebruikt in logica apps B2B werkstromen maken. Voordat u de onderdelen in een app logica gebruiken kunt, moet u uw account integratie koppelen aan uw logica-app. Na het koppelen, wordt uw app logica hebben toegang tot de onderdelen van de integratie-account.  

## <a name="why-should-you-use-enterprise-integration"></a>Waarom moet u enterprise-integratie gebruiken?
- Met de ondernemingsintegratie van de bent u kunnen worden opgeslagen van alle uw onderdelen op één plaats, namelijk uw account integratie. 
- U kunt gebruikmaken van de logica apps-engine en alle bijbehorende verbindingslijnen voor bouwen B2B werkstromen en integreren met 3e SaaS toepassingen van derden, on-premises implementatie-apps, evenals aangepaste toepassingen
- U kunt ook gebruikmaken van Azure-functies

## <a name="how-to-get-started-with-enterprise-integration"></a>Hoe u aan de slag met enterprise-integratie?
U kunt maken en beheren van B2B-apps met de Enterprise-integratie Pack via de logica apps ontwerpfunctie op **Azure-portal**.  

U kunt ook [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "logica apps onderwerpen PowerShell") gebruiken voor het beheren van uw apps logica. 

Hier volgt een overzicht van de stappen die u uitvoeren moet voordat u apps in de portal van Azure maken kunt: ![overzicht afbeelding](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Wat zijn enkele veelvoorkomende scenario's?

Integratie van de onderneming worden ondersteund in deze industrienormen:   

- Bewerken - Electronic Data Interchange  
- EAI - integratie van bedrijfstoepassingen  

## <a name="heres-what-you-need-to-get-started"></a>Hier ziet u wat u moet aan de slag
- Een Azure-abonnement met een account van de integratie
- Visual Studio-2015 kaarten en schema's maken
- [Integratie van Microsoft Azure logica Apps Enterprise Tools voor Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Het uitproberen
[Probeer het nu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) om te implementeren van een volledig operationele steekproef AS2 verzenden en ontvangen logica-app waarin de functies B2B van logica Apps.

## <a name="learn-more-about"></a>Meer informatie over:
- [Overeenkomsten] (./app-service-logic-enterprise-integration-agreements.md "Algemene informatie over de enterprise-integratie overeenkomsten")
- [Scenario's voor bedrijven (B2B)] (./app-service-logic-enterprise-integration-b2b.md "Informatie over het maken van logica-apps gebruiken met B2B-functies")  
- [Certificaten] (./app-service-logic-enterprise-integration-certificates.md "Algemene informatie over de enterprise-integratie certificaten")
- [Plat bestand coderen/decoderen] (./app-service-logic-enterprise-integration-flatfile.md "Meer informatie over het coderen en decoderen plat bestandsinhoud")  
- [Integratie-accounts] (./app-service-logic-enterprise-integration-accounts.md "Algemene informatie over de integratie-accounts")
- [Kaarten] (./app-service-logic-enterprise-integration-maps.md "Algemene informatie over de integratie van de enterprise-kaarten")
- [Partners] (./app-service-logic-enterprise-integration-partners.md "Algemene informatie over de enterprise-integratiepartners")
- [Schema's maken] (./app-service-logic-enterprise-integration-schemas.md "Algemene informatie over de enterprise-integratie schema's maken")
- [XML-bericht validatie] (./app-service-logic-enterprise-integration-xml.md "Meer informatie over het voor het valideren van XML-berichten met logica apps")
- [XML-transformatie] (./app-service-logic-enterprise-integration-transform.md "Algemene informatie over de integratie van de enterprise-kaarten")
- [Enterprise-integratie verbindingslijnen] (../connectors/apis-list.md "Algemene informatie over de enterprise-integratie pack verbindingslijnen")



