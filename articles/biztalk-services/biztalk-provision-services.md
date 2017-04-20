<properties
    pageTitle="Azure BizTalk Services maken in de portal van Azure | Microsoft Azure"
    description="Informatie over het inrichten van of Azure BizTalk Services in de beheerportal Azure; maken MAB, WABS"
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>BizTalk-Services met behulp van de Azure portal maken

Azure BizTalk Services in de portal van Azure maken.

> [AZURE.TIP] Als u zich aanmeldt bij de Azure-portal, moet u een Azure-account en Azure-abonnement. Als u geen account hebt, kunt u een gratis proefabonnement-account maken binnen enkele minuten. Zie [Azure gratis proefversie](http://go.microsoft.com/fwlink/p/?LinkID=239738).

## <a name="create-a-biztalk-service"></a>Een Service BizTalk maken
Afhankelijk van de versie die u kiest, mogelijk niet alle BizTalk Service-instellingen beschikbaar.

1. Meld u aan bij de [portal van Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het navigatiedeelvenster onder de optie **Nieuw**:  
![Selecteer de knop Nieuw][NEWButton]

3. Selecteer de **APP SERVICES** > **BIZTALK SERVICE** > **aangepaste maken**:  
![Selecteer BizTalk Service en schakel aangepaste maken][NewBizTalkService]

4. Voer in het BizTalk Service-instellingen:

    <table border="1">
    <tr>
    <td><strong>BizTalk servicenaam</strong></td>
    <td>U kunt elke naam invoeren maar zo specifiek mogelijk. Enkele voorbeelden:<br/><br/>
    <em>mijnbedrijf</em>. biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>MyApplication wordt gedefinieerd</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net ' automatisch toegevoegd aan de naam die u invoert. Hiermee maakt u een URL die wordt gebruikt voor toegang wilt tot uw BizTalk Service <strong>https://<em>MyApplication wordt gedefinieerd</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edition</strong></td>
    <td>Als u zich in de testen/ontwikkelingsfase, kiest u <strong>ontwikkelaars</strong>. Als u zich in de productiefase, gebruikt u de <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: edities grafiek</a> om te bepalen of <strong>Premium</strong>, <strong>standaard</strong>of <strong>eenvoudige</strong> de juiste keuze voor uw bedrijfsscenario.
    </td>
    </tr>
    <tr>
    <td><strong>Regio</strong></td>
    <td>Selecteer het geografische gebied voor het hosten van uw BizTalk Service.</td>
    </tr>
    <tr>
    <td><strong>URL voor het domein</strong></td>
    <td><strong>Optioneel</strong>. De URL van het domein is standaard <em>YourBizTalkServiceName</em>. biztalk.windows.net. Een aangepast domein kan ook worden ingevoerd. Bijvoorbeeld: als uw domein <em>contoso is</em>, kunt u invoeren: <br/><br/>
    <em>Mijnbedrijf</em>. contoso.com<br/>
    <em>MyCompanyMyApplication</em>. contoso.com<br/>
    <em>MyApplication staat</em>. contoso.com<br/>
    <em>YourBizTalkServiceName</em>. contoso.com<br/>
    </td>
    </tr>
    </table>
Selecteer de pijl-rechts.

5. Voer de opslag en instellingen Database:

    <table border="1">
    <tr>
    <td><strong>Cmdlets voor controle/archivering opslag-account</strong></td>
    <td>Selecteer een bestaande opslag-account of maak een nieuw account voor de opslag. <br/><br/>Als u een nieuw opslag-account hebt gemaakt, voert u de <strong>Opslagaccountnaam</strong>.</td>
    </tr>
    <tr>
    <td><strong>De database bijhouden</strong></td>
    <td>Als u een bestaande Azure SQL-Database gebruikt, kan deze kan niet worden gebruikt door een andere BizTalk-Service. Moet u eerst de aanmeldingsnaam en wachtwoord ingevoerd wanneer dat Azure SQL Database-Server is gemaakt.<br/><br/><strong>TIP</strong> De database bijhouden en controle/archivering opslag-account maken in hetzelfde gebied, als de BizTalk-Service.</td>
    </tr>
    </table>
Selecteer de pijl-rechts.

6. Voer in de Database-instellingen:

    <table border="1">
    <tr>
    <td><strong>Naam</strong></td>
    <td>Beschikbaar wanneer <strong>een nieuw exemplaar van de SQL-Database maken</strong> is geselecteerd in het vorige scherm.
    <br/><br/>
   Voer de naam van een SQL-Database moet worden gebruikt door uw BizTalk-Service.</td>
    </tr>
    <tr>
    <td><strong>Server</strong></td>
    <td>Beschikbaar wanneer <strong>een nieuw exemplaar van de SQL-Database maken</strong> is geselecteerd in het vorige scherm.
    <br/><br/>
   Selecteer een bestaande SQL-databaseserver of maak een nieuwe SQL Database-server.</td>
    </tr>
    <tr>
    <td><strong>Server-aanmeldingsnaam</strong></td>
    <td>Voer de naam van de gebruiker login.</td>
    </tr>
    <tr>
    <td><strong>Wachtwoord voor aanmelden bij Server</strong></td>
    <td>Voer het wachtwoord voor aanmelden.</td>
    </tr>
    <tr>
    <td><strong>Regio</strong></td>
    <td>Beschikbaar wanneer <strong>een nieuw exemplaar van de SQL-Database maken</strong> is geselecteerd. Selecteer het geografische gebied voor het hosten van uw SQL-Database.</td>
    </tr>
    </table>

Selecteer het vinkje om de wizard te voltooien. Het pictogram voortgang wordt weergegeven:  
![Pictogram voortgang wordt weergegeven wanneer u voltooid][ProgressComplete]

Als alles compleet is, wordt de Azure BizTalk-Service is gemaakt en is klaar voor uw toepassingen. De standaardinstellingen zijn geschikt. Als u wijzigen van de standaardinstellingen wilt, **BIZTALK SERVICES** selecteren in het linker navigatiedeelvenster en selecteer vervolgens uw BizTalk-Service. Aanvullende instellingen worden weergegeven in de [tabbladen Dashboard, Monitor, en schaal](biztalk-dashboard-monitor-scale-tabs.md) aan de bovenkant.

Afhankelijk van de status van de BizTalk Service, zijn er enkele bewerkingen die kunnen niet worden voltooid. Ga naar [BizTalk Services staat grafiek](biztalk-service-state-chart.md)voor een lijst van deze bewerkingen.


## <a name="post-provisioning-steps"></a>Na inrichting stappen

-  [Het certificaat installeren op een lokale computer](#InstallCert)
-  [Een certificaat productie gereed toevoegen](#AddCert)
-  [De naamruimte toegangsbeheer ophalen](#ACS)

#### <a name="InstallCert"></a>Het certificaat installeren op een lokale computer
Als onderdeel van BizTalk Service inrichting, is een zelfondertekend certificaat gemaakt en dat is gekoppeld aan uw abonnement BizTalk-Service. U moet dit certificaat downloaden en installeren op computers waarop u BizTalk Service-toepassingen implementeren of berichten verzenden naar een BizTalk Service-eindpunt.

1. Meld u aan bij de [portal van Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. **BIZTALK SERVICES** selecteren in het linker navigatiedeelvenster en selecteer vervolgens uw abonnement BizTalk Service.
3. Selecteer het tabblad **Dashboard** .
4. Selecteer **SSL-certificaat te downloaden**:  
![SSL-certificaat wijzigen][QuickGlance]
5. Dubbelklik op het certificaat en uitvoeren met de wizard het certificaat te installeren. Zorg ervoor dat u het certificaat onder de **Hoofdmap certificeringsinstanties vertrouwde** store installeren.

#### <a name="AddCert"></a>Een certificaat productie gereed toevoegen
Het zelfondertekend certificaat dat automatisch gemaakt wordt wanneer BizTalk Services maken is bedoeld voor gebruik in alleen development-omgevingen. Voor productie scenario's, kunt u het vervangen door een certificaat productie gereed.

1. Selecteer op het tabblad **Dashboard** **Update SSL-certificaat**.
2. Blader naar uw persoonlijke SSL-certificaat (*CertificateName*.pfx) waarin de naam van uw BizTalk Service, voert u het wachtwoord en klik vervolgens op het vinkje.

#### <a name="ACS"></a>De naamruimte toegangsbeheer ophalen

1. Meld u aan bij de [portal van Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. **BIZTALK SERVICES** selecteren in het linker navigatiedeelvenster en selecteer vervolgens uw BizTalk-Service.
3. Selecteer in de taakbalk **Verbindingsgegevens**:  
![Selecteer verbindingsgegevens][ACSConnectInfo]

4. Kopieer de waarden toegangsbeheer.

Wanneer u een project BizTalk Service uit Visual Studio implementeert, voert u deze naamruimte beheren in Access. De naamruimte toegangsbeheer wordt automatisch gemaakt voor uw BizTalk-Service.

De waarden toegangsbeheer kunnen worden gebruikt met een toepassing. Als Azure BizTalk Services is gemaakt, controleert deze naamruimte toegangsbeheer de verificatie met uw BizTalk Service-implementatie. Als u wilt wijzigen van het abonnement of het beheren van de naamruimte, het **ACTIVE DIRECTORY** selecteren in het linker navigatiedeelvenster en selecteer vervolgens de naamruimte. De taakbalk lijsten de gewenste opties.

Op **beheren** te klikken, wordt de beheerportal van de Access-besturingselement geopend. De BizTalk Service gebruikt in de Portal Access Control Management **Service-identiteiten**:  
![Identiteiten ACS-Service in de Access Control Management Portal][ACSServiceIdentities]

De identiteit van de service toegangsbeheer is een set referenties waarmee toepassingen en -clients verificatie rechtstreeks met toegangsbeheer en ontvangen van een token.

> [AZURE.IMPORTANT]**De eigenaar van** de BizTalk-Service gebruikt voor de standaardidentiteit service en de waarde **wachtwoord** . Als u de Symmetric sleutelwaarde in plaats van de waarde wachtwoord gebruikt, wordt de volgende fout kan optreden.<br/><br/>*Kan geen verbinding met het Access Control Management Service-account met de opgegeven referenties*

[Uw ACS Namespace beheren](https://msdn.microsoft.com/library/azure/hh674478.aspx) bevat een aantal richtlijnen en aanbevelingen.

## <a name="requirements-explained"></a>Uitleg over vereisten

Deze vereisten is voldaan, worden niet toegepast op de gratis versie.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Wat u nodig hebt</strong></td>
        <td><strong>Waarom u deze nodig hebt</strong></td>
</tr>
<tr>
<td>Azure-abonnement</td>
<td>Het abonnement bepaalt wie kan aanmelden bij de portal van Azure. De eigenaar van een Account maakt het abonnement op <a HREF="https://account.windowsazure.com/Subscriptions">Azure-abonnementen</a>.
<br/><br/>
De Azure-account kan bevatten meerdere abonnementen en kan worden beheerd door iedereen die is toegestaan. Bijvoorbeeld uw Azure accounts Hiermee maakt u een abonnement met de naam <em>BizTalkServiceSubscription</em> en geeft het beheerders BizTalk binnen uw bedrijf (bijvoorbeeld ContosoBTSAdmins@live.com) toegang tot dit abonnement. In dit scenario de beheerders BizTalk Meld u aan bij de portal van Azure en beheerdersrechten volledige voor de gehoste services in het abonnement, met inbegrip van Azure BizTalk-Services. De BizTalk-beheerders zijn niet de houders Azure-account en kunnen daarom hebben geen toegang tot alle informatie over facturering.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Abonnementen beheren en opslag-Accounts in de Azure-portal</a> vindt u meer informatie.
</td>
</tr>
<tr>
<td>Azure SQL-Database</td>
<td>De tabellen, weergaven en opgeslagen procedures die worden gebruikt door de BizTalk-Service, met inbegrip van de gegevens bijhouden worden opgeslagen.
<br/><br/>
Wanneer u een BizTalk Service maakt, kunt u een bestaande Azure SQL Server, Azure SQL-Database, gebruiken of automatisch een nieuwe Server of -Database maken.
<br/><br/>
De schaal van de SQL-Database wordt automatisch geconfigureerd. De schaal van de standaardinstelling is meestal voldoende voor een BizTalk-Service. De schaal wijzigen van invloed op prijzen. Zie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">Accounts en facturering in Azure SQL-Database</a>
<br/><br/>
<strong>Notities</strong>
<br/>
<ul>
<li> Wanneer u een nieuwe Azure SQL Server en Database maakt, wordt automatisch Azure Services ingeschakeld. De BizTalk-Service is vereist dat Azure Services zijn ingeschakeld.</li>
<li>Als u een nieuwe Azure SQL-Database op een bestaande Azure SQL-Server maakt, worden de firewallregels van de Server niet gewijzigd. Hierdoor is het mogelijk dat andere Azure-Services zijn niet toegestaan voor toegang tot van de Server-databases.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure toegangsbeheer naamruimte</td>
<td>Verifieert met Azure BizTalk-Services. Wanneer u een project BizTalk Service uit Visual Studio implementeert, voert u deze naamruimte beheren in Access. Wanneer u een BizTalk Service maakt, wordt automatisch de naamruimte toegangsbeheer gemaakt.</td>
</tr>

<tr>
<td>Azure opslag-account</td>
<td>Toegang krijgen tot tabellen, BLOB's en wachtrijen wordt gebruikt door uw BizTalk-Service voor het opslaan van de volgende handelingen uit:

<ul>
<li>De logboekbestanden die door de BizTalk Service worden gecontroleerd. De controle uitvoer wordt ook weergegeven op het tabblad **controleren** in de portal van Azure.</li>
<li>Wanneer u maakt een X12 of AS2 overeenkomst tussen de partners, kunt u de functie archiveren om op te slaan berichteigenschappen inschakelen. Deze gegevens zijn opgeslagen in de opslagruimte-account.</li>
</ul>
<br/>
Wanneer u een BizTalk Service maakt, kunt u een bestaand account voor de opslag gebruiken of automatisch maken een nieuw account voor de opslag.
<br/><br/>
De standaardinstellingen voor de opslag zijn geschikt voor een BizTalk-Service.
<br/><br/>
Wanneer u een account opslag maakt, worden automatisch een primaire sleutel en de tweede sleutel gemaakt. Deze toetsen beheren toegang tot uw account opslag. De BizTalk-Service automatisch de primaire sleutel worden gebruikt.
<br/><br/>
Zie de <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">opslagruimte</a> voor meer informatie.
</td>
</tr>

<tr>
<td>Privé SSL-certificaat</td>
<td>
Wanneer een Azure BizTalk-Service is gemaakt, wordt ook een HTTPS-URL die de naam van uw BizTalk Service bevat gemaakt. Deze URL wordt automatisch geconfigureerd voor het gebruik van een zelfondertekend certificaat voor de ontwikkeling alleen-lezen. Voor productie moet u een persoonlijke SSL-certificaat.
<br/><br/>
<strong>Gegevens van belangrijke SSL-certificaat</strong>

<ul>
<li>De vervaldatum van het certificaat moet minder dan 5 jaar.</li>
<li>Alle persoonlijke certificaten vereisen een wachtwoord. Dit wachtwoord kent en een aanbevolen om dit wachtwoord met uw beheerders te delen.</li>
<li>Zelfondertekende certificaten worden gebruikt in een test/ontwikkelomgeving. Wanneer u een zelfondertekende certificaten gebruikt, kunt u het certificaat importeren op uw persoonlijke archief en de hoofdmap certificeringsinstanties vertrouwde certificaat-winkel.</li>
</ul>
<br/>Wanneer u de aanvraag productie verzendt naar uw Certificate authority, geven de volgende certificaateigenschappen:
<br/>

<ul>
<li><strong>Enhanced-toets gebruik</strong>: ten minste Azure BizTalk Services Server-verificatie is vereist.</li>
<li><strong>De naam van de algemene</strong>: Typ de FQDN-naam (Fully Qualified Domain Name) van de Azure BizTalk-Service-URL. Zie <a HREF="#BizTalk">maken een BizTalk-Service</a> in dit artikel.</li>
</ul>
<br/>
Een nieuwe of andere certificaat kan worden toegevoegd nadat de BizTalk-Service is gemaakt.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hybride verbindingen

Wanneer u een Azure-BizTalk Service maakt, is het tabblad **Verbindingen hybride** beschikbaar:

![Tabblad voor hybride-verbindingen][HybridConnectionTab]

Hybride verbindingen worden gebruikt om verbinding te maken van een Azure website of Azure mobiele service alle lokale bronnen die gebruikmaakt van een statische TCP-poorten, zoals SQL Server, MySQL HTTP-Web-API's, mobiele Services en de meeste aangepaste Web-Services.  Hybride verbindingen en de BizTalk Adapter Service verschillen. De BizTalk Adapter-Service wordt gebruikt om te Azure BizTalk Services verbinden met een on-premises lijn van Business (LOB)-systeem.

 Zie [Hybride verbindingen](integration-hybrid-connection-overview.md) voor meer informatie, inclusief maken en beheren van hybride verbindingen.


## <a name="next-steps"></a>Volgende stappen

Nu dat een BizTalk-Service is gemaakt, Maak uzelf vertrouwd met de verschillende [BizTalk Services: tabbladen Dashboard, Monitor en schaal](biztalk-dashboard-monitor-scale-tabs.md). Uw BizTalk Service is gereed voor uw toepassingen. Als u wilt beginnen met het maken van toepassingen, gaat u naar [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Zie ook
- [BizTalk Services: Edities grafiek](biztalk-editions-feature-chart.md)<br/>
- [BizTalk Services: Provinciale grafiek](biztalk-service-state-chart.md)<br/>
- [BizTalk Services: Back-up maken en deze herstellen](biztalk-backup-restore.md)<br/>
- [BizTalk-Services: beperken](biztalk-throttling-thresholds.md)<br/>
- [BizTalk Services: De naam van de uitgever en de uitgever sleutel](biztalk-issuer-name-issuer-key.md)<br/>
- [Hoe kan ik gebruiken de SDK van Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hybride verbindingen](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
