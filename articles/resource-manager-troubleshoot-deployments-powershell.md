<properties
   pageTitle="Implementatie bewerkingen met PowerShell weergeven | Microsoft Azure"
   description="Wordt beschreven hoe u met de PowerShell Azure problemen uit resourcemanager implementatie opsporen."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Implementatie bewerkingen weergeven met Azure PowerShell

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API](resource-manager-troubleshoot-deployments-rest.md)

U kunt de bewerkingen voor een implementatie via de PowerShell Azure weergeven. U mogelijk interessant bekijken van de bewerkingen als er een fout optreedt zodat in dit artikel bevat informatie over het weergeven van de bewerkingen die zijn mislukt tijdens de implementatie hebt. PowerShell biedt cmdlets waarmee u kunt eenvoudig zoeken de fouten en mogelijke oplossingen bepalen.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

U kunt enkele fouten voorkomen door het valideren van uw sjabloon en de infrastructuur vóór implementatie. U kunt ook extra aanvraag en antwoord informatie tijdens de implementatie die mogelijk nuttig later voor probleemoplossing vastleggen. Zie voor meer informatie over valideren en vergaderverzoeken en antwoorden logboekinformatie, [Deploy resourcegroep met Azure resourcemanager sjabloon](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Implementatie bewerkingen gebruiken om op te lossen

1. Als u de algemene status van een implementatie, gebruikt u de opdracht **Get-AzureRmResourceGroupDeployment** . De resultaten voor alleen die implementaties die niet zijn, kunt u filteren.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Die de mislukte implementaties resulteert in de volgende indeling:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Elke implementatie is meestal bestaat uit meerdere bewerkingen, met elke bewerking dat staat voor een stap in het implementatieproces. Als u wilt ontdekken wat er fout is gegaan met een implementatie, moet u meestal voor meer informatie over de implementatie-bewerkingen. Hier ziet u de status van de bewerkingen met **Get-AzureRmResourceGroupDeploymentOperation**.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Die resulteert meerdere bewerkingen met elke rij in de volgende indeling:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Als u meer informatie over mislukte bewerkingen, de eigenschappen voor bewerkingen met **mislukt** die worden opgehaald.

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Welke retourneert alle mislukte bewerkingen met elke rij in de volgende indeling:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Opmerking de bijhouden-ID voor de bewerking. U gebruikt die in de volgende stap kunt richten op een bepaalde bewerking.

4. Als u het statusbericht van een bepaalde mislukte bewerking, gebruikt u de volgende opdracht uit:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Welke geeft als resultaat:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Controlelogboeken gebruiken om op te lossen

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Als u wilt zien fouten voor een implementatie, gebruik de volgende stappen uit:

1. Als u wilt ophalen logboekvermeldingen, voert u de opdracht **Get-AzureRmLog** . U kunt de parameters **ResourceGroup** en **Status** gebruiken om terug te keren alleen de gebeurtenissen die is voor een één resourcegroep is mislukt. Als u een begin- en -tijd niet opgeeft, worden de vermeldingen voor het laatste uur geretourneerd.
Als u bijvoorbeeld om op te halen de mislukte bewerkingen voor de afgelopen uur uitvoeren:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    U kunt een bepaalde tijdspanne opgeven. In het volgende voorbeeld, zullen we voor mislukte acties voor de laatste dag. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Of, kunt u een exacte begin- en eindtijd voor mislukte acties instellen:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Als deze opdracht het resultaat te veel items en eigenschappen, kunt u zich richten de controle-inspanningen door op te halen van de eigenschap **Properties** . We wordt ook de parameter **DetailedOutput** als u wilt zien van de foutberichten bevatten.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Welke eigenschappen van de logboekvermeldingen resulteert in de volgende indeling:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Op basis van deze resultaten, laten we richten op het tweede element. U kunt de resultaten verder verfijnen door te zoeken bij het statusbericht voor dat item.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Welke geeft als resultaat:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Volgende stappen

- Zie [oplossen van veelvoorkomende fouten bij de implementatie van Azure met Azure resourcemanager-informatiebronnen](resource-manager-common-deployment-errors.md)voor hulp bij het oplossen van implementatiefouten bepaalde.
- Voor meer informatie over het gebruik van de controlelogboeken om de andere soorten acties te houden, raadpleegt u [bewerkingen met resourcemanager controleren](resource-group-audit.md).
- Zie [Deploy resourcegroep met Azure resourcemanager sjabloon](resource-group-template-deploy.md)valideren van uw implementatie voordat deze wordt uitgevoerd.

