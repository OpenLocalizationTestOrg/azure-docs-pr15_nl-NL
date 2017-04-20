<properties
   pageTitle="De vereiste waarden ophalen voor het verifiëren van een toepassing voor toegang tot SQL-Database vanaf code | Microsoft Azure"
   description="Maak de hoofdsom van een service voor het werken met SQL-Database van code."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>De vereiste waarden ophalen voor het verifiëren van een toepassing voor toegang tot SQL-Database vanaf code

Als u wilt maken en beheren van SQL-Database van code moet u uw app registreren in het domein Azure Active Directory (AAD) in het abonnement waarop uw Azure resources zijn gemaakt.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Maak een service hoofdsom voor toegang tot bronnen van een toepassing

Moet u beschikken over de meest recente [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) geïnstalleerd en uit te voeren. Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor gedetailleerde informatie.

Het volgende PowerShell-script Hiermee maakt u de toepassing AD (Active Directory) en de service-hoofdsom die we nodig hebt om te verifiëren van onze C#-app. De waarden die we nodig voor de voorgaande C#-steekproef uitgevoerd. Zie [Azure PowerShell gebruiken om te maken van een service principal toegang krijgen tot bronnen](../resource-group-authenticate-service-principal.md)voor gedetailleerde informatie.

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Zie ook

- [Een SQL-database maken met C#](sql-database-get-started-csharp.md)
- [Verbinding maken met SQL-Database met Azure Active Directory-verificatie](sql-database-aad-authentication.md)


