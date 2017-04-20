<properties
    pageTitle="Methoden voor het doorsturen van verkeer Manager configureren | Microsoft Azure"
    description="In dit artikel wordt uitgelegd hoe u Hiermee configureert u verschillende methoden voor het doorsturen in verkeer Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Methoden voor het doorsturen van verkeer Manager configureren

Azure verkeer Manager biedt drie routeren methoden waarmee wordt bepaald hoe verkeer wordt doorgestuurd naar beschikbare service-eindpunten. De methode verkeer-routing wordt toegepast op elke ontvangen om te bepalen welke eindpunt moet worden geretourneerd in de reactie van de DNS-DNS-query's.

Er zijn drie methoden voor het doorsturen van verkeer beschikbaar is in verkeer Manager:

- **Prioriteit:** Selecteer 'Prioriteit' als u wilt gebruiken, een primaire service-eindpunt en geef de back-ups geval de primaire niet beschikbaar is.
- **Gewogen:** Selecteer 'gewogen' wanneer u het verkeer verdelen over een reeks eindpunten wilt wijzigen, hetzij gelijkmatig of op basis van de gewichten en welke u definiëren.
- **Prestaties:** Selecteer 'Prestaties' wanneer u de eindpunten in verschillende geografische locaties hebt en u wilt dat eindgebruikers gebruik van het eindpunt "eerstvolgende" wat betreft de laagste netwerklatentie.

## <a name="configure-priority-routing-method"></a>Prioriteit routeren methode configureren

Ongeacht de modus website bieden Azure Websites al foutvrije functionaliteit voor websites binnen een datacenter (ook wel bekend als een regio). Verkeer Manager biedt failover voor websites in verschillende datacenters.

Er is een algemene patroon voor service failover verkeer naar een primaire service verzenden en een reeks van identieke back-services voor failover. De volgende stappen wordt uitgelegd hoe u deze prioriteit failover configureert met Azure cloudservices en websites:

1. Klik op het pictogram **Verkeer Manager** u opent het deelvenster verkeer Manager in de Azure klassieke portal, in het linkerdeelvenster.
2. Zoek in het deelvenster verkeer Manager in de portal van Azure klassieke het verkeer Manager-profiel waarin de instellingen die u wilt wijzigen. Als u wilt openen de profielpagina van de instellingen, klikt u op de pijl rechts van de naam van een profiel.
3. Klik op de profielpagina op **eindpunten** boven aan de pagina. Controleer of de cloudservices en de websites die u wilt opnemen in uw configuratie aanwezig zijn.
4. Klik op **configureren** Klik boven aan de configuratiepagina openen.
5. Controleer of dat de methode verkeer rondsturen **overname is**voor **verkeer routeren methode-instellingen**. Als dit niet het geval is, klikt u op **Failover** in de vervolgkeuzelijst.
6. Pas de failover voor uw eindpunten voor **De lijst prioriteit Failover**. Wanneer u de methode **Failover** verkeer rondsturen selecteert, wordt de volgorde van de geselecteerde eindpunten het belang is. Het primaire eindpunt bevindt zich bovenaan. Gebruik de pijl omhoog en pijl-omlaag om de volgorde naar wens. Zie [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880)voor informatie over het instellen van de prioriteit failover met Windows PowerShell.
7. Controleer of de **Controle-instellingen** correct zijn geconfigureerd. Cmdlets voor controle zorgt ervoor dat eindpunten die offline zijn niet verkeer worden verzonden. U moet een pad en de bestandsnaam opgeven om de eindpunten, te houden. A schuine streep naar voren "/" geldige invoer voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard).
8. Nadat u uw configuratiewijzigingen hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
9. Test de wijzigingen in uw configuratie.
10. Nadat u uw profiel verkeer Manager werkt, bewerk de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het verkeer Manager-domein.

## <a name="configure-weighted-routing-method"></a>Gewogen routeren methode configureren

Een patroon algemene verkeer routeren methode is een reeks identieke eindpunten, waaronder cloudservices en websites, bieden en verkeer naar elk label in een manier round robin verzenden. De volgende stappen wordt uitgelegd hoe u dit type verkeer routeren methode configureert.

>[AZURE.NOTE] Azure Websites bieden al round robin van taakverdeling functionaliteit voor websites binnen een datacenter (ook wel bekend als een regio). Verkeer Manager kunt u opgeven round robin verkeer routeren methode voor websites in verschillende datacenters.

1. Klik op het pictogram **Verkeer Manager** u opent het deelvenster verkeer Manager in de Azure klassieke portal, in het linkerdeelvenster.
2. Zoek in het deelvenster verkeer Manager het verkeer Manager profiel waarin de instellingen die u wilt wijzigen. Als u wilt openen de profielpagina van de instellingen, klikt u op de pijl rechts van de naam van een profiel.
3. Klik op de pagina voor uw profiel op **eindpunten** boven aan de pagina en controleer of de eindpunten van de service die u wilt opnemen in uw configuratie aanwezig zijn.
4. Klik op **configureren** Klik boven aan de configuratiepagina openen op uw profielpagina.
5. Voor **verkeer routeren methode instellingen**, controleert u of de methode verkeer rondsturen **Round Robin**. Als dit niet het geval is, klikt u **Round Robin** in de vervolgkeuzelijst worden weergegeven.
6. Controleer of de **Controle-instellingen** correct zijn geconfigureerd. Cmdlets voor controle zorgt ervoor dat eindpunten die offline zijn niet verkeer worden verzonden. U moet een pad en de bestandsnaam opgeven om de eindpunten, te houden. A schuine streep naar voren "/" geldige invoer voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard).
7. Nadat u uw configuratiewijzigingen hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
8. Test de wijzigingen in uw configuratie.
9. Nadat u uw profiel verkeer Manager werkt, bewerk de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het verkeer Manager-domein.

## <a name="configure-performance-traffic-routing-method"></a>Prestaties verkeer routeren methode configureren

De prestaties verkeer routeren methode kunt u-verkeer door naar het eindpunt met de laagste latentie van netwerk van de klant. Het datacenter met de laagste latentie is meestal het dichtst in geografische afstand. Deze methode verkeer kan voor realtime wijzigingen in netwerkconfiguratie-account of laden.

1. Klik op het pictogram **Verkeer Manager** u opent het deelvenster verkeer Manager in de Azure klassieke portal, in het linkerdeelvenster.
2. Zoek in het deelvenster verkeer Manager het verkeer Manager-profiel waarin de instellingen die u wilt wijzigen. Als u wilt openen de profielpagina van de instellingen, klikt u op de pijl rechts van de naam van een profiel.
3. Klik op de pagina voor uw profiel op **eindpunten** boven aan de pagina en controleer of de eindpunten van de service die u wilt opnemen in uw configuratie aanwezig zijn.
4. Klik op **configureren** Klik boven aan de configuratiepagina openen op de pagina voor uw profiel.
5. Voor **verkeer routeren methode instellingen**, controleert u of de methode verkeer rondsturen * *prestaties*. Als dit niet het geval is, klikt u op * *prestaties** in de vervolgkeuzelijst.
6. Controleer of de **Controle-instellingen** correct zijn geconfigureerd. Cmdlets voor controle zorgt ervoor dat eindpunten die offline zijn niet verkeer worden verzonden. U moet een pad en de bestandsnaam opgeven om de eindpunten, te houden. A schuine streep naar voren "/" geldige invoer voor het relatieve pad en houdt in dat het bestand zich in de hoofdmap (standaard).
7. Nadat u uw configuratiewijzigingen hebt voltooid, klikt u op **Opslaan** onder aan de pagina.
8. Test de wijzigingen in uw configuratie.
9. Nadat u uw profiel verkeer Manager werkt, bewerk de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het verkeer Manager-domein.

## <a name="next-steps"></a>Volgende stappen

* [Verkeer Manager profielen beheren](traffic-manager-manage-profiles.md)
* [Methoden voor het doorsturen van verkeer Manager](traffic-manager-routing-methods.md)
* [Verkeer Manager instellingen testen](traffic-manager-testing-settings.md)
* [Een bedrijfsdomein Internet wijst u een domein verkeer Manager](traffic-manager-point-internet-domain.md)
* [Verkeer Manager eindpunten beheren](traffic-manager-manage-endpoints.md)
* [Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)
