<properties
    pageTitle="Aan de slag met de bibliotheek van Azure CDN voor .NET | Microsoft Azure"
    description="Leer hoe u schrijven voor het beheren van Azure CDN gebruik van Visual Studio .NET-toepassingen."
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Aan de slag met Azure CDN ontwikkeling

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

U kunt de [Azure CDN bibliotheek voor .NET](https://msdn.microsoft.com/library/mt657769.aspx) automatiseren maken en beheren van CDN-profielen en eindpunten.  Deze zelfstudie begeleidt bij het maken van een eenvoudige .NET-console-toepassing, waarop u verscheidene van de beschikbare bewerkingen.  Deze zelfstudie is niet bedoeld om te beschrijven van alle aspecten van de Azure CDN-bibliotheek voor .NET uitgebreid beschreven.

Hebt u Visual Studio-2015 om te voltooien van deze zelfstudie nodig.  [Visual Studio-Community-2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) is gratis te downloaden.

> [AZURE.TIP] Het [project uit deze zelfstudie voltooid](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) is beschikbaar voor downloaden op MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Uw project maken en toevoegen van Nuget-pakketten

Nu we hebt gemaakt van een resourcegroep voor onze CDN-profielen en ze onze Azure AD-toepassing toestemming om CDN profielen en eindpunten in deze groep te beheren, we kunt beginnen met het maken van de toepassing.

Klik in Visual Studio-2015 verlengt, op **bestand**, **Nieuw** **Project …** om het nieuwe project-dialoogvenster.  Vouw **Visual C#**en selecteer vervolgens **Windows** in het deelvenster aan de linkerkant.  Klik op **Console-toepassing** in het middelste deelvenster.  Naam van uw project en klik vervolgens op **OK**.  

![Nieuw Project](./media/cdn-app-dev-net/cdn-new-project.png)

Onze project dient te sommige Azure bibliotheken in de Nuget-pakketten gebruiken.  Laten we deze toevoegen aan het project.

1. Klik op het menu **Extra** , **Nuget Package Manager**en klik op **Package Manager-Console**.

    ![Nuget-pakketten beheren](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. Voer de volgende opdracht voor het installeren van de **Active Directory verificatie bibliotheek (ADAL)**in de Console Package Manager:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. De volgende handelingen uit om te kunnen installeren van de **Azure CDN Management bibliotheek**uitvoeren:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Richtlijnen, constanten belangrijkste methode en methoden

We gaan de basisstructuur van ons programma voor geschreven.

1. Vervang terug in het tabblad Program.cs de `using` richtlijnen aan de bovenkant met de volgende handelingen uit:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. Moeten we enkele constanten die onze methoden wordt gebruikt definiëren.  In de `Program` klasse, maar voordat de `Main` methode toevoegen de volgende handelingen uit.  Zorg ervoor dat u voor het vervangen van de tijdelijke aanduidingen, inclusief de ** &lt;punthaken&gt;**, met uw eigen waarden naar wens.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Ook op het niveau van class deze twee variabelen definiëren.  We gebruiken deze later om te bepalen als ons profiel- en eindpunt al bestaan.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Vervang de `Main` methode als volgt:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Enkele van onze andere methoden gaat het bericht met "Ja/Nee" vragen.  De volgende methode om te vereenvoudigen die iets toevoegen:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Nu de basisstructuur van ons programma is geschreven, we moet maken de methoden aangeroepen door de `Main` methode.

## <a name="authentication"></a>Verificatie

Voordat we de bibliotheek van Azure CDN Management gebruiken kunt, moeten we onze hoofdsom service verifiëren en verkrijgen van een verificatietoken.  Deze methode wordt ADAL om op te halen het token.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Als u afzonderlijke gebruikersverificatie, gebruikt de `GetAccessToken` methode wordt er enigszins anders uitzien.

>[AZURE.IMPORTANT] In dit voorbeeld wordt alleen gebruiken als u ervoor kiest om afzonderlijke gebruikersverificatie in plaats van een service principal.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Zorg ervoor dat u voor het vervangen van `<redirect URI>` met de omleiding URI ingevoerde wanneer u de toepassing geregistreerd in Azure AD.

## <a name="list-cdn-profiles-and-endpoints"></a>Lijst met CDN profielen en eindpunten

Nu we gaan CDN bewerkingen uit te voeren.  Het eerste wat u doet onze methode lijst is alle profielen en eindpunten in onze resourcegroep en als er een overeenkomst wordt gevonden voor de profiel- en eindpunt namen die zijn opgegeven in onze constanten, maakt een notitie van die in voor later zodat we niet probeert te maken van duplicaten.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN-profielen en eindpunten maken

Vervolgens gaan we een profiel maken.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Nadat het profiel is gemaakt, gaan we een eindpunt te maken.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] Het bovenstaande voorbeeld wordt de toegewezen het eindpunt een map *Contoso* met een hostname origin `www.contoso.com`.  U moet dit te laten verwijzen naar uw eigen oorsprong van hostname wijzigen.

## <a name="purge-an-endpoint"></a>Een eindpunt verwijderen

Als dat het eindpunt is gemaakt, is een algemene taak die we mogelijk wilt uitvoeren in ons programma voor de inhoud in onze eindpunt verwijderen.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] In het voorbeeld hierboven, de tekenreeks `/*` wordt aangegeven dat ik wil alles in de hoofdmap van het pad eindpunt verwijderen.  Dit is gelijk aan **Alle leegmaken** inchecken van de Azure portal "verwijderen" dialoogvenster. In de `CreateCdnProfile` methode, ik heb onze profiel gemaakt als een **Azure CDN uit Verizon** profiel met de code `Sku = new Sku(SkuName.StandardVerizon)`, zodat dit geverifieerd worden kan.  **Azure CDN van Akamai** profielen niet ondersteunen echter **Alle leegmaakt**, zodat er als ik een Akamai-profiel voor deze zelfstudie gebruikt is, zou ik nodig om op te nemen specifieke paden wissen.

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN-profielen en eindpunten verwijderen

De laatste methoden verwijderd onze eindpunt en een profiel.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Het programma

We kunnen nu compileren en het programma door te klikken op de knop **Start** in Visual Studio uit te voeren.

![Programma wordt uitgevoerd](./media/cdn-app-dev-net/cdn-program-running-1.png)

Wanneer het programma voor de bovenstaande vraag bereikt, moet u mogelijk zijn terug naar de resourcegroep in de portal van Azure en ziet u dat het profiel is gemaakt.

![Succes!](./media/cdn-app-dev-net/cdn-success.png)

We kunnen de aanwijzingen voor het uitvoeren van de rest van het programma vervolgens bevestigen.

![Programma voltooien](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Volgende stappen

U wilt zien van de voltooide project van deze procedure, [downloaden van de steekproef](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Bekijk de [verwijzing op MSDN](https://msdn.microsoft.com/library/mt657769.aspx)aanvullende documentatie op de bibliotheek van Azure CDN Management voor .NET vindt.

Het beheren van uw resources CDN met [PowerShell](./cdn-manage-powershell.md).


