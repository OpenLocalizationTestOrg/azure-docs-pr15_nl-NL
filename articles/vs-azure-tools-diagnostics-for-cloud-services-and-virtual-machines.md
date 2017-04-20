<properties
   pageTitle="Diagnostische hulpprogramma's configureren voor Azure Cloudservices en virtuele Machines | Microsoft Azure"
   description="Wordt beschreven hoe diagnostische informatie voor foutopsporing Azure cloude services en virtuele machines (VMs) in Visual Studio configureren."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Diagnostische hulpprogramma's configureren voor Azure Cloudservices en virtuele Machines

Wanneer u problemen oplossen met een Azure cloudservice of Azure virtuele machines moet, kunt u eenvoudiger Azure diagnostische gegevens configureren met behulp van Visual Studio. Azure diagnostisch hulpprogramma wordt vastgelegd systeem en gegevens van de logboekregistratie op de virtuele machines en VM-exemplaren die uw cloudservice worden uitgevoerd en brengt die gegevens naar een account opslag van uw keuze. Zie [diagnostische hulpprogramma's voor web-apps in Azure App Service logboekregistratie inschakelen](./app-service-web/web-sites-enable-diagnostic-log.md) voor meer informatie over diagnostische gegevens vastleggen in Azure wordt aangegeven.

In dit onderwerp ziet u hoe u inschakelen en configureren van Azure diagnostische gegevens in Visual Studio, zowel voor en na implementatie, maar ook in Azure virtuele machines. Deze ziet u ook het selecteren van de soorten diagnostische informatie voor het verzamelen en hoe u de informatie weergeven nadat deze zijn verzameld.

U kunt Diagnostisch hulpprogramma Azure configureren op de volgende manieren:

- Diagnostisch hulpprogramma configuratie-instellingen in het dialoogvenster **Configuratie van diagnostische** in Visual Studio, kunt u wijzigen. De instellingen worden opgeslagen in een bestand met de naam van diagnostics.wadcfgx (diagnostics.wadcfg in Azure SDK 2,4 of eerder). U kunt ook rechtstreeks het configuratiebestand wijzigen. Als u het bestand handmatig bijwerkt, wijzigingen in de configuratie wordt pas van kracht de volgende keer dat u de cloud implementeren naar Azure service of de service uitvoeren in de emulator.

- **Cloud Explorer** of **Server Explorer** gebruiken in Visual Studio om de instellingen van de diagnostische hulpprogramma's voor een actieve cloudservice of VM te wijzigen.

## <a name="azure-26-diagnostics-changes"></a>Wijzigingen van Azure 2.6 diagnostische gegevens

Azure SDK 2.6 projecten in Visual Studio, zijn de volgende wijzigingen zijn aangebracht. (Deze wijzigingen ook toegepast op nieuwere versies van Azure SDK.)

- De lokale emulator ondersteunt nu diagnostische gegevens. Dit betekent dat u kunt naar diagnostische gegevens verzamelen en controleer of dat uw toepassing is de juiste traces maken terwijl u bent ontwikkelen en testen in Visual Studio. De verbindingsreeks `UseDevelopmentStorage=true` naar diagnostische gegevens verzamelen kunt terwijl u werkt met het uw project cloud-service in Visual Studio met behulp van de emulator Azure opslag. Diagnostisch hulpprogramma dat alle gegevens worden verzameld in het account van de opslagruimte (ontwikkeling opslag).

- De verbindingsreeks van diagnostische gegevens opslag-account (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) wordt opnieuw opgeslagen in de service-configuratiebestand (.cscfg). Het hulpprogramma's voor diagnose opslag-account is in Azure SDK 2,5 opgegeven in het bestand diagnostics.wadcfgx.

Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in Azure SDK 2,4 en eerdere heeft gewerkt en hoe dit werkt in Azure SDK 2.6 en hoger.

- In Azure SDK 2,4 en eerdere versies, is de verbindingsreeks gebruikt als een runtime door de invoegtoepassing hulpprogramma's voor diagnose om de gegevens voor het account van opslag voor het overbrengen naar diagnostische logboeken.

- In Azure SDK 2.6 en hoger wordt de verbindingsreeks diagnostische hulpprogramma's voor het configureren van de extensie diagnostische gegevens met de juiste opslag-accountgegevens tijdens publicatie door Visual Studio gebruikt. De verbindingsreeks kunt u verschillende opslag-accounts voor verschillende configuraties Visual Studio worden gebruikt wanneer publiceren definiëren. Omdat de invoegtoepassing hulpprogramma's voor diagnose (na Azure SDK 2,5) niet langer beschikbaar is, inschakelen niet het bestand .cscfg op zichzelf echter de extensie diagnostische hulpprogramma's. U moet de extensie afzonderlijk tot en met hulpprogramma's zoals Visual Studio of PowerShell inschakelen.

- Als u wilt het proces van het configureren van de extensie diagnostische hulpprogramma's met PowerShell vereenvoudigen, bevat het pakket-uitvoer van Visual Studio ook de openbare configuratie XML voor de extensie diagnostische hulpprogramma's voor elke rol. Visual Studio gebruikt de verbindingsreeks diagnostische hulpprogramma's om te vullen de opslag-accountgegevens die aanwezig zijn in de openbare configuratie. De openbare configuratiebestanden zijn gemaakt in de map extensies en het patroon PaaSDiagnostics volgen. &lt;Functienaam >. PubConfig.xml. Een PowerShell op basis-implementaties kunnen dit patroon gebruiken om elke configuratie toewijzen aan een rol.

- De verbindingsreeks in het bestand .cscfg wordt ook gebruikt door de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) voor toegang tot de gegevens diagnostische gegevens zodat deze kan worden weergegeven in het tabblad **controle** . De verbindingsreeks vereist voor het configureren van de service als u wilt weergeven van uitgebreide controlegegevens in de portal.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migreren projecten op Azure SDK 2.6 en hoger

