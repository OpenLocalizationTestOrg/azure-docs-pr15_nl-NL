## <a name="how-to-deploy-with-azure-cli"></a>Hoe u met Azure CLI implementeren

1. Meld u aan uw Azure-account.

        azure login

  Na het opgeven van uw referenties, resultaat de opdracht het van uw aanmelding.

        ...
        info:    login command OK

2. Als u meerdere abonnementen hebt, vindt u de abonnements-id die u wilt gebruiken voor implementatie.

        azure account set <YourSubscriptionNameOrId>

3. Ga naar de resourcemanager Azure-module

        azure config mode arm

   U ontvangt bevestiging van de nieuwe modus.

        info:     New mode is arm

4. Als u een bestaande resourcegroep niet hebt, maakt u een nieuwe resourcegroep. Geef de naam van de resourcegroep en de locatie die u nodig voor uw oplossing hebt.

        azure group create -n ExampleResourceGroup -l "West US"

   Een overzicht van de nieuwe resourcegroep wordt geretourneerd.

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Als u wilt een nieuwe installatie voor uw resourcegroep maakt, voer de volgende opdracht en geef de benodigde parameters. De parameters bevat een naam voor de implementatie, de naam van uw resourcegroep, het pad of de URL naar de sjabloon die u hebt gemaakt en eventuele andere parameters die u nodig hebt voor uw scenario.

   Hebt u de volgende opties voor het leveren van parameterwaarden:

   - Inline-parameters gebruiken en een lokale sjabloon.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Inline-parameters gebruiken en een koppeling naar een sjabloon.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Gebruik een parameterbestand.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Wanneer de resourcegroep is geïmplementeerd, ziet u een overzicht van de implementatie.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Voor informatie over de meest recente implementatie.

         azure group log show -l ExampleResourceGroup

7. Voor gedetailleerde informatie over fouten voor implementatie.

         azure group log show -l -v ExampleResourceGroup
