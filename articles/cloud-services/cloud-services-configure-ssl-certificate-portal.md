<properties 
    pageTitle="SSL configureren voor een cloudservice | Microsoft Azure" 
    description="Leer hoe u een HTTPS eindpunt opgeven voor een Webrol en het uploaden van een SSL-certificaat als u wilt beveiligen van uw toepassing. In deze voorbeelden wordt de Azure-portal gebruiken." 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>SSL configureren voor een toepassing in Azure wordt aangegeven

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-configure-ssl-certificate-portal.md)
- [Azure klassieke portal](cloud-services-configure-ssl-certificate.md)

Secure Socket Layer (SSL)-versleuteling is de meest gebruikte methode van het beveiligen van gegevens die zijn verzonden via internet. Deze algemene taak wordt beschreven hoe u een HTTPS eindpunt opgeven voor een Webrol en het uploaden van een SSL-certificaat als u wilt beveiligen van uw toepassing.

> [AZURE.NOTE] De procedures in deze taak toepassen op Azure Cloud Services; voor App-Services, raadpleegt u [deze](../app-service-web/web-sites-configure-ssl-certificate.md).

Voor deze taak wordt een productie-implementatie; informatie over het gebruik van een tijdelijk opslaan implementatie wordt aangeboden aan het einde van dit onderwerp.

[In dit](cloud-services-how-to-create-deploy-portal.md) als eerste lezen als u niet hebt nog een cloudservice gemaakt.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a>Stap 1: Een SSL-certificaat downloaden

SSL configureren voor een toepassing, moet u eerst een SSL-certificaat dat is ondertekend door een certificeringsinstantie (CA), een vertrouwde derde partij die daartoe certificaten ophalen. Als u nog een, moet u aanvragen bij een bedrijf dat SSL-certificaten verkoopt.

Het certificaat moet voldoen aan de volgende vereisten voor SSL-certificaten in Azure wordt aangegeven:

-   Het certificaat moet een persoonlijke sleutel bevatten.
-   Het certificaat moet worden gemaakt voor belangrijke exchange, geëxporteerd naar een bestand Personal Information Exchange (.pfx).
-   Onderwerpnaam van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de cloudservice. U kunt geen een SSL-certificaat aanvragen bij een certificeringsinstantie (CA) voor het domein cloudapp.net. U moet in het bezit van een aangepaste domeinnaam gebruiken wanneer toegang tot uw service. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt de naam van de onderwerp van het certificaat moet overeenkomen met de naam van het aangepaste domein gebruikt voor toegang tot uw toepassing worden weergegeven. Bijvoorbeeld als de naam van uw aangepaste domein **contoso.com is** u zou een certificaat aanvragen bij uw CA voor * **. contoso.com** of * *www.contoso.com**.
-   Het certificaat moet ten minste 2048-bits codering gebruiken.

Voor testdoeleinden, u kunt [maken](cloud-services-certs-create.md) en gebruiken van een zelfondertekend certificaat. Een zelfondertekend certificaat is niet geverifieerd door een Certificeringsinstantie en het domein cloudapp.net kunt gebruiken als de URL van de website. De taak die hieronder wordt bijvoorbeeld een zelfondertekend certificaat waarin de algemene naam (CN) gebruikt in het certificaat **sslexample.cloudapp.net**is gebruikt.

Vervolgens moet u informatie over het certificaat opnemen in uw servicedefinitie en bestanden van de service-configuratie.

<a name="modify"> </a>
## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Stap 2: De service-definitie en configuratie van bestanden wijzigen

Uw toepassing moet worden geconfigureerd voor gebruik van het certificaat en een HTTPS-eindpunt moet worden toegevoegd. Hierdoor de servicedefinitie en service configuratiebestanden moeten worden bijgewerkt.

1.  In uw ontwikkelomgeving, opent u het bestand in de service-definition Language (CSDEF), een sectie **certificaten** in de sectie **WebRole** toevoegen en opnemen van de volgende informatie over het certificaat (en de tussenliggende certificaten):

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    De sectie **certificaten** Hiermee definieert u de naam van onze certificaat, de locatie en de naam van de winkel waar deze zich bevindt.
    
    Machtigingen (`permisionLevel` kenmerk) kan worden ingesteld op een van de volgende handelingen uit:

  	| Waarde van machtiging  | Beschrijving |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Standaard)** Alle processen van de rol hebben toegang tot de persoonlijke sleutel. |
  	| verhoogde          | Alleen verhoogde processen hebben toegang tot de persoonlijke sleutel.|

