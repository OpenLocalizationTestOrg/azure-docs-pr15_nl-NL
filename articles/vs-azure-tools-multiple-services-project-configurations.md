<properties
   pageTitle="Het configureren van uw Azure project met meerdere configuraties | Microsoft Azure"
   description="Leer hoe u een project Azure cloud-service configureren door te wijzigen van de ServiceDefinition.csdef en ServiceConfiguration.cscfg-bestanden."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Uw Azure Project met meerdere configuraties configureren

Een project Azure cloud-service bevat twee configuratiebestanden: ServiceDefinition.csdef en ServiceConfiguration.cscfg. Deze bestanden worden verpakt met uw Azure cloud-servicetoepassing en geïmplementeerd in Azure.

- Het bestand **ServiceDefinition.csdef** bevat de metagegevens die is vereist door de Azure-omgeving voor de vereisten van uw cloud-servicetoepassing, inclusief welke rollen die deze bevat. Dit bestand bevat ook configuratie-instellingen die van toepassing op alle exemplaren. Deze configuratie-instellingen kunnen worden gelezen gedurende runtime de Azure-Service hostingprovider Runtime-API gebruiken. Dit bestand kan niet worden bijgewerkt terwijl uw service wordt uitgevoerd in Azure wordt aangegeven.

- Het bestand **ServiceConfiguration.cscfg** sets waarden voor de configuratie-instellingen die is gedefinieerd in het definitiebestand service en geeft het aantal exemplaren om uit te voeren voor elke rol. Dit bestand kan worden bijgewerkt terwijl uw cloudservice wordt uitgevoerd in Azure wordt aangegeven.

De hulpmiddelen Azure voor Microsoft Visual Studio bieden pagina's die u gebruiken kunt om in te stellen die zijn opgeslagen in deze bestanden configuratie-instellingen. Voor toegang tot de eigenschappenvensters, dubbelklik op de verwijzing rol onder het project Azure cloud-service in Solution Explorer of met de rechtermuisknop op de verwijzing rol en kies **Eigenschappen**, zoals wordt weergegeven in de volgende afbeelding.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Voor informatie over de onderliggende schema's voor de servicedefinitie en bestanden van de service-configuratie, raadpleegt u [Schemaverwijzing](https://msdn.microsoft.com/library/azure/dd179398.aspx). Zie [hoe u Cloud-Services configureren](./cloud-services/cloud-services-how-to-configure.md)voor meer informatie over de service te configureren.

## <a name="configuring-role-properties"></a>Roleigenschappen van de configureren

De eigenschappenvensters voor een Webrol en een rol werknemer zijn vergelijkbaar, maar er zijn ook enkele verschillen, in de volgende secties duidelijk.

U kunt de Azure caching-services configureren op de pagina **cache** .

### <a name="configuration-page"></a>Configuratiepagina

Klik op de pagina **configuratie** kunt u deze eigenschappen instellen:

**Exemplaren**

Stel de eigenschap van de tellen **exemplaar van de werkstroom** voor het aantal exemplaren die voor deze functie moet worden uitgevoerd door de service.

Stel de eigenschap **VM grootte** naar **Extra kleine**, **klein**, **Gemiddeld**, **groot**of **Extra groot**.  Zie [grootten voor Cloud Services](./cloud-services/cloud-services-sizes-specs.md)voor meer informatie.

**Opstartactie** (Alleen Webrol)

Stel deze eigenschap kunt u opgeven dat Visual Studio met een webbrowser voor de HTTP-eindpunten of de eindpunten HTTPS, of beide starten moet als u foutopsporing start.

De optie HTTPS-eindpunt is alleen beschikbaar als u al een HTTPS-eindpunt voor uw rol hebt gedefinieerd. U kunt een HTTPS-eindpunt definiëren op de pagina van de eigenschap **eindpunten** .

Als u een HTTPS-eindpunt al hebt toegevoegd, de optie HTTPS-eindpunt is standaard ingeschakeld en Visual Studio wordt een browser voor dit eindpunt starten tijdens het starten van foutopsporing, naast een browser voor uw HTTP-eindpunt. Hiermee wordt ervan uitgegaan dat beide opstartopties zijn ingeschakeld.

**Diagnostische hulpprogramma 's**

Diagnostische hulpprogramma's is standaard ingeschakeld voor de rol van het Web. Het project en opslag-account voor de service voor Azure cloud zijn ingesteld op de lokale opslag emulator gebruikt. Wanneer u klaar om te implementeren naar Azure bent, kunt u de knop met de opbouwfunctie voor (**...**) bij de opslag-account als u wilt gebruiken van Azure opslag in de cloud. U kunt de gegevens diagnostische hulpprogramma's overbrengen naar het opslag-account op aanvraag of regelmatig automatisch. Zie voor meer informatie over diagnostische gegevens van Azure [Diagnostische hulpprogramma's inschakelen in Azure Cloud Services en virtuele Machines](./cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Pagina-instellingen

Klik op de pagina **Instellingen** kunt u configuratie-instellingen voor uw service toevoegen. Configuratie-instellingen zijn naam-waardeparen. Code die wordt uitgevoerd in de rol kan de waarden van uw configuratie-instellingen tijdens runtime met behulp van klassen beschikbaar zijn in de [Bibliotheek van Azure beheerde](http://go.microsoft.com/fwlink?LinkID=171026)lezen. De methode [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) specifiek, geeft als resultaat de waarde van een benoemde configuratie-instelling gedurende runtime.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Een verbindingsreeks voor een opslag-account configureren

Een verbindingsreeks is een configuratie-instelling waarmee verbindings-en verificatie voor de opslag emulator of voor een Azure opslag-account. Wanneer uw code Azure opslag services-gegevens – dat wil zeggen blob, wachtrij of tabelgegevens – vanuit de code die wordt uitgevoerd in een rol openen moet, moet u een verbindingsreeks voor dat account opslag definiëren.

Een verbindingsreeks die naar een account Azure opslag verwijst moet een gedefinieerde indeling gebruiken. Zie voor informatie over het maken van verbindingstekenreeksen [Verbindingsreeks van de Azure-opslag configureren](./storage/storage-configure-connection-string.md).

Wanneer u klaar bent voor het testen van uw service ten opzichte van de Azure storage-services, of wanneer u klaar bent voor uw cloudservice implementeren naar Azure, kunt u de waarde van de verbindingstekenreeksen zodat deze verwijzen naar uw account Azure opslag kunt wijzigen. Selecteer (**...**), **Enter opslag-accountreferenties**. Voer de gegevens van uw account met uw accountnaam en accountsleutel. In het dialoogvenster **Verbindingsreeks voor opslag-Account** , kunt u ook aangeven of u wilt gebruiken voor de standaard HTTPS eindpunten (de standaardoptie), de standaard HTTP-eindpunten of aangepaste eindpunten. U kunt aangepaste eindpunten gebruiken als u een aangepaste domeinnaam voor uw service hebt geregistreerd, zoals wordt beschreven in [een aangepaste domeinnaam voor blob-gegevens in een Azure opslag-account configureren](./storage/storage-custom-domain-name.md).

>[AZURE.IMPORTANT] Uw verbindingstekenreeksen zodat deze verwijzen naar een Azure opslag-account voordat u uw service implementeert, moet u wijzigen. Verbroken u dit wilt doen, is het mogelijk dat uw rol niet te beginnen of om te bladeren door de Staten geïnitialiseerd, bezet en stoppen.

## <a name="endpoints-page"></a>Pagina eindpunten

De rol van een werknemer kan een willekeurig aantal HTTP, HTTPS of TCP eindpunten hebben. Eindpunten zijn invoer eindpunten, die beschikbaar zijn voor externe klanten, of interne eindpunten, die beschikbaar zijn voor andere functies die worden uitgevoerd in de service.

- Wijzigen van het type eindpunt invoeren om een HTTP-eindpunt beschikbaar te stellen externe klanten en webbrowsers, en geef een naam en een openbare poortnummer.

- Als u een HTTPS-eindpunt beschikbaar voor externe klanten en webbrowsers, het type eindpunt wijzigen naar **invoer**en geef een naam, het poortnummer in een openbare en de naam van een management-certificaat.

    Houd er rekening mee voordat u een certificaat management opgeeft kunt, u het certificaat op de pagina van de eigenschap **certificaten definiëren moet** .

- Als u een eindpunt beschikbaar voor interne toegang door andere rollen in de cloudservice, het type eindpunt wijzigen naar interne en geef een naam en mogelijke privé poorten voor dit eindpunt.

## <a name="local-storage-page"></a>Pagina van de lokale opslag

U kunt de pagina van de eigenschap **Lokale opslag** reserveren van resources voor een of meer lokale opslag voor een rol. Een resource lokale opslag is een gereserveerde map in het bestandssysteem van de Azure virtuele machine waarin een exemplaar van een rol wordt uitgevoerd.

## <a name="certificates-page"></a>Pagina certificaten

U kunt op de pagina **certificaten** certificaten koppelen aan uw rol. De certificaten die u toevoegt, kunnen worden gebruikt voor het configureren van uw HTTPS-eindpunten op de pagina van de eigenschap **eindpunten** .

De pagina van de eigenschap **certificaten** worden gegevens over uw certificaten toegevoegd aan uw service te configureren. Houd er rekening mee dat uw certificaten niet worden geleverd met uw service; u moet uw certificaten afzonderlijk naar Azure via de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)uploaden.

Als u wilt een certificaat koppelen aan uw rol, Geef een naam voor het certificaat. Gebruikt u deze naam te verwijzen naar het certificaat wanneer u een HTTPS-eindpunt op de pagina van de eigenschap **eindpunten configureren** . Vervolgens opgeven of het archief certificaten **Lokale computer** of de **Huidige gebruiker** en de naam van de store. Tot slot Voer vingerafdruk van het certificaat. Als het certificaat in de huidige User\Personal (Mijn) store is, kunt u van het certificaat vingerafdruk door het certificaat van een ingevulde lijst te selecteren. Als het zich in een andere locatie bevindt, voert u de vingerafdrukwaarde met de hand in.

Wanneer u een certificaat in de store certificaat toevoegt, wordt een tussenliggende certificaten worden automatisch toegevoegd aan de configuratie-instellingen voor u. Deze tussenliggende certificaten moeten ook worden geüpload naar Azure om te kunnen uw service voor SSL correct configureren.

Een management certificaten die u aan uw service koppelen toepassen op uw service alleen wanneer deze wordt uitgevoerd in de cloud. Wanneer uw service wordt uitgevoerd in de lokale ontwikkelomgeving, wordt een standaard-certificaat dat wordt beheerd door de emulator berekeningscluster gebruikt.

## <a name="configuring-the-azure-cloud-service-project"></a>Het project Azure cloud-service configureren

Configureren van instellingen die van toepassing op een volledige Azure cloud service project u het snelmenu voor dat projectknooppunt voor het eerst opent en kiest u eigenschappen die u kunt de eigenschappenvensters openen. De volgende tabel ziet u deze eigenschap pagina's.

|Eigenschappenpagina|Beschrijving|
|---|---|
|Toepassing|Op deze pagina kunt u informatie over de versie van Azure-hulpprogramma's waarmee u dit project van de service cloud gebruikt en u een upgrade uitvoert naar de huidige versie van de hulpmiddelen weergeven.|
|Gebeurtenissen maken|Op deze pagina kunt u gebeurtenissen oude opbouwen en na het samenstellen instellen.|
|Ontwikkeling|Op deze pagina kunt u opgeven opbouwen configuratie-instructies en de voorwaarden waaronder na het samenstellen gebeurtenissen worden uitgevoerd.|
|Web|U kunt de instellingen die zijn gerelateerd aan de webserver configureren op deze pagina.|
