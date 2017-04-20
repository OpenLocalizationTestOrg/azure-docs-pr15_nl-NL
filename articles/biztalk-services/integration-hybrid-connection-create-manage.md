<properties 
    pageTitle="Hybride verbindingen maken en beheren | Microsoft Azure" 
    description="Informatie over het maken van een hybride verbinding, de verbinding beheren en installeren van de hybride Connection Manager. MAB, WABS" 
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
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Hybride verbindingen maken en beheren


## <a name="overview-of-the-steps"></a>Overzicht van de stappen
1. Een hybride-verbinding maken met het invoeren van de **hostnaam** of **FQDN** van de resource on-premises implementatie in uw privé-netwerk.
2. Uw Azure-WebApps of Azure mobiele apps koppelen aan de hybride verbinding.
3. De hybride Connection Manager installeren op uw lokale resource en verbinding maken met de specifieke hybride verbinding. De Azure portal biedt een ervaring met één klik als u wilt installeren en verbinding te maken.
4. Hybride verbindingen en de verbindingssleutels beheren.

In dit onderwerp vindt u deze stappen. 

> [AZURE.IMPORTANT] Het is mogelijk een hybride verbinding-eindpunt instellen op een IP-adres. Als u een IP-adres hebt gebruikt, kunt u mogelijk of de resource on-premises implementatie, mogelijk niet heeft bereikt afhankelijk van de klant. De verbinding hybride, is afhankelijk van de client een DNS-zoekopdracht doen. De __client__ is in de meeste gevallen de toepassingscode van uw. Als de client een DNS-zoekopdracht, geen voert (deze probeert niet voor het oplossen van het IP-adres alsof het een domeinnaam (x.x.x.x)), en vervolgens verkeer niet via de hybride verbinding verzonden wordt.
>
> Bijvoorbeeld (pseudocode), kunt u **10.4.5.6** definiëren als uw on-premises implementatie-host:
> 
> **De volgende scenario werkt:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **De volgende scenario werkt niet:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Een hybride verbinding maken

De verbinding van een hybride kan worden gemaakt in de Azure-portal met Web Apps **of** met behulp van BizTalk-Services. 

**Hybride verbindingen met Web Apps maken**, raadpleegt u [Azure-WebApps koppelen aan een Resource On-Premises](../app-service-web/web-sites-hybrid-connection-get-started.md). U kunt ook de hybride verbinding Manager (HCM) installeren vanaf uw web-app, dat wil de voorkeursmethode zeggen. 

**Hybride verbindingen in BizTalk-Services te maken**:

1. Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. In het linkernavigatiedeelvenster **BizTalk Services** selecteren en selecteer uw BizTalk-Service. 

    Als u een bestaande BizTalk-Service niet hebt, kunt u [maken een BizTalk-Service](biztalk-provision-services.md).
3. Selecteer het tabblad **Verbindingen hybride** :  
![Tabblad voor hybride-verbindingen][HybridConnectionTab]

4. Selecteer **een hybride verbinding maken** of Selecteer de knop **toevoegen** op de taakbalk. Voer de volgende in:

    Eigenschap | Beschrijving
--- | ---
Naam | De naam van de hybride verbinding moet uniek zijn en mogen niet dezelfde naam als de BizTalk-Service. U kunt elke naam invoeren maar specifiek met het doel. Voorbeelden hiervan zijn:<br/><br/>Salarisadministratie*SQL Server*<br/>SupplyList*SharepointServer*<br/>Klanten*OracleServer*
Host Name | Voer de volledig gekwalificeerde hostnaam, alleen de hostnaam of het IPv4-adres van de resource on-premises implementatie. Voorbeelden hiervan zijn:<br/><br/>mySQLServer<br/>*mySQLServer*. *Domein*. corp.*uw bedrijf*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *uw bedrijf*.com<br/>10.100.10.10<br/><br/>Als u het IPv4-adres hebt gebruikt, houd er rekening mee dat uw code client of een toepassing niet is verholpen het IP-adres nadat mogelijk. Zie de opmerking aan het begin van dit onderwerp belangrijk.
Poort | Voer het poortnummer van de resource on-premises implementatie. Als u de Web Apps gebruikt, voert u bijvoorbeeld poort 80 of poort 443. Als u SQL Server gebruikt, voert u poort 1433.

5. Selecteer het selectievakje de installatie te voltooien. 

#### <a name="additional"></a>Extra

- Meerdere hybride verbindingen kunnen worden gemaakt. Zie de [BizTalk Services: edities grafiek](biztalk-editions-feature-chart.md) voor het aantal verbindingen die zijn toegestaan. 
- Elke hybride verbinding wordt gemaakt met een paar verbindingstekenreeksen: toepassing pijltoetsen die verzenden en On-premises toetsen die LUISTEREN. Elk paar heeft een primaire en secundaire sleutel. 


## <a name="LinkWebSite"></a>Uw Azure App-Service in de browser of mobiele App koppelen

