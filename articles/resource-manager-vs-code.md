<properties
   pageTitle="TEGENOVER Code gebruiken met resourcemanager sjablonen | Microsoft Azure"
   description="Ziet u hoe u Visual Studio-Code kunt configureren op Azure resourcemanager sjablonen maken."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Werken met Azure resourcemanager sjablonen in Visual Studio-Code

Azure resourcemanager sjablonen zijn JSON-bestanden die een resource en bijbehorende afhankelijkheden beschrijven. Deze bestanden kunnen soms groot zijn en dus gereedschap ondersteuning belangrijk is dit ingewikkeld. Visual Studio-Code is een nieuwe, lichte, open source, platforms code-editor. Het ondersteunt maken en bewerken van resourcemanager sjablonen met een [nieuwe extensie](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). TEGENOVER Code wordt uitgevoerd de overal en internettoegang niet vereist, tenzij u ook wilt implementeren van uw resourcemanager-sjablonen.

Als u tegenover Code niet al hebt, kunt u dit installeren op [https://code.visualstudio.com/](https://code.visualstudio.com/).

## <a name="install-the-resource-manager-extension"></a>Installeren van de extensie resourcemanager

Als u wilt werken met de JSON-sjablonen in de VS Code, moet u extensie installeren. De volgende stappen downloaden en installeren van de ondersteuning voor resourcemanager JSON-sjablonen:

1. TEGENOVER Code starten 
2. Open snel openen (Ctrl + P) 
3. Voer de volgende opdracht: 

        ext install azurerm-vscode-tools

4. Start opnieuw tegenover Code wanneer u wordt gevraagd om in te schakelen van de extensie. 

 De taak is klaar!

## <a name="set-up-resource-manager-snippets"></a>Resourcemanager fragmenten instellen

De voorgaande stappen het tooling-ondersteuning is geïnstalleerd, maar nu moeten we tegenover Code voor het gebruik van JSON sjabloon fragmenten configureren.

1. De inhoud van het bestand uit de bibliotheek [azure-xplat-arm-tooling](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) naar het Klembord kopiëren.
2. TEGENOVER Code starten 
3. In de VS Code, kunt u de JSON fragmenten-bestand openen door beide navigeren naar het **bestand** -> **Voorkeuren** -> **Gebruiker fragmenten** -> **JSON**, of door te selecteren **F1** en typ **Voorkeuren** totdat u kunt selecteren **Voorkeuren: fragmenten**.

    ![voorkeur fragmenten](./media/resource-manager-vs-code/preferences-snippets.png)

    Selecteer **JSON**met de opties.

    ![Selecteer json](./media/resource-manager-vs-code/select-json.png)

4. De inhoud van het bestand op stap 1 in een gebruiker fragmenten-bestand voordat het uiteindelijke plakken "}" 
5. Zorg ervoor dat de JSON OK eruitziet en dat er geen tijdens een willekeurige plaats het typen. 
6. Opslaan en sluit het gebruiker fragmenten-bestand.

Die wordt gevormd door alle die nodig is om aan de slag met de resourcemanager fragmenten. Vervolgens gaat we deze instellingen naar de test plaatsen.

## <a name="work-with-template-in-vs-code"></a>Werken met de sjabloon in de VS Code

De eenvoudigste manier om te beginnen met het werken met een sjabloon is een van de werkbalk Snel starten sjablonen beschikbaar op [Github](https://github.com/Azure/azure-quickstart-templates) pak of gebruik een van uw eigen. U kunt eenvoudig [een sjabloon exporteren](resource-manager-export-template.md) naar een of meer van uw resourcegroepen via de portal. 

1. Als u een sjabloon uit een resourcegroep geëxporteerd, opent u de uitgepakte bestanden in tegenover Code.

    ![bestanden weergeven](./media/resource-manager-vs-code/show-files.png)

2. Open het bestand template.json zodat u kunt bewerken en toevoegen van extra informatiebronnen. Na het **"resources": [** druk op enter om een nieuwe regel beginnen. Als u **arm**typt, wordt er worden weergegeven met een lijst met opties. Deze opties zijn de sjabloon fragmenten die u hebt geïnstalleerd. Deze ziet er als volgt: 

    ![fragmenten weergeven](./media/resource-manager-vs-code/type-snippets.png)

3. Kies het gewenste fragment. Ik ben **arm ip -** maken van een nieuwe openbare IP-adres te kiezen voor dit artikel. Zet een komma achter de vierkante haak sluiten "}" van de zojuist gemaakte bron om ervoor te zorgen dat uw sjabloon syntaxis geldig is.

     ![door komma's toevoegen](./media/resource-manager-vs-code/add-comma.png)

4. TEGENOVER Code heeft ingebouwde IntelliSense. Als u uw sjablonen bewerken, stelt tegenover Code beschikbare waarden. Stel dat u een sectie variabelen toevoegen aan uw sjabloon toevoegt **""** (twee dubbele aanhalingstekens) en selecteert u **Ctrl + spatiebalk** tussen deze offertes. U krijgt opties waaronder **variabelen**.

    ![variabelen toevoegen](./media/resource-manager-vs-code/add-variables.png)

5. IntelliSense kunt ook voorstellen beschikbaar waarden of functies. Als een eigenschap instelt om een parameterwaarde op te geven, maakt u een expressie met **"[]"** en **Ctrl + spatiebalk**. Typ de naam van een functie, kunt u beginnen. Selecteer **tabblad** wanneer u de gewenste functie hebt gevonden.

    ![parameter toevoegen](./media/resource-manager-vs-code/select-parameters.png)

6. **Ctrl + spatiebalk** opnieuw binnen de functie voor een overzicht van de beschikbare parameters binnen uw sjabloon selecteren

    ![parameter toevoegen](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Als u een schema validatieproblemen in uw sjabloon hebt, ziet u de vertrouwde tijdens het typen in de editor. U kunt de lijst met fouten en waarschuwingen weergeven door te typen **Ctrl + Shift + M** of de symbolen te selecteren in de onderste links statusbalk.

    ![fouten](./media/resource-manager-vs-code/errors.png)

    Validatie van de sjabloon kunt u de syntaxis van de opsporen; echter ook mogelijk dat er fouten die u kunt negeren. In sommige gevallen, is de editor vergelijking van de sjabloon tegen een schema die niet is bijgewerkt en daarom meldt fout, hoewel u weet dat deze juist is. Stel een functie onlangs is toegevoegd aan resourcemanager maar het schema niet is bijgewerkt. De editor rapporten een fout ondanks het feit dat de functie tijdens implementatie correct werkt.

    ![foutbericht](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Uw nieuwe resources implementeren

Wanneer uw sjabloon klaar is, kunt u de nieuwe bronnen de onderstaande instructies kunt implementeren: 

### <a name="windows"></a>Windows

1. Open een opdrachtprompt PowerShell 
2. Naar login type: 

        Login-AzureRmAccount 

3. Als u meerdere abonnementen hebt, kunt u een lijst met abonnementen:

        Get-AzureRmSubscription

    En selecteer het abonnement te gebruiken.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. De parameters in uw bestand parameters.json bijwerken
5. De Deploy.ps1 als u wilt implementeren van de sjabloon op Azure uitvoeren

### <a name="osxlinux"></a>OSX/Linux

1. Een terminal-venster openen 
2. Naar login type:

        azure login 

3. Als u meerdere abonnementen hebt, selecteert u het juiste abonnement met:

        azure account set <subscriptionNameOrId> 

4. Werk de parameters in het bestand parameters.json.
5. Als u wilt implementeren van de sjabloon, voert u het:

        azure group deployment create -f <PathToTemplate> 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over sjablonen, [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md).
- Zie voor meer informatie over functies van de sjabloon, [Azure resourcemanager sjabloon functies](resource-group-template-functions.md).
- Zie voor meer voorbeelden van het werken met Visual Studio-Code [Opbouwen cloud-apps gebruiken met Visual Studio-Code](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) van de verbinding maken met [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Zie voor meer QuickStart uit de demo HealthClinic.biz [Azure ontwikkelaars hulpmiddelen voor QuickStart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
