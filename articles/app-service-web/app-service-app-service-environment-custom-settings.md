<properties
    pageTitle="Aangepaste instellingen voor App-serviceomgevingen"
    description="Aangepaste configuratie-instellingen voor App-serviceomgevingen"
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
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Aangepaste configuratie-instellingen voor App-serviceomgevingen

## <a name="overview"></a>Overzicht ##
Omdat het App-serviceomgevingen zijn beperkt tot één klant, zijn er bepaalde configuratie-instellingen die uitsluitend voor App-Service-omgevingen kunnen worden toegepast. In dit artikel worden de verschillende specifieke aanpassingen die beschikbaar voor App-Service-omgevingen zijn.

Als u een omgeving van de Service-App niet hebt, raadpleegt u [het maken van een App-Service-omgeving](app-service-web-how-to-create-an-app-service-environment.md).

U kunt App-omgeving aanpassingen opslaan met behulp van een matrix in het nieuwe **clusterSettings** -kenmerk. Dit kenmerk is gevonden in de woordenlijst "Eigenschappen" van de *hostingEnvironments* Azure resourcemanager entiteit.

Het volgende afgekorte resourcemanager sjabloon fragment wordt het kenmerk **clusterSettings** :


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Het kenmerk **clusterSettings** kan worden opgenomen in een sjabloon resourcemanager bij de App-Service-omgeving.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Gebruik Azure Resource Explorer bijwerken van een App-Service-omgeving
U kunt ook kunt u de App-Service-omgeving bijwerken met behulp van [Azure Resource Explorer](https://resources.azure.com).  

1. In de Verkenner Resource, gaat u naar het knooppunt voor de App-Service-omgeving (**abonnementen** > **resourceGroups** > **providers** > **Micrososft.Web** > **hostingEnvironments**). Klik vervolgens op de specifieke App Service-omgeving, die u wilt bijwerken.

2. Klik op **Alleen-lezen/schrijven** in de bovenste werkbalk toe te staan dat interactieve bewerken in een Resource Explorer in het rechterdeelvenster.  

3. Klik op de blauwe **bewerken** knop de sjabloon resourcemanager om bewerkbaar te maken.

4. Schuif naar de onderkant van het rechterdeelvenster. Het kenmerk **clusterSettings** is helemaal onderaan, waar u kunt opgeven of de waarde bijwerken.

5. Typ (of kopieer en plak) de matrix van configuratiewaarden die u wilt dat in het kenmerk **clusterSettings** .  

6. Klik op de groene **plaatsen** naar de knop die aan de bovenkant van het rechterdeelvenster de wijziging doorvoeren in de App-Service-omgeving heeft gevonden.

Hoewel u de wijziging indient, duurt het ongeveer 30 minuten vermenigvuldigd met het aantal voorste eindigt op de omgeving van de App-Service zodat de wijziging van kracht.
Bijvoorbeeld als een App-Service-omgeving vier voorste uiteinden heeft, duurt het ongeveer twee uur voor de configuratie-update om te voltooien. Tijdens het wijzigen van de configuratie is die wordt geïmplementeerd, kunnen geen andere schaal bewerkingen of configuratie wijzigen bewerkingen plaatsvinden in de App-omgeving.

## <a name="disable-tls-10"></a>TLS 1.0 uitschakelen ##
Een terugkerende vraag van klanten, met name voor klanten die zijn omgaan met de naleving van de PCI controles, is het uitschakelen van expliciet TLS 1.0 voor hun apps.

TLS 1.0 kan worden uitgeschakeld door de volgende **clusterSettings** -vermelding:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Volgorde wijzigen TLS gecodeerde-suite ##
Een andere vraag van klanten is als ze de lijst met versleuteling gebruikt door hun server wijzigen en dit kan worden bereikt kunnen doordat de **clusterSettings** zoals hieronder wordt weergegeven. De lijst met beschikbare gecodeerde-suites uit [dit MSDN-artikel] kan worden opgehaald (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Als onjuiste waarden zijn ingesteld voor de gecodeerde-suite die SChannel kan geen kennen, alle TLS communicatie toe aan uw server mogelijk niet meer werkt. In dat geval moet u de vermelding *FrontEndSSLCipherSuiteOrder* verwijderen uit **clusterSettings** en indienen van de bijgewerkte resourcemanager-sjabloon als u wilt terugkeren naar de standaardinstellingen voor de gecodeerde-suite.  Gebruik deze functionaliteit voorzichtig.

## <a name="get-started"></a>Aan de slag
De sjabloonsite Azure Quickstart resourcemanager bevat een sjabloon met de basis definitie voor het [maken van een App-Service-omgeving](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).


<!-- LINKS -->

<!-- IMAGES -->
