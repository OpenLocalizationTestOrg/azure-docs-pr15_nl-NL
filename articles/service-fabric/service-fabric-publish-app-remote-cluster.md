<properties
    pageTitle="Een app publiceren naar een externe cluster met Visual Studio | Microsoft Azure"
    description="Informatie over het publiceren van een toepassing aan een externe service stof cluster met behulp van Visual Studio."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Het publiceren van een toepassing aan een externe cluster met behulp van Visual Studio

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

De extensie Azure-Service stof voor Visual Studio biedt een eenvoudige, herhaald en aanpasbare manier te publiceren van een toepassing aan een cluster Service stof.

## <a name="the-artifacts-required-for-publishing"></a>De onderdelen die zijn vereist voor publicatie

### <a name="deploy-fabricapplicationps1"></a>Implementeren FabricApplication.ps1

Dit is een PowerShell-script met een pad naar het profiel van het publiceren als een parameter voor publicerende stof Service-toepassingen. Aangezien dit script deel uit van uw toepassing maakt, bent u Welkom bij wijzig deze zo nodig is voor uw toepassing.

### <a name="publish-profiles"></a>Profielen publiceren

Een map in de Service stof application-project genaamd **PublishProfiles** bevat XML-bestanden die alle noodzakelijke informatie voor het publiceren van een toepassing, zoals opslaan:

- Service stof cluster verbindingsparameters
- Pad naar een bestand van de parameter toepassing
- Upgrade-instellingen

Standaard uw toepassing bevat twee publiceren profielen: Local.xml en Cloud.xml. U kunt meer profielen toevoegen door te kopiëren en plakken van een van de standaardbestanden.

### <a name="application-parameter-files"></a>Parameter toepassingsbestanden

Een map in de Service stof application-project genoemd **ApplicationParameters** bevat XML-bestanden voor de gebruiker ingestelde toepassing manifest parameterwaarden. Toepassing manifest-bestanden kunnen zijn ingesteld, zodat u verschillende waarden voor de implementatie-instellingen kunt. Meer informatie over de toepassing van parameters voorzien, raadpleegt u [meerdere omgevingen in Service stof beheren](service-fabric-manage-multiple-environment-app-configuration.md).

>[AZURE.NOTE] Voor acteur-services, moet u het project eerst voordat u het bestand in een editor of via het dialoogvenster publiceren bewerken maken. Dit is omdat het deel van de manifest-bestanden worden gegenereerd tijdens het bouwen.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>Een toepassing met behulp van het dialoogvenster publiceren-stof servicetoepassing publiceren

De volgende stappen wordt getoond hoe u een toepassing met behulp van het dialoogvenster **Servicetoepassing stof publiceren** is verstrekt door de Visual Studio Service stof Tools publiceert.

1. Kies op het snelmenu van het project-stof servicetoepassing **publiceren...** weergeven in het dialoogvenster **Servicetoepassing stof publiceren** .

    ![De ** publiceren Service stof toepassing **-dialoogvenster][0]

    Het bestand dat is geselecteerd in de vervolgkeuzelijst van de **doel-profiel** is waar alle instellingen, behalve de **bestandenlijst versies**, worden opgeslagen. U kunt een bestaand profiel opnieuw of u kunt een nieuwe record door te kiezen **< profielen beheren... >** in de vervolgkeuzelijst van de **doel-profiel** maken. Als u een profiel publiceren kiest, wordt de inhoud ervan weergegeven in de bijbehorende velden van het dialoogvenster. Om uw wijzigingen op elk gewenst moment hebt opgeslagen, kiest u de koppeling **Profiel opslaan** .    

2. Geef in de sectie **verbindingseindpunt** een lokale of externe Service stof cluster van publicerende eindpunt. Als u wilt toevoegen of wijzigen van het verbindingseindpunt, klik op de vervolgkeuzelijst **Verbindingseindpunt** . De lijst bevat de beschikbare Service stof cluster verbinding eindpunten waaraan u kunt publiceren op basis van uw Azure abonnementen. Houd er rekening mee dat als u bent niet al aangemeld naar Visual Studio, wordt u gevraagd dat te doen.

    Gebruik het dialoogvenster cluster selecteren en kies uit de set beschikbare abonnementen en clusters.

    ![De ** Selecteer Service stof Cluster **-dialoogvenster][1]

    >[AZURE.NOTE] Als u wilt publiceren naar een willekeurige eindpunt (zoals een cluster partijen), raadpleegt u de **publicatie op een willekeurige cluster eindpunt** sectie hieronder.

    Zodra u een eindpunt kiest, wordt de verbinding met de geselecteerde Service stof cluster in Visual Studio gevalideerd. Als het cluster niet is beveiligd, kunt Visual Studio verbinden met dit direct. Echter als het cluster beveiligd is, moet u een certificaat installeren op uw lokale computer voordat u verdergaat. Lees [hoe u de beveiligde verbindingen configureren](service-fabric-visualstudio-configure-secure-connections.md) voor meer informatie. Wanneer u klaar bent, klikt u op **OK** . Het geselecteerde cluster wordt weergegeven in het dialoogvenster **Servicetoepassing stof publiceren** .

