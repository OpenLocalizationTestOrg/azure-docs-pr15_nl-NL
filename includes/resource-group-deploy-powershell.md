## <a name="how-to-deploy-with-powershell"></a>Het implementeren van met PowerShell

1. Meld u aan uw Azure-account.

          Add-AzureAccount

   De opdracht na het opgeven van uw referenties, geeft als resultaat informatie over uw account.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Als u meerdere abonnementen hebt, vindt u de abonnements-id die u wilt gebruiken voor implementatie. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Ga naar de resourcemanager Azure-module.

          Switch-AzureMode AzureResourceManager

4. Als u een bestaande resourcegroep niet hebt, maakt u een nieuwe resourcegroep. Geef de naam van de resourcegroep en de locatie die u nodig voor uw oplossing hebt.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   Een overzicht van de nieuwe resourcegroep wordt geretourneerd.

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Als u wilt een nieuwe installatie voor uw resourcegroep maakt, voert u de opdracht **Nieuw AzureResourceGroupDeployment** en geef de benodigde parameters. De parameters bevat een naam voor de implementatie, de naam van uw resourcegroep, het pad of de URL naar de sjabloon die u hebt gemaakt en eventuele andere parameters die u nodig hebt voor uw scenario. 
   
   Hebt u de volgende opties voor het leveren van parameterwaarden: 
   
   - Inline parameters gebruiken.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Gebruik een parameter-object.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Een parameterbestand gebruiken.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Wanneer de resourcegroep is geïmplementeerd, ziet u een overzicht van de implementatie.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Voor informatie over fouten voor implementatie.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Voor gedetailleerde informatie over fouten voor implementatie.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
