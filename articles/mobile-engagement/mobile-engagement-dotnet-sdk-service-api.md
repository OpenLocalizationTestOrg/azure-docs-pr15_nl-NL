<properties 
    pageTitle=".NET SDK gebruiken voor toegang tot Azure Mobile betrokkenheid Service API 's" 
    description="Wordt beschreven hoe u de mobiele betrokkenheid .NET SDK gebruiken voor toegang tot Azure Mobile betrokkenheid Service API 's"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>.NET SDK gebruiken voor toegang tot Azure Mobile betrokkenheid Service API 's

Azure Mobile betrokkenheid beschrijft een reeks API's voor het beheer van apparaten, bereik/Push campagnes, enzovoort. Interactie met deze API's, ook krijgt u een [Swagger-bestand](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) dat u kunt gebruiken met hulpmiddelen voor SDK's genereren voor de voorkeurstaal. Het is raadzaam om uw SDK genereren uit onze Swagger-bestand met het hulpprogramma [AutoRest](https://github.com/Azure/AutoRest) . 

We hebben een .NET-SDK gemaakt op een soortgelijke wijze die kunt u werken met deze API's met behulp van een C#-Wikkel en u niet hoeft te doen? de verificatie token-onderhandelingen en vernieuwen zelf.  

In dit voorbeeld wordt de reeks stappen te volgen als u wilt de .NET SDK gebruiken:

1. Ten eerste moet u de verificatie voor uw API's bij het gebruik van de Azure Active Directory als beschreven [hier](mobile-engagement-api-authentication.md#authentication)instellen. Aan het einde van deze stappen, kunt u een geldig **SubscriptionId**, **TenantId**, **ApplicationId** en **geheim**nodig hebt. 

2. We een eenvoudige Windows-Console-app gebruiken om te laten zien werken met de .NET SDK met het scenario voor het maken van een aankondiging campagne. Zo opent u Visual Studio en een **Console-toepassing**maken.   

3. Vervolgens moet u de .NET SDK die beschikbaar als **Microsoft Azure betrokkenheid Management bibliotheek** in de galerie Nuget is downloaden [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Als u de Nuget vanuit Visual Studio installeert, moet u zorgen dat u de optie **voorlopige versie opnemen hebt** bij het zoeken naar het pakket ingeschakeld:

    ![][1]

4. In de `Program.cs` bestand, voegt u de volgende naamruimten:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Vervolgens moet u de volgende constanten die we gebruiken voor verificatie en interactief werken met de Mobile-betrokkenheid-App waarin u de aankondiging campagne maakt definiëren:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Definieer de `EngagementManagementClient` variabele die we gebruiken om te bellen van de mobiele betrokkenheid SDK methoden:

        static EngagementManagementClient engagementClient; 

7. Voeg de volgende naar uw `Main` methode:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. De volgende methode die voor geïnitialiseerd zorgt definiëren de `EngagementManagementClient` door eerst verificatie en zelf vervolgens koppelen aan de Mobile-betrokkenheid-App waarin u van plan bent om de campagne aankondiging te maken:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Houd er rekening mee dat u de **Naam van de Resource App** gebruikt moet, zoals gedefinieerd in de portal Azure management voor de parameter toepassingsnaam. 

9. Ten slotte definiëren de CreateCampaign methode die handelingen kunt verrichten wordt van het gebruik van de eerder geïnitialiseerde EngagementClient voor het maken van een eenvoudige **op elk gewenst moment** & **melding alleen-lezen** campagne met een titel en een bericht: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. De console-app uitvoeren en ziet u de volgende handelingen uit op de installatie van de campagne:

    **Campagne-Id '159' is gemaakt en is de status 'Concept'**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
