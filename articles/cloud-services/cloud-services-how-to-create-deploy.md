<properties
    pageTitle="Het maken en implementeren van een cloudservice | Microsoft Azure"
    description="Informatie over het maken en implementeren van een cloudservice met de methode snelle maken in Azure."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Het maken en implementeren van een Cloudservice

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-create-deploy-portal.md)
- [Azure klassieke portal](cloud-services-how-to-create-deploy.md)

De portal van Azure klassieke zijn er twee manieren om berichten te maken en implementeren van een cloudservice: **Snelle maken** en **Aangepaste te maken**.

In dit onderwerp wordt uitgelegd hoe u een nieuwe cloudservice maken en gebruik vervolgens **uploaden** om te uploaden en implementeren van een pakket met de cloud-service in Azure wordt aangegeven met de methode snelle maken. Wanneer u deze methode gebruikt, kunt u de Azure klassieke portal beschikbaar handige koppelingen voor het voltooien van alle vereisten die u gaat. Als u klaar bent om te implementeren van uw cloudservice wanneer u deze hebt gemaakt, kunt u beide tegelijkertijd gebruik **Maken van aangepaste**kunt doen.

> [AZURE.NOTE] Als u van plan bent om te publiceren van uw cloudservice van Visual Studio Team Services (VSTS), gebruikt u snelle maken, en stel VSTS publiceren van **Snel starten** of het dashboard. Zie voor meer informatie, [Continue bezorging bij Azure door gebruik van Visual Studio Team Services][TFSTutorialForCloudService], of raadpleegt u help voor de pagina **Snel starten** .

## <a name="concepts"></a>Concepten
Drie onderdelen zijn vereist om te kunnen een toepassing als een cloudservice in Azure wordt aangegeven:

- **Definitie van service**  
  Het hulpprogramma voor het definitie van de cloud-servicebestand (.csdef) Hiermee definieert u de ServiceModel, zoals het aantal rollen.

- **Service te configureren**  
  Het configuratiebestand (.cscfg) van een cloud-service biedt configuratie-instellingen voor de cloud service en afzonderlijke rollen, inclusief het aantal exemplaren van de rol.

- **Servicepakket**  
  Het servicepakket (.cspkg) bevat de toepassingscode en configuraties en het definitiebestand service.
  
