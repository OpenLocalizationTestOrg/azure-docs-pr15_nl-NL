<properties 
    pageTitle="Algemene cloud service management taken (klassieke) | Microsoft Azure" 
    description="Informatie over het beheren van cloudservices in de portal van Azure klassieke." 
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
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>Het beheren van Cloudservices

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-manage-portal.md)
- [Azure klassieke portal](cloud-services-how-to-manage.md)

In het gebied **Cloudservices** van de Azure klassieke portal kunt u een rol service of een implementatie bijwerken, verhogen van een gefaseerde implementatie naar productie, resources aan uw cloudservice koppelen, zodat u kunt zien van de resource afhankelijkheden en schaal van de bronnen samen en verwijderen van een cloudservice of een implementatie.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Hoe u: een cloud-service rol of implementatie bijwerken

Als u bijwerken van de toepassingscode voor uw cloudservice wilt, gebruikt u de **bijwerken** op het dashboard, **Cloud Services** -pagina of pagina **exemplaren** . U kunt een enkele of alle meerdere rollen bijwerken. U moet een nieuwe servicepakket en configuratie van servicebestand te uploaden.

1. Klik in de [klassieke Azure-portal](https://manage.windowsazure.com/)op het dashboard, **Cloud Services** -pagina of **exemplaren** pagina, klikt u op **bijwerken**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. Voer een naam voor de implementatie (bijvoorbeeld mycloudservice4) in **implementatie-label**. Het label implementatie onder **aan de slag** vindt u op het dashboard.

3. In **pakket**, gebruikt u **Bladeren** om het bestand in de service-pakket (.cspkg) te uploaden.

4. In **configuratie**, gebruikt u **Bladeren** om het bestand in de service-configuratie (.cscfg) te uploaden.

5. In **rol**, selecteer **alle** als u wilt bijwerken van alle rollen in de cloudservice. Als u wilt een één functie bijwerkt, selecteer de rol die u wilt bijwerken. Zelfs als u een specifieke rol bijwerken selecteert, worden de updates in het configuratiebestand service zijn toegepast op alle rollen.

6. Als de update het aantal rollen of de grootte van een rol wijzigt, selecteert u het selectievakje **toestaan bijwerken als rol formaat of het aantal rollen wijzigt** zodat de update te gaan. 

    Worden als u de grootte van een rol (dat wil zeggen, de grootte van een virtuele machine die een rol exemplaar is gehost) of het aantal rollen wijzigt, wordt elke rol-exemplaar (virtuele machine) opnieuw afbeelding moet en een lokale gegevens verloren die zijn.

7. Als de rollen van elke service slechts één exemplaar van de rol hebt, selecteert u de **zelfs als een of meer rol bevatten een selectievakje één exemplaar bijwerken** om te schakelen van de upgrade om verder te gaan. 

    Azure kan alleen beschikbaarheid van de service 99.95 percentage tijdens een update van de service cloud garanderen als elke rol ten minste twee rol-exemplaren (virtuele machines) heeft. Waarmee één virtuele machine clientaanvragen verwerken terwijl de andere wordt bijgewerkt.

8. Klik op **OK** (vinkje) om te beginnen met het bijwerken van de service.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Hoe u: implementaties te bevorderen van een gefaseerde implementatie aan productie uitwisselen

Met **uitwisselen** kunt u de implementatie van een tijdelijk opslaan van een cloudservice aan productie verhogen. Wanneer u besluit om te implementeren van een nieuwe versie van een cloudservice, kunt u fase en testen van uw nieuwe versie in de cloud-service-testomgeving terwijl uw klanten in productie de huidige versie gebruikt. Wanneer u klaar bent om te promoveren van de nieuwe versie aan productie, kunt u **uitwisselen** gebruiken om te schakelen van de URL's waarmee de twee implementaties zijn gericht. 

U kunt implementaties vanuit de **Cloud Services** -pagina of het dashboard uitwisselen.

1. In de [klassieke Azure-portal](https://manage.windowsazure.com/), klikt u op **Cloudservices**.

2. Klik in de lijst met cloudservices, op de cloudservice om deze te selecteren.

3. Klik op **uitwisselen**.

    Hiermee opent u het volgende bevestiging wordt gevraagd.

    ![Cloud Services uitwisselen](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Nadat u de implementatiegegevens controleren, klikt u op **Ja** om de implementaties uitwisselen.

    De implementatie-omwisselen snel gebeurt omdat het enige wat veranderende virtual IP-adressen (VIP's is) voor de implementaties.

    Als u wilt berekenen kosten hebt opgeslagen, kunt u de implementatie in de testomgeving verwijderen als u zeker weet dat de nieuwe productie-implementatie wordt uitgevoerd, zoals verwacht.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Hoe u: een resource koppelen naar een cloudservice

Als u wilt weergeven uw cloud van service afhankelijkheden van andere bronnen, kunt u een exemplaar van de Azure SQL-Database of een opslag-account koppelen aan de cloudservice. U kunt koppelen en ontkoppelen van resources op de pagina **Gekoppelde Resources** en klikt u vervolgens hun gebruik op het dashboard van de service cloud controleren. Als een gekoppelde opslag-account ingeschakeld voor controle heeft, kunt u totaal aantal aanvragen controleren op het dashboard van de service cloud.

Gebruik **koppelen** aan een nieuw of bestaand SQL-Database exemplaar of opslag account koppelen aan uw cloudservice. Vervolgens kunt u de database samen met de rol van de cloud-service die wordt gebruikt door op de pagina **schaal** schalen. (Een opslag-account wordt automatisch als gebruik toeneemt.) Zie [hoe u de schaal van een Cloudservice en gekoppelde bronnen](cloud-services-how-to-scale.md)voor meer informatie. 

U kunt ook bewaken, beheren en de schaal van de database in het knooppunt **Databases** van de Azure klassieke portal. 

Een resource in deze zin ' koppelen' verbinden niet met uw app de resource. Als u een nieuwe database via **koppeling**maakt, moet u de verbindingstekenreeksen toevoegen aan uw toepassingscode en vervolgens upgrade van de cloudservice. U moet ook verbindingstekenreeksen toevoegen als uw app resources in een gekoppelde opslag-account gebruikt.

De volgende procedure wordt beschreven hoe een nieuw exemplaar van SQL-Database, geïmplementeerd op een nieuwe SQL-Database-server, naar een cloudservice koppelen.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Een exemplaar van de SQL-Database koppelen naar een cloudservice

1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**. Klik vervolgens op de naam van de cloudservice om te openen van het dashboard.

2. Klik op **gekoppelde bronnen**.

    De pagina **Gekoppelde Resources** wordt geopend.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klik op de **koppeling een Resource** of de **koppeling**.

    De wizard **Koppeling Resource** wordt gestart.

    ![Koppeling pagina 1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klik op **een nieuwe resource maken** of **koppeling een bestaande bron**.

5. Kies het type resource wilt koppelen. Klik in de [klassieke Azure-portal](http://manage.windowsazure.com/)op **SQL-Database**. (De klassieke Preview Azure-portal biedt geen ondersteuning voor het koppelen van een opslag-account naar een cloudservice.)

6. U voltooit de databaseconfiguratie, volgt u de instructies in de help voor het gebied van de **SQL-Databases** van de Azure klassieke portal.

    U kunt de voortgang van de koppelingsbewerking in het berichtgebied volgen.

    ![Voortgang van koppelen](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Wanneer koppelen voltooid is, kunt u de status van de gekoppelde resource op het dashboard van de service cloud controleren. Zie voor informatie over de schaal van een gekoppelde SQL-Database, [hoe u de schaal van een Cloudservice en gekoppelde Resources](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Een gekoppelde resource Ontkoppel

1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**. Klik vervolgens op de naam van de cloudservice om te openen van het dashboard.

2. Klik op **Gekoppelde Resources**en selecteer vervolgens de resource.

3. Klik op **ontkoppelen**. Klik vervolgens op **Ja** bevestiging.

    Een SQL-Database losgekoppeld heeft geen invloed op de database of van de toepassing verbindingen met de database. U kunt nog steeds de database in het gebied van de **SQL-Databases** van de Azure klassieke-portal beheren.



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Hoe u: implementaties en een cloudservice verwijderen

Voordat u een cloudservice verwijderen kunt, moet u elke bestaande implementatie verwijderen.

Als u wilt berekenen kosten hebt opgeslagen, kunt u uw tijdelijk opslaan implementatie verwijderen nadat u controleren of uw productie-implementatie is werkt zoals verwacht. U bent gefactureerde berekeningscluster kosten voor rol exemplaren zelfs als een cloudservice niet wordt uitgevoerd.

Gebruik de volgende procedure om een implementatie of uw cloudservice te verwijderen. 

1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**.

2. Selecteer de cloudservice en klik vervolgens op **verwijderen**. (Selecteren een cloudservice zonder te openen van het dashboard, klik op een willekeurige plaats behalve de naam in de cloud-service-vermelding.)

    Als u een implementatie in staging- of hebt, ziet u een menu met opties die vergelijkbaar is met de volgende onderaan in het venster. Voordat u de cloudservice verwijderen kunt, moet u een bestaande implementaties verwijderen.

    ![Menu verwijderen](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. Als u wilt een implementatie verwijderen, klikt u op **productie-implementatie verwijderen** of **verwijderen van tijdelijk opslaan implementatie**. Klik vervolgens op bevestiging op **Ja**. 

4. Als u van plan bent om te verwijderen van de cloudservice, herhaalt u stap 3, indien nodig de andere implementatie verwijderen.

5. Als u wilt verwijderen in de cloudservice, klikt u op **Delete-cloudservice**. Klik vervolgens op bevestiging op **Ja**.

> [AZURE.NOTE]
> Als uitgebreide controle is geconfigureerd voor uw cloudservice, worden Azure gegevens niet verwijderd het controleren van uw opslag-account wanneer u de cloudservice verwijdert. U moet de gegevens handmatig verwijderen. Zie voor informatie over waar vind ik de tabellen aan de doelstellingen, "hoe: Access uitgebreide buiten de Azure klassieke portal-gegevens bewaken ' in [het beeldscherm Cloud Services](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Volgende stappen

 * [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure.md).
* Leer hoe u [een cloudservice](cloud-services-how-to-create-deploy.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name.md)configureren.
* [Ssl-certificaten](cloud-services-configure-ssl-certificate.md)configureren.
