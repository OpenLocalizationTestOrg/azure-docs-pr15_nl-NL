<properties
   pageTitle="Naamgevingsconventies voor Azure resources aanbevolen | Richtlijnen | Microsoft Azure"
   description="Aanbevolen naamgevingsconventies voor Azure resources. Hoe u de naam van virtuele machines, opslag-accounts, netwerken, virtuele netwerken, subnetten en andere Azure entiteiten"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Aanbevolen naamgevingsconventies voor Azure resources

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

De keuze van een naam voor een resource in Microsoft Azure is belangrijk omdat:

- Het is moeilijk te later een naam wijzigen.
- Namen moeten voldoen aan de vereisten van hun specifieke brontype.

Consistente naamconventies makkelijker resources om te zoeken. Ze kunnen ook de rol van een resource in een oplossing aan te geven.

Dit artikel is bedoeld voor een overzicht van de regels voor naamgeving en beperkingen voor Azure resources en een set basislijn aanbevelingen voor naamgevingsconventies.  U kunt deze aanbevelingen gebruiken als uitgangspunt voor uw eigen specifieke conventies aan uw wensen voldoet.  

Behaalt u succes met naamgevingsconventies met is tot stand te brengen en ze tijdens uw toepassingen en organisaties te volgen. 

## <a name="naming-subscriptions"></a>Naamgeving van abonnementen

Naam van Azure abonnementen, Controleer uitgebreide namen informatie over de context en het doel van elk abonnement wissen.  Als u werkt in een omgeving met veel abonnementen, kunt volgen van een gedeelde naamgevingsconventie duidelijkheid verbeteren.

Een aanbevolen patroon voor naming abonnementen luidt als volgt:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Bedrijf is meestal hetzelfde voor elk abonnement. Sommige bedrijven hebben echter onderliggende bedrijven binnen de organisatiestructuur. Deze bedrijven kunnen worden beheerd door een centrale IT-groep. In dat geval kunnen ze worden onderscheiden doordat de bovenliggende bedrijfsnaam (*Contoso*) en de onderliggende bedrijfsnaam (*Noord-Wind*).

- Afdeling is een naam in de organisatie waarop een groep personen werkt. Dit item binnen de naamruimte als optioneel.

- Productlijn is een specifieke naam voor een product of de functie die wordt uitgevoerd vanuit binnen de afdeling.
Dit is gewoonlijk optioneel voor interne services en toepassingen. Het is echter ten zeerste aanbevolen voor openbare services waarvoor eenvoudig scheiding en identificatie (zoals wissen scheiding van records voor factureringsperiode).

- Omgeving is de naam waarmee de levenscyclus van de implementatie van toepassingen en services, zoals ontwikkelaar, q & a of productcatalogus wordt beschreven.

| Bedrijf | Afdeling | Regel voor product of Service | Omgeving | Volledige naam  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Contoso | SocialGaming | AwesomeService | Productie | Contoso SocialGaming AwesomeService productie |
| Contoso | SocialGaming | AwesomeService | Ontwikkelaar | Contoso SocialGaming AwesomeService ontwikkelaar |
| Contoso | DEZE | InternalApps | Productie | Contoso IT InternalApps productie |
| Contoso | DEZE | InternalApps | Ontwikkelaar | Contoso IT InternalApps-ontwikkelaar |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Gebruik van voor-en achtervoegsel om te voorkomen dubbelzinnigheden

Bij het geven van namen aan resources in Azure wordt aangegeven, wordt het wordt aanbevolen algemene voor- of achtervoegsels gebruiken om te identificeren van het type en de context van de resource.  Terwijl alle informatie over het type, metagegevens, context beschikbaar is via een programma, algemene en achtervoegsel toepassen eenvoudiger visuele identificatie.  Wanneer de voor-en achtervoegsel opnemen in uw naamgevingsconventie, is het is belangrijk duidelijk opgeven of het voorvoegsel aan het begin van de naam (voorvoegsel) of aan het eind (achtervoegsel is).  

Hier zijn bijvoorbeeld twee mogelijke namen voor een service hosten van een berekening-engine:

- SvcCalculationEngine (voorvoegsel)
- CalculationEngineSvc (achtervoegsel)

Voor-en achtervoegsel kunnen verwijzen naar verschillende aspecten die de bepaalde bronnen beschrijven. De volgende tabel ziet u enkele voorbeelden doorgaans worden gebruikt.

| Aspecten | Voorbeeld | Notities |
| ------ | ------- | ----- |
| Omgeving | ontwikkelaar, productcatalogus, q & a | Geeft aan wat de omgeving voor de resource |
| Locatie | uw (ons West), ue (ons Oost) | Geeft aan wat het gebied waarin de resource wordt geïmplementeerd |
| Exemplaar | 01, 02 | Voor resources hebt die meer dan één benoemde (endwebservers, enzovoort). |
| Product of Service | Service | Geeft aan wat het product, toepassing of service die ondersteuning biedt voor de resource |
| Rol | SQL, het web messaging | Bepaalt de rol van de bijbehorende resource |

Wanneer u een specifieke naamgevingsconventie op voor uw bedrijf of projecten ontwikkelt, is is van belang dat een gemeenschappelijke set met voor-en achtervoegsel en hun positie (achtervoegsel of voorvoegsel) kiezen.

## <a name="naming-rules-and-restrictions"></a>Naamgeving van regels en beperkingen

Elk type bron of service in Azure worden toegepast in een reeks naming beperkingen en bereik; een naamgevingsconventie of patroon moet voldoen aan de vereiste naming regels en bereik.  Bijvoorbeeld, terwijl de naam van een VM wordt toegewezen aan een DNS-naam (en dus moet uniek zijn voor alle Azure), wordt de naam van een VNET beperkt tot de resourcegroep waaraan deze is gemaakt in.

In het algemeen wordt voorkomen dat geen speciale tekens (`-` of `_`) als het eerste of laatste teken in een willekeurige. Deze tekens bevat, worden de meeste regels voor gegevensvalidatie mislukt.

| Categorie | Service of entiteit | Bereik | Lengte | Behuizing | Geldige tekens | Voorgestelde patroon | Voorbeeld |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Resourcegroep | Resourcegroep | Globale | 1-64 | Kleine letters | Alfanumeriek, onderstrepingsteken en afbreekstreepje | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Resourcegroep | Beschikbaarheid instellen | Resourcegroep | 1-80 | Kleine letters | Alfanumeriek, onderstrepingsteken en afbreekstreepje | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Algemene | Tag | Gekoppelde entiteit | 512 (naam), 256 (waarde) | Kleine letters | Alfanumerieke | `"key" : "value"` | `"department" : "Central IT"` |
| Berekenen | VM | Resourcegroep | 1-15 | Kleine letters | Alfanumeriek, onderstrepingsteken en afbreekstreepje | `<name>-<role>-<instance>` | `profx-sql-001` |
| Opslag | Opslagaccountnaam (gegevens) | Globale | 3-24 | Kleine letters | Alfanumerieke | `<service short name><type><number>` | `profxdata001` |
| Opslag | Opslagaccountnaam (schijven) | Globale | 3-24 | Kleine letters | Alfanumerieke | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Opslag | De containernaam van de | Opslag-account | 3-63 |   Kleine letters | Alfanumeriek en -streepje | `<context>` | `logs` |
| Opslag | De naam van de BLOB | Container | 1-1024 | Hoofdletters en kleine letters | Een URL-teken | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Opslag | De wachtrijnaam van de | Opslag-account | 3-63 | Kleine letters | Alfanumeriek en -streepje | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Opslag | Tabelnaam | Opslag-account | 3-63 |Kleine letters | Alfanumerieke | `<service short name>-<context>` | `awesomeservice-logs` |
| Opslag | Bestandsnaam | Opslag-account | 3-63 | Kleine letters | Alfanumerieke | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Netwerken | Virtual Network (VNet) | Resourcegroep | 2-64 | Niet hoofdlettergevoelig | Alfanumeriek, streepje onderstrepingsteken en periode | `<service short name>-[section]-vnet` | `profx-vnet` |
| Netwerken | Subnet | Bovenliggende VNet | 2-80 | Niet hoofdlettergevoelig | Alfanumeriek, onderstrepingsteken streepje en periode | `<role>-subnet` | `gateway-subnet` |
| Netwerken | Netwerkinterface | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepje onderstrepingsteken en periode | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Netwerken | Beveiligingsgroep netwerk | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepje onderstrepingsteken en periode | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Netwerken | Regel voor netwerk-beveiligingsgroep | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepje onderstrepingsteken en periode | `<descriptive context>` | `sql-allow` |
| Netwerken | Openbare IP-adres | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepje onderstrepingsteken en periode | `<vm or service name>-pip` | `profx-sql1-pip` |
| Netwerken | De belasting voor documentconversies | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepje onderstrepingsteken en periode | `<service or role>-lb` | `profx-lb` |
| Netwerken | Gebalanceerde regels Config laden | De belasting voor documentconversies | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepje onderstrepingsteken en periode | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Resources met Tags ordenen

De Manager van de Resource Azure ondersteunt sociaalnetwerklabels entiteiten met willekeurige tekenreeksen herkennen context en stroomlijnen automatisering.  Bijvoorbeeld de tag `"sqlVersion: "sql2014ee"` VMs in een implementatie met SQL Server 2014 Enterprise Edition voor het uitvoeren van een geautomatiseerd script tegen deze kunnen worden geïdentificeerd.  Tags moeten worden gebruikt om te verbeteren en context aan de zijkant van de gekozen naamgevingsconventies te verbeteren.

> [AZURE.TIP]Een andere voordeel van labels is dat labels resourcegroepen, zodat u kunt een koppeling en entiteiten relateren over verschillende implementaties beslaan.

Elke resource of resourcegroep mag maximaal **15** tags bevatten. De naam van de code is beperkt tot 512 tekens en de tagwaarde is beperkt tot 256 tekens.

Voor meer informatie over resource labelen, raadpleegt u [werken met tags om u te organiseren van uw Azure resources](../resource-group-using-tags.md).

Enkele van de algemene sociaalnetwerklabels gebruik gevallen zijn:

- **Facturering**; Resources groeperen en deze te koppelen met facturering of boete voor back-codes.
- **Service Context-id**; Groepen resources identificeren via resourcegroepen voor algemene bewerkingen en groeperen
- **Toegangsbeheer en beveiligingscontext**; Beheerdersrol identificatie op basis van portfolio, systeem, service, app, exemplaar, enzovoort.

> [AZURE.TIP]Labelen vroeg - tag vaak.  Beter om een basislijn labelen kleurenschema op hun plaats staan en pas over tijd en hoeven om optimaal na het feit.  

Een voorbeeld van enkele veelvoorkomende sociaalnetwerklabels methoden:

| Labelnaam | Toets | Voorbeeld | Opmerking |
| -------- | --- | ------- | ------- |
| Factuur naar / interne financiële-ID | billTo  | `IT-Chargeback-1234` | Een interne i/o- of factureringsbeheerder code |
| Operator of rechtstreeks verantwoordelijk persoon (DRI) | managedBy | `joe@contoso.com`  | Alias of e-mailadres |
| De projectnaam van het | naam van het project | `myproject`  | Naam van de regel project of product |
| Versie van project | Project-versie | `3.4`  | Versie van de regel project of product |
| Omgeving | omgeving | `<Production, Staging, QA >` | Milieu id | 
| Laag | laag | `Front End, Back End, Data` | Laag of rol/context identificatie |
| Gegevens-profiel | dataProfile | `Public, Confidential, Restricted, Internal` | Gevoeligheid van gegevens die zijn opgeslagen in de bron |
 
## <a name="tips-and-tricks"></a>Tips en trucs

Sommige typen resources u mogelijk extra noodzakelijk op conventies en een naam geven.

### <a name="virtual-machines"></a>Virtuele Machines

Met name in grotere topologieën stroomlijnt zorgvuldig naming virtuele machines de rol en het doel van elke computer te identificeren en taalprogrammatalen bieden uitvoeren van scripts.

> [AZURE.WARNING]Elke virtuele machine in Azure heeft zowel een Azure resourcenaam en de naam van een besturingssysteem-host.  
> Als de resourcenaam en hostnaam verschillen, het beheren van de VMs lastig zijn mogelijk en te voorkomen.
> Als een virtuele machine is gemaakt op basis van een VHD dat al bevat bijvoorbeeld een geconfigureerde besturingssysteem wordt uitgevoerd met een hostnaam.

- [Naamgevingsconventies voor Windows Server VMs](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Opslag-accounts en opslag entiteiten

Er zijn twee primaire gebruik dozen voor opslag-accounts: een back-up schijven voor VMs en gegevens opslaat in BLOB's, wachtrijen en tabellen.  Opslag-accounts die worden gebruikt voor VM schijven moeten volgen de naamgevingsconventie op van de bovenliggende VM naam koppelt (en met potentiële dat u meerdere accounts van de opslagruimte voor geavanceerde VM SKU's, moet u ook een achtervoegsel toepassen).

