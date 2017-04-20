<properties 
    pageTitle="Business-to-Business verbindingslijnen en API-Apps in logica Apps | Microsoft Azure" 
    description="Meer informatie over het maken en bewerken, EDIFACT AS2 en TPM connectors; configureren microservices architectuur" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Business-to-Business verbindingslijnen en API-Apps

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Logica Apps bevat veel BizTalk-API-Apps die essentieel voor integratie-omgevingen zijn. Deze API-Apps zijn gebaseerd op concepten en hulpprogramma's voor in BizTalk Server gebruikt, maar zijn nu beschikbaar als onderdeel van de logica Apps. 

Een categorie deze API-Apps zijn de Business-to-Business (B2B) API-Apps. Deze B2B API-Apps gebruikt, kunt u eenvoudig partners toevoegen, overeenkomsten maken, en voer alles als u zou on-premises met bewerken, AS2 en EDIFACT.  

Deze B2B API-Apps bieden "Trigger" en "Actie" mogelijkheden. Een Trigger start een nieuwe kopie op basis van een bepaalde gebeurtenis, zoals de ontvangst van een X12 bericht van een partner. Een actie is het resultaat, zoals na een X12 ontvangen bericht en verzend het bericht met AS2.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Wat is een Business-to-Business Connector of API-Apps
De functie Business-to-Business (B2B) bevat bestaande, vooraf gedefinieerde API-Apps die andere bedrijven, afdelingen, -toepassingen, enzovoort om te communiceren met AS2, bewerken en EDIFACT toestaan. 

De B2B API-Apps zijn: 

Verbindingslijn of API-Apps | Beschrijving
--- | ---
BizTalk handelsdag Partner Management | Een API-App die wordt gemaakt van business-to-business (B2B) relaties met partners en overeenkomsten. Deze relaties gebruikmaken van de AS2, EDIFACT en X12 protocol.<br/><br/>De TPM API-App is de basistoewijzing vereiste van de verbindingslijn AS2 en de X12 of EDIFACT API-Apps. 
AS2 verbindingslijn | Een verbindingslijn die berichten in het transport AS2 verzendt en ontvangt. De verbindingslijn transacties gegevens veilig en betrouwbaar via Internet.
BizTalk EDIFACT | Een API-App die EDIFACT-mailberichten verzendt en ontvangt. EDIFACT is ook zogenaamde UN/EDIFACT (United Nations/elektronische Data Interchange voor beheer, e-Commerce en Transport) en sterk uiteen worden gebruikt in de industrie.
BizTalk X12 | Een API-App die berichten in de X12 verzendt en ontvangt protocol. X12 is ook zogenaamde ASC X 12 (erkende standaarden Commissie X12) en sterk uiteen worden gebruikt in de industrie. 


Deze API-Apps gebruikt, kunt u verschillende bewerken messaging taken uitvoeren. Bijvoorbeeld de verbindingslijn AS2 gebruikt, u kunt veilig ontvangen en verschillende soorten berichten verzenden (bewerken, XML, platte-bestand, enzovoort) aan een klant, zoals een afdeling binnen uw bedrijf Human Resources of iedereen die wordt gebruikt AS2. 

U kunt zo veel API-Apps als u wilt en deze gemakkelijk maken maken. U kunt ook opnieuw gebruiken in één API App vanuit meerdere scenario's of werkstromen.

U kunt dit doen een code te schrijven. Laten we aan de slag. 


## <a name="requirements-to-get-started"></a>Vereisten aan de slag
Als u B2B API-Apps hebt gemaakt, zijn er enkele vereiste resources. Deze items moeten worden gemaakt door u voordat ze in andere API-Apps kunnen worden gebruikt. Deze vereisten omvatten: 

