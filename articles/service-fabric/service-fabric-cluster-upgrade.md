<properties
   pageTitle="Upgraden van een cluster Azure-Service stof | Microsoft Azure"
   description="Upgrade van de code-Service stof en/of de configuratie die wordt uitgevoerd als een Service stof cluster, inclusief het instellen van cluster updatemodus, een upgrade van certificaten, toe te voegen toepassing poorten, doen OS patches, enzovoort. Wat kunt u verwachten tijdens de upgrades worden uitgevoerd?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Upgraden van een cluster stof van Azure-Service

> [AZURE.SELECTOR]
- [Azure Cluster](service-fabric-cluster-upgrade.md)
- [Zelfstandige Cluster](service-fabric-cluster-upgrade-windows-server.md)

Voor elk moderne systeem is ontwerpen voor kunnen sleutel bij het bereiken van langdurige succes van uw product. Een cluster stof van Azure-Service is een resource dat u eigenaar bent, maar gedeeltelijk wordt beheerd door Microsoft. In dit artikel wordt beschreven wat automatisch wordt beheerd en wat u kunt zelf.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>De stof-versie die wordt uitgevoerd op uw Cluster bepalen

U kunt instellen dat uw cluster automatische stof upgrades, ontvangen wanneer geeft een nieuwe versie van Microsoft of kiest u een ondersteunde stof-versie die u wilt dat uw cluster op te selecteren.

Dit doet u door de configuratie van het 'upgradeMode' cluster instellen op de portal of resourcemanager op het moment van maken of hoger op een live cluster gebruiken 

