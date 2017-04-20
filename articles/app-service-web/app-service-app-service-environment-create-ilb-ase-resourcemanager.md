<properties 
    pageTitle="Het maken van een ILB ASE met Azure resourcemanager sjablonen | Microsoft Azure" 
    description="Informatie over het maken van een interne laden verdeling ASE Azure resourcemanager sjablonen gebruiken." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Het maken van een ILB ASE Azure resourcemanager sjablonen gebruiken

## <a name="overview"></a>Overzicht ##
App Service-omgevingen kunnen worden gemaakt met een virtueel netwerk interne adres in plaats van een openbare VIP.  Dit interne adres is opgegeven door een Azure onderdeel genaamd de interne taakverdeling (ILB).  Een ASE ILB kunnen worden gemaakt met behulp van de Azure portal.  Dit kan ook worden gemaakt met automatisering in Azure resourcemanager sjablonen.  In dit artikel worden doorlopen de stappen en syntaxis die nodig zijn voor het maken van een ASE ILB met Azure resourcemanager sjablonen.

Er zijn drie stappen voor het maken van een ASE ILB automatiseren:
1. Eerst is de basis ASE gemaakt in een virtueel netwerk met van het adres van de verdeling van interne laden in plaats van een openbare VIP.  Een domeinnaam hoofdsite wordt als onderdeel van deze stap is toegewezen aan de ASE ILB.
2. Nadat de ASE ILB is gemaakt, wordt een SSL-certificaat wordt geüpload.  
3. De geüploade SSL-certificaat is expliciet toegewezen aan de ASE ILB als de "standaard" SSL-certificaat.  Dit SSL-certificaat wordt gebruikt voor SSL-verkeer naar apps op de ASE ILB wanneer de apps zijn gericht met het algemene hoofddomein die zijn toegewezen aan de ASE (bijvoorbeeld https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>Het grondtal ILB ASE maken ##
Een voorbeeld resourcemanager Azure-sjabloon en de bijbehorende parameters-bestand, zijn beschikbaar op GitHub [hier][quickstartilbasecreate].

De meeste parameters in het bestand *azuredeploy.parameters.json* gelden voor het maken van zowel ILB ASEs, evenals ASEs die is gebonden aan een openbare VIP.  De lijst onder oproepen parameters van speciale notitie uit of die zijn uniek, bij het maken van een ASE ILB:


- *interalLoadBalancingMode*: In de meeste gevallen set dit tot 3, wat betekent dat zowel HTTP-/ HTTPS-verkeer is toegestaan op poort 80/443 en de controlegegevens kanaal poorten luistert de FTP-service op de ASE, wordt zijn gekoppeld aan een ILB virtueel netwerk interne adres toegewezen.  Als deze eigenschap wordt in plaats daarvan ingesteld op 2, klikt u vervolgens op alleen de FTP-service poorten (zowel besturingselement en gegevens kanalen) afhankelijk zijn van een adres ILB, terwijl de HTTP-/ HTTPS-verkeer ongewijzigd op de openbare VIP gerelateerd.
-  *dnsSuffix*: deze parameter wordt het hoofddomein standaard die worden toegewezen aan de ASE gedefinieerd.  In de openbare variatie van Azure App-Service is het hoofddomein standaard voor alle web-apps *azurewebsites.net*.  Echter omdat een ASE ILB interne met virtueel netwerk van een klant, niet dat u naar het hoofddomein van de openbare-service standaard gebruiken.  Een ASE ILB moet in plaats daarvan een standaard-hoofddomein die relevant is voor gebruik in een interne virtuele bedrijfsnetwerk hebben.  Bijvoorbeeld, een hypothetische Contoso Corporation een standaard-hoofddomein van *interne contoso.com* gebruiken voor apps die zijn bedoeld voor alleen worden omgezet en toegankelijke binnen het virtuele netwerk van Contoso. 
-  *ipSslAddressCount*: deze parameter wordt automatisch opgehaald op de waarde 0 in het bestand *azuredeploy.json* omdat ILB ASEs slechts één ILB adres hebben.  Er zijn geen expliciete IP-SSL-adressen voor een ASE ILB en dus de groep met de IP-SSL-adressen voor een ASE ILB moet zijn ingesteld op nul, anders een inrichten foutbericht weergegeven. 

Nadat het bestand *azuredeploy.parameters.json* is ingevuld voor een ASE ILB, kunnen de ASE ILB vervolgens worden gemaakt met het volgende Powershell-codefragment.  Wijzig het bestand uit om te voldoen aan waar de sjabloonbestanden Azure resourcemanager zich bevinden op uw computer.  Denk eraan dat naar uw eigen waarden voor de naam van de implementatie van Azure resourcemanager en de naam van de resource-groep.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Na de resourcemanager van Azure sjabloon is ingediend duurt het enkele uren voor de ASE ILB moet worden gemaakt.  Zodra de aanmaakdatum is voltooid, wordt de ASE ILB in de portal UX in de lijst met App-serviceomgevingen voor het abonnement dat de implementatie geactiveerd weergegeven.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Uploaden en configureren van de SSL-certificaat "Standaard" ##

Nadat de ASE ILB is gemaakt, moet een SSL-certificaat worden gekoppeld aan de ASE terwijl de "standaard" SSL-certificaat voor SSL-verbindingen aan apps gebruikt.  Kunt verdergaan met het hypothetische Contoso Corporation voorbeeld, als van de ASE standaard DNS achtervoegsel *interne contoso.com*en dan een verbinding met *https://some-random-app.internal-contoso.com* vereist een SSL-certificaat dat geschikt is voor **.internal-contoso.com*. 

Er zijn verschillende manieren een geldig SSL-certificaat inclusief interne certificeringsinstanties, aanschaffen van een certificaat van een externe uitgever en het gebruik van een zelfondertekend certificaat.  Ongeacht de bron van het SSL-certificaat moeten de volgende kenmerken van certificaat correct zijn geconfigureerd:

- *Onderwerp*: dit kenmerk moet zijn ingesteld op **.your-hoofdsite-domein-here.com*
- *Alternatieve naam voor onderwerp*: dit kenmerk moet bevatten beide * *.your-hoofdsite-domein-here.com*, en * *.scm.your-hoofdsite-domein-here.com*.  De reden voor het tweede item is dat SSL-verbindingen met de SCM/Kudu-site die is gekoppeld aan elke app worden uitgevoerd met het adres van het formulier *your-app-name.scm.your-root-domain-here.com*.

Met een geldig SSL-certificaat in Handje, zijn twee extra voorbereidende stappen nodig.  Het SSL-certificaat moet worden geconverteerd/opgeslagen als een .pfx-bestand.  Houd er rekening mee dat het .pfx-bestand moet alle tussenliggende opnemen en root certificaten en ook moet worden beveiligd met een wachtwoord.

De resulterende .pfx-bestand moet worden geconverteerd naar een base64-tekenreeks omdat het SSL-certificaat worden geüpload met een sjabloon van Azure resourcemanager.  Aangezien resourcemanager Azure-sjablonen tekstbestanden zijn, wordt het .pfx-bestand moet worden geconverteerd naar een base64-tekenreeks zodat deze toegevoegd als een parameter van de sjabloon worden kan.

Het codefragment Powershell toont een voorbeeld van een zelfondertekend certificaat genereren, het certificaat exporteren als een .pfx-bestand, de .pfx-bestand converteren naar een base64 gecodeerde tekenreeks en vervolgens op te slaan de base64 gecodeerde tekenreeks in een afzonderlijk bestand.  De Powershell-code voor het coderen van base64 is gebaseerd op de [Powershell-Scripts Blog][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Als het SSL-certificaat is is gegenereerd en geconverteerd naar een base64-gecodeerde tekenreeks, de resourcemanager Azure voorbeeldsjabloon voor het [configureren van de standaard-SSL-certificaat] op GitHub[ configuringDefaultSSLCertificate] kunnen worden gebruikt.

Hieronder ziet u de parameters in het bestand *azuredeploy.parameters.json* :

- *appServiceEnvironmentName*: de naam van de ILB ASE worden geconfigureerd.
- *existingAseLocation*: tekstreeks met de Azure regio waar de ASE ILB is geïmplementeerd.  Bijvoorbeeld: "Zuid Centraal US".
- *pfxBlobString*: de based64 codering tekenreeks van de .pfx-bestand.  Het codefragment eerder weergegeven gebruikt, zou u de tekenreeks in 'exportedcert.pfx.b64' kopiëren en plakt u deze in als de waarde van het kenmerk *pfxBlobString* .
- *wachtwoord*: het wachtwoord voor het .pfx-bestand beveiligen.
- *certificateThumbprint*: vingerafdruk van het certificaat.  Als u deze waarde uit Powershell ophaalt (bijvoorbeeld *$certificate. Vingerafdruk* uit de eerdere codefragment), kunt u de waarde als-is.  Echter als u de waarde van het dialoogvenster Windows certificaat kopiëren, moet u het verwijderen van de overbodige spaties.  De *certificateThumbprint* moet er ongeveer zo uitzien: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *certificateName*: een beschrijvende tekenreeks-id van uw eigen keuze kon u de identiteit het certificaat.  De naam wordt gebruikt als onderdeel van de unieke id van Azure resourcemanager voor de *Microsoft.Web/certificates* entity dat staat voor het SSL-certificaat.  De naam **moet** einde met het volgende achtervoegsel: \_yourASENameHere_InternalLoadBalancingASE.  Dit achtervoegsel wordt gebruikt door de portal als dat het certificaat wordt gebruikt voor het beveiligen van een ASE ILB ingeschakelde indicator.


Hieronder vindt u een afgekorte voorbeeld van *azuredeploy.parameters.json* :


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Nadat het bestand *azuredeploy.parameters.json* is ingevuld, kan de standaard-SSL-certificaat worden geconfigureerd met het volgende Powershell-codefragment.  Wijzig het bestand uit om te voldoen aan waar de sjabloonbestanden Azure resourcemanager zich bevinden op uw computer.  Denk eraan dat naar uw eigen waarden voor de naam van de implementatie van Azure resourcemanager en de naam van de resource-groep.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nadat de sjabloon Azure resourcemanager is ingediend dat het duurt ongeveer 40 minuten minuten per ASE front de wijziging.  Bijvoorbeeld, met een standaard grootte ASE met twee voorgrond-einden, duurt de sjabloon ongeveer een uur en twintig minuten om te voltooien.  Terwijl de sjabloon wordt uitgevoerd de ASE worden niet kan worden aangepast.  

Zodra de sjabloon is voltooid, apps op de ASE ILB kunnen worden geraadpleegd via HTTPS en de verbindingen worden beveiligd met de standaard-SSL-certificaat.  De standaard-SSL-certificaat wordt gebruikt wanneer apps op de ASE ILB zijn gericht met een combinatie van de naam van de toepassing plus de standaard-hostnaam.  Bijvoorbeeld *https://mycustomapp.internal-contoso.com* zou gebruikt u de standaard-SSL-certificaat voor **.internal-contoso.com*.

Echter net als apps waarop de service voor openbare meerdere tenant, kunnen ontwikkelaars ook aangepaste hostnamen voor afzonderlijke apps configureren en configureer unieke SNI SSL-certificaatbindingen voor afzonderlijke apps.  


## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen met App-serviceomgevingen, raadpleegt u [Inleiding tot de App-omgeving](app-service-app-service-environment-intro.md)

Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