> [AZURE.TIP]Opslag-accounts: voor gegevens of schijven - moeten volgen een naamgevingsconventie waarmee meerdere opslag-accounts (dat wil zeggen altijd via een numerieke achtervoegsel) worden gebruikt.

Dit kunt configureren voor een aangepaste domeinnaam voor toegang tot blob-gegevens in uw opslagruimte van Azure-account.
Het eindpunt van de standaardinstelling voor de service Blob heeft `https://mystorage.blob.core.windows.net`.

Maar als u een aangepast domein (zoals www.contoso.com) aan het eindpunt blob voor uw account opslag toewijzen, u kunt ook toegang tot gegevens in uw account opslag blob met behulp van dat domein. Met een aangepaste domeinnaam, bijvoorbeeld `http://mystorage.blob.core.windows.net/mycontainer/myblob` kan worden geopend als `http://www.contoso.com/mycontainer/myblob`.

Verwijzen naar [een aangepaste domeinnaam voor uw Blob storage eindpunt configureren](../storage/storage-custom-domain-name.md)voor meer informatie over het configureren van deze functie.

Voor meer informatie over het geven van namen aan BLOB's, containers en tabellen:

- [Verwijst naar Containers, BLOB's en metagegevens en een naam geven](https://msdn.microsoft.com/library/dd135715.aspx)
- [Naamgeving van wachtrijen en metagegevens](https://msdn.microsoft.com/library/dd179349.aspx)
- [Namen van tabellen](https://msdn.microsoft.com/library/azure/dd179338.aspx)

De naam van een blob kan elke combinatie van tekens bevatten, maar gereserveerde URL tekens correct moeten worden voorafgegaan. Voorkomen dat blob namen die eindigt op een punt (.), een slash (/), of een reeks of een combinatie van beide. De schuine streep is congres, het **virtuele** mapscheidingsteken. Gebruik niet een backslash (\) in een blob-naam. De client API's mogelijk gebruik hiervan is toegestaan, maar mislukt voor correct hashing en komen niet overeen met de handtekeningen.

Het is niet mogelijk is om te wijzigen van de naam van een opslag-account of de container nadat deze is gemaakt.
Desgewenst kunt u een nieuwe naam te gebruiken, moet u deze verwijderen en een nieuwe record maken.

> [AZURE.TIP] Het is raadzaam dat u een naamgevingsconventie voor alle accounts voor opslagruimte en typen voordat u de ontwikkeling van een nieuwe service of toepassing maakt.

## <a name="example---deploying-an-n-tier-service"></a>Bijvoorbeeld: een service n lagen implementeren

In dit voorbeeld definiëren we een n lagen service te configureren, bestaande uit front IIS-servers (die worden gehost in Windows Server VMs) met SQL Server (die worden gehost in twee Windows Server VMs), een ElasticSearch cluster (ingesloten in een 6 Linux VMs) en de bijbehorende opslag accounts, virtuele netwerken resource groeperen en de belasting voor documentconversies.

We beginnen door te definiëren de contextuele conventies van deze toepassing:

| Entiteit | Congres | Beschrijving  |
| ------ | ---------- | ------------ |  
| De servicenaam van de | `profx` | De korte naam van de toepassing of service wordt geïmplementeerd |
| Omgeving | `prod` | Dit is bedoeld voor de productie-implementatie (in plaats van q & a, test, enz.) |

Uit die volgens de basislijn kunt vervolgens wijst u uit de conventies voor elk van de resourcetypen:

| Resourcetype | Congres Base | Voorbeeld |
| ------------- | --------------- | ------- |
| Abonnement | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Resourcegroep | `servicename-rg` | `profx-rg` |
| Virtual Network | `servicename-vnet` | `profx-vnet` |
| Subnet | `role-subnet` | `sql-vnet` |
| De belasting voor documentconversies | `servicename-lb` | `profx-lb` |
| VM | `servicename-role[number]` | `profx-sql0` |
| Opslag-Account | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Zoals u in het volgende diagram:

![toepassing topologie-diagram] (media/guidance-naming-conventions/guidance-naming-convention-example.png "De topologie zoektoepassing steekproef")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Voorbeeld - Azure CLI script voor de implementatie van het bovenstaande voorbeeld

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
