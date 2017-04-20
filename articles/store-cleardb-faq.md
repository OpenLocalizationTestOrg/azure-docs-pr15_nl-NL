<properties
    pageTitle="Veelgestelde vragen over ClearDB MySql-databases met Azure App Service | Microsoft Azure"
    description="Antwoorden op veelgestelde vragen over het gebruik van ClearDB MySQL-databases met Azure App-Service."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Veelgestelde vragen over ClearDB MySql-databases met Azure App-Service

Deze Veelgestelde vragen over antwoorden op veelgestelde vragen over het gebruiken en ClearDB MySQL-databases kopen voor Azure Web Apps.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Welke opties heb ik voor MySQL op Azure?

Hebt u verschillende opties:

* [ClearDB gedeeld MySQL-database](/marketplace/partners/cleardb/databases/)

* [ClearDB MySQL Premium clusters](/marketplace/partners/cleardb-clusters/cluster/)

* [MySQL cluster op een Azure-VM](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [Één exemplaar van MySQL uitgevoerd op een Azure-VM](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB een MySQL hostingservice is en de MySQL-infrastructuur voor u beheert. Wanneer u uw eigen MySQL cluster of een database op een Azure virtuele machines uitvoert, moet u de MySQL-server instellen en werken met patches.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Heb ik een creditcard nodig voor de Web-app + MySQL-sjabloon in de Azure Marketplace?

Dit hangt af van het type abonnement dat u gebruikt. Hier volgen enkele typen veelgebruikte abonnement:

* [Omslagstelsel](/offers/ms-azr-0003p/): zijn uitgerust met een creditcard en wanneer u een betaald MySQL-database uw creditcard is geheven koopt.

* [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/): tegoeden bevat voor gebruik met Microsoft Azure services maar aankoop van derden resources niet is toegestaan. Om aan te schaffen derden services of een betaald MySQL-database, moet u een creditcard gebruiken ingeschakeld abonnement. U kunt Web-Apps voor een gratis ClearDB MySQL-database maken.

* [MSDN-abonnement](/pricing/member-offers/msdn-benefits/) en **MSDN ontwikkelaar Test betalen als u Ga**: vergelijkbaar met gratis proefversie, een MSDN-abonnement, moet u een creditcard naar een betaald MySQL-oplossing Schaf via ClearDB hebben.

* [Enterprise Agreement (EA)](/pricing/enterprise-agreement/): EA klanten worden gefactureerd ten opzichte van hun EA elk kwartaal voor alle van hun aankopen Azure Marketplace (externe) op een afzonderlijke, geconsolideerde factuur. U zijn gefactureerd buiten de monetaire vastlegging voor elke aankopen marketplace. Houd er rekening mee dat op dit moment kan Azure Store niet beschikbaar voor klanten in Azerbeidzjan, Kroatië, Noorwegen en Puerto Rico geregistreerd is. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): U kunt alleen gratis ClearDB databases maken voor Web Apps. Er is geen limiet van het aantal gratis ClearDB MySQL-databases die u kunt maken. Houd er rekening mee dat gratis databases niet worden gebruikt voor productie WebApps, zijn zoals deze service is alleen bedoeld voor proefabonnement.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Waarom is ik $3,50 voor een Web-app + MySQL van Azure Marketplace betalen?

De standaardoptie voor de database is Titan, dat wil $3,50 zeggen. We de kosten niet weergeven tijdens het maken van de database en u per ongeluk een database die u niet wilde aanschaffen. We probeert te zoeken naar een manier om de te verbeteren, maar tot dan moet u alle uw geselecteerde prijzen lagen voor WebApp en database controleren voordat u op **maken** te klikken en de implementatie van de bronnen starten.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Ik heb MySQL op mijn eigen Azure virtuele machines. Kan ik verbinding mijn Azure WebApp maken met mijn database?

Ja. Zo lang maken als uw VM Azure externe toegang heeft verleend tot uw web-app, kunt u uw web-app naar uw database. Zie [MySQL installeren op een virtuele machine](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)voor meer informatie.

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Waar zijn de landen ClearDB Premium MySQL-clusters ondersteund?

[ClearDB Premium MySQL-clusters](/marketplace/partners/cleardb-clusters/cluster/) zijn beschikbaar in alle Azure regio's overal ter wereld met uitzondering van India, Australië Brazilië Zuid en China.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Kan ik een nieuw cluster vóór het maken van een database met ClearDB premium clusteroplossing maken?

Nee, u lege ClearDB clusters maakt wordt niet ondersteund. De portal van Azure kunt u databases maken in een cluster, waarmee een nieuw cluster op hetzelfde moment kan worden gemaakt.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Ik gewaarschuwd als ik me probeer aan het verwijderen van een ClearDB MySQL-database wordt gebruikt door een van mijn-toepassingen?

Nee, Azure ontvangt u geen waarschuwing als u een marketplace-aankoop die afhankelijk van de toepassing verwijderen.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Welke regio's kan ik ClearDB databases in maken?

Azure Marketplace is niet beschikbaar voor klanten in Azerbeidzjan, Kroatië, Noorwegen of Puerto Rico geregistreerd. ClearDB is niet beschikbaar in deze regio's.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Welke prijzen laag moet ik kiezen voor een productie WebApp en de database?

Basic of een hogere prijzen laag gebruiken voor de Web Apps. Voor ClearDB raden een Saturnus of Jupiter-abonnement. Bekijk de functies en beperkingen van elke laag prijzen voor zowel [Web Apps](/pricing/details/app-service/) en [ClearDB MySQL-databases](/marketplace/partners/cleardb/databases/) om te kiezen dat aan uw wensen voldoet.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Hoe kan ik mijn ClearDB-database van het ene plan upgraden naar de andere?

Klik in de [portal van Azure](https://portal.azure.com)kunt u de schaal van een ClearDB gedeelde hostingprovider-database. Lees dit [artikel](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) voor meer informatie. We de upgrade momenteel geen ondersteuning voor ClearDB Premium clusters in de portal van Azure.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Ik zie niet mijn database ClearDB in Azure-portal?

Als we ClearDB-database met behulp van Azure resourcemanager of [Nieuwe Azure-Portal](https://portal.azure.com)maakt, kunt deze is niet zichtbaar in de [Oude Azure-Portal](https://manage.windowsazure.com). Als u wilt werken-rond dit is naar uw database handmatig koppeling naar de web-app. Op dezelfde manier als ClearDB-database maken in de [oude portal](https://manage.windowsazure.com) is niet mogelijk om de database in de [Nieuwe Portal van Azure](https://portal.azure.com)weer te geven. Er is geen tijdelijke oplossing voor het tweede scenario.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Wie ik contact opnemen met voor ondersteuning wanneer mijn database uitgeschakeld is?

Contactpersonen [ClearDB ondersteuning](https://www.cleardb.com/developers/help/support) voor elke database gerelateerde problemen. Opgesteld zodat ze uw Azure abonnementsgegevens.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Kan ik meer gebruikers maken voor mijn ClearDB MySQL-database clusteroplossing? 

Nee. U kunt geen extra gebruikers maken, maar u kunt extra databases op uw cluster ClearDB-database maken.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Kan Basic/Pro reeks databases worden in-place vergelijkbaar is met vandaag planeten abonnementen op ClearDB portal bijgewerkt?

Ja, eenvoudige reeks databases kunnen worden bijgewerkt in-place (eenvoudige 60 tot en met eenvoudige 500). Pro reeks kan worden bijgewerkt in-place (Pro 125 tot en met Pro 1000), behalve voor Pro 60. Opwaarderen Pro 60 momenteel wordt niet ondersteund. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Wanneer ik mijn resources uit één abonnement naar een andere overzet, Mijn ClearDB MySQL-database worden overgezet ook? 

Wanneer u een resource migratie uitvoert in abonnementen, gelden enkele [beperkingen](./app-service-web/app-service-move-resources.md) . Een ClearDB MySQL-database is een derde partij-service en dus niet krijgen gemigreerd tijdens de migratie van Azure-abonnement. Als u niet de migratie van uw MySQL-database vóór migreren Azure resources beheren, kan uw ClearDB MySQL-databases kunnen worden uitgeschakeld. Handmatig eerst migreren uw databases en voer Azure abonnement migratie voor uw web-app. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Kan ik een database ClearDB van een abonnement creditcard wilt overzetten naar een EA-abonnement?

Bestaande ClearDB databases gebruiken de creditcard die is gekoppeld aan de bestaande abonnementen. Als een EA-abonnement wilt gebruiken die u wilt migreren van uw gegevens naar een nieuwe database:

* Een nieuwe database maken ClearDB met uw EA-abonnement kopen.
* Uw gegevens naar uw nieuwe database migreren.
* Werk uw toepassing voor het gebruik van de nieuwe database.
* Verwijder de oude ClearDB-database.

Wanneer u een nieuwe WebApp met MySQL (ClearDB maken) of een MySQL-database (ClearDB) maakt, bepaalt het abonnement dat u kiezen hoe u betaalt voor de service. Met een abonnement EA blokkeert we niet de aanschaf van de derde partij services zoals ClearDB in de portal van Azure. EA abonnementen zijn gefactureerd buiten monetaire resourcebeperkingen en per kwartaal en achterstallig zijn gefactureerd. De klant EA zou moeten een betalingsmethode zoals een creditcard instellen om te betalen voor een derde partij marketplace-services.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Waar kan ik de kosten voor resources ClearDB in een abonnement EA zien?

Voor klanten met directe EA zijn de kosten van Azure Marketplace zichtbaar is op de Enterprise-Portal. Houd er rekening mee dat alle marketplace aankopen en verbruik zijn gefactureerd buiten monetaire resourcebeperkingen en per kwartaal en achterstallig zijn gefactureerd. EA klanten moeten betalen rechtstreeks naar de derde partij serviceprovider en kunnen dit doen doordat een betalingsmethode zoals een creditcard met hun EA-account.

Indirect EA klanten hun Azure Marketplace-abonnementen kunnen vinden op de pagina **Abonnementen beheren** van de Enterprise-Portal, maar prijzen is verborgen. Klanten contact moeten opnemen met hun gelaagde Serviceprovider voor meer informatie over marketplace kosten.

Toegang tot Azure Marketplace voor externe services kan worden beheerd door uw EA Azure registratie-beheerders. Ze kunnen uitschakelen of opnieuw inschakelen access op 3e partijen aankopen uit het archief in abonnementen en Accounts beheren onder de sectie Accounts in de Enterprise-Portal.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Wie ik contact opnemen met voor vragen over mijn factuur voor ClearDB services in mijn abonnement EA?

Neem contact op met [De klantenondersteuning Enterprise](http://aka.ms/AzureEntSupport) met betrekking tot facturering onder hun registratie EA. Het ondersteuningsteam van EA Portal wordt uw vraag beantwoorden of uw probleem op te lossen.

 



## <a name="more-information"></a>Meer informatie

[Veelgestelde vragen over Azure Marketplace](/marketplace/faq/)
