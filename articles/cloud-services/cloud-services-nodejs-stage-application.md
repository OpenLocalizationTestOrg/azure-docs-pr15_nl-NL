<properties 
    pageTitle="Fase een cloud-service-implementatie (Node.js) | Microsoft Azure" 
    description="Leer hoe u uw Azure-toepassingen naar een testomgeving distribueren en implementeren naar een productieomgeving virtuele IP-(VIP) uitwisselen." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="staging-an-application-in-azure"></a>Tijdelijke van een toepassing in Azure wordt aangegeven

Een verpakt toepassing kan worden toegepast op de testomgeving in Azure te testen voordat u deze verplaatst naar de productieomgeving waarin de toepassing toegankelijk op Internet is. De testomgeving is precies zoals de productieomgeving, behalve dat u kunt alleen toegang tot de gefaseerde toepassing met een verborgen URL die wordt gegenereerd door Azure. Nadat u hebt gecontroleerd of uw toepassing correct werkt, kan deze door een virtueel IP-(VIP) omwisselen te voeren naar de productieomgeving worden geïmplementeerd.

> [AZURE.NOTE] De stappen in dit artikel zijn alleen van toepassing op knooppunt toepassingen die worden gehost als een Azure-Cloudservice.

## <a name="step-1-stage-an-application"></a>Stap 1: Een toepassing van fase

Deze taak wordt uitgelegd hoe u een toepassing met behulp van de **Microsoft Azure PowerShell**installeert.

1.  Bij het publiceren van een service, geven de **-Slot** parameter voor de cmdlet **Publiceren-AzureServiceProject** .

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Meld u aan bij de [portal van Azure klassieke] en selecteer **Cloudservices**. Nadat de cloudservice is gemaakt en de status van de kolom **tijdelijke** **actief**is bijgewerkt, klikt u op de naam van de service.

    ![Portal voor het weergeven van een actieve service][cloud-service]

3.  Selecteer het **Dashboard**en selecteer vervolgens **tijdelijke**.

    ![cloud servicedashboard][cloud-service-dashboard]

4. Houd rekening met de waarde in de vermelding van de **Site-URL** naar rechts. De DNS-naam is een verborgen interne ID die Azure gegenereerd.

    ![site-url][cloud-service-staging-url]

U kunt nu controleren of de toepassing correct in de testomgeving werkt via het tijdelijk opslaan site-URL.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Stap 2: Een toepassing in door een Bezig VIP's upgraden

Nadat u de bijgewerkte versie van een toepassing in de testomgeving hebt gecontroleerd, kunt u snel het beschikbaar maken in productie door het virtuele IP-adressen (VIP's) van de tijdelijke en productieomgevingen verwisselen.

> [AZURE.NOTE] Deze stap wordt ervan uitgegaan dat u hebt al een toepassing voor de productie geïmplementeerd en de bijgewerkte versie van de toepassing gefaseerde.

1.  Meld u aan bij de [portal van Azure klassieke] **Cloudservices** op en selecteer vervolgens de servicenaam van de.

2.  Vanuit het **Dashboard**, selecteert u **tijdelijke**en klik vervolgens op **wisselen** onder aan de pagina. Hiermee opent u het dialoogvenster VIP uitwisselen.

    ![VIP omwisselen dialoogvenster][vip-swap-dialog]

3.  Controleer de gegevens en klik vervolgens op **OK**. De twee implementaties beginnen met het bijwerken van terwijl het tijdelijk opslaan implementatie schakelt u naar productie en schakelt u het productie-implementatie naar tijdelijke.

U hebt een implementatie gefaseerde en een productie-implementatie bijgewerkt door VIP's met de implementatie in tijdelijke verwisselen.

## <a name="additional-resources"></a>Aanvullende informatie

- [Het implementeren van een Service-Upgrade voor productie op wisselen VIP's in Azure wordt aangegeven]

[Azure klassieke portal]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Het implementeren van een Service-Upgrade voor productie op wisselen VIP's in Azure wordt aangegeven]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
