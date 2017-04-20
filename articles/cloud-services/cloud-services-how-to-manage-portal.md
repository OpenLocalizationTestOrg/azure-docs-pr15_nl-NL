<properties 
    pageTitle="Veelvoorkomende beheertaken voor cloud-service | Microsoft Azure" 
    description="Informatie over het beheren van cloudservices in de portal van Azure. In deze voorbeelden wordt de Azure-portal gebruiken." 
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
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>Het beheren van Cloudservices

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-manage-portal.md)
- [Azure klassieke portal](cloud-services-how-to-manage.md)

Uw cloudservice wordt beheerd in het gebied **Cloudservices (klassieke)** van de Azure-portal. In dit artikel worden enkele veelgebruikte acties die u uitvoeren wilt tijdens het beheren van uw cloudservices. Waaronder de bijwerken, verwijderen, schaalbaarheid en promoveren van een gefaseerde implementatie naar productie.

Meer informatie over hoe u de schaal van de cloudservice vindt u [hier](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Hoe u: een cloud-service rol of implementatie bijwerken

Als u bijwerken van de toepassingscode voor uw cloudservice wilt, gebruikt u de **bijwerken** op het blad cloud-service. U kunt een enkele of alle meerdere rollen bijwerken. Als u wilt bijwerken, kunt u een nieuwe servicepakket of service configuratiebestand uploaden.

1. Selecteer in de [portal van Azure][], de cloudservice die u wilt bijwerken. Deze stap wordt het cloud service exemplaar blad geopend.

2. Klik in het blad, op de knop **bijwerken** .

    ![Knop bijwerken](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Werk de implementatie met een nieuwe service pakketbestand (.cspkg) en de configuratie servicebestand (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **(Optioneel)** bijwerken het label implementatie en het account opslag. 

5. Als geen rollen slechts één exemplaar van de rol hebt, selecteert u de **implementeren, zelfs als een of meer rollen één exemplaar bevatten** om in te schakelen van de upgrade om verder te gaan. 

    Azure kan alleen beschikbaarheid van de service 99.95 percentage tijdens een update van de service cloud garanderen als elke rol ten minste twee rol-exemplaren (virtuele machines) heeft. Met twee instanties van de rol, wordt één virtuele machine clientaanvragen verwerken, terwijl de andere wordt bijgewerkt.

6. Controleer de **implementatie starten** om de update is toegepast, nadat het uploaden van het pakket is voltooid.

7. Klik op **OK** om te beginnen met het bijwerken van de service.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Hoe u: implementaties te bevorderen van een gefaseerde implementatie aan productie uitwisselen

Wanneer u een nieuwe versie van een cloudservice, fase implementeren en test u uw nieuwe versie in de cloud-service-testomgeving bepaalt. Gebruik **uitwisselen** om te schakelen van de URL's waarop de twee implementaties zijn gericht en een nieuwe versie aan productie verhogen. 

U kunt implementaties vanuit de **Cloud Services** -pagina of het dashboard uitwisselen.

1. Selecteer in de [portal van Azure][], de cloudservice die u wilt bijwerken. Deze stap wordt het cloud service exemplaar blad geopend.

2. Klik in het blad, op de knop **uitwisselen** .

    ![Cloud Services uitwisselen](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Hiermee opent u het volgende bevestiging wordt gevraagd.

    ![Cloud Services uitwisselen](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Nadat u de implementatiegegevens controleren, klikt u op **OK** om de implementaties uitwisselen.

    De implementatie-omwisselen snel gebeurt omdat het enige wat veranderende virtual IP-adressen (VIP's is) voor de implementaties.

    Als u wilt berekenen kosten hebt opgeslagen, kunt u het tijdelijk opslaan implementatie verwijderen nadat u controleren of uw productie-implementatie is werkt zoals verwacht.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Hoe u: een resource koppelen naar een cloudservice

De Azure-portal niet is gekoppeld resources samen zoals de huidige Azure klassieke portal bevat. In plaats daarvan aanvullende informatiebronnen dashboard implementeren naar dezelfde resourcegroep wordt gebruikt door de Cloud-Service.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Hoe u: implementaties en een cloudservice verwijderen

Voordat u een cloudservice verwijderen kunt, moet u elke bestaande implementatie verwijderen.

Als u wilt berekenen kosten hebt opgeslagen, kunt u het tijdelijk opslaan implementatie verwijderen nadat u controleren of uw productie-implementatie is werkt zoals verwacht. U zijn gefactureerd voor berekeningscluster kosten voor geïmplementeerd rol-exemplaren die zijn gestopt.

Gebruik de volgende procedure om een implementatie of uw cloudservice te verwijderen. 

1. Selecteer in de [portal van Azure][], de cloudservice die u wilt verwijderen. Deze stap wordt het cloud service exemplaar blad geopend.

2. Klik in het blad, op de knop **verwijderen** .

    ![Cloud Services uitwisselen](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. U kunt de volledige cloudservice verwijderen door te schakelen **cloudservice en bijbehorende implementaties** of kiest u de **productie-implementatie** of de **tijdelijke-implementatie**.

    ![Cloud Services uitwisselen](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. Klik op de knop **verwijderen** onder.

5. Als u wilt verwijderen in de cloudservice, klikt u op **Delete-cloudservice**. Klik vervolgens op bevestiging op **Ja**.

> [AZURE.NOTE]
> Wanneer een cloudservice wordt verwijderd en uitgebreide controle is geconfigureerd, moet u de gegevens handmatig verwijderen uit uw opslag-account. Zie [Dit](cloud-services-how-to-monitor.md) artikel voor informatie over waar vind ik de tabellen aan de doelstellingen.

[Azure-portal]: https://portal.azure.com

## <a name="next-steps"></a>Volgende stappen

* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Leer hoe u [een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md)configureren.
* [Ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md)configureren.