2.  In uw definitiebestand service toevoegen een element **InputEndpoint** in de sectie **eindpunten** HTTPS inschakelen:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  In het bestand service definitie toevoegen een element **binden** in de sectie **Sites** . Hiermee voegt u een HTTPS-binding naar het eindpunt toewijzen aan uw site:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    Alle vereiste wijzigingen aan de definitie servicebestand zijn voltooid, maar u nog steeds moet informatie over het certificaat toevoegen aan een bestand voor de configuratie van de service.

4.  Uw service configuratiebestand (CSCFG), ServiceConfiguration.Cloud.cscfg, voegt u een sectie **certificaten** in de sectie **rol** worden vervangen door de vingerafdruk-waarde van het voorbeeld hieronder wordt weergegeven met die van uw certificaat toe:

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(Het bovenstaande voorbeeld wordt **sha1** voor de algoritme van de vingerafdruk. Geef de juiste waarde voor de algoritme van de vingerafdruk van het certificaat.)

Nu dat de servicedefinitie en service configuratiebestanden zijn bijgewerkt, een pakket uw implementatie van uploaden naar Azure. Als u **cspack**gebruikt, niet de vlag **/generateConfigurationFile** gebruikt zoals overschreven die de gegevens van het certificaat dat u zojuist hebt ingevoegd.

## <a name="step-3-upload-a-certificate"></a>Stap 3: Een certificaat uploaden

Verbinding maken met de portal en...

1. Selecteer uw cloudservice in de Portal, uw **Cloudservice**. (Als dat in de sectie **alle resources** is.) 
    
    ![Uw cloudservice publiceren](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Klik op **certificaten**.

    ![Klik op het pictogram certificaten](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Geef het **bestand**, **wachtwoord**, klik op **uploaden**.

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Stap 4: Verbinding maken met het exemplaar van de rol via HTTPS

Nu dat uw implementatie actief in Azure wordt aangegeven is, kunt u een verbinding maken met deze via HTTPS.
    
1.  Klik op de **URL van de Site** om de webbrowser te openen.

    ![Klik op Site-URL](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2.  Wijzigen van de koppeling om te **https** gebruiken in plaats van **http**in uw webbrowser en Ga naar de pagina.

    >[AZURE.NOTE] Als u een zelfondertekend certificaat gebruikt wanneer u naar een HTTPS-eindpunt dat is gekoppeld aan het zelfondertekend certificaat ziet u mogelijk een certificaatfout in de browser. Met behulp van een certificaat ondertekend door een vertrouwde certificeringsinstantie lost dit probleem; in de tussentijd kunt u de fout negeren. (Er is een andere optie is het zelfondertekend certificaat toevoegen aan van de gebruiker vertrouwd certificaat certificaatarchief.)

    ![Voorbeeld van de site](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

    >[AZURE.TIP] Als u SSL gebruiken voor een tijdelijk opslaan implementatie in plaats van een productie-implementatie wilt, moet u eerst om te bepalen de URL die wordt gebruikt voor het tijdelijk opslaan implementatie. Nadat uw cloudservice is geïmplementeerd, betekent dit dat de URL voor de testomgeving zich in deze indeling wordt bepaald door de **Implementatie-ID** GUID:`https://deployment-id.cloudapp.net/`  
      
    >Een digitaal certificaat met de algemene naam (CN) gelijk is aan de URL GUID gebaseerde (bijvoorbeeld **328187776e774ceda8fc57609d404462.cloudapp.net**) maken. Het certificaat toevoegen aan uw gefaseerde cloudservice via de portal. Informatie over het certificaat vervolgens toevoegen aan uw bestanden CSDEF en CSCFG, uw toepassing verpakken en bijwerken van uw gefaseerde implementatie als u wilt gebruiken het nieuwe pakket.

## <a name="next-steps"></a>Volgende stappen

* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Leer hoe u [een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md)configureren.
* [Uw cloudservice beheren](cloud-services-how-to-manage-portal.md).