3. Ga naar een bestand van de parameter toepassing in de vervolgkeuzelijst van de **Toepassing parameterbestand** . Een toepassing parameter-bestand bevat de gebruiker gedefinieerde waarden voor parameters in een bestand van de toepassing manifest. Als u wilt toevoegen of wijzigen van een parameter, klikt u op de knop **bewerken** . Opgeven of wijzigen van de parameter waarde in het raster **Parameters** . Wanneer u klaar bent, klikt u op de knop **Opslaan** .

    ![De ** bewerken Parameters **-dialoogvenster][2]

4. Gebruik het selectievakje **upgraden van de toepassing** om op te geven of deze actie publiceren is een upgrade. Upgrade publiceert acties die verschillen van de normale acties publiceren. Zie [Service stof toepassing upgraden](service-fabric-application-upgrade.md) voor een overzicht van de verschillen. Als u wilt configureren upgrade-instellingen, kiest u de koppeling **Upgrade-instellingen configureren** . De upgrade parameter-editor wordt weergegeven. Zie [de upgrade van een toepassing voor de Service stof configureren](service-fabric-visualstudio-configure-upgrade.md) voor meer informatie over parameters voor het bijwerken.

5. Kies de **bestandenlijst versie...** knop weergeven in het dialoogvenster **Versies bewerken** . U moet bijwerken-toepassing en Serviceversies voor een upgrade naar plaatsvinden. Zie [Service stof toepassing zelfstudie upgraden](service-fabric-application-upgrade-tutorial.md) voor meer informatie over hoe bestandenlijst versies impact een upgradeproces toepassing en service.

    ![De ** bewerken versies **-dialoogvenster][3]

    Als de toepassing en Serviceversies semantic versiebeheer zoals 1.0.0 of numerieke waarden in de notatie van 1.0.0.0 gebruiken, selecteert u de optie **automatisch bijwerken-toepassing en Serviceversies** . Wanneer u deze optie wordt de service kiezen en toepassing versienummers worden automatisch bijgewerkt wanneer de versie van een code, config of gegevens pakket wordt bijgewerkt. Als u liever de versies handmatig bewerken, schakelt u het selectievakje in als u wilt deze functie uitschakelen.

    >[AZURE.NOTE] Voor alle pakket posten voor een project acteur moet worden weergegeven, moet u eerst het project om te genereren van de items in de bestandenlijst van Service-bestanden maken.

6. Wanneer u klaar bent geven alle benodigde instellingen, klik op de knop **publiceren** naar uw toepassing aan de geselecteerde Service stof cluster publiceren. De instellingen die u hebt opgegeven worden toegepast op het proces publiceren.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Publiceren naar een willekeurige cluster eindpunt (inclusief partijen clusters)

De Visual Studio publiceren ervaring is geoptimaliseerd voor publicatie voor externe clusters die is gekoppeld aan een van uw Azure-abonnementen. Het is echter mogelijk te publiceren op een willekeurige eindpunten (zoals Service stof partijen clusters) door het profiel publiceren XML-rechtstreeks te bewerken. Zoals hierboven is beschreven, twee publiceren profielen worden verstrekt door standaard--**Local.xml** en **Cloud.xml**--, maar u bent Welkom bij het maken van extra profielen voor verschillende omgevingen. U wilt bijvoorbeeld een profiel voor publicatie voor nieuwjaarsfeest kolomgroepen misschien benoemde **Party.xml**maken.

Als u verbinding maakt met een onbeveiligde cluster, alle die is vereist is van het eindpunt van de verbinding cluster zoals `partycluster1.eastus.cloudapp.azure.com:19000`. In dat geval het verbindingseindpunt in het profiel publiceren aangepaste gegevens er ongeveer zo uitziet:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Als u verbinding met een beveiligde cluster maakt, moet u ook de gegevens van het clientcertificaat uit het lokale archief moet worden gebruikt voor verificatie. Zie [configureren beveiligde verbindingen naar een Service stof cluster](service-fabric-visualstudio-configure-secure-connections.md)voor meer informatie.

  Nadat uw profiel publiceren is ingesteld, kunt u het overzicht in het dialoogvenster publiceren zoals hieronder wordt weergegeven.

  ![Nieuw profiel publiceren in het dialoogvenster publiceren][4]

  Houd er rekening mee dat in dit geval het nieuwe profiel voor publiceren naar een van de standaard-toepassingsbestanden parameter verwijst. Deze instelling als u wilt dat de configuratie van de dezelfde toepassing publiceren naar een aantal omgevingen. Daarentegen in gevallen waarop u laten van verschillende configuraties voor elke omgeving die u publiceren wilt wilt naar zou het zinvol om een bijbehorende toepassing parameter-bestand te maken.

## <a name="next-steps"></a>Volgende stappen

Als u wilt weten hoe u het automatiseren van het publicatieproces in een omgeving met continue integratie, Zie [Service stof continue integratie instellen](service-fabric-set-up-continuous-integration.md).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