Vereiste | Beschrijving
--- | ---
Azure SQL-Database | B2B items zoals partners, schema's, certificaten en agreeements worden opgeslagen. Een eigen Azure SQL-Database voor elk van de B2B API-Apps is vereist. <br/><br/>**Opmerking** Kopieer de verbindingsreeks met deze database.<br/><br/>[Een Azure SQL-Database maken](../sql-database/sql-database-get-started.md)
Azure-blobopslag container | Winkels bericht eigenschappen wanneer AS2 archivering is ingeschakeld. Als u niet nodig AS2 bericht archiveren hebt, is een container opslag niet nodig. <br/><br/>**Opmerking** Als u archiveren inschakelen wilt, kopieert u de verbindingsreeks met deze-blobopslag.<br/><br/>[Over Accounts Azure opslag](../storage/storage-create-storage-account.md)
Service Bus Namespace en de toets-waarden | X12 en EDIFACT batchen gegevens opgeslagen. Als u niet nodig batchen hebt, is een naamruimte Service Bus niet nodig.<br/><br/>**Opmerking** Als u batchen inschakelen wilt, kopieert u deze waarden.<br/><br/>[Een Service Bus Namespace maken](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM exemplaar | Een exemplaar BizTalk handelsdag Partner Management (Trusted) is vereist voor het maken van een connector AS2 en X12 of EDIFACT API-App. Wanneer u de TPM API-App hebt gemaakt, maakt u het exemplaar TPM. <br/><br/>**Opmerking** De naam weet van uw TPM API-App. 


## <a name="create-the-api-apps"></a>Maak de API-Apps
B2B API-Apps kunnen worden gemaakt met behulp van de Azure portal of met REST API's. 


### <a name="create-the-api-apps-using-rest-apis"></a>Maak de API-Apps met REST API 's
[Zie de documentatie over het gebruik van de REST API's.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>De B2B API-Apps maken in de Portal van Azure
U kunt in de portal Azure B2B API-Apps maken bij het maken van logica Apps, Web Apps of Mobile-Apps. Of, kunt u er met een eigen blade maken. Beide manieren zijn eenvoudig zodat dit hangt af van uw behoeften of voorkeuren. Sommige gebruikers liever de Apps voor de API B2B eerst met hun specifieke eigenschappen maken. Vervolgens de logica Apps/Web Apps/Mobile-Apps maken en toevoegen van de B2B API-Apps die u hebt gemaakt.  

De volgende stappen uit maken de B2B API-Apps met behulp van het blad API-Apps.


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>De Partner Management (Trusted) API Apps handelsdag BizTalk maken

> [AZURE.NOTE] Een exemplaar BizTalk handelsdag Partner Management (Trusted) is vereist voor het maken van een connector AS2 en X12 of EDIFACT API-App. Wanneer u de TPM API-App hebt gemaakt, maakt u het exemplaar TPM.

De volgende stappen uit maken het exemplaar TPM:

1. Selecteer in de portal Startboard (de introductiepagina) voor Azure **Marketplace**. **API-Apps** worden alle bestaande API-Apps en verbindingslijnen. U kunt ook **Zoeken** voor de specifieke B2B API-Apps.
2. Selecteer **BizTalk Partner Management handelsdag**. Selecteer in het nieuwe blad **maken**. 
3. Voer in de eigenschappen: 

    Eigenschap | Beschrijving
--- | ---
Naam | Voer een naam voor het exemplaar TPM. Bijvoorbeeld: u kunt noem deze *AccountsPayableTPM*.
Package Settings | Voer de ADO.NET- **Verbindingsreeks van de Database** met de Azure SQL-Database die u hebt gemaakt. <br/><br/>Wanneer u de verbindingsreeks kopieert, wordt het wachtwoord niet toegevoegd aan de verbindingsreeks. Zorg ervoor dat het wachtwoord invoeren in de verbindingstekenreeks.
App-abonnement | Uw abonnement betaling bevat. Als u meer of minder resources nodig hebt, kunt u deze wijzigen.
Prijzen van laag | Alleen-lezen eigenschap waarin de prijzen categorie binnen uw Azure-abonnement. 
Resourcegroep | Maak een nieuwe of een bestaande groep gebruiken. Alle API-Apps en verbindingslijnen voor uw logica Apps, Web Apps en Mobile-Apps moeten zich in dezelfde resourcegroep. <br/><br/>[Gebruik van resourcegroepen](../azure-resource-manager/resource-group-overview.md) , wordt deze eigenschap uitgelegd. 
Abonnement | Alleen-lezen eigenschap waarin uw huidige abonnement.
Locatie | De geografische locatie waarop uw Azure-service. 
Toevoegen aan Startboard | Selecteer deze optie de B2B API-App toevoegen aan uw Starboard (de introductiepagina).

4. Selecteer **maken**. 

Nadat de TPM API-APP (TPM exemplaar) is gemaakt, kunt u vervolgens de verbindingslijn AS2 en/of de X12 of EDIFACT API-Apps maken. 


#### <a name="create-the-as2-connector"></a>De verbindingslijn AS2 maken

1. Selecteer in de portal Startboard (de introductiepagina) voor Azure **Marketplace**. **API-Apps** worden alle bestaande API-Apps en verbindingslijnen. U kunt ook **Zoeken** voor de specifieke B2B API-Apps.
2. Selecteer **AS2 verbindingslijn**. Selecteer in het nieuwe blad **maken**. 
3. Voer in de eigenschappen: 

    Eigenschap | Beschrijving
--- | ---
Naam | Voer een naam voor de verbindingslijn AS2. Bijvoorbeeld: u kunt noem deze *AS2Connector*.
Package Settings | Geef de specifieke instellingen op die API-Apps, zoals de naam van het TPM exemplaar. <br/><br/>Zie [Toevoegen AS2 Package Settings](#AddAS2Conn) in dit onderwerp voor de specifieke eigenschappen. 
App-abonnement | Uw abonnement betaling bevat. Als u meer of minder resources nodig hebt, kunt u deze wijzigen.
Prijzen van laag | Alleen-lezen eigenschap waarin de prijzen categorie binnen uw Azure-abonnement. 
Resourcegroep | Maak een nieuwe of een bestaande groep gebruiken. [Gebruik van resourcegroepen](../azure-resource-manager/resource-group-overview.md) , wordt deze eigenschap uitgelegd. 
Abonnement | Alleen-lezen eigenschap waarin uw huidige abonnement.
Locatie | De geografische locatie waarop uw Azure-service. 
Toevoegen aan Startboard | Selecteer deze optie de B2B API-App toevoegen aan uw Starboard (de introductiepagina).

    **<a name="AddAS2Conn"></a>AS2 verbindingslijn Package Settings**

    Eigenschap | Beschrijving
--- | --- 
Database-verbindingsreeks | Voer de ADO.NET-verbindingsreeks met de Azure SQL-Database die u hebt gemaakt. Wanneer u de verbindingsreeks kopieert, wordt het wachtwoord niet toegevoegd aan de verbindingsreeks. Zorg ervoor dat het wachtwoord invoeren in de verbindingstekenreeks voordat u plakt.
Archivering inschakelen voor binnenkomende berichten | Optioneel. Deze eigenschap voor de opslag van de berichteigenschappen van een binnenkomend AS2 bericht ontvangen van een partner inschakelen. 
Azure Blob Storage-verbindingsreeks  | Voer de verbindingsreeks aan de Azure-blobopslag container die u hebt gemaakt. Wanneer archivering is ingeschakeld, worden gecodeerd en gecodeerde berichten worden opgeslagen in deze container opslag.
TPM exemplaarnaam | Voer de naam van de **BizTalk handelsdag Partner Management** API-App u eerder hebt gemaakt. Wanneer u de verbindingslijn AS2 maakt, wordt alleen de AS2 overeenkomsten binnen dit specifieke exemplaar van de TPM door deze connector uitgevoerd.

4. Selecteer **maken**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>Maak de X12 of EDIFACT API-Apps

1. Selecteer in de portal Startboard (de introductiepagina) voor Azure **Marketplace**. **API-Apps** worden alle bestaande API-Apps en verbindingslijnen. U kunt ook **Zoeken** voor de specifieke B2B API-Apps.
2. Selecteer **BizTalk X 12** of **BizTalk EDIFACT**. Selecteer in het nieuwe blad **maken**. 
3. Voer in de eigenschappen: 

    Eigenschap | Beschrijving
--- | ---
Naam | Voer een naam voor de B2B API-App. Bijvoorbeeld: u kunt noem deze *EDI850APIApp*.
Package Settings | Geef de specifieke instellingen op die API-Apps, zoals de naam van het TPM exemplaar. <br/><br/>Zie [X12 of EDIFACT Package Settings](#AddX12) in dit onderwerp voor de specifieke eigenschappen. 
App-abonnement | Uw abonnement betaling bevat. Als u meer of minder resources nodig hebt, kunt u deze wijzigen.
Prijzen van laag | Alleen-lezen eigenschap waarin de prijzen categorie binnen uw Azure-abonnement. 
Resourcegroep | Maak een nieuwe of een bestaande groep gebruiken. [Gebruik van resourcegroepen](../azure-resource-manager/resource-group-overview.md) , wordt deze eigenschap uitgelegd. 
Abonnement | Alleen-lezen eigenschap waarin uw huidige abonnement.
Locatie | De geografische locatie waarop uw Azure-service. 
Toevoegen aan Startboard | Selecteer deze optie de B2B API-App toevoegen aan uw Starboard (de introductiepagina).

    **<a name="AddX12"></a>X12 en EDIFACT API Apps Package Settings**  

    Eigenschap | Beschrijving
--- | --- 
Database-verbindingsreeks | Voer de ADO.NET-verbindingsreeks met de Azure SQL-Database die u hebt gemaakt. Wanneer u de verbindingsreeks kopieert, wordt het wachtwoord niet toegevoegd aan de verbindingsreeks. Zorg ervoor dat het wachtwoord invoeren in de verbindingstekenreeks voordat u plakt.
Service Bus Namespace | Voer de Service Bus naamruimte die u hebt gemaakt. Vereist alleen wanneer batchen is ingeschakeld. 
De naam van de service Bus Namespace gedeeld toegangstoets | Voer de Service Bus naamruimte toegangstoets die u hebt gemaakt. Vereist alleen wanneer batchen is ingeschakeld. 
Waarde van de service Bus Namespace gedeeld toegangstoets | Voer de Service Bus naamruimte toegangstoets waarde die u hebt gemaakt. Vereist alleen wanneer batchen is ingeschakeld. 
TPM exemplaarnaam | Voer de naam van de **BizTalk handelsdag Partner Management** API-App u eerder hebt gemaakt. Wanneer u de X12 of EDIFACT API Apps maakt, wordt alleen de X12/EDFIACT overeenkomsten binnen dit specifieke exemplaar van de TPM door deze App API uitgevoerd.

4. Selecteer **maken**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>Uw partners, overeenkomsten, certificaten en schema's toevoegen 
Open uw TPM API-App in de portal Azure. Klik in de sectie **onderdelen** toevoegen uw Partners, overeenkomsten, certificaten en schema's. 

U kunt ook overeenkomsten toevoegen aan uw AS2 verbindingslijnen, X12 API-Apps en EDIFACT API-Apps. 


## <a name="monitor-your-api-apps"></a>Uw API-Apps controleren
Open uw TPM API-App in de portal Azure. U kunt verschillende management bewerkingen weergeven in de sectie **bewerkingen** . U kunt bijvoorbeeld het volgende doen:

- Gebeurtenissen weergeven ter informatie en fout
- Geheugen gebruik en thread telling van het werkproces (w3wp) weergeven
- Weergeven van de toepassing en web server-Logboeken

Meer op de [Monitor uw logica-Apps](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>De API-Apps toevoegen aan uw toepassing 
App-Service van Microsoft Azure beschrijft de verschillende toepassingstypen die deze B2B API-Apps kunnen gebruiken. U kunt een nieuwe maken of uw bestaande B2B API-Apps toevoegen aan Apps logica, Mobile-Apps of een Web-Apps. 

Binnen uw App gewoon Apps te kiezen de B2B API in de galerie automatisch toegevoegd aan uw App.  

> [AZURE.IMPORTANT] Als u wilt toevoegen aan verbindingslijnen en API-Apps die u eerder hebt gemaakt, de logica Apps, Mobile-Apps of Web Apps in dezelfde resourcegroep te maken. 

De volgende stappen toevoegen de B2B API-Apps logica Apps, Mobile-Apps of Web Apps: 

1. Ga naar de **Office Store**en zoek uw logica, Mobile of Web-Apps in de Azure-portal Startboard (introductiepagina). 

    Als u een nieuwe App maakt, kunt u zoeken naar logica Apps, Mobile-Apps of Web-Apps. Selecteer de App en selecteer in het nieuwe blad **maken**. [Een App logica maken](app-service-logic-create-a-logic-app.md) vindt u de stappen. 

2. Open uw App en selecteer **Triggers en acties**. 

3. Selecteer de B2B API-App, dat deze automatisch toegevoegd aan uw App uit de **Galerie**. U kunt ook een nieuwe B2B API-App maken.

    > [AZURE.IMPORTANT] De verbindingslijn AS2 en X12, vereisen EDIFACT API Apps een exemplaar TPM. Als u maakt nieuwe B2B API-Apps, eerst de TPM API-App maken en maak vervolgens de verbindingslijn AS2, X12 API-App of EDIFACT API-App. 

4. Selecteer **OK** om uw wijzigingen op te slaan. 

>[AZURE.NOTE] Aan de slag met Azure logica Apps voordat u zich registreert voor een Azure-account, [Probeert u logica Apps](https://tryappservice.azure.com/?appservice=logic). U kunt direct een tijdelijk starter logica-app maken. Geen creditcards vereist; geen verplichtingen.

## <a name="more-b2b-resources"></a>Meer B2B informatiebronnen

[Maken van een proces B2B](app-service-logic-create-a-b2b-process.md)<br/>
[Maken van een handelen partnerovereenkomst](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Wat zijn de verbindingslijnen en BizTalk-API Apps](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>Lees meer over de logica Apps en WebApps
[Wat zijn de logica Apps?](app-service-logic-what-are-logic-apps.md)<br/>
[Websites en Web-Apps in Azure App-Service](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>Meer verbindingslijnen

[Verbindingslijnen en de lijst met de API-Apps](app-service-logic-connectors-list.md)<br/><br/>
[Wat zijn de verbindingslijnen en BizTalk-API Apps](app-service-logic-what-are-biztalk-api-apps.md) 
