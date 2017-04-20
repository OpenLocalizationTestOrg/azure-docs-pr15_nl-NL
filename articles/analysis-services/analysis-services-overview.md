<properties
   pageTitle="Wat is Azure Analysis Services | Microsoft Azure"
   description="Krijg het totaalbeeld van Analysis Services in Azure wordt aangegeven."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="what-is-azure-analysis-services"></a>Wat is Azure Analysis Services?
![Azure analyseservices](./media/analysis-services-overview/aas-overview-aas-icon.png)

Gebaseerd op de beproefde analyse-engine in Microsoft SQL Server Analysis Services en biedt Azure Analysis Services voor enterprise-grade-gegevens modelleren in de cloud.

> [AZURE.IMPORTANT] Azure Analysis Services is in de **proefversie**. Er zijn enkele dingen die alleen nog niet werken. Zorg ervoor dat u wilt controleren [Preview](#preview-expectations) verwachtingen verderop in dit artikel. En zorg ervoor dat u onze [Azure Analysis Services-blog](https://go.microsoft.com/fwlink/?linkid=830920) voor de meest recente informatie gaten houden.

## <a name="built-on-sql-server-analysis-services"></a>Gebouwd op SQL Server analyseservices
Azure Analysis Services is compatibel met dezelfde SQL Server 2016 Analysis Services Enterprise Edition u al kent. Azure Analysis Services ondersteunt tabellaire modellen op het niveau van de compatibiliteit 1200. DirectQuery, partities, beveiliging op gebruikersniveau rij, bidirectionele relaties en vertalingen worden allemaal ondersteund.

## <a name="use-the-tools-you-already-know"></a>Gebruik de hulpmiddelen die u al weten
![Hulpmiddelen voor BI ontwikkelaars](./media/analysis-services-overview/aas-overview-dev-tools.png)

Bij het maken van gegevensmodellen voor Azure Analysis Services, kunt u dezelfde hulpmiddelen voor SQL Server Analysis Services gebruiken. Ontwerpen en implementeren van tabellaire modellen met behulp van de meest recente versies van [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx) of via [Azure Powershell](../powershell-install-configure.md) en [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) sjablonen in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connect-to-data-sources"></a>Verbinding maken met gegevensbronnen
Gegevensmodellen geïmplementeerd op servers van Azure support verbinding maken met gegevensbronnen on-premises implementatie in uw organisatie of in de cloud. Combineer gegevens vanuit zowel on-premises en cloud gegevensbronnen voor een hybride BI-oplossing.

![Gegevensbronnen](./media/analysis-services-overview/aas-overview-data-sources.png)

Omdat de server in de cloud is, is verbinding maken met gegevensbronnen cloud naadloze. Als verbinding maakt met on-premises gegevensbronnen, zorgt de [On-premises gegevensgateway](analysis-services-gateway.md) snelle, beveiligde verbindingen met de Analysis Services-server in de cloud.  

 \*Sommige gegevensbronnen worden nog niet ondersteund in de proefversie. Meer informatie raadpleegt u [Preview verwachtingen](#preview-expectations) verderop in dit artikel.

## <a name="explore-your-data-from-anywhere"></a>Uw gegevens verkennen vanaf elke locatie
Verbinding maken en [gegevens ophalen](analysis-services-connect.md) uit uw servers van over waar u ook bent. Azure Analysis Services ondersteunt verbindingen van Power BI Desktop, Excel, aangepaste apps en browser-programma's.

![Gegevensvisualisaties](./media/analysis-services-overview/aas-overview-visualization.png)

 \*Power BI ingesloten is nog niet ondersteund in de proefversie.

## <a name="secure"></a>Secure

#### <a name="user-authentication"></a>Gebruikersverificatie
Gebruikersverificatie voor Azure Analysis services wordt uitgevoerd door [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Tijdens een poging om het aanmelden bij een Azure Analysis Services-database, gebruikers de identiteit van een organisatie-account gebruiken met toegang tot de database die deze probeert te openen. Deze gebruikersidentiteiten moeten lid zijn van de standaardindeling Azure Active Directory voor het abonnement waarin de Azure Analysis Services-server zich bevindt. [Directory-integratie](https://technet.microsoft.com/library/jj573653.aspx) tussen AAD en een lokale Active Directory vormt een uitstekende manier om uw on-premises gebruikerstoegang krijgen tot een Azure Analysis Services-database, maar niet vereist voor alle scenario's.

Gebruikers Meld u aan met de UPN (User Principal Name) van hun-account en hun wachtwoord. Wanneer dit wordt gesynchroniseerd met een lokale Active Directory, is van de gebruiker UPN vaak hun organisatie-e-mailadres.

Machtigingen voor het beheer van de resource Azure Analysis Services-server worden verwerkt door het toewijzen van gebruikers aan rollen in uw Azure-abonnement. Beheerders van een abonnement hebben standaard eigenaarsmachtigingen voor de Serverbron in Azure wordt aangegeven. Extra gebruikers kunnen worden toegevoegd met behulp van Azure resourcemanager.

#### <a name="data-security"></a>Beveiliging van gegevens
Azure Analysis Services wordt gebruikgemaakt van Azure-blobopslag persistent opslagcapaciteit en de metagegevens voor Analysis Services-databases maken. Gegevensbestanden binnen Blob zijn versleuteld met Azure Blob Server kant versleuteling (SSE). Wanneer u met directe querymodus, wordt alleen metagegevens opgeslagen; de werkelijke gegevens wordt geraadpleegd uit de gegevensbron op query moment.

#### <a name="on-premises-data-sources"></a>On-premises gegevensbronnen
Beveiligde toegang tot gegevens tijdelijk on-premises implementatie in uw organisatie kunnen worden bereikt door installeren en configureren van een [On-premises gegevensgateway](analysis-services-gateway.md). Gateways bieden toegang tot gegevens voor directe Query zowel in het geheugen modi. Wanneer een Azure Analysis Services-model verbinding met een lokale gegevensbron maakt, wordt een query samen met de versleutelde referenties voor de on-premises gegevensbron gemaakt. De gateway-cloudservice analyseert de query en de aanvraag verplaatst naar een Bus Azure-Service. De gateway on-premises implementatie controleert de Bus van de Azure-Service voor aanvragen in behandeling. De gateway vervolgens wordt de query, ontsleutelt de referenties en maakt verbinding met de gegevensbron voor uitvoering. De resultaten worden vervolgens verzonden vanuit de gegevensbron, terug naar de gateway en klik vervolgens op de Azure Analysis Services-database.

Azure Analysis Services is onderworpen aan de [Microsoft Online Services-termen](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [Privacyverklaring van Microsoft Online Services](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Meer informatie over de beveiliging van Azure, raadpleegt u het [Vertrouwenscentrum in Microsoft](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="get-help"></a>U kunt hulp krijgen
Azure Analysis Services is eenvoudig om in te stellen en om te beheren. U vindt alle informatie die u wilt maken en beheren van een server hier. Bij het maken van een gegevensmodel om te implementeren naar uw server, is dit vergelijkbaar met omdat deze voor het maken van een gegevensmodel die u dashboard naar een on-premises implementatie-server implementeren. Er is een uitgebreide bibliotheek met concepten, procedures, zelfstudies en verwijzing artikelen op [Analysis Services op MSDN](https://msdn.microsoft.com/library/bb522607.aspx).

We hebben een aantal handige video's ook op [Azure Analysis Services op kanaal 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

Dingen wilt snel wijzigen. U krijgt altijd de meest recente informatie over het [blog van Azure Analysis Services](https://go.microsoft.com/fwlink/?linkid=830920).

## <a name="community"></a>Community
Analyseservices heeft een levendige community van gebruikers. Deelnemen aan het gesprek op [Azure Analysis Services-forum](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Feedback
Hebt u suggesties of functieverzoeken verzenden? Zorg ervoor dat u uw reacties op [Azure Analysis Services Feedback](https://aka.ms/azureanalysisservicesfeedback).

Suggesties over de documentatie hebben? U kunt opmerkingen met Disqus onder van elk artikel toevoegen.

## <a name="preview-expectations"></a>Voorbeeld verwachtingen
Azure Analysis Services is momenteel in de proefversie. Er zijn enkele dingen die u houden van moet rekening.

##### <a name="server-modes"></a>Servermodi
Azure Analysis Services ondersteunt momenteel in tabelvorm modus voor tabellaire modellen op het niveau van de compatibiliteit 1200. Multidimensionale en datamining modus en Power Pivot voor SharePoint-modus worden niet ondersteund.

##### <a name="data-sources"></a>Gegevensbronnen
Voor preview, worden de volgende gegevensbronnen worden ondersteund in tabelvorm 1200 modellen die zijn geïmplementeerd in een Azure Analysis Services-server.

| **Cloud** | **On-premises implementatie** |
|--------------|------------|
| SQL-database | SQL Server |
| SQL datawarehouse | APS |
|      | Oracle |
|       | Teradata |


### <a name="data-source-providers"></a>Leveranciers van gegevensbronnen
Gegevensmodellen in Azure Analysis Services mogelijk verschillende gegevensproviders verbinding maken met gegevensbronnen dan gegevensmodellen in SQL Server Analysis Services. Vereisten voor providers van gegevens, hangt af van de gegevensbron wordt in de cloud of on-premises implementatie en het type gegevensmodel; in het geheugen of Direct Query. Zie voor meer informatie, [Uitvoergegevensbron verbindingen](analysis-services-datasource.md).


### <a name="client-connections"></a>Clientverbindingen
Power BI ingesloten is nog niet ondersteund in de proefversie.

Excel-werkmappen met live verbindingen naar een Azure Analysis Services-server en opgeslagen op OneDrive of SharePoint Online worden niet ondersteund.



## <a name="next-steps"></a>Volgende stappen
Nu u meer weten over Azure Analysis Services, is het tijd aan de slag. Informatie over het [maken van een server](analysis-services-create-server.md) in Azure en [implementeren van een tabellair model](analysis-services-deploy.md) toe.
