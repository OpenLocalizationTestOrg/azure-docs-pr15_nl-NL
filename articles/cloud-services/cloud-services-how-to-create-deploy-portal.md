<properties
    pageTitle="Het maken en implementeren van een cloudservice | Microsoft Azure"
    description="Informatie over het maken en implementeren van een cloudservice met behulp van de Azure portal."
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Het maken en implementeren van een cloudservice

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-create-deploy-portal.md)
- [Azure klassieke portal](cloud-services-how-to-create-deploy.md)

De Azure-portal zijn er twee manieren om berichten te maken en implementeren van een cloudservice: *Snelle maken* en *Aangepaste te maken*.

In dit artikel wordt uitgelegd hoe u een nieuwe cloudservice maken en gebruik vervolgens **uploaden** om te uploaden en implementeren van een pakket met de cloud-service in Azure wordt aangegeven met de methode snelle maken. Wanneer u deze methode te gebruiken, kunt u met de portal van Azure beschikbaar handige koppelingen voor het voltooien van alle vereisten die u gaat. Als u klaar bent om te implementeren van uw cloudservice wanneer u deze hebt gemaakt, kunt u beide tegelijkertijd gebruik maken van aangepaste kunt doen.

> [AZURE.NOTE] Als u van plan bent om te publiceren van uw cloudservice van Visual Studio Team Services (VSTS), gebruikt u snelle maken, en stel VSTS publiceren van de Azure Quickstart of het dashboard. Zie voor meer informatie, [Continue bezorging bij Azure door gebruik van Visual Studio Team Services][TFSTutorialForCloudService], of raadpleegt u help voor de pagina **Snel starten** .

## <a name="concepts"></a>Concepten
Drie onderdelen vereist zijn voor een toepassing als een cloudservice in Azure wordt aangegeven:

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

- Als u wilt implementeren van een cloudservice die wordt Secure Sockets Layer (SSL) gebruikt om gegevens te coderen, [uw toepassing configureren](cloud-services-configure-ssl-certificate-portal.md#modify) voor SSL.

- Als u configureren extern bureaublad-verbindingen met rol exemplaren, [de rollen configureren](cloud-services-role-enable-remote-desktop.md) voor extern bureaublad wilt. Dit kan alleen worden uitgevoerd in de klassieke portal.

- Als u wilt configureren voor uitgebreide controle voor uw cloudservice, schakelt u Azure diagnostische hulpprogramma's voor de cloudservice. *Minimale bewaken* (de standaardinstelling monitoring niveau) wordt gebruikt voor items die worden verzameld door de host-besturingssystemen voor rol-exemplaren (virtuele machines). *Uitgebreid monitoring* verzamelt extra statistieken op basis van de prestatiegegevens binnen de rol exemplaren om in te schakelen nabij analyse van problemen die tijdens het verwerken van toepassing optreden. Informatie over het inschakelen van Azure diagnostische hulpprogramma's, Zie [Diagnostisch hulpprogramma inschakelen in Azure wordt aangegeven](cloud-services-dotnet-diagnostics.md).

Als u wilt een cloudservice met implementaties van web rollen of werknemer rollen maakt, moet u [de servicepakket maken](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Voordat u begint

- Als u de SDK Azure nog niet hebt geïnstalleerd, klikt u op **Azure SDK installeren** om de [pagina Downloads van Azure](https://azure.microsoft.com/downloads/)te openen en vervolgens downloaden de SDK voor de taal waarin u liever het ontwikkelen van uw code. (U hebt een verkoopkans later hiervoor.)

- Als alle exemplaren van de rol een certificaat vereist, de certificaten hebt gemaakt. Cloudservices vragen om een .pfx-bestand met een persoonlijke sleutel. [Dat u kunt de certificaten Azure uploaden]() als u maken en implementeren van de cloudservice.

- Als u van plan bent om te implementeren van de cloudservice aan een groep affiniteit, de groep affiniteit hebt gemaakt. U kunt een groep affiniteit gebruiken om te implementeren van uw cloudservice en andere Azure services op dezelfde locatie in een gebied. U kunt de affiniteit-groep maken in het gebied **netwerken** van de Azure klassieke portal en klik op de pagina **affiniteit groepen** .


## <a name="create-and-deploy"></a>Maak en implementeer

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **Nieuw > virtuele Machines**, en schuif omlaag naar en klik op **Cloudservice**.

    ![Uw cloudservice publiceren](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. Onderaan op de informatiepagina die wordt weergegeven, klikt u op **maken**. 
4. Voer een waarde voor de **DNS-naam**in het nieuwe blad van de **Cloudservice** .
5. Een nieuwe **Resourcegroep** maken of Selecteer een bestaande eigenschap.
6. Selecteer een **locatie**.
7. Klik op **pakket**. Hiermee opent u het blad **een pakket uploaden** . Vul de vereiste velden.  

     Als een van de rollen één exemplaar bevatten, zorg **dat implementeren, zelfs als een of meer rollen één exemplaar bevatten** is geselecteerd.

8. Zorg ervoor dat de **begin-implementatie** is geselecteerd.
9. Klik op **OK** , die wordt het **uploaden van een pakket** blad te sluiten.
10. Als u geen certificaten om toe te voegen, klikt u op **maken**.

    ![Uw cloudservice publiceren](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Een certificaat uploaden

Als uw implementatiepakket [geconfigureerd voor gebruik van certificaten is](cloud-services-configure-ssl-certificate-portal.md#modify), kunt u het certificaat nu uploaden.

1. Selecteer **certificaten**, en klik op het blad **toevoegen certificaten** de SSL-certificaat .pfx-bestand en vervolgens het **wachtwoord** opgeven voor het certificaat
2. Klik op **bijvoegen certificaat**en klik vervolgens op **OK** in het blad **toevoegen certificaten** .
3. Klik op **maken** op het blad **Cloudservice** . Wanneer de implementatie de status **Gereed** bereikt is, kunt u doorgaan met de volgende stappen.

    ![Uw cloudservice publiceren](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>Controleer of uw implementatie is voltooid

1. Klik op het exemplaar van de cloud.

    De status moet worden weergegeven dat de service **uitgevoerd is**.

2. Klik onder **Essentials**, klikt u op de **Site-URL** naar de cloudservice in een webbrowser.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Volgende stappen

* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md)configureren.
* [Uw cloudservice beheren](cloud-services-how-to-manage-portal.md).
* [Ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md)configureren.