Bij het migreren van Azure SDK 2,5 naar Azure SDK 2.6 of hoger, als er een diagnostisch hulpprogramma opslag-account in het bestand .wadcfgx opgegeven, klikt u vervolgens blijft er. Om te profiteren van de flexibiliteit van het gebruik van verschillende opslag accounts voor verschillende opslagconfiguraties, moet u de verbindingsreeks handmatig toevoegen aan uw project. Als u bent een project met de migratie van Azure SDK 2,4 of eerder naar Azure SDK 2.6, blijven klikt u vervolgens de tekenreeksen van de verbinding diagnostische gegevens behouden. Houd rekening echter de wijzigingen in hoe verbindingstekenreeksen zijn verwerkt in Azure SDK 2.6 als opgegeven in de vorige sectie.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Hoe Visual Studio bepaalt het hulpprogramma's voor diagnose opslag-account

- Als een verbindingsreeks diagnostische gegevens in het bestand .cscfg is opgegeven, wordt deze door Visual Studio gebruikt voor het configureren van de extensie diagnostische hulpprogramma's publiceren als bij het genereren van de openbare configuratie XML-bestanden tijdens de verpakking.

- Als u geen verbindingsreeks diagnostische gegevens in het bestand .cscfg is opgegeven, klikt u vervolgens valt Visual Studio terug op met de opslag-account dat is opgegeven in het bestand .wadcfgx voor het configureren van de extensie diagnostisch hulpprogramma wanneer publiceren en het genereren van de openbare configuratie XML-bestanden wanneer de verpakking.

- De verbindingsreeks diagnostische gegevens in het bestand .cscfg heeft voorrang op het account opslagruimte in het bestand .wadcfgx. Als een verbindingsreeks diagnostische gegevens is opgegeven in het bestand .cscfg, Visual Studio die wordt gebruikt en de opslag-account in .wadcfgx, worden genegeerd.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Wat betekent dat de "Update ontwikkeling opslag verbindingstekenreeksen..." selectievakje doen?

Het selectievakje voor **Update ontwikkeling opslag verbindingstekenreeksen voor diagnostisch hulpprogramma en cache met Microsoft Azure opslag accountreferenties wanneer publiceren naar Microsoft Azure** biedt een handige manier voor het bijwerken van de ontwikkeling opslag account verbindingstekenreeksen met de opgegeven tijdens publicatie Azure opslag-account.

Stel dat u dit selectievakje inschakelt en de verbindingsreeks diagnostisch hulpprogramma `UseDevelopmentStorage=true`. Wanneer u het project naar Azure publiceert, wordt Visual Studio de verbindingsreeks diagnostische gegevens automatisch bijgewerkt met de opslag-account dat u hebt opgegeven in de wizard publiceren. Echter als een reële opslag-account is opgegeven als de verbindingsreeks diagnostische hulpprogramma's, wordt dat account gebruikt in plaats daarvan.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostisch hulpprogramma functionaliteit verschillen tussen Azure SDK 2,4 en eerdere en Azure SDK 2,5 en hoger

Als u uw project een van Azure SDK 2,4 met Azure SDK 2,5 of hoger upgrade bent, moet u Houd er rekening mee de volgende verschillen van de hulpprogramma's voor diagnose-functionaliteit.

- **Configuratie-API's zijn afgeschaft** -programma van de configuratie van diagnostische gegevens is beschikbaar in Azure SDK 2,4 of eerdere versies, maar is afgeschaft in Azure SDK 2,5 en hoger. Als de configuratie van uw diagnostische is momenteel gedefinieerd in code, moet u deze instellingen maken in het gemigreerde project in de volgorde voor diagnostische gegevens blijven werken configureren. Het configuratiebestand diagnostische hulpprogramma's voor Azure SDK 2,4 is diagnostics.wadcfg en diagnostics.wadcfgx voor Azure SDK 2,5 en hoger.

- **Diagnostische hulpprogramma's voor servicetoepassingen cloud kan alleen worden geconfigureerd op het niveau van de rol, niet op het niveau exemplaar.**

- **Telkens wanneer u Implementeer de app, de configuratie van de diagnostische wordt bijgewerkt** – Hierdoor kunnen overeenkomst problemen als u de configuratie van uw diagnostische van Server Explorer wijzigen en vervolgens uw app implementeren.

- **In Azure SDK 2,5 en later, loopt vast dumps zijn geconfigureerd in het configuratiebestand diagnostische hulpprogramma's, niet in code** – als er crashdumps geconfigureerd in code, moet u handmatig de configuratie van code overbrengen naar het configuratiebestand, omdat de crashdumps tijdens de migratie worden niet worden overgedragen naar Azure SDK 2,6 af.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Diagnostische gegevens in de cloud serviceprojecten inschakelen voordat deze wordt geïmplementeerd

In Visual Studio, kunt u kiezen voor het verzamelen van gegevens diagnostische hulpprogramma's voor rollen die in Azure wordt aangegeven, worden uitgevoerd wanneer u de service in de emulator uitvoert voordat deze wordt geïmplementeerd. Alle wijzigingen in de instellingen in Visual Studio diagnostische gegevens worden opgeslagen in het configuratiebestand diagnostics.wadcfgx. Deze configuratie-instellingen opgeven het opslag-account naar diagnostische gegevens opslaglocatie wanneer u uw cloudservice implementeert.

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Diagnostische hulpprogramma's in Visual Studio voordat implementatie inschakelen

1. Kies **Eigenschappen**in het snelmenu voor de rol die u interesseert, en kies vervolgens het tabblad **configuratie** in **het eigenschappenvenster van de rol** .

1. Zorg dat het selectievakje **Diagnostische hulpprogramma's inschakelen** is geselecteerd in de sectie **diagnostische hulpprogramma's** .

    ![Toegang krijgen tot de optie inschakelen diagnostische gegevens](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)

1. Kies de knop weglatingsteken (...) om op te geven van de opslag-account waar u de gegevens die diagnostische gegevens moeten worden opgeslagen. Het opslag-account dat u kiest, worden de locatie waar naar diagnostische gegevens worden opgeslagen.

    ![Geef de opslag-account om toegang te gebruiken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)

1. Geef in het dialoogvenster **Opslag verbindingsreeks maken** of u wilt verbinding maken met behulp van de Azure opslag Emulator, een Azure-abonnement of handmatig ingevoerd referenties.

    ![Dialoogvenster voor opslag-account](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)

  - Als u ervoor kiest de Microsoft Azure opslag Emulator optie, de verbindingsreeks is ingesteld op UseDevelopmentStorage = true.

  - Als u ervoor kiest de optie van uw abonnement, kunt u het Azure abonnement dat u wilt gebruiken en de naam van het account. U kunt de knop Accounts beheren voor het beheren van uw Azure-abonnementen.

  - Als u de optie handmatig ingevoerde referenties kiest, wordt u gevraagd naar de naam en de sleutel van de Azure-account die u wilt gebruiken.

