

In dit artikel leest u hoe u een Azure virtuele machines schaal instellen met een Visual Studio Resource groep-implementatie.


[Azure virtuele machines schaal Sets](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) zijn een resource Azure berekenen om te implementeren en beheren van een verzameling soortgelijke virtuele machines met eenvoudig geïntegreerde opties voor automatisch schalen en taakverdeling. U kunt inrichten en implementeren van VM schaal Sets met [Azure Resource Manager (ARM)-sjablonen](https://github.com/Azure/azure-quickstart-templates). Op ARM sjablonen kunnen worden geïmplementeerd met Azure CLI, PowerShell REST en ook rechtstreeks vanuit de Visual Studio. Visual Studio biedt een aantal voorbeeld sjablonen die kunnen worden geïmplementeerd als onderdeel van een project Azure Resource groep implementatie.

Azure resourcegroep implementaties zijn een manier om te groeperen en publiceren van een reeks verwante Azure resources in een enkel implementatie-bewerking. U vindt u meer informatie over deze hier: [maken en implementeren van Azure resourcegroepen tot en met Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy/).

## <a name="pre-requisites"></a>Minimumvereisten

Als u wilt beginnen VM schaal Sets in Visual Studio implementeert nodig u het volgende:

- Visual Studio 2013 of 2015
- Azure SDK 2.7 of 2,8

Opmerking: Deze instructies wordt ervan uitgegaan dat u Visual Studio-2015 met [Azure SDK 2,8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)gebruikt.

## <a name="creating-a-project"></a>Een Project te maken

1. Een nieuw project maakt in Visual Studio-2015 door te **bestand kiezen | Nieuwe | Project**.

    ![Bestand nieuw][file_new]

2. Klik onder **Visual C# | Cloud**, kies **Azure resourcemanager** om een project voor het implementeren van een sjabloon ARM te maken.

    ![Project maken][create_project]

3.  In de lijst met sjablonen, de Linux of Windows VM schaal instellen sjabloon te selecteren.

    ![Selecteer sjabloon][select_Template]

4. Nadat uw project is gemaakt ziet u PowerShell-scripts voor implementatie, een Azure resourcemanager-sjabloon en een parameterbestand voor de virtuele Machine schaal instellen.

    ![Oplossing Explorer][solution_explorer]

## <a name="customize-your-project"></a>Uw project aanpassen

Nu kunt u deze voor de behoeften van uw toepassing, zoals VM extensie-eigenschappen toevoegen of bewerken van taakverdeling regels aanpassen zodat de sjabloon bewerken. Al dan niet standaard zijn de VM-sjablonen voor het instellen van schaal geconfigureerd voor het implementeren van de extensie AzureDiagnostics waardoor u deze gemakkelijk kunt automatisch schalen regels toevoegen. Deze implementeert u ook een taakverdeling op een openbare IP-adres, geconfigureerd met binnenkomende NAT regels welke toestaan dat u verbinding maakt met de exemplaren VM met SSH (Linux) of RDP (Windows) – het bereik van de front-end van poorten wordt gestart met 50000, wat betekent in het geval van Linux, dat als u SSH met poort 50000 van het openbare IP-adres (of domain name) u worden doorgestuurd naar poort 22 van de eerste VM in de schaal instellen. Verbinding maken met poort 50001 worden gerouteerd naar poort 22 van de tweede VM enzovoort.

 Er is een goede manier om uw sjablonen met Visual Studio bewerken naar de JSON-overzicht gebruiken om de parameters, variabelen en resources te organiseren. Met inzicht in het schema kunt Visual Studio fouten aanwijzen in uw sjabloon voordat u deze implementeert.

![JSON Explorer][json_explorer]

## <a name="deploy-the-project"></a>Implementeer het project

6. De sjabloon ARM dashboard implementeren naar Azure maken van de resource VM schaal instellen. Klik met de rechtermuisknop op het projectknooppunt, kiest u **Deploy | Nieuwe implementatie**.

    ![Sjabloon implementeren][5deploy_Template]

7. Selecteer uw abonnement in het dialoogvenster "Implementeren naar resourcegroep".

    ![Sjabloon implementeren][6deploy_Template]

8. Hier kunt u ook een nieuwe resourcegroep van Azure als u wilt implementeren van de sjabloon wilt maken.

    ![Nieuwe resourcegroep][new_resource]

9. Naast de knop **Bewerken van Parameters** om in te voeren parameters die wordt doorgegeven aan de sjabloon, bepaalde waarden zoals de gebruikersnaam en wachtwoord voor het besturingssysteem vereist voor het maken van de implementatie.

    ![Bewerken van Parameters][edit_parameters]

10. Klik nu op **Deploy**. **Het uitvoervenster** , wordt de voortgang implementatie weergegeven. Houd er rekening mee dat het de actie het script **Deploy-AzureResourceGroup.ps1** wordt uitgevoerd.

    ![Uitvoervenster][output_window]

## <a name="exploring-your-vm-scale-set"></a>Een Set van de schaal VM verkennen

Zodra de implementatie is voltooid, kunt u de nieuwe VM schaal instellen kunt weergeven in de Visual Studio **Cloud Explorer** (de lijst vernieuwen). Cloud Explorer kunt u Azure resources in Visual Studio tijdens het ontwikkelen van toepassingen beheren. U kunt ook uw VM schaal instellen weergeven in de Portal van Azure en Azure Resource Explorer.

![Cloud Explorer][cloud_explorer]

 De portal biedt de beste manier om visueel beheren van uw Azure infrastructuur met een webbrowser, terwijl Azure Resource Explorer een gemakkelijke manier explorer biedt en fouten opsporen in Azure resources, geeft een venster in de weergave' instantie' en ook waarin PowerShell-opdrachten voor de resources die u bekijkt. Hoewel VM schaal Sets in de proefversie zijn, wordt de Resource Explorer de meeste details weergeven voor uw VM schaal Sets.

## <a name="next-steps"></a>Volgende stappen

Zodra u hebt geïmplementeerd VM schaal Sets tot en met Visual Studio kunt u uw project aan de toepassingsvereisten van uw verder aanpassen. Bijvoorbeeld bij het instellen van automatisch schalen door het toevoegen van een resource inzichten, infrastructuur toevoegen aan uw sjabloon, bijvoorbeeld zelfstandige VMs of toepassingen met de extensie aangepast script implementeren. Een goede bron van voorbeeld sjablonen vindt u in de [Azure Quickstart sjablonen](https://github.com/Azure/azure-quickstart-templates) GitHub opslagplaats (zoekt "vmss").

[file_new]: ./media/virtual-machines-common-scale-sets-visual-studio/1-FileNew.png
[create_project]: ./media/virtual-machines-common-scale-sets-visual-studio/2-CreateProject.png
[select_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/6-DeployTemplate.png
[new_resource]: ./media/virtual-machines-common-scale-sets-visual-studio/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machines-common-scale-sets-visual-studio/8-EditParameter.png
[output_window]: ./media/virtual-machines-common-scale-sets-visual-studio/9-Output.png
[cloud_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/12-CloudExplorer.png