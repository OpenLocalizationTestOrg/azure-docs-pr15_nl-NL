<properties
  pageTitle="Azure IoT Suite Veelgestelde vragen over | Microsoft Azure"
  description="Veelgestelde vragen over IoT Suite"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Veelgestelde vragen over IoT Suite

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Wat is het verschil tussen het verwijderen van een resourcegroep in de portal van Azure en te klikken op delete op een vooraf geconfigureerde oplossing in azureiotsuite.com?

- Als u de vooraf geconfigureerde oplossing in [azureiotsuite.com]verwijderen[lnk-azureiotsuite], u alle bronnen die heeft gekregen tijdens het maken van de vooraf geconfigureerde oplossing; verwijderen Als u aanvullende informatie hebt toegevoegd aan de resourcegroep, worden deze ook verwijderd. 

- Als u de resourcegroep in de [portal van Azure]verwijdert[lnk-azure-portal], u alleen de resources in die resourcegroep; verwijderen u moet ook verwijderen van de Azure Active Directory-toepassing die is gekoppeld aan de vooraf geconfigureerde oplossing in de [portal van Azure klassieke][lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van IoT Hub kan ik inrichten van een abonnement op? 

Tien. U kunt maken met een [Azure ondersteunen tickets] [ link-azuresupportticket] die deze limiet, maar al dan niet standaard, u kunt alleen inrichten tien IoT Hubs per abonnement, zoals wordt beschreven in [Azure abonnementen][link-azuresublimits]. Aangezien elke vooraf geconfigureerde oplossing een nieuwe IoT-Hub bepalingen, kunt u daardoor alleen maximaal tien vooraf geconfigureerde oplossingen in een bepaald abonnement inrichten. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van DocumentDB kan ik inrichten van een abonnement op?

Twaalf. U kunt maken met een [Azure ondersteunen tickets] [ link-azuresupportticket] die deze limiet, maar al dan niet standaard, kunt u alleen twaalf DocumentDB exemplaren per abonnement inrichten. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hoeveel gratis Bing Maps-API's kan ik inrichten van een abonnement op?

Twee. U kunt slechts twee interne transacties niveau 1 Bing Maps voor Enterprise-abonnementen maken in een Azure-abonnement. De externe controle oplossing is ingericht al dan niet standaard met het interne transacties niveau 1-abonnement. U kunt maximaal twee externe controle kan worden uitgevoerd in een abonnement zonder wijzigingen daardoor alleen inrichten.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Ik heb een externe controle oplossing-implementatie waarbij een statische kaart, hoe kan ik een interactieve Bing-kaart toevoegen? 
1. Uw Bing Maps-API voor Enterprise QueryKey krijgen van [Azure-portal][lnk-azure-portal]: 
 1. Navigeer naar de resourcegroep waarbij uw Bing Maps-API voor Enterprise zich in de [portal van Azure][lnk-azure-portal].
 2. Klik op alle instellingen, klikt u vervolgens Key Management. 
 3. U ziet twee sleutels: MasterKey en QueryKey. Kopieer de waarde voor QueryKey.

     > [AZURE.NOTE] Zie geen een Bing Maps-API voor Enterprise-account? Maken in de [portal van Azure] [ lnk-azure-portal] door te klikken op + nieuwe, te zoeken naar Bing Maps API voor Enterprise en volg de aanwijzingen om u te maken.

2. Open de meest recente code van de [Azure-IoT-Remote-cmdlets voor controle][lnk-remote-monitoring-github].

3. Een lokaal uitvoeren of cloud-implementatie opdrachtregel implementatierichtlijnen in de map /docs/ in de bibliotheek te volgen. 

4. Nadat u een lokale hebt uitgevoerd of cloud implementatie, zoek in de hoofdmap voor de *. user.config bestand gemaakt tijdens de implementatie. Dit bestand niet openen in een teksteditor. 

5. Wijzig de volgende regel als u wilt opnemen van de waarde die u hebt gekopieerd uit uw QueryKey: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kan ik een vooraf geconfigureerde oplossing maken als ik Microsoft Azure voor DreamSpark heb?
U kunt op dit moment kan een vooraf geconfigureerde oplossing maken met een [Microsoft Azure voor DreamSpark] [ lnk-dreamspark] account. U kunt echter een [gratis proefversie voor Azure-account] maken[ lnk-30daytrial] in een paar minuten waarmee u een vooraf geconfigureerde oplossing maakt.

### <a name="how-do-i-delete-an-aad-tenant"></a>Hoe verwijder ik een AAD-tenant?

Zie [Stapsgewijze instructies voor het verwijderen van een Azure AD-Tenant]voor blogbericht van Eric Golpe[lnk-delete-aad-tennant].

## <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de oplossingen IoT Suite vooraf geconfigureerde verkennen:

- [Overzicht van de blog onderhoud vooraf geconfigureerde-oplossingen][lnk-predictive-overview]
- [IoT beveiliging helemaal omhoog][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