Als u wilt een Web App of de Mobile-App in Azure App Service koppelen aan een bestaande verbinding van de hybride, selecteert u **een bestaande verbinding van de hybride gebruiken** in het blad hybride verbindingen. Zie [toegang tot lokale bronnen van hybride-verbindingen in Azure App-Service](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Installeren van de hybride Connection Manager on-premises

Nadat de verbinding van een hybride is gemaakt, installeert u de hybride Connection Manager op de lokale bron. Worden kan gedownload vanaf uw Azure-WebApps of uw BizTalk-Service. BizTalk Services stappen: 

1. Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. In het linkernavigatiedeelvenster **BizTalk Services** selecteren en selecteer uw BizTalk-Service. 
3. Selecteer het tabblad **Verbindingen hybride** :  
![Tabblad voor hybride-verbindingen][HybridConnectionTab]
4. Selecteer in de taakbalk, **On-Premises installatie**:  
![On-Premises-instelling][HCOnPremSetup]
5. Selecteer **installeren en configureren** wilt uitvoeren of downloaden van de hybride Connection Manager op de on-premises-systeem. 
6. Selecteer het vinkje om de installatie te starten. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Extra
- Hybride Connection Manager kan worden geïnstalleerd op de volgende besturingssystemen:

    - Windows Server 2008 R2 (.NET Framework 4.5 + en Windows Management Framework 4.0 + vereist)
    - Windows Server 2012 (Windows Management Framework 4.0 + vereist)
    - Windows Server 2012 R2


- Nadat u de hybride Connection Manager hebt geïnstalleerd, gebeurt het volgende: 

    - De hybride verbinding die worden gehost op Azure wordt automatisch geconfigureerd voor het gebruik van de primaire-toepassing-verbindingsreeks. 
    - De On-Premises resource wordt automatisch geconfigureerd voor het gebruik van de primaire On-Premises-verbindingsreeks.

- De hybride Connection Manager moet een geldige on-premises implementatie-verbindingsreeks gebruiken voor autorisatie. De Azure Web Apps of Mobile-Apps moet een verbindingsreeks geldige-toepassing voor autorisatie gebruiken.
- U kunt hybride verbindingen schalen door een ander exemplaar van de hybride Connection Manager op een andere server installeren. Configureer de on-premises implementatie luisteraar ervan af om hetzelfde adres gebruiken als de eerste on-premises implementatie luisteraar ervan af. In dit geval wordt het verkeer willekeurig verdeelde (round robin) tussen de lokale active listeners. 


## <a name="ManageHybridConnection"></a>Hybride verbindingen beheren
Als u wilt uw hybride verbindingen beheren, kunt u het volgende doen:

- Gebruik van de Azure-portal en gaat u naar uw BizTalk-Service. 
- Gebruik [REST API's](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopie/opnieuw genereren de hybride verbindingstekenreeksen

1. Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. In het linkernavigatiedeelvenster **BizTalk Services** selecteren en selecteer uw BizTalk-Service. 
3. Selecteer het tabblad **Verbindingen hybride** :  
![Tabblad voor hybride-verbindingen][HybridConnectionTab]
4. Selecteer de hybride verbinding. Selecteer in de taakbalk, **Verbinding beheren**:  
![Opties beheren][HCManageConnection]

    **Verbinding beheren** bevat de toepassing en On-Premises verbindingstekenreeksen. U kunt de verbindingstekenreeksen kopiëren of genereren van de Access-toets gebruikt in de verbindingstekenreeks. 

    **Als u opnieuw genereren**, de toegangstoets gedeeld in de verbindingsreeks gebruikt is gewijzigd. Ga als volgt:
    - Selecteer in de portal voor Azure klassieke **Synchronisatie toetsen** in de toepassing Azure.
    - Voer de **On-Premises installatie**opnieuw uit. Wanneer u de On-premises implementatie-installatie opnieuw uitvoert, wordt de on-premises implementatie resource automatisch geconfigureerd voor het gebruik van de bijgewerkte primaire verbindingsreeks.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Gebruik Groepsbeleid om te bepalen de on-premises implementatie-resources die worden gebruikt door een hybride-verbinding

1. De [hybride Connection Manager administratieve sjablonen](http://www.microsoft.com/download/details.aspx?id=42963)downloaden.
2. Pak de bestanden.
3. Klik op de computer die groepsbeleid wijzigt, als volgt te werk:  

    - Kopie de. ADMX-bestanden naar de map *%WINROOT%\PolicyDefinitions* .
    - Kopie de. ADML-bestanden naar de map *%WINROOT%\PolicyDefinitions\en-us* .

Zodra u hebt gekopieerd, kunt u Groepsbeleid-Editor om het beleid te wijzigen.




## <a name="next"></a>Volgende

[Azure-WebApps verbinden met een On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Verbinding maken met SQL-Server on-premises implementatie van Azure WebApps](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Overzicht van de hybride-gegevensverbindingen](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Zie ook

[REST API voor het beheren van BizTalk-Services op Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Edities grafiek](biztalk-editions-feature-chart.md)  
[Een BizTalk-Service met behulp van Azure klassieke portal maken](biztalk-provision-services.md)  
[BizTalk Services: De tabbladen Dashboard, Monitor en schaal](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