U kunt meer informatie over deze en het maken van een pakket [hier](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Uw app voorbereiden
Voordat u een cloudservice implementeren kunt, moet u het pakket in de cloud-service (.cspkg) uit uw toepassingscode en het hulpprogramma voor het configureren van een cloud-servicebestand (.cscfg). De SDK Azure biedt hulpmiddelen voor het voorbereiden van deze bestanden vereist implementatie. U kunt de SDK installeren via de pagina [Downloads van Azure](https://azure.microsoft.com/downloads/) in de taal waarin u liever uw toepassingscode ontwikkelen.

Drie cloud service-functies zijn speciale configuraties voordat u een servicepakket exporteren:

- Als u wilt implementeren van een cloudservice die wordt Secure Sockets Layer (SSL) gebruikt om gegevens te coderen, [uw toepassing configureren](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) voor SSL.

- Als u configureren extern bureaublad-verbindingen met rol exemplaren, [de rollen configureren](cloud-services-role-enable-remote-desktop.md) voor extern bureaublad wilt.

- Als u wilt configureren voor uitgebreide controle voor uw cloudservice, schakelt u Azure diagnostische hulpprogramma's voor de cloudservice. *Minimale bewaken* (de standaardinstelling monitoring niveau) wordt gebruikt voor items die worden verzameld door de host-besturingssystemen voor rol-exemplaren (virtuele machines). "Uitgebreide controle * worden verzameld extra statistieken op basis van de prestatiegegevens binnen de rol exemplaren om in te schakelen nabij analyse van problemen die tijdens het verwerken van toepassing optreden. Informatie over het inschakelen van Azure diagnostische hulpprogramma's, Zie [Diagnostisch hulpprogramma inschakelen in Azure wordt aangegeven](cloud-services-dotnet-diagnostics.md).

Als u wilt een cloudservice met implementaties van web rollen of werknemer rollen maakt, moet u [de servicepakket maken](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Voordat u begint

- Als u de SDK Azure nog niet hebt geïnstalleerd, klikt u op **Azure SDK installeren** om de [pagina Downloads van Azure](https://azure.microsoft.com/downloads/)te openen en vervolgens downloaden de SDK voor de taal waarin u liever het ontwikkelen van uw code. (U hebt een verkoopkans later hiervoor.)

- Als alle exemplaren van de rol een certificaat vereist, de certificaten hebt gemaakt. Cloudservices vragen om een .pfx-bestand met een persoonlijke sleutel. U kunt [de certificaten Azure uploaden](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) bij het maken en implementeren van de cloudservice.

- Als u van plan bent om te implementeren van de cloudservice aan een groep affiniteit, de groep affiniteit hebt gemaakt. U kunt een groep affiniteit gebruiken om te implementeren van uw cloudservice en andere Azure services op dezelfde locatie in een gebied. U kunt de affiniteit-groep maken in het gebied **netwerken** van de Azure klassieke portal en klik op de pagina **affiniteit groepen** .


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Hoe u: maken van een cloudservice met snelle maken

1. Klik op **Nieuw**in de [portal van Azure klassieke](http://manage.windowsazure.com/)>**berekenen**>**Cloudservice**>**Snelle maken**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. Voer in de **URL**, de subdomeinnaam van een in de openbare URL gebruiken voor toegang tot uw cloudservice in productie implementaties. De indeling van de URL voor productie implementaties is: http://*myURL*. cloudapp.net.

3. Selecteer in de **regio of affiniteit groep**, de geografische regio of de affiniteit groep naar de cloudservice om te implementeren. Selecteer een groep affiniteit als u wilt implementeren van uw cloudservice op dezelfde locatie als andere Azure services in een gebied.

4. Klik op **Cloudservice maken**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    U kunt de status van het proces in het berichtgebied onderaan in het venster controleren.

    Het gebied **Cloud Services** wordt geopend, met de nieuwe cloudservice die wordt weergegeven. Wanneer het veld status wordt gewijzigd op gemaakt, is maken van de cloud-service voltooid.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Hoe u: een certificaat voor een cloudservice uploaden

1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**, klikt u op de naam van de cloudservice en klik vervolgens op **certificaten**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. Klik op **een certificaat uploaden** of **uploaden**.

3. In **bestand**gebruiken **Bladeren** om het certificaat (.pfx-bestand) te selecteren.

4. Voer de persoonlijke sleutel voor het certificaat in het vak **wachtwoord**.

5. Klik op **OK** (vinkje).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    U kunt de voortgang van de upload in het berichtgebied, hieronder bekijken. Wanneer de upload is voltooid, wordt het certificaat toegevoegd aan de tabel. Klik in het berichtgebied, op OK om het bericht te sluiten.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Hoe u: een cloudservice implementeren

1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**, klikt u op de naam van de cloudservice en klik vervolgens op **Dashboard**.

2. Klik op **een nieuwe productie-implementatie uploaden** of **uploaden**.

3. Voer een naam voor de nieuwe implementatie - bijvoorbeeld MyCloudServicev4 in **implementatie-label**.

3. In **pakket**, gebruikt u **Bladeren** om te selecteren van de service-pakketbestand (.cspkg) te gebruiken.

4. In **configuratie**, gebruikt u **Bladeren** om de service configureren-bestand (.cscfg) gebruik te selecteren.

5. Als de cloudservice alle functies met slechts één exemplaar bevat, schakelt u het selectievakje **implementeren, zelfs als een of meer rollen één exemplaar bevatten** om in te schakelen van de implementatie.

    Azure kan alleen 99.95 percentage toegang tot de cloudservice garanderen tijdens onderhoud en service-updates als elke rol ten minste twee exemplaren heeft. Indien nodig kunt u de rol van de extra exemplaren toevoegen op de pagina **schaal** nadat u de cloudservice geïmplementeerd. Zie [Niveau serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/)voor meer informatie.

6. Klik op **OK** (vinkje) om te beginnen met de cloud-service-implementatie.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    U kunt de status van de implementatie in het berichtgebied controleren. Klik op OK als u wilt verbergen van het bericht.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Controleer of uw implementatie is voltooid

1. Klik op **Dashboard**.

    De status moet worden weergegeven dat de service **uitgevoerd is**.

2. Klik onder **snel aan te duiden**, klikt u op de site-URL naar de cloudservice in een webbrowser.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Volgende stappen

* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name.md)configureren.
* [Uw cloudservice beheren](cloud-services-how-to-manage.md).
* [Ssl-certificaten](cloud-services-configure-ssl-certificate.md)configureren.