>[AZURE.NOTE] Zorg ervoor dat uw cluster met een ondersteunde stof-versie altijd behouden. Als en dat we de versie van een nieuwe versie van service stof aankondigen, betekent dit dat de vorige versie zich na ten minste 60 dagen vanaf die datum is gemarkeerd voor een einde van de ondersteuning. de versies van de nieuwe zijn aangekondigde [op het teamblog van service stof](https://blogs.msdn.microsoft.com/azureservicefabric/ ). De nieuwe versie is beschikbaar voor kies vervolgens. 

14 dagen vóór het verstrijken van de versie die uw cluster actief is, een gebeurtenis systeemstatus gegenereerd die uw cluster in de status van een waarschuwing wordt geplaatst. Het cluster blijft de status van een waarschuwing totdat u een upgrade naar een ondersteunde stof-versie uitvoeren.


### <a name="setting-the-upgrade-mode-via-portal"></a>De upgrade modus via de portal instellen 

Wanneer u het cluster maakt, kunt u het cluster naar automatisch of handmatig instellen.

![Create_Manualmode][Create_Manualmode]

U kunt het cluster instellen op automatisch of handmatig wanneer u zich in een live cluster, de ervaring beheren gebruikt. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Een upgrade naar een nieuwe versie op een cluster die is ingesteld op handmatige modus via de portal.
 
U hoeft te is upgraden naar een nieuwe versie, selecteer de versie in de vervolgkeuzelijst en opslaan. De upgrade stof wordt automatisch gestarte. Het beleid van de servicestatus cluster (een combinatie van knooppunt gezondheid en de status alle toepassingen uitgevoerd in het cluster) wordt gehouden aan tijdens de upgrade.

Als het beleid van de servicestatus cluster is voldaan, wordt de upgrade hersteld. Schuif omlaag dit document voor meer informatie over het statusbeleid van de aangepaste instellen. 

Nadat u de problemen waardoor het terugdraaien hebt gecorrigeerd, moet u de upgrade opnieuw te starten door de stappen hierboven te volgen.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>De upgrade modus via een sjabloon resourcemanager instellen 

De configuratie 'upgradeMode' toevoegen aan de definitie van de resource Microsoft.ServiceFabric/clusters en de "clusterCodeVersion" ingesteld op een van de ondersteunde stof versies, zoals hieronder wordt weergegeven en vervolgens implementeren de sjabloon. De geldige waarden voor "upgradeMode" zijn 'Handmatig' of 'Automatisch'
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Een upgrade naar een nieuwe versie op een cluster die is ingesteld op handmatige modus via een resourcemanager-sjabloon.
 
Als het cluster handmatige modus, upgraden naar een nieuwe versie, wijzig de "clusterCodeVersion" naar een ondersteunde versie en het dashboard implementeren. De implementatie van de sjabloon, gang van de upgrade stof krijgt gestarte automatisch. Het beleid van de servicestatus cluster (een combinatie van knooppunt gezondheid en de status alle toepassingen uitgevoerd in het cluster) wordt gehouden aan tijdens de upgrade.

Als het beleid van de servicestatus cluster is voldaan, wordt de upgrade hersteld. Schuif omlaag dit document voor meer informatie over het statusbeleid van de aangepaste instellen. 

Nadat u de problemen waardoor het terugdraaien hebt gecorrigeerd, moet u de upgrade opnieuw te starten door de stappen hierboven te volgen.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Lijst met alle beschikbare versie voor alle omgevingen voor een bepaald abonnement ophalen

Voer de volgende opdracht en moet krijgt u een uitvoer ongeveer als volgt.

"supportExpiryUtc" Hiermee wordt aan uw wanneer een bepaalde versie is bijna verlopen of is verlopen. De meest recente versie is geen geldige datum - deze waarde heeft "9999-12-31T23:59:59.9999999 ', wat betekent dat de vervaldatum niet nog is ingesteld.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Stof upgrade gedrag wanneer het cluster Upgrade-modus automatisch is

Microsoft behoudt de stof code en configuratie die wordt uitgevoerd in een Azure cluster. We uitvoeren automatische upgrades voor gecontroleerde tot de software op basis van wens. Upgrades mogelijk code, configuratie of beide. Om ervoor te zorgen dat uw toepassing suffers geen impact of minimale impact vanwege upgrades, uitvoeren we de upgrades in de volgende stappen:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Fase 1: Een upgrade is uitgevoerd met behulp van beleidsregels voor alle cluster systeemstatus

Tijdens deze fase de upgrades één upgrade domein tegelijk gaan en de toepassingen die zijn uitgevoerd in het cluster worden uitgevoerd zonder eventuele uitvaltijd. Het beleid van de servicestatus cluster (een combinatie van knooppunt gezondheid en de status alle toepassingen uitgevoerd in het cluster) wordt gehouden aan tijdens de upgrade.

Als het beleid van de servicestatus cluster is voldaan, wordt de upgrade hersteld. En vervolgens een e-mailbericht is verzonden naar de eigenaar van het abonnement. Het e-mailbericht bevat de volgende informatie:

- Melding dat we hadden een upgrade cluster terugkeren.
- Voorgestelde corrigerende acties, indien van toepassing.
- Het aantal dagen (n) totdat we fase 2 uitvoeren.

We proberen om uit te voeren de dezelfde upgrade een paar vaker geval eventuele upgrades is mislukt vanwege de infrastructuur. De n dagen vanaf de datum die het e-mailbericht is verzonden, we verder gaan met fase 2.

Als het beleid van de servicestatus cluster wordt voldaan, is de upgrade als geslaagd beschouwd en deze als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen bevestiging e-mail van een juist is uitgevoerd. Dit is om te voorkomen dat u verzendt u te veel e-mailberichten--ontvangen een e-mailbericht moet worden beschouwd als een uitzondering op normaal. We verwachten grootste deel van de upgrades cluster te kunnen uitvoeren zonder die invloed hebben op de beschikbaarheid van uw toepassingen.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Fase 2: Een upgrade is uitgevoerd met behulp van de servicestatus standaardbeleidsregels alleen

Het beleid van de servicestatus in deze fase worden zodanig dat het nummer van de toepassingen die aan het begin van de upgrade in orde zijn hetzelfde voor de duur van het upgradeproces blijft ingesteld. Zoals fase 1, in de fase 2-upgrades één upgrade domein tegelijk gaan en de toepassingen die zijn uitgevoerd in het cluster worden uitgevoerd zonder eventuele uitvaltijd. Het beleid van de servicestatus cluster (een combinatie van knooppunt gezondheid en de status alle toepassingen uitgevoerd in het cluster) wordt gehouden aan voor de duur van de upgrade.

Als het beleid van de servicestatus cluster van kracht is voldaan, wordt de upgrade hersteld. En vervolgens een e-mailbericht is verzonden naar de eigenaar van het abonnement. Het e-mailbericht bevat de volgende informatie:

- Melding dat we hadden een upgrade cluster terugkeren.
- Voorgestelde corrigerende acties, indien van toepassing.
- Het aantal dagen (n) totdat we fase 3 uitvoeren.

We proberen om uit te voeren de dezelfde upgrade een paar vaker geval eventuele upgrades is mislukt vanwege de infrastructuur. Een herinnering is een paar dagen voordat n dagen omhoog zijn verzonden. De n dagen vanaf de datum die het e-mailbericht is verzonden, we verder gaan naar de fase 3. Het e-mailberichten die wij u in fase 2 sturen moeten worden genomen serieus en corrigerende acties moeten worden genomen.

Als het beleid van de servicestatus cluster wordt voldaan, is de upgrade als geslaagd beschouwd en deze als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen bevestiging e-mail van een juist is uitgevoerd.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Fase 3: Een upgrade is uitgevoerd met behulp van beleidsregels voor agressieve systeemstatus

Dit beleid servicestatus in deze fase zijn gericht aan de upgrade in plaats van de status van de toepassingen. Weinig cluster upgrades uiteindelijk in deze fase. Als uw cluster, wordt onmiddellijk deze fase, ziet een grote kans dat uw toepassing wordt beschadigd en/of beschikbaarheid kwijtraakt.

Vergelijkbaar met de andere twee fasen, fase 3 upgrades Ga verder één upgrade domein tegelijk.

Als het beleid van de servicestatus cluster is voldaan, wordt de upgrade hersteld. We proberen om uit te voeren de dezelfde upgrade een paar vaker geval eventuele upgrades is mislukt vanwege de infrastructuur. Het cluster wordt daarna vastgemaakt, zodat deze niet meer ondersteuning en/of upgrades, ontvangt.

Een e-mailbericht met deze informatie wordt verzonden naar de eigenaar van het abonnement, samen met de corrigerende acties. Wij verwachten niet alle clusters om in de modus waarin fase 3 is mislukt.

Als het beleid van de servicestatus cluster wordt voldaan, is de upgrade als geslaagd beschouwd en deze als voltooid gemarkeerd. Dit kan gebeuren tijdens de upgrade of een van de upgrade herhalingen in deze fase. Er is geen bevestiging e-mail van een juist is uitgevoerd.

## <a name="cluster-configurations-that-you-control"></a>Clusterconfiguraties waarmee u kunt u bepalen

Upgrade uitvoeren modus Naast de mogelijkheid om in te stellen van het cluster, Hier volgen de configuraties die u op een live cluster kunt wijzigen.

### <a name="certificates"></a>Certificaten

U kunt nieuwe toevoegen of verwijderen van certificaten voor de client via de portal voor cluster en eenvoudig. Raadpleeg [Dit document voor gedetailleerde instructies](service-fabric-cluster-security-update-certs-azure.md)

![Schermafbeelding waarin de certificaat-vingerafdrukken wordt in de portal van Azure.][CertificateUpgrade]


### <a name="application-ports"></a>Toepassing-poorten

U kunt de toepassing poorten wijzigen door de eigenschappen van taakverdeling resource die zijn gekoppeld aan het knooppunttype te wijzigen. U kunt de portal of kunt u rechtstreeks resourcemanager PowerShell gebruiken.

Ga als volgt te werk om een nieuwe poort op alle VMs openen in een knooppunttype:

1. Voeg een nieuwe test aan de juiste taakverdeling.

    Als u uw cluster met behulp van de portal geïmplementeerd, de netwerktaakverdelers heten "Kg-naam van de Resource groep-NodeTypename", één telefoonnummer voor elk knooppunttype. Aangezien de namen van de verdeling van laden alleen uniek binnen een resourcegroep zijn, is het beste als u deze onder een bepaalde resourcegroep opzoeken.

    ![Schermafbeelding waarin wordt weergegeven voor het toevoegen van een test aan een taakverdeling in de portal.][AddingProbes]

2. Een nieuwe regel toevoegen aan de taakverdeling.

    Een nieuwe regel toevoegen aan de dezelfde taakverdeling met behulp van de test die u in de vorige stap hebt gemaakt.

    ![Een nieuwe regel toevoegen aan een taakverdeling in de portal.][AddingLBRules]


### <a name="placement-properties"></a>Plaatsing eigenschappen

Voor elk van de knooppunttypen, kunt u de plaatsing van de aangepaste eigenschappen die u wilt gebruiken in uw toepassingen toevoegen. NodeType is een standaardeigenschap die u gebruiken kunt zonder deze expliciet toe te voegen.

>[AZURE.NOTE] Voor meer informatie over het gebruik van plaatsing beperkingen, eigenschappen knooppunt en hoe u deze definiëren, raadpleegt u de sectie 'Plaatsing beperkingen en eigenschappen knooppunt' in het servicebeheer stof Cluster Resource-Document op [Uw Cluster waarin wordt beschreven](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="capacity-metrics"></a>De doelstellingen van de capaciteit

Voor elk van de knooppunttypen, kunt u aangepaste capaciteit aan de doelstellingen die u wilt gebruiken in uw rapport laden-toepassingen toevoegen. Voor meer informatie over het gebruik van de capaciteit maatstelsel rapport laden, verwijst naar de Service stof Cluster resourcemanager-documenten op [Uw Cluster waarin wordt beschreven](service-fabric-cluster-resource-manager-cluster-description.md) en [aan de doelstellingen en laden](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Stof upgrade-instellingen - systeemstatus-beleid

Aangepaste status beleid voor de upgrade stof, kunt u opgeven. Als u uw cluster hebt ingesteld voor automatische stof upgrades, krijgen tot de fase-1 van de automatische stof upgrades dit beleid toegepast.
Als u hebt uw cluster handmatige stof upgrades, krijgen dit beleid telkens wanneer die u een nieuwe versie activeert het systeem op de upgrade stof in uw cluster gang selecteert toegepast. Als u niet het beleid boven, worden de standaardwaarden gebruikt.

U kunt het statusbeleid aangepaste opgeven of de huidige instellingen onder het blad 'stof upgrade' door te selecteren van de geavanceerde upgrade-instellingen te controleren. Bekijk de volgende afbeelding over het. 

![Aangepaste statusbeleid beheren][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Stof-instellingen voor uw cluster aanpassen

Ga naar [service stof cluster stof-instellingen](service-fabric-cluster-fabric-settings.md) op wat en hoe u ze kunt aanpassen.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>OS patches op de VMs waaruit het cluster

Deze functie is gepland voor de toekomst als een geautomatiseerde functie. Maar momenteel, u bent verantwoordelijk voor uw VMs patch. U moet deze één VM stap per keer, zodat u pas van omlaag meer dan één voor één kracht worden.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>Klik op de VMs waaruit het cluster upgrades voor het besturingssysteem

Als u de afbeelding OS op de virtuele machines van het cluster bijwerkt moet, moet u dit één VM doen tegelijk. U bent verantwoordelijk voor deze upgrade--er is momenteel geen automatisering hiervoor.

## <a name="next-steps"></a>Volgende stappen
- Informatie over het aanpassen van enkele van de [service stof cluster stof-instellingen](service-fabric-cluster-fabric-settings.md)
- Leer hoe u [uw cluster in-en uitfaden schaal](service-fabric-cluster-scale-up-down.md)
- Meer informatie over de [toepassingsupgrades](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG