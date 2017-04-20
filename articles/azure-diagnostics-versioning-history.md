<properties
    pageTitle="Versiegeschiedenis van Azure diagnostische gegevens"
    description="Uitleg van wijzigingen in de verschillende versies van Azure diagnostische gegevens als verzonden met verschillende versies van Microsoft Azure SDK's."
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Versiegeschiedenis van Microsoft Azure diagnostische gegevens

Ervaring met Azure diagnostische gegevens? Zie [overzicht van de Azure diagnostische gegevens](azure-diagnostics.md).

Met elke versie van Azure SDK is meestal wordt geleverd met een nieuwe versie van Azure diagnostische gegevens. De volgende tabel bevat de Azure SDK en diagnostisch hulpprogramma Azure-versies die is gekoppeld aan de SDK-versie.



Azure SDK versie | Versie van Azure diagnostische gegevens | Model
--- | --- | ---
1.x      | 1.0 | invoegtoepassing
2.0-2,4| 1.0 | "
2,5      | 1.2 | extensie
2.6      | 1.3 | "
2.7      | 1.4 | "
2,8      | 1,5 | "


De meest recente versie is 1,5, dat wordt geleverd bij Azure SDK 2,8. De versie van Azure diagnostisch hulpprogramma extensie die wordt geleverd met de SDK wordt alleen gebruikt voor lokale emulator scenario's. Een uitgevouwen toepassing wordt automatisch de meest recente versie gebruikt als actief in Azure wordt aangegeven, ongeacht de versie van de SDK de toepassing is gemaakt met. Echter, tenzij u de meest recente Azure SDK hebt geïnstalleerd, hebt u geen alle gebruikmaken van de hulpmiddelen waarmee u de nieuwe functies.

Verschillende functies en wijzigingen hieronder beschreven.

## <a name="azure-sdk-28"></a>Azure SDK 2,8
Azure SDK 2,8 toegevoegd de mogelijkheid om te verzenden naar diagnostische gegevens [Toepassing inzichten](./application-insights/app-insights-cloudservices.md) wordt eenvoudiger problemen op te sporen in uw toepassing, alsmede het niveau van de systeem- en -infrastructuur.

## <a name="azure-26-diagnostics-changes"></a>Wijzigingen van Azure 2.6 diagnostische gegevens

Azure SDK 2.6 Cloudservice projecten in Visual Studio, zijn de volgende wijzigingen zijn aangebracht. (Deze wijzigingen ook toegepast op nieuwere versies van Azure SDK.)

- De lokale emulator ondersteunt nu diagnostische gegevens. Dit betekent dat u kunt naar diagnostische gegevens verzamelen en controleer of dat uw toepassing is de juiste traces maken terwijl u bent ontwikkelen en testen in Visual Studio. De verbindingsreeks `UseDevelopmentStorage=true` naar diagnostische gegevens verzamelen kunt terwijl u werkt met het uw project cloud-service in Visual Studio met behulp van de emulator Azure opslag. Diagnostisch hulpprogramma dat alle gegevens worden verzameld in het account van de opslagruimte (ontwikkeling opslag).

- De verbindingsreeks van diagnostische gegevens opslag-account (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) wordt opnieuw opgeslagen in de service-configuratiebestand (.cscfg). Het hulpprogramma's voor diagnose opslag-account is in Azure SDK 2,5 opgegeven in het bestand diagnostics.wadcfgx.

Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in Azure SDK 2,4 en eerdere heeft gewerkt en hoe dit werkt in Azure SDK 2.6 en hoger.

- In Azure SDK 2,4 en eerdere versies, is de verbindingsreeks gebruikt als een runtime door de invoegtoepassing hulpprogramma's voor diagnose om de gegevens voor het account van opslag voor het overbrengen naar diagnostische logboeken.

- In Azure SDK 2.6 en hoger wordt de verbindingsreeks diagnostische hulpprogramma's voor het configureren van de extensie diagnostische gegevens met de juiste opslag-accountgegevens tijdens publicatie door Visual Studio gebruikt. De verbindingsreeks kunt u verschillende opslag-accounts voor verschillende configuraties Visual Studio worden gebruikt wanneer publiceren definiëren. Omdat de invoegtoepassing hulpprogramma's voor diagnose (na Azure SDK 2,5) niet langer beschikbaar is, inschakelen niet het bestand .cscfg op zichzelf echter de extensie diagnostische hulpprogramma's. U moet de extensie afzonderlijk tot en met hulpprogramma's zoals Visual Studio of PowerShell inschakelen.

- Als u wilt het proces van het configureren van de extensie diagnostische hulpprogramma's met PowerShell vereenvoudigen, bevat het pakket-uitvoer van Visual Studio ook de openbare configuratie XML voor de extensie diagnostische hulpprogramma's voor elke rol. Visual Studio gebruikt de verbindingsreeks diagnostische hulpprogramma's om te vullen de opslag-accountgegevens die aanwezig zijn in de openbare configuratie. De openbare configuratiebestanden zijn gemaakt in de map extensies en het patroon PaaSDiagnostics volgen. <RoleName>. PubConfig.xml. Een PowerShell op basis-implementaties kunnen dit patroon gebruiken om elke configuratie toewijzen aan een rol.

- De verbindingsreeks in het bestand .cscfg wordt ook gebruikt door de Azure-portal voor toegang tot de gegevens diagnostische gegevens zodat deze kan worden weergegeven in het tabblad **controle** . De verbindingsreeks vereist voor het configureren van de service als u wilt weergeven van uitgebreide controlegegevens in de portal.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migreren projecten op Azure SDK 2.6 en hoger

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
