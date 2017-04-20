<properties 
   pageTitle="Azure SDK voor .NET 2,9 releaseopmerkingen" 
   description="Azure SDK voor .NET 2,9 releaseopmerkingen" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-29-release-notes"></a>Azure SDK voor .NET 2,9 releaseopmerkingen

##<a name="overview"></a>Overzicht

Dit document bevat de releaseopmerkingen voor Azure SDK voor .NET 2,9 release. 

Zie voor gedetailleerde informatie over de updates die in deze release de [aankondiging van Azure SDK 2,9 posten](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Voorbeeld van Azure SDK 2,9 voor Visual Studio 2015 Update 2 en Visual Studio "15"
 
Deze update bevat de volgende correcties:

- Probleem die betrekking hebben op de plaats API-Client generatie in waarin de tekenreeks "Onbekend Type" wordt weergegeven als de naam van de map code generatie en/of de naam van de naamruimte niet weergegeven in de gegenereerde code.
- Het probleem is gerelateerd aan gepland WebJobs waarin de verificatiegegevens worden doorgegeven aan de inrichting van proces planner is verbroken.

Deze update bevat de volgende functie:

- Ondersteuning voor secundaire App-Services op het tabblad "Services" van het dialoogvenster voor het inrichten van App-Service. 

##<a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure Data Lake Tools voor Visual Studio 2015 Update 2
 
Deze updates omvat het volgende:

- **Azure Data Lake Tools** voor Visual Studio is nu in de SDK Azure voor .NET release samengevoegd. Het hulpmiddel wordt automatisch geïnstalleerd tijdens de installatie van Azure SDK. 

    Het hulpmiddel regelmatig worden bijgewerkt, ga [hier](http://aka.ms/datalaketool) om de updates te downloaden.

- **Server Explorer** kunt u nu alle weergeven en maken van sommige entiteiten I-SQL-metagegevens. Zie voor meer informatie [in dit](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.


##<a name="hdinsight-tools"></a>Hulpmiddelen voor HDInsight 

**HDInsight Tools** voor Visual Studio ondersteunt nu HDInsight versie 3.3, inclusief waarin Tez grafieken en andere taal worden opgelost.


##<a name="azure-resource-manager"></a>Azure resourcemanager 

Deze release toevoegen [KeyVault](../resource-manager-keyvault-parameter.md) ondersteuning voor ARM sjablonen.

##<a name="see-also"></a>Zie ook

[Azure SDK 2,9 aankondiging bericht](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)
