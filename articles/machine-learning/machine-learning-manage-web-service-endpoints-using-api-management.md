<properties
    pageTitle="Meer informatie over het beheren van AzureML webservices API te beheren | Microsoft Azure"
    description="Een hulplijn laat zien hoe u voor het beheren van AzureML webservices API te beheren."
    keywords="machine learning,-api management"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Meer informatie over het beheren van AzureML webservices API te beheren

##<a name="overview"></a>Overzicht

Deze handleiding ziet u hoe u snel aan de slag met API-beheer voor het beheren van uw AzureML-webservices.

##<a name="what-is-azure-api-management"></a>Wat is Azure API Management?

Azure API Management is een Azure-service waarmee u uw REST API-eindpunten beheren door de toegang van gebruikers, gebruik beperken en dashboard monitoring te definiëren. Klik [hier](https://azure.microsoft.com/services/api-management/) voor meer informatie over het beheer van Azure-API. Klik [hier](../api-management/api-management-get-started.md) voor instructies over het aan de slag met Azure API Management. Deze andere gids, dat deze handleiding is gebaseerd op, vindt meer onderwerpen, inclusief configuraties melding, laag prijzen, antwoord afhandeling, gebruikersverificatie, producten, ontwikkelaars abonnementen en gebruik dashboarding maken.

##<a name="what-is-azureml"></a>Wat is AzureML?

AzureML is een Azure-service voor machine learning-waarmee u kunt eenvoudig bouwen, en delen van geavanceerde analyses oplossingen. Klik [hier](https://azure.microsoft.com/services/machine-learning/) voor meer informatie over AzureML.

##<a name="prerequisites"></a>Vereisten voor

Als u wilt deze handleiding hebt voltooid, hebt u het volgende nodig:

* Een Azure-account. Als u geen een Azure-account hebt, klikt u op [hier](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie over het maken van een gratis proefabonnement-account.
* Een account AzureML. Als u niet een AzureML-account hebt, klikt u op [hier](https://studio.azureml.net/) voor meer informatie over het maken van een gratis proefabonnement-account.
* De werkruimte, service en api_key voor een AzureML experiment geïmplementeerd als een webservice. Klik [hier](machine-learning-create-experiment.md) voor meer informatie over het maken van een experiment AzureML. Klik [hier](machine-learning-publish-a-machine-learning-web-service.md) voor meer informatie over het implementeren van een experiment AzureML als een webservice. U kunt ook bevat bijlage A instructies voor het maken en testen van een eenvoudige AzureML experiment en deze te implementeren als een webservice.

##<a name="create-an-api-management-instance"></a>Een exemplaar API Management maken

Hieronder vindt u de stappen voor het gebruik van API-beheer voor het beheren van uw AzureML-webservice. Maak eerst een exemplaar van service. Meld u aan bij de [Portal van klassieke](https://manage.windowsazure.com/) en klik op **Nieuw** > **App Services** > **API Management** > **maken**.

![instantie maken](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Een unieke **URL**opgeeft. Deze handleiding wordt gebruikt voor **demoazureml** : u moet kiezen iets anders. Kies het gewenste **abonnement** en **regio** voor uw exemplaar van de service. Nadat u de gewenste opties, klikt u op de knop Volgende.

![Maak-service-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Geef een waarde voor de **Naam van organisatie**. Deze handleiding wordt gebruikt voor **demoazureml** : u moet kiezen iets anders. Voer uw e-mailadres in het veld **beheerder e-mail** . Deze e-mailadres wordt gebruikt voor meldingen voor het beheer van de API-systeem.

![Maak-service-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Klik op het selectievakje in als u wilt maken van uw exemplaar van de service. *Het duurt maximaal 30 minuten voor een nieuwe service moet worden gemaakt*.

##<a name="create-the-api"></a>De API maken

Nadat het exemplaar van de service is gemaakt, is de volgende stap het opzetten van de API. Een API bestaat uit een reeks bewerkingen die kan worden gestart door een clienttoepassing. API-bewerkingen zijn via proxy met bestaande-webservices. Deze handleiding maakt API's die proxy met de bestaande AzureML RRS en BES-webservices.

API's worden gemaakt en geconfigureerd vanaf de API publisher-portal, die wordt geopend met de klassieke Azure-Portal. Te bereiken de publisher-portal, selecteert u uw exemplaar van de service.

![Selecteer-service-exemplaar](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Klik op **beheren** in de klassieke Azure-Portal voor uw service API Management.

![beheren-service](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

**API's** in het menu **API beheer** aan de linkerkant op en klik vervolgens op **API toevoegen**.

![menu-beheer-API](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Typ **AzureML Demo API** als de **naam van de Web API**. Typ **https://ussouthcentral.services.azureml.net** als de **URL van de Web-service**. Typ **azureml-demo** als het **achtervoegsel van Web API-URL**. Controleer **HTTPS** als het schema **Web API-URL** . Selecteer **Starter** als **producten**. Wanneer u klaar bent, klikt u op **Opslaan** als u wilt maken van de API.

![toevoegen-nieuwe-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>De bewerkingen toevoegen

Klik op **Add-bewerking** om bewerkingen toevoegen aan deze API.

![toevoegen-bewerking](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Het venster **nieuwe bewerking** wordt weergegeven en wordt het tabblad **handtekening** al dan niet standaard geselecteerd.

##<a name="add-rrs-operation"></a>Records bewerking toevoegen

Maak eerst een bewerking voor de service AzureML RRS. Selecteer **posten** als de **HTTP-woord**. Type **/services/ /workspaces/ {werkruimte} {service} / execute?api-versie {apiversion} = & details = {details}** als de **URL van sjabloon**. Typ de **Records uitvoeren** als de **naam weer te geven**.

![records-bewerking-handtekening toevoegen](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Klik op **antwoorden** > **toevoegen** aan de linker- en selecteer **200 OK**. Klik op **Opslaan** om op te slaan deze bewerking.

![toevoegen-records-bewerking-antwoord](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>BES bewerkingen toevoegen

Schermafbeeldingen is niet inbegrepen voor de BES bewerkingen terwijl ze vergelijkbaar met de referenties zijn voor de bewerking records toevoegen.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Een taak Batch Execution indienen (maar niet wilt starten)

Klik op de **bewerking toevoegen** als u wilt toevoegen met de bewerking AzureML BES tot de API. **Bericht** voor de **HTTP-woord**selecteren. Type **/services/ /workspaces/ {werkruimte} {service} / jobs?api-versie = {apiversion}** voor de **URL van sjabloon**. Typ **BES indienen** voor de **naam weer te geven**. Klik op **antwoorden** > **toevoegen** aan de linker- en selecteer **200 OK**. Klik op **Opslaan** om op te slaan deze bewerking.

###<a name="start-a-batch-execution-job"></a>Een taak Batch Execution starten

Klik op de **bewerking toevoegen** als u wilt toevoegen met de bewerking AzureML BES tot de API. **Bericht** voor de **HTTP-woord**selecteren. Type **/workspaces/ {werkruimte} /services/ {service} /jobs/ {taak-id} / start?api-versie = {apiversion}** voor de **URL van sjabloon**. Typ **BES starten** voor de **naam weer te geven**. Klik op **antwoorden** > **toevoegen** aan de linker- en selecteer **200 OK**. Klik op **Opslaan** om op te slaan deze bewerking.

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>De status of de resultaten van een taak Batch Execution ophalen

Klik op de **bewerking toevoegen** als u wilt toevoegen met de bewerking AzureML BES tot de API. Selecteer **ophalen** voor het **HTTP-woord**. Type **/workspaces/ {werkruimte} /services/ {service} {taak-id} /jobs/ ?api-versie = {apiversion}** voor de **URL van sjabloon**. Typ **BES Status** voor de **naam weer te geven**. Klik op **antwoorden** > **toevoegen** aan de linker- en selecteer **200 OK**. Klik op **Opslaan** om op te slaan deze bewerking.

###<a name="delete-a-batch-execution-job"></a>Een taak Batch Execution verwijderen

Klik op de **bewerking toevoegen** als u wilt toevoegen met de bewerking AzureML BES tot de API. Selecteer **verwijderen** voor de **HTTP-woord**. Type **/workspaces/ {werkruimte} /services/ {service} {taak-id} /jobs/ ?api-versie = {apiversion}** voor de **URL van sjabloon**. Typ **BES verwijderen** voor de **naam weer te geven**. Klik op **antwoorden** > **toevoegen** aan de linker- en selecteer **200 OK**. Klik op **Opslaan** om op te slaan deze bewerking.

##<a name="call-an-operation-from-the-developer-portal"></a>Een bewerking bellen vanuit de Developer Portal

Bewerkingen kunnen worden aangeroepen rechtstreeks vanuit de Developer portal, die is een handige manier om te bekijken en de bewerkingen van een API testen. In deze handleiding voor-stap wordt u de methode voor **Records uitvoeren** die is toegevoegd aan de **AzureML Demo API**bellen. Klik op **Developer portal** van het menu boven in de klassieke Portal.

![Developer portal](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

**API's** in het bovenste menu op en klik vervolgens op **AzureML Demo API** om te zien van de beschikbare bewerkingen.

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Selecteer de **Records uitvoeren** voor de bewerking. Klik op **het uitproberen**.

![Probeer it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Voor parameters van de aanvraag, typt u uw **werkruimte**, **service**, **2.0** voor de **apiversion**en **waar** voor de **details**. U kunt uw **werkruimte** en de **service** vinden in het dashboard AzureML web service (Zie **Test de webservice** in bijlage A).

Voor de kop van de aanvraag, klikt u op **koptekst toevoegen** en typ **Inhoudstype** en **toepassing/json**, en vervolgens klikt u op **koptekst toevoegen** en typ **autorisatie** en **vruchtdragende <YOUR AZUREML SERVICE API-KEY> **. U kunt uw **api-sleutel** vinden in het dashboard AzureML web service (Zie **Test de webservice** in bijlage A).

Type **{"Invoer": {"input1": {"ColumnNames": ["Kol2"], "waarden": [[' Dit is een goede dag']]}}, "GlobalParameters": {}}** voor het hoofdgedeelte van de aanvraag.

![azureml-demo-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Klik op **verzenden**.

![Verzenden](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Wanneer een bewerking wordt gestart, wordt de developer portal de **Gevraagde URL** uit de back-enddatabase-service, de **antwoordstatus**, de **reactie kopteksten**en alle **inhoud van de reactie**.

![antwoordstatus](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Bijlage A - maken en te testen van een eenvoudige AzureML webservice

###<a name="creating-the-experiment"></a>Maken van het experiment

Hieronder vindt u de stappen voor het maken van een eenvoudige AzureML experiment en implementeren van deze als een webservice. De service krijgt als invoer van een kolom met willekeurige tekst en geeft als resultaat een reeks functies die worden weergegeven als gehele getallen. Bijvoorbeeld:

Tekst | Opgedeelde tekst
--- | ---
Dit is een goede dag | 1 1 2 2 0 2 0-1

Eerst, met een browser van uw keuze, navigeer naar: [https://studio.azureml.net/](https://studio.azureml.net/) en voer uw referenties voor aanmelding. Maak vervolgens een nieuwe lege experiment.

![zoeken-experiment-sjablonen](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Wijzig de **SimpleFeatureHashingExperiment**. Vouw **Gegevenssets opgeslagen** en sleep **Adresboek recensies van Amazon** naar uw experiment.

![eenvoudige-functie-hashing-experiment](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Vouw **Gegevenstransformatie** en **manipuleren** en sleep **De kolommen selecteren in de gegevensset** naar uw experiment. Verbinden met **adresboek recensies van Amazon** **Selecteer kolommen in de gegevensset**.

![Selecteer-kolommen](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Klik op **Kolommen selecteren in de gegevensset** en klik op **kolom Gegevenskiezer starten** en selecteer **Kol2**. Klik op het vinkje als deze wijzigingen wilt toepassen.

![Selecteer-kolommen](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Vouw **Tekst Analytics** en sleep **Functie Hashing** naar het experiment. Verbinden met **Selecteer kolommen in de gegevensset** **functie Hashing**.

![verbinding maken met-project-kolommen](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Typ **3** voor de **Hashing bitsize**. Hiermee maakt u 8 (23) kolommen.

![hashing bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

U wilt nu, klik op **uitvoeren** als u wilt testen van het experiment.

![uitvoeren](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Een webservice maken

Maak nu een webservice. Vouw **Webservice** en sleep **invoer** naar uw experiment. Verbinden met **invoer** **functie Hashing**. Sleep ook **uitvoer** naar uw experiment. Verbinden met **uitvoer** **functie Hashing**.

![uitvoer-naar-functie-hashing](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Klik op **publiceren webservice**.

![publiceren-web-service](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Klik op **Ja** als u wilt publiceren het experiment.

![Ja om te publiceren](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>Test de webservice

Een webservice AzureML bestaat uit RSS (aanvraag en respons service) en BES (batch execution service) eindpunten. RSS is voor synchrone uitvoering. BES is voor asynchrone taak kan worden uitgevoerd. Als u wilt testen uw webservice met het onderstaande voorbeeld Python bron, wellicht moet u downloaden en installeren van de Azure-SDK voor Python (Zie: [het installeren van Python](../python-how-to-install.md)).

U moet ook de **werkruimte**, **service**en **api_key** van uw experiment voor het onderstaande voorbeeld-bron. U kunt de werkruimte en de service vinden door te klikken op **Aanvraag en respons** of **Batch Execution** voor uw experiment in het dashboard van de service web.

![zoeken-werkruimte-en-service](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

U kunt de **api_key** vinden door te klikken op uw experiment in het dashboard van de service web.

![zoeken-api-sleutel](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Test records eindpunt

#####<a name="test-button"></a>Knop testafbeelding

Er is een eenvoudige manier om te testen van het eindpunt van de records te **testen** te klikken op het web servicedashboard.

![testen](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

**Dit is een goede dag** voor **Kol2**typen. Klik op het vinkje.

![Voer gegevens](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Worden er ongeveer zo

![Voorbeeld van-uitvoer](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Voorbeeld van een Code

Een andere manier om te testen van uw records is vanuit uw clientcode. Als u **aanvraag en respons** op het dashboard en blader naar de onderkant klikt, ziet u voorbeelden van code voor C#, Python en R. U ziet ook de syntaxis van de aanvraag records, inclusief het verzoek URI, koppen en hoofdtekst.

Deze handleiding ziet u een werkende Python voorbeeld. U moet deze verder wijzigen met de **werkruimte**, **service**en **api_key** van uw experiment.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Test BES eindpunt
Klik op **Batch execution** op het dashboard en schuif naar beneden. Ziet u voorbeelden van code voor C#, Python en R. U ziet ook de syntaxis van de BES aanvragen voor het verzenden van een taak, een taak starten, krijgen de status of de resultaten van een taak en verwijderen van een taak.

Deze handleiding ziet u een werkende Python voorbeeld. Moet u deze met de **werkruimte**, **service**en **api_key** van uw experiment wijzigen. Daarnaast moet u de **opslagaccountnaam**, **opslag accountsleutel**en **naam van de container gegevensopslag**wijzigen. Ten slotte, moet u voor het wijzigen van de locatie van de **invoer bestand** en de locatie van het **uitvoerbestand**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
