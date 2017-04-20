<properties
    pageTitle="via een programma controleren taken op Stream Analytics | Microsoft Azure"
    description="Leer hoe u via programmacode bewaken Stream Analytics-taken die zijn gemaakt via een REST API's, Azure SDK of Powershell."
    keywords=".NET-beeldscherm, taak monitor, app bewaken"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Via een programma een monitor Stream Analytics-taak maken
 In dit artikel wordt beschreven hoe controle voor een taak Stream Analytics inschakelen. Stream Analytics taken die zijn gemaakt via een REST API's, Azure SDK of Powershell doen monitoring niet hebt ingeschakeld al dan niet standaard.  U kunt handmatig dit inschakelen in de Portal Azure door te navigeren naar de pagina Monitor van de taak en te klikken op de knop inschakelen of kunt u dit proces automatiseren volgens de stappen in dit artikel. De controlegegevens wordt weergegeven in het tabblad 'Beeldscherm' in de Portal Azure voor uw taak Stream Analytics.

![monitor met een taak tabblad taken](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Vereisten voor
Voordat u in dit artikel begint, hebt u het volgende:

- Visual Studio 2012 of 2013.
- Download en installeer [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Een bestaande Stream Analytics-taak die nog moet worden ingeschakeld voor controle.

## <a name="setup-a-project"></a>Een project instellen

1.  Maak een Visual Studio C# .net-console-toepassing.
2.  In de Console Package Manager, voer de volgende opdrachten voor het installeren van de NuGet-pakketten. De eerste fase is de Azure Stream Analytics Management .NET SDK. Het tweede is de SDK van Azure Monitor die worden gebruikt om in te schakelen voor controle. De laatste waarde de Azure Active Directory-client die wordt gebruikt voor verificatie is.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  De volgende sectie appSettings toevoegen aan het bestand App.config.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Waarden voor *SubscriptionId* en *ActiveDirectoryTenantId* vervangen door uw Azure-abonnement en id's tenant. U kunt deze waarden krijgen door het volgende PowerShell-cmdlet uit te voeren:

    ```
    Get-AzureAccount
    ```
4.  Voeg de volgende beweringen met in het bronbestand (Program.cs) in het project.

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Voeg een helper verificatiemethode.

        public static string GetAuthorizationHeader()
            {
                AuthenticationResult result = null;
                var thread = new Thread(() =>
                {
                    try
                    {
                        var context = new AuthenticationContext(
                            ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                            ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                        result = context.AcquireToken(
                            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                            clientId: ConfigurationManager.AppSettings["AsaClientId"],
                            redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                            promptBehavior: PromptBehavior.Always);
                    }
                    catch (Exception threadEx)
                    {
                        Console.WriteLine(threadEx.Message);
                    }
                });

                thread.SetApartmentState(ApartmentState.STA);
                thread.Name = "AcquireTokenThread";
                thread.Start();
                thread.Join();

                if (result != null)
                {
                    return result.AccessToken;
                }

                throw new InvalidOperationException("Failed to acquire token");
        }

## <a name="create-management-clients"></a>Management Clients maken
De volgende code wordt de benodigde variabelen en management clients instellen.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Controle voor een bestaande taak van de Stream-analyses inschakelen

De volgende code wordt de controle voor een **bestaande** taak van de Stream Analytics inschakelen. Het eerste deel van de code kunt u een GET-aanvraag ten opzichte van de Stream Analytics-service om op te halen informatie over de geselecteerde taak van de Stream analyses uitvoeren. Wordt de eigenschap "Id" (opgehaald uit de GET-aanvraag) gebruikt als een parameter van de methode opslag in de tweede helft van de code die wordt verzonden een opslag verzoek aan de service inzichten voor de taak Stream Analytics-controle inschakelen.

> [AZURE.WARNING]
> Als u eerder hebt ingeschakeld monitoring voor een andere baan van Stream analyses, via de Portal Azure of via een programma via de onderstaande code, **het wordt aanbevolen dat u opgeeft dat dezelfde opslag accountnaam toen u eerder hebt ingeschakeld voor controle.**
>
> Het account opslag is gekoppeld aan het gebied dat u uw werk Stream Analytics in hebt gemaakt, niet specifiek aan het project zelf.
>
> Alle Stream Analytics taak (en alle andere Azure resources) in dat dezelfde gebied delen dit account opslag om op te slaan controlegegevens. Als u een andere opslag-account, kan dit onverwachte vraagt om de controle van uw andere Stream Analytics-taken en/of andere Azure resources veroorzaken.
>
> De naam van het opslag-account gebruikt voor het vervangen van ```“<YOUR STORAGE ACCOUNT NAME>”``` hieronder moet een opslag-account dat is in hetzelfde abonnement, zoals de Stream analytische gegevens van taken u wilt inschakelen voor controle.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Ondersteuning
Probeer onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor meer hulp.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
