<properties
pageTitle="Gebruik van gegevens uit een lokale SQL Server-database in Machine Learning | Azure"
description="Gegevens uit een lokale SQL Server-database gebruiken om het uitvoeren van geavanceerde analyses met Azure Machine Learning."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Geavanceerde analyses uitvoeren met Azure Machine Learning gebruikmaken van gegevens uit een lokale SQL Server-database


Vaak ondernemingen die met on-premises gegevens werken wilt profiteren van de schaal en flexibiliteit van de cloud voor hun machine learning-werkbelasting meenemen. Maar deze niet wilt hun huidige bedrijfsprocessen en werkstromen verstoort door hun on-premises gegevens verplaatsen naar de cloud. Azure Machine Learning ondersteunt nu het lezen van uw gegevens uit een lokale SQL Server-database en klik vervolgens training en scoren een model met deze gegevens. U hoeft niet langer handmatig kopiëren en de gegevens tussen de cloud en uw on-premises-server gesynchroniseerd. De module **Gegevens importeren** in Azure Machine Learning Studio kunt in plaats daarvan nu lezen rechtstreeks vanuit uw lokale SQL Server-database voor uw training en scoren taken. 

Dit artikel bevat een overzicht van hoe u ingress lokale SQL server-gegevens in Azure Machine Learning. Het wordt ervan uitgegaan dat u bekend met Azure Machine Learning-concepten zoals werkruimten, modules, gegevenssets, experimenten, *enzovoort bent*...

> [AZURE.NOTE] Deze functie is niet beschikbaar voor gratis werkruimten. Zie [Azure Machine Learning prijzen](https://azure.microsoft.com/pricing/details/machine-learning/)voor meer informatie over Machine Learning prijzen en lagen.

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>De Microsoft Data Management Gateway installeren

Toegang tot een lokale SQL Server-database in Azure Machine Learning die u wilt downloaden en installeren van de Microsoft Data Management Gateway. Wanneer u de gateway-verbinding in Machine Learning Studio configureert hebt u de mogelijkheid om te downloaden en installeren van de gateway met het **downloaden en register gegevensgateway** dialoogvenster hieronder beschreven.

U kunt ook de Data Management Gateway tijd vooraf installeren door te downloaden en uitvoeren van de MSI-installatiepakket van het [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=39717). Kies de meest recente versie, 32-bits of 64-bits voor uw computer te selecteren. Het MSI-bestand kan ook worden gebruikt om te upgraden van een bestaande Data Management Gateway naar de nieuwste versie, met alle instellingen behouden.

De gateway heeft de volgende vereisten:

- De ondersteunde versies van de Windows-besturingssysteem zijn Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.
- De aanbevolen configuratie voor de gatewaycomputer is ten minste 2 GHz, 4 cores 8 GB RAM en 80 GB schijf.
- Als de host in slaapstand, kunnen de gateway om te reageren op gegevensaanvragen niet mogelijk. Een juiste energiebeheerschema op de computer daarom configureren voordat u de gateway installeert. De gateway-installatie, wordt er een bericht weergegeven als de computer is geconfigureerd slaapstand.
- Omdat kopiëren activiteit aan de frequentie van een bepaalde plaatsvindt, wordt hetzelfde patroon met piek en niet-actieve tijden ook gevolgd door de weergave Resourcegebruik (CPU, geheugen) op de computer. Resourcegebruik is ook intensief afhankelijk van de hoeveelheid gegevens die wordt verplaatst. Wanneer meerdere kopie taken uitgevoerd worden gaat u omhoog gaan piek momenten Resourcegebruik toekijken. Terwijl de minimale configuratie bovenstaande technisch voldoende is, wilt u mogelijk een configuratie met meer bronnen dan de minimale configuratie afhankelijk van uw specifieke belasting voor verplaatsing van gegevens.

De volgende handelingen uit bij het instellen en gebruiken van een Data Management Gateway, moet u overwegen:

- U kunt slechts één exemplaar van Data Management Gateway installeren op één computer.
- U kunt één gateway gebruiken voor meerdere on-premises implementatie-gegevensbronnen.
- U kunt meerdere gateways op verschillende computers verbinden met dezelfde lokale gegevensbron.
- U kunt een gateway voor slechts één werkruimte configureren tegelijk. Gateways kunnen niet worden gedeeld met werkruimten op dit moment.
- U kunt meerdere gateways voor een enkele werkruimte configureren. U wilt bijvoorbeeld een gateway die gekoppeld aan uw gegevensbronnen testen tijdens de ontwikkeling en een gateway productie gebruiken wanneer u klaar bent om te mogelijk maken.
- De gateway niet hoeft te worden op dezelfde computer als de gegevensbron, maar blijven dichter bij de gegevensbron Hiermee reduceert u de tijd voor de gateway bij naar de verbinding maken met de gegevensbron. Het is raadzaam dat u de gateway op een computer die verschilt van degene die als host de on-premises gegevensbron installeren fungeert zodat de gateway en de gegevensbron niet geconfigureerd voor resources.
- Als u al een gateway is geïnstalleerd op uw computer voor Power BI of Azure gegevens Factory-scenario's, installeert u een aparte gateway voor Azure Machine Learning op een andere computer. 

    > [AZURE.NOTE] U kunt Data Management Gateway en Power BI Gateway niet uitvoeren op dezelfde computer.

- U moet de Data Management Gateway voor Azure Machine Learning gebruiken, zelfs als u van Azure ExpressRoute voor andere gegevens gebruikmaakt. Uw gegevensbron moet worden beschouwd als een lokale gegevensbron (die zich achter een firewall) zelfs wanneer u gebruik ExpressRoute en gebruik de Data Management Gateway tot stand brengen van de connectiviteit tussen Machine Learning en de gegevensbron. 

U vindt gedetailleerde informatie over installatievereisten, installatiestappen en tips voor probleemoplossing in het artikel [gegevens tussen on-premises bronnen en cloud met Data Management Gateway verplaatsen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway), beginnend met de sectie [Overwegingen bij het gebruik van Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Ingress gegevens uit uw lokale SQL Server-database naar Azure Machine Learning


In dit scenario wordt u een Data Management Gateway instellen in een werkruimte Azure Machine Learning, configureert u deze en klikt u vervolgens gegevens uit een lokale SQL Server-database te lezen.

> [AZURE.TIP] Voordat u begint, uitschakelen van uw browser pop-upblokkering voor `studio.azureml.net`. Als u de Google Chrome-browser gebruikt, downloaden en installeren op een van de verschillende plug-ins beschikbaar bij Google Chrome webwinkel [Eenmaal extensie App klikt u op](https://chrome.google.com/webstore/search/clickonce?_category=extensions).

### <a name="step-1-create-a-gateway"></a>Stap 1: Een gateway maken

De eerste stap is om te maken en de gateway instellen voor toegang tot uw on-premises implementatie SQL-database.

1. Meld u aan [Azure Machine Learning Studio](https://studio.azureml.net/Home/) en selecteer de werkruimte die u werken wilt in.

2. Klik op het blad **Instellingen** aan de linkerkant en klik vervolgens op het tabblad **GEGEVENSGATEWAYS** aan de bovenkant.

3. Klik op **Nieuwe GEGEVENSGATEWAY** onderaan in het scherm.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. Voer de **Naam van de Gateway** in het dialoogvenster **nieuwe gegevensgateway** en desgewenst een **Beschrijving**toevoegen. Klik op de pijl in de rechterbenedenhoek om te gaan naar de volgende stap van de configuratie.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. Klik in het downloaden en register gegevens gateway dialoogvenster de GATEWAY-sleutel voor registratie naar het Klembord te kopiëren.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Als u nog niet gedownload en geïnstalleerd de Microsoft Data Management Gateway, klikt u op **downloaden van data management gateway**. Hiermee gaat u naar het Microsoft Download Center waar u kunt selecteren de gatewayversie die u nodig hebt, kunt downloaden en installeren. U vindt meer informatie over installatievereisten, installatiestappen en tips voor probleemoplossing in de secties van het begin van het artikel [gegevens tussen on-premises bronnen en cloud met Data Management Gateway verplaatsen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

7. Nadat de gateway is geïnstalleerd, wordt de Data Management Gateway Configuration Manager wordt geopend en wordt het dialoogvenster **registreren gateway** wordt weergegeven. De **Gateway registratiegegevens sleutel** die u hebt gekopieerd naar het Klembord plakken en klikt u op **registreren**.

8. Als u al een gateway is geïnstalleerd, de Data Management Gateway Configuration Manager uitvoeren, klikt u op **de toets wijzigen**, de  **Registratie-Gateway-sleutel** die u hebt gekopieerd naar het Klembord plakken en klik op **OK**.

9. Wanneer de installatie voltooid is, wordt het dialoogvenster **gateway hebt geregistreerd** voor Microsoft Data Management Gateway Configuration Manager wordt weergegeven. Plak de GATEWAY REGISTRATIEGEGEVENS sleutel die u hebt gekopieerd naar het Klembord bovenstaande en klikt u op **registreren**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. De gatewayconfiguratie is voltooid wanneer de volgende waarden zijn ingesteld op het tabblad **Start** in Microsoft Data Management Gateway Configuration Manager:

    - **De naam van de gateway** en de **exemplaarnaam** zijn ingesteld op de naam van de gateway.

    - **Registratie** is ingesteld op **geregistreerd**.

    - **Status** is ingesteld op **gestart**.

    - De statusbalk onder worden **verbonden met de Data Management Gateway-Cloudservice** samen met een groen vinkje weergegeven.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure Machine Learning Studio wordt ook bijgewerkt als de registratie gelukt is.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. Klik op het vinkje om de installatie te voltooien in het dialoogvenster **downloaden en gegevensgateway te registreren** . De pagina **Instellingen** geeft de status van gateway als "Online". In het rechterdeelvenster vindt u de status en andere nuttige informatie.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. Klik in het Microsoft Data Management Gateway Configuration Manager-overschakelen naar het tabblad **certificaat** . Het certificaat dat is opgegeven op dit tabblad wordt gebruikt voor referenties voor de on-premises implementatie gegevensopslag die u in de portal opgeeft versleutelen/ontsleutelen. Dit is de standaardcertificaat dat is gegenereerd. U wordt aangeraden wijzigen in uw eigen certificaat dat u in het certificaat management bestandssysteem back-up. Klik op **wijzigen** als u wilt uw eigen certificaat in plaats daarvan gebruiken.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (optioneel) Als u uitgebreide logboekregistratie wilt om oplossen van problemen met de gateway inschakelen, wordt in de Microsoft Data Management Gateway Configuration Manager Ga naar het tabblad **hulpprogramma's voor diagnose** en schakelt u de optie **logboekregistratie voor probleemoplossing inschakelen** . De logboekinformatie vindt u in de Windows-Logboeken onder de **Logboeken toepassingen en Services**  - &gt; **Data Management Gateway** knooppunt. U kunt ook het tabblad **Diagnostische gegevens** gebruiken om de verbinding met een lokale gegevensbron met de gateway te testen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

Dit is de gateway proces in Azure Machine Learning instellen voltooid.
U bent nu klaar voor gebruik van uw on-premises gegevens.

U kunt maken en instellen van meerdere gateways in Studio voor elke werkruimte. Zoals wellicht u een gateway die u wilt verbinden met uw gegevensbronnen testen tijdens de ontwikkeling, en een andere gateway voor uw gegevensbronnen productie. Azure Machine Learning hebt u de flexibiliteit voor het instellen van meerdere gateways, afhankelijk van uw bedrijfsomgeving gebruikt. Op dit moment u kan geen gateway tussen werkruimten delen en slechts één gateway kan worden geïnstalleerd op één computer. Zie voor meer overwegingen bij het installeren van de gateway [Overwegingen bij het gebruik van Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) in het artikel [gegevens tussen on-premises bronnen en cloud met Data Management Gateway verplaatsen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Stap 2: De gateway gebruiken om te lezen van gegevens uit een lokale gegevensbron

Nadat u de gateway hebt ingesteld, kunt u een module **Gegevens importeren** kunt toevoegen aan een experiment die de gegevens van de on-premises implementatie SQL Server-database-ingangen.

1.  Machine Learning Studio, selecteer het tabblad **EXPERIMENTEN** , klikt u op **+ Nieuw** in de linkerbenedenhoek en selecteer **Lege Experiment** (of Selecteer een van de verschillende steekproeven experimenten beschikbaar).

2.  Zoek en sleept u de module **Gegevens importeren** naar het tekenpapier experiment.

3.  Klik op **Opslaan als** onder het tekenpapier. Voer "Azure Machine Learning On-Premises SQL Server zelfstudie' voor de naam experiment, selecteert u de werkruimte en klik op het vinkje **OK** .

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  Klik op de module **Gegevens importeren** om deze te selecteren en klik in het deelvenster **Eigenschappen** aan de rechterkant van het tekenpapier, selecteert u 'On-Premises SQL-Database' in de vervolgkeuzelijst van de **gegevensbron** .

5.  Selecteer de **gegevensgateway** u geïnstalleerd en geregistreerd. U kunt een andere gateway instellen door in te schakelen "(nieuwe gegevensgateway... toevoegen)".

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Voer de SQL **server-databasenaam** en de **naam van de Database**, samen met de SQL- **databasequery's** u wilt uitvoeren.

7.  Klik op **Enter waarden** onder **gebruikersnaam en wachtwoord** en voer de databasereferenties van uw. U kunt Windows-verificatie of SQL Server-verificatie is afhankelijk van hoe uw on-premises implementatie SQL Server is geconfigureerd.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Het bericht 'waarden vereist' wordt gewijzigd in "waarden instellen" met een groen vinkje. U moet alleen Voer de referenties eenmaal tenzij de databasegegevens of het wachtwoord wordt gewijzigd. Azure Machine Learning wordt gebruikt voor het certificaat dat u hebt opgegeven toen u de gateway bij naar de referenties in de cloud versleutelen hebt geïnstalleerd. Azure wordt nooit store-referenties van de on-premises implementatie zonder codering.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Klik op **uitvoeren** om uit te voeren het experiment.

Zodra het experiment is voltooid kunt waarop u de gegevens die u hebt geïmporteerd uit de database door te klikken op de uitvoerpoort van de module **Gegevens importeren** en te selecteren **visualiseren**visualiseren.

Wanneer u klaar bent met het ontwikkelen van uw experiment, kunt u deze kunt implementeren en uw model mogelijk te maken. Gebruik van de Batch Execution Service wordt gegevens uit de lokale SQL Server-database geconfigureerd in de module **Gegevens importeren** lezen en gebruikt voor het scoren. U kunt de reactie aanvragen Service gebruiken voor het on-premises gegevens scoren, Microsoft raadt de [Excel-invoegtoepassing](machine-learning-excel-add-in-for-web-services.md) in plaats daarvan. Op dit moment wordt schrijven met een lokale SQL Server-database via **Gegevens exporteren** niet ondersteund in uw experimenten of gepubliceerde webservices.