1. Kies de knop **configureren** om weer te geven in het dialoogvenster **Diagnostisch hulpprogramma Configuratie** . Elk tabblad (behalve **algemene** en **Log mappen**) vertegenwoordigt een diagnostische gegevensbron die u kunt verzamelen. Het standaardtabblad, **algemene**, biedt de volgende opties van diagnostische gegevens gegevens siteverzameling: **fouten alleen**, **alle gegevens**en **aangepaste-abonnement**. De standaardoptie, **alleen fouten**, gaat de minste hoeveelheid opslagruimte omdat deze geen waarschuwingen of berichten voor tracering overbrengen. De optie Alles informatie worden de meeste gegevens overgenomen en daardoor, de optie meest dure met opslag.

    ![Azure diagnostisch hulpprogramma en configuratie inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

1. In dit voorbeeld de optie **aangepaste plan** zodat u de gegevens die worden verzameld kunt aanpassen.

1. Het vak **Schijfquotum in MB** Hiermee geeft u hoeveel ruimte u in uw account opslag voor diagnostische gegevens wilt toewijzen. Als u wilt, kunt u de standaardwaarde wijzigen.

1. Selecteer op elk tabblad van het hulpprogramma's voor diagnose-gegevens die u wilt verzamelen, de **inschakelen overbrengen van <log type> ** selectievakje. Als u wilt de toepassingslogboeken verzamelen, selecteert u bijvoorbeeld het selectievakje **overdracht van toepassingslogboeken inschakelen** op het tabblad **Logboeken van toepassing** . Geef ook eventuele andere informatie vereist door het gegevenstype van elke diagnostische gegevens. Zie het gedeelte **configureren diagnostisch hulpprogramma gegevensbronnen** verderop in dit onderwerp voor configuratiegegevens op elk tabblad.

1. Nadat u alle naar diagnostische gegevens die u wilt gebruiken, klikt u op de knop **OK** verzameling hebt ingeschakeld.

1. Uw project Azure cloud-service zoals u gewend bent in Visual Studio uitvoeren. Terwijl u uw toepassing gebruikt, wordt de logboekgegevens die u ingeschakeld is opgeslagen op de opslag van Azure-account die u hebt opgegeven.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Diagnostische gegevens in Azure virtuele machines inschakelen

In Visual Studio, kunt u kiezen voor het verzamelen van gegevens diagnostische hulpprogramma's voor Azure virtuele machines.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Diagnostische gegevens in Azure virtuele machines inschakelen

1. Kies het Azure knooppunt in **Server Explorer**en sluit vervolgens aan uw Azure-abonnement als u nog niet bent verbonden.

1. Vouw het knooppunt **virtuele Machines** . U kunt een nieuwe virtuele machine maken of Selecteer een die daar al staat.

1. Kies in het snelmenu voor de virtuele machine die u interesseert, **configureren**. Hiermee worden in het dialoogvenster voor VM-configuratie.

    ![Een Azure virtuele machines configureren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)

1. Als dit nog niet geïnstalleerd, moet u de uitbreiding Microsoft Monitoring Agent diagnostische gegevens toevoegen. Dit toestel kunt u gegevens van de diagnostische hulpprogramma's voor de Azure virtuele machine verzamelen. In de lijst extensies is geïnstalleerd, kiest u de selecteren een vervolgkeuzemenu beschikbaar extensie en kies vervolgens Microsoft Monitoring Agent diagnostische gegevens.

    ![Installatie van de extensie Azure virtuele machines](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)

    >[AZURE.NOTE] Andere extensies diagnostische gegevens zijn beschikbaar voor uw virtuele machines. Zie Azure VM Extensions en functies voor meer informatie.

1. Kies de knop **toevoegen** aan de extensie toevoegen en weergeven van het dialoogvenster **configuratie van diagnostische** .

1. Kies de knop **configureren** om een account opslag opgeven en kies vervolgens de knop **OK** .

    Elk tabblad (behalve **algemene** en **Log mappen**) vertegenwoordigt een diagnostische gegevensbron die u kunt verzamelen.

    ![Azure diagnostisch hulpprogramma en configuratie inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

    Het standaardtabblad, **algemene**, biedt de volgende opties van diagnostische gegevens gegevens siteverzameling: **fouten alleen**, **alle gegevens**en **aangepaste-abonnement**. De standaardoptie, **alleen fouten**, gaat de minste hoeveelheid opslagruimte omdat deze geen waarschuwingen of berichten voor tracering overbrengen. De optie **alle gegevens** worden de meeste gegevens overgenomen en daardoor, de meest dure optie in de opslagruimte.

1. In dit voorbeeld de optie **aangepaste plan** zodat u de gegevens die worden verzameld kunt aanpassen.

1. Het vak **Schijfquotum in MB** Hiermee geeft u hoeveel ruimte u in uw account opslag voor diagnostische gegevens wilt toewijzen. Als u wilt, kunt u de standaardwaarde wijzigen.

1. Selecteer op elk tabblad van het hulpprogramma's voor diagnose-gegevens die u wilt verzamelen, de **inschakelen overbrengen van <log type> ** selectievakje.

    Als u wilt de toepassingslogboeken verzamelen, selecteert u bijvoorbeeld het selectievakje **overdracht van toepassingslogboeken inschakelen** op het tabblad **Logboeken van toepassing** . Geef ook eventuele andere informatie vereist door het gegevenstype van elke diagnostische gegevens. Zie het gedeelte **configureren diagnostisch hulpprogramma gegevensbronnen** verderop in dit onderwerp voor configuratiegegevens op elk tabblad.

1. Nadat u alle naar diagnostische gegevens die u wilt gebruiken, klikt u op de knop **OK** verzameling hebt ingeschakeld.

1. De bijgewerkte project opslaan.

    Hier ziet u een bericht in het venster **Microsoft Azure activiteitenlogboek** dat de virtuele machine is bijgewerkt.

## <a name="configure-diagnostics-data-sources"></a>Diagnostisch hulpprogramma gegevensbronnen configureren

Nadat u diagnostische gegevens verzamelen hebt ingeschakeld, kunt u precies welke gegevensbronnen die u wilt verzamelen en welke gegevens worden verzameld. Hier volgt een lijst met tabbladen in het dialoogvenster **configuratie van diagnostische** en welke elke optie voor de configuratie.

### <a name="application-logs"></a>Toepassingslogboeken aan de

**Toepassingslogboeken** bevatten diagnostische informatie geproduceerd door een webtoepassing. Als u vastleggen toepassingslogboeken aan de wilt, selecteert u het selectievakje **overdracht van toepassingslogboeken inschakelen** in. U kunt vergroot of verkleint het aantal minuten wanneer de toepassingslogboeken worden overgebracht bij uw account opslag door de waarde van de **Periode doorverbinden (min)** te wijzigen. U kunt ook de hoeveelheid informatie die zijn opgenomen in het logboek door in te stellen van de waarde van het logboek niveau wijzigen. U kunt bijvoorbeeld **uitgebreid** als u meer informatie of kiest u **kritiek** om vast te leggen alleen kritieke fouten. Als u een specifieke hulpprogramma's voor diagnose-provider die toepassingslogboeken genereert hebt, kunt u ze kunt vastleggen door de provider GUID toe te voegen aan het vak **Provider-GUID** .

  ![Toepassingslogboeken aan de](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Zie [diagnostische hulpprogramma's voor web-apps in Azure App Service logboekregistratie inschakelen](./app-service-web/web-sites-enable-diagnostic-log.md) voor meer informatie over de toepassingslogboeken.

### <a name="windows-event-logs"></a>Gebeurtenislogboeken van Windows

Als u vastleggen gebeurtenislogboeken van Windows wilt, selecteert u het selectievakje **inschakelen doorverbinden gebeurtenislogboeken van Windows** . U kunt vergroot of verkleint het aantal minuten wanneer de gebeurtenislogboeken worden overgebracht bij uw account opslag door de waarde van de **Periode doorverbinden (min)** te wijzigen. Schakel de selectievakjes voor de typen gebeurtenissen die u wilt bijhouden.

  ![Gebeurtenislogboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Als u Azure SDK 2.6 of hoger gebruikt en u wilt opgeven van een aangepaste gegevensbron, voert u deze in de **<Data source name>** tekst en kies de knop **toevoegen** naast het. De gegevensbron wordt toegevoegd aan het bestand diagnostics.cfcfg.

Als u Azure SDK 2,5 gebruikt en geef een aangepaste gegevensbron wilt, kunt u het toevoegen aan de `WindowsEventLog` gedeelte van de diagnostics.wadcfgx bestand, zoals in het volgende voorbeeld.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Prestatie-items

Informatie over het item van prestaties kunt u Zoek systeemknelpunten en verfijnen systeemprestaties en de toepassingen. Zie [maken en items van de prestaties gebruiken in een Azure-toepassing](https://msdn.microsoft.com/library/azure/hh411542.aspx) voor meer informatie. Als u vastleggen prestatie-items wilt, selecteert u het selectievakje **overdracht van prestatiemeteritems inschakelen** . U kunt vergroot of verkleint het aantal minuten wanneer de gebeurtenislogboeken worden overgebracht bij uw account opslag door de waarde van de **Periode doorverbinden (min)** te wijzigen. Schakel de selectievakjes voor de items die u wilt bijhouden.

  ![Prestatie-items](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Als u wilt bijhouden een prestatie-item die niet wordt weergegeven, voert u het met behulp van de syntaxis van de voorgestelde en kies vervolgens de knop **toevoegen** . Het besturingssysteem op de virtuele machine bepaalt welke prestatiemeteritems die u kunt bijhouden. Zie [een itempad opgeven](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx)voor meer informatie over de syntaxis van.

### <a name="infrastructure-logs"></a>Logboeken aan de infrastructuur

Als u vastleggen infrastructuur Logboeken, waarin informatie over de Azure diagnostische infrastructuur, de module Remote Access en de module RemoteForwarder wilt, selecteert u het selectievakje **overdracht van infrastructuur Logboeken inschakelen** in. U kunt vergroot of verkleint het aantal minuten wanneer de logboeken worden overgebracht bij uw account opslag door de waarde van de periode doorverbinden (min) te wijzigen.

  ![Diagnostisch hulpprogramma infrastructuur Logboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Zie [Logboekregistratie gegevens verzamelen door gebruik van Azure diagnostisch hulpprogramma](https://msdn.microsoft.com/library/azure/gg433048.aspx) voor meer informatie.

### <a name="log-directories"></a>Log mappen

Als u vastleggen log mappen, die gegevens die worden verzameld uit log mappen voor Internet Information Services (IIS) aanvragen wilt bevatten, mislukte aanvragen of mappen die u kiest, schakel het selectievakje **overdracht van Log mappen inschakelen** . U kunt vergroot of verkleint het aantal minuten wanneer de logboeken worden overgebracht bij uw account opslag door de waarde van de **Periode doorverbinden (min)** te wijzigen.

U kunt de vakjes in van de logboekbestanden die u verzamelen wilt, zoals **IIS-logboeken** en logboeken **Is mislukt aanvragen** selecteren. Standaardnamen opslag container worden geleverd, maar u kunt de namen als u wilt wijzigen.

U kunt ook Logboeken vanuit een willekeurige map vastleggen. Alleen het pad opgeven in de sectie **Log uit Absolute adreslijst** en kies vervolgens de knop **Map toevoegen** . De logboeken wordt aan de opgegeven containers worden opgenomen.

  ![Log mappen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>ETW Logboeken

Als u [Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803(v=vs.85).aspx) (ETW) en wilt vastleggen ETW Logboeken, selecteert u het selectievakje **overdracht van ETW Logboeken inschakelen** in. U kunt vergroot of verkleint het aantal minuten wanneer de logboeken worden overgebracht bij uw account opslag door de waarde van de **Periode doorverbinden (min)** te wijzigen.

De gebeurtenissen worden vastgelegd van gebeurtenisbronnen en gebeurtenis manifesten die u opgeeft. Een bron van gebeurtenis opgeven, typ een naam in de sectie **Bronnen van gebeurtenissen** en kies vervolgens de knop **Bron van gebeurtenis toevoegen** . U kunt op dezelfde manier een manifest gebeurtenis in de sectie **Gebeurtenis manifesten** opgeven en kies vervolgens de knop **Gebeurtenis bestandenlijst toevoegen** .

  ![ETW Logboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  Het kader ETW wordt ondersteund in ASP.NET tot en met klassen in de [System.Diagnostics.aspx] (naamruimte https://msdn.microsoft.com/library/system.diagnostics (v=vs.110). De Microsoft.WindowsAzure.Diagnostics naamruimte overneemt van en breidt de mogelijkheden standaard [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) klassen, kunt u gebruikmaken van [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) als een kader logboekregistratie in de Azure-omgeving. Zie [beheer van logboekregistratie duren en traceren in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) en [Diagnostische hulpprogramma's inschakelen in Azure Cloud Services en virtuele Machines](./cloud-services/cloud-services-dotnet-diagnostics.md)voor meer informatie.

### <a name="crash-dumps"></a>Crashdumps

Als u vastleggen van informatie wilt over wanneer een exemplaar van de rol loopt vast, selecteert u het selectievakje **overdracht van het vastlopen wordt inschakelen** in. (Omdat ASP.NET meeste uitzonderingen verwerkt, dit is gewoonlijk alleen nuttig voor werknemer rollen.) U kunt vergroot of verkleint de hoeveelheid opslagruimte besteed aan de crashdumps door de waarde van het **Quotum voor Directory (%)** te wijzigen. U kunt de container opslagruimte waar de crashdumps zijn opgeslagen, en kunt u aangeven of u wilt vastleggen van een **volledige** of **Mini** dump wijzigen.

De processen die momenteel worden bijgehouden, worden vermeld. Schakel de selectievakjes voor de processen die u wilt vastleggen. Een ander proces toevoegen aan de lijst, voer de procesnaam en kies vervolgens de knop **Proces toevoegen** .

  ![Crashdumps](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Zie [beheer van logboekregistratie duren en traceren in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) en [Microsoft Azure diagnostisch hulpprogramma deel 4: aangepaste logboekregistratie onderdelen en Azure diagnostisch hulpprogramma 1.3 wijzigingen](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) voor meer informatie.

## <a name="view-the-diagnostics-data"></a>Weergeven van de gegevens diagnostische gegevens

Als u de gegevens diagnostische hulpprogramma's voor een cloudservice of een virtuele machine hebt verzameld, kunt u deze kunt weergeven.

### <a name="to-view-cloud-service-diagnostics-data"></a>Cloud service naar diagnostische gegevens moeten worden weergegeven

1. Uw cloudservice gebruikelijke manier te implementeren en vervolgens uit te voeren.

1. U kunt de gegevens diagnostische gegevens weergeven in een rapport dat Visual Studio genereert of tabellen in uw account opslag. Als u wilt de gegevens in een rapport bekijkt, **Cloud Explorer** of **Server Explorer**openen, opent u het snelmenu van het knooppunt voor de rol die u interesseert en kiest u **Diagnostische gegevens weergeven**.

    ![Gegevens van de diagnostische gegevens weergeven](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)

    Een rapport met de beschikbare gegevens wordt weergegeven.

    ![Microsoft Azure diagnostisch hulpprogramma rapport in Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)

    Als u de meest recente gegevens niet ziet, is het wellicht moet wachten om door de overdracht periode te klikken.

    Kies de koppeling **Vernieuw** de gegevens direct bijwerken of kiest u een interval in de vervolgkeuzelijst voor **Automatisch vernieuwen** moet de gegevens worden automatisch bijgewerkt. De om foutgegevens te exporteren, kies de knop **exporteren naar CSV** maken van een CSV-bestand die u in een werkblad openen kunt.

    Open de opslag-account dat is gekoppeld aan de implementatie in de **Cloud Explorer** of **Server Verkenner**.

1. Open de tabellen diagnostische gegevens in de tabel-viewer en controleer vervolgens de gegevens die u verzameld. Voor IIS-logboeken en aangepaste logboeken, kunt u een container blob openen. Aan de hand van de volgende tabel vindt u de tabel of blob container met de gegevens die u interesseert. Naast de gegevens voor die logboekbestand, items in de tabel EventTickCount, DeploymentId, rol en RoleInstance om te identificeren welke virtuele machine en rol gegenereerd de gegevens bevatten en wanneer. 

  	|Diagnostische gegevens|Beschrijving|Locatie|
  	|---|---|---|
  	|Toepassingslogboeken aan de|Logboeken die uw code wordt gegenereerd door te bellen methoden van de klasse System.Diagnostics.Trace.|WADLogsTable|
  	|Gebeurtenislogboeken|Deze gegevens zijn uit de gebeurtenislogboeken van Windows op de virtuele machines. Windows informatie in deze logboeken opgeslagen, maar toepassingen en -services ook gebruiken om te rapportfouten of logboekgegevens.|WADWindowsEventLogsTable|
  	|Prestatie-items|U kunt gegevens ophalen op een prestatie-item dat is beschikbaar op de virtuele machine. Het besturingssysteem bevat prestatiemeteritems, waaronder meer statistische gegevens zoals geheugen gebruik en processor tijd.|WADPerformanceCountersTable|
  	|Logboeken aan de infrastructuur|Deze logboeken zijn gegenereerd op basis van de hulpprogramma's voor diagnose-infrastructuur zelf.|WADDiagnosticInfrastructureLogsTable|
  	|IIS-logboeken|Deze logboeken record webaanvragen. Als uw cloudservice een aanzienlijke hoeveelheid verkeer krijgt, deze logboeken kunnen helemaal in beslag nemen, zodat u moet verzamelen en bewaren van deze gegevens alleen wanneer u deze nodig hebt.|U kunt Logboeken op de aanvraag voor is mislukt in de container blob onder af-iis-failedreqlogs onder een pad dat implementatie, rol en exemplaar. U kunt de volledige Logboeken onder af-iis-logboekbestanden vinden. Items voor elk bestand worden aangebracht in de tabel WADDirectories.|
  	|Crashdumps|Deze informatie bevat binaire afbeeldingen van uw cloudservice proces (meestal de rol van een werknemer).|af-crush-dumps blob container|
  	|Aangepaste logboekbestanden|Logboeken van gegevens die u vooraf gedefinieerd.|U kunt opgeven in code de locatie van logboekbestanden met aangepaste in uw account opslag. U kunt bijvoorbeeld een container aangepaste blob opgeven.|

1. Als de gegevens van elk type worden afgekapt, kunt u proberen de buffer voor die gegevens met groter wordende type of het interval tussen overbrengen van gegevens in van de virtuele machine bij uw account opslag te verkorten.

1. (optioneel) Gegevens verwijderen uit het account opslag af en toe aan de algehele opslag verlagen.

1. Wanneer u een volledige implementatie doet, het diagnostics.cscfg-bestand (.wadcfgx voor Azure SDK 2,5) wordt bijgewerkt in Azure wordt aangegeven en uw cloudservice wijzigingen van de configuratie van uw diagnostische hervat. Als u, in plaats daarvan een bestaande implementatie bijwerkt, wordt het .cscfg-bestand niet is bijgewerkt in Azure wordt aangegeven. U kunt nog steeds diagnostisch hulpprogramma instellingen wijzigen, echter volgens de stappen in het volgende gedeelte. Zie voor meer informatie over de uitvoering van een volledige implementatie en bijwerken van een bestaande implementatie, [Wizard van Azure-toepassing publiceren](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>VM naar diagnostische gegevens moeten worden weergegeven

1. Kies in het snelmenu voor de virtuele machine, **Gegevens van de diagnostische hulpprogramma's weergeven**.

    ![Diagnostisch hulpprogramma gegevens weergeven in Azure virtuele machines](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)

    Hiermee opent u het **hulpprogramma's voor diagnose samenvatting** -venster.

    ![Azure virtuele machines diagnostisch hulpprogramma samenvatting](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  

    Als u de meest recente gegevens niet ziet, is het wellicht moet wachten om door de overdracht periode te klikken.

    Kies de koppeling **Vernieuw** de gegevens direct bijwerken of kiest u een interval in de vervolgkeuzelijst voor **Automatisch vernieuwen** moet de gegevens worden automatisch bijgewerkt. De om foutgegevens te exporteren, kies de knop **exporteren naar CSV** maken van een CSV-bestand die u in een werkblad openen kunt.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Diagnostisch hulpprogramma cloud-service configureren na implementatie

Als u een probleem met een wolk bent onderzoeken service die al wordt uitgevoerd, kunt u voor het verzamelen van gegevens die u hebt opgegeven voordat u de rol oorspronkelijk geïmplementeerd. In dit geval kunt u die gegevens verzamelen met behulp van de instellingen in Server Explorer starten. U kunt diagnostische hulpprogramma's voor één exemplaar of de overal in een rol, afhankelijk van of u in het dialoogvenster Diagnostisch hulpprogramma configuratie in het snelmenu voor het exemplaar of de rol opent configureren. Als u het knooppunt rol configureert, worden wijzigingen toepassen op alle exemplaren. Als u het knooppunt exemplaar is geconfigureerd, worden alle wijzigingen toepassen op alleen die instantie.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Diagnostische hulpprogramma's voor een actieve cloudservice configureren

1. Vouw het knooppunt **Cloud Services** in Server Explorer en vouwt u knooppunten om te zoeken van de rol of exemplaar dat u wilt onderzoeken of beide.

    ![Diagnostische gegevens configureren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)

1. In het snelmenu voor een exemplaar knooppunt of een rol knooppunt, kiest u **Instellingen voor diagnostische gegevens bijwerken**en kies vervolgens de diagnostische instellingen die u wilt verzamelen.

    Zie voor informatie over de configuratie-instellingen **configureren diagnostisch hulpprogramma gegevensbronnen** in dit onderwerp. Zie voor informatie over het weergeven van de gegevens diagnostische hulpprogramma's, **weergave de diagnostische gegevens** in dit onderwerp.

    Als u gegevens verzamelen in **Server Explorer**wijzigt, wordt deze wijzigingen van kracht blijven totdat u volledig Implementeer deze opnieuw uw cloudservice. Als u de standaardwaarde gebruiken publicatie-instellingen, de wijzigingen worden niet overschreven, aangezien het publiceren van de standaard-instelling is bijwerken van de bestaande implementatie, in plaats van een volledige opnieuw installeren doen. Om ervoor te zorgen dat de instellingen op implementatie moment wissen, gaat u naar het tabblad **Geavanceerde instellingen** in de wizard Publiceren en schakel het selectievakje **implementatie bijwerken** . Wanneer u Implementeer deze opnieuw met dit selectievakje uitgeschakeld, terug de instellingen die in het bestand .wadcfgx (of .wadcfg) als set via de editor eigenschappen voor de rol. Als u uw implementatie bijwerkt, blijft Azure de oude instellingen.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Problemen met Azure cloud service oplossen

Als er problemen met uw projecten cloud-service, zoals een rol die in een "bezet" status, blijft hangen herhaaldelijk wordt herhaald of genereert een interne serverfout, er zijn hulpprogramma's en technieken die kunt u deze problemen opsporen en oplossen. Zie [Gegevens van Azure PaaS berekenen diagnostische hulpprogramma's](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)voor specifieke voorbeelden van veelvoorkomende problemen en oplossingen, evenals een overzicht van de concepten en hulpprogramma's voor automatisch opsporen en oplossen van dergelijke fouten.

## <a name="q--a"></a>Q & A

**Wat is de grootte van de, en hoe groot moet deze zijn?**

Op elk exemplaar VM beperken quota hoeveel diagnostische gegevens kan worden opgeslagen op het lokale bestandssysteem. Bovendien kunt u de buffergrootte van een voor elk type diagnostische gegevens die beschikbaar opgeven. Hiermee akkoord fungeert als een afzonderlijke quotum voor dit type gegevens. Door te schakelen onder in het dialoogvenster, kunt u het algehele quotum en de hoeveelheid geheugen dat nog moet bepalen. Als u grotere buffers of meer typen gegevens opgeeft, moet u het algehele quotum benadert. U kunt het quotum voor algemene wijzigen door de diagnostics.wadcfg/.wadcfgx configuratie-bestand te wijzigen. De gegevens diagnostische gegevens is opgeslagen op het bestandssysteem hetzelfde als de gegevens van uw toepassing, dus als uw toepassing gebruikmaakt van een groot aantal schijfruimte, kunt u het quotum voor de algehele diagnostische gegevens niet mag vergroten.

**Wat is de periode doorverbinden en hoe lang moet deze zijn?**

De overdracht periode is de hoeveelheid tijd die is verstreken tussen gegevens wordt vastgelegd. Na elke periode doorverbinden wordt gegevens verplaatst van de lokale bestandssysteem op een virtuele machine naar tabellen in uw account opslag. Als de hoeveelheid gegevens die worden verzameld groter is dan het quotum voor het einde van een termijn doorverbinden, oudere gegevens verwijderd. U kunt de periode doorverbinden verkleinen als u bent gegevensverlies omdat uw gegevens groter is dan de grootte van de of het quotum voor algemene.

**Welke tijdzone zijn de tijdstempels in?**

De tijdstempels zijn opgeslagen in de lokale tijdzone van het datacenter waarop uw cloudservice. De volgende drie tijdstempelkolommen in het logboek met tabellen worden gebruikt.

  - **PreciseTimeStamp** is het ETW tijdstempel van de gebeurtenis. Dat wil zeggen de tijd van de client wordt de gebeurtenis vastgelegd.

  - **Tijdstempel** wordt PreciseTimeStamp naar beneden afgerond op de rand van de frequentie uploaden. Ja, als de frequentie van uw uploaden 5 minuten en de gebeurtenis tijd 00:17:12 is, tijdstempel zijn 00:15:00.

  - **Tijdstempel** wordt de tijdstempel waarop de entiteit is gemaakt in de tabel Azure.

**Hoe beheer ik kosten bij het verzamelen van diagnostische gegevens?**

De standaardinstellingen (**niveau voor logboekregistratie** is ingesteld op **fout** en **overbrengen periode** ingesteld op **1 minuut**) zijn ontworpen om te minimaliseren kosten. Uw kosten berekenen, neemt toe als u meer diagnostische gegevens verzamelen of verkleinen van de periode doorverbinden. Geen verzamelen meer gegevens dan u nodig hebt en vergeet niet voor het verzamelen van gegevens uitschakelen wanneer u deze niet meer nodig hebt. U kunt altijd deze weer inschakelen, zelfs gedurende runtime, zoals wordt weergegeven in de vorige sectie.

**Hoe ik is mislukt-request logboeken verzamelen van IIS?**

Standaard verzamelen niet IIS is mislukt-request Logboeken. U kunt IIS ze verzamelen als u het bestand web.config voor de Webrol van uw bewerken configureren.

**Ik krijg niet doelcellen informatie uit RoleEntryPoint methoden zoals OnStart. Wat is er aan de hand?**

De methoden voor het RoleEntryPoint worden genoemd in de context van WAIISHost.exe, niet IIS. Daarom de configuratiegegevens in web.config die normaal gesproken kunt aanwijzen niet van toepassing. U lost dit probleem op door een .config-bestand toevoegen aan uw project van de rol web en de naam van het bestand zodat deze overeenkomt met de uitvoer constructie dat de code RoleEntryPoint bevat. Klik in de standaard web rol project zou de naam van het bestand .config WAIISHost.exe.config. Tel de volgende regels bij dit bestand:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Nu in het venster **Eigenschappen** voor het **kopiëren naar uitvoer Directory** eigenschap instellen op **altijd kopiëren**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over diagnostische gegevens vastleggen in Azure wordt aangegeven, [Diagnostische hulpprogramma's inschakelen in Azure Cloud Services en virtuele Machines](./cloud-services/cloud-services-dotnet-diagnostics.md) en [Diagnostische gegevens vastleggen voor web-apps in Azure App-Service inschakelen](./app-service-web/web-sites-enable-diagnostic-log.md).
