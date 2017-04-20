<properties
    pageTitle="Implementatie van Azure Resources met C# | Microsoft Azure"
    description="Leer hoe u C# en Azure resourcemanager Microsoft Azure resources maken."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Azure Resources met C implementeren# 

In dit artikel leest u het maken van Azure resources met C#.

Moet u eerst om ervoor te zorgen dat u klaar bent met het volgende taken kunnen uitvoeren:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx) installeren
- Controleer of de installatie van [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Een [verificatietoken](../resource-group-authenticate-service-principal.md) ophalen

Het duurt ongeveer 30 minuten moet u deze stappen doen.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Stap 1: Een project Visual Studio maken en de bibliotheken installeren

NuGet-pakketten zijn de eenvoudigste manier om het installeren van de bibliotheken die u moet deze zelfstudie hebt voltooid. Als u de bibliotheken die u nodig in Visual Studio hebt, volgt u deze stappen:

1. Klik op **bestand** > **nieuwe** > **Project**.

2. In **sjablonen** > **Visual C#**, selecteer **Console-toepassing**, voert u de naam en locatie van het project en klik vervolgens op **OK**.

3. Met de rechtermuisknop op de naam van het project in de Solution Explorer en klik op **NuGet-pakketten beheren**.

4. Type, *Active Directory* in het zoekvak, klikt u op **installeren** voor de bibliotheek van Active Directory-verificatie-pakket en volg de instructies voor het installeren van het pakket.

5. Selecteer boven aan de pagina, **Opnemen voorlopige versie**. Type *Microsoft.Azure.Management.Compute* in het zoekvak, klikt u op **installeren** voor de bibliotheken .NET berekenen en volg de instructies voor het installeren van het pakket.

6. Type *Microsoft.Azure.Management.Network* in het zoekvak, klikt u op **installeren** voor de netwerk .NET-bibliotheken en volg de instructies voor het installeren van het pakket.

7. Type *Microsoft.Azure.Management.Storage* in het zoekvak, klikt u op **installeren** voor de opslag .NET-bibliotheken en volg de instructies voor het installeren van het pakket.

8. Typ *Microsoft.Azure.Management.ResourceManager* in het zoekvak en klik op **installeren** voor de Resource Management-bibliotheken.

U bent nu klaar om te beginnen met het gebruik van de bibliotheken voor het maken van uw toepassing.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Stap 2: De referenties die worden gebruikt voor verificatie van aanvragen maken

Nu opmaken u de toepassingsgegevens dat u eerder hebt gemaakt in de referenties die worden gebruikt voor verificatie van aanvragen naar Azure resourcemanager.

1. Open het bestand Program.cs voor het project dat u hebt gemaakt en voegt u deze met instructies naar het begin van het bestand:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Als wilt maken van de token die nodig is, voegt u deze methode aan de klasse programma:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    {Client-id} vervangen door de id van de Azure Active Directory-toepassing, {client-geheim} met de access-sleutel van de AD-toepassing en {tenant-id} aan de tenant-id voor uw abonnement. U vindt de id van de tenant door Get-AzureRmSubscription uit te voeren. U kunt de toegangstoets vinden met behulp van de Azure-portal.

3. Als u wilt bellen de methode die u eerder hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven in het bestand Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Sla het bestand Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Stap 3: De resource-providers hebt geregistreerd en de resources maken

### <a name="register-the-providers-and-create-a-resource-group"></a>De providers hebt geregistreerd en het maken van een resourcegroep

Alle resources moeten worden opgenomen in een resourcegroep. Voordat u resources aan een groep toevoegen kunt, moet u uw abonnement registreren met andere aanbieders van de resource.

1. Variabelen toevoegen aan de methode Main van de klasse programma de namen die u wilt gebruiken voor de resources opgeven:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    De variabele waarden vervangen door de namen en de id die u wilt gebruiken. U vindt de id van het abonnement door Get-AzureRmSubscription uit te voeren.

2. De resourcegroep maken en om te registreren om de providers deze methode toevoegen aan de klas programma:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Als u wilt bellen de methode die u eerder hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Een opslag-account maken

Een [opslag-account](../storage/storage-create-storage-account.md) nodig is voor de opslag van het virtuele harde schijf-bestand dat is gemaakt voor de virtuele machine.

1. Om de opslag-account maken, door deze methode te toevoegen aan de klas programma:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Als u wilt bellen de methode die u eerder hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven van de klasse programma:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken

Een openbare IP-adres is nodig om te communiceren met de virtuele machine.

1. Als wilt het openbare IP-adres van de virtuele machine maken, voegt u deze methode aan de klasse programma:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Als u wilt bellen de methode die u eerder hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven van de klasse programma:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Het virtuele netwerk maken

Een virtuele machine die gemaakt met het implementatiemodel resourcemanager moeten zich in een virtueel netwerk.

1. Als wilt maken een subnet en een virtueel netwerk, voegt u deze methode aan de klasse programma:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Als u wilt bellen de methode die u eerder hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven van de klasse programma:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>De netwerkinterface maken

Een virtuele machine moet een netwerkinterface om te communiceren op het virtuele netwerk.

1. Als wilt maken van een netwerkinterface, voegt u deze methode aan de klasse programma:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Als u wilt bellen de methode die u eerder hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven van de klasse programma:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Een set beschikbaarheid maken

Beschikbaarheid sets kunnen u eenvoudiger kunt beheren van het onderhoud van de virtuele machines gebruikt door de toepassing.

1. Als wilt de beschikbaarheid van de set maken, voegt u deze methode aan de klasse programma:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Als u wilt bellen de methode die u eerder hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven van de klasse programma:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Nu u de ondersteunende bronnen hebt gemaakt, kunt u een virtuele machine maken.

1. Als wilt maken van de virtuele machine, voegt u deze methode aan de klasse programma:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] Deze zelfstudie Hiermee maakt u een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van de andere afbeeldingen, [navigeren en selecteer Azure virtuele machines afbeeldingen met Windows PowerShell en de CLI Azure](virtual-machines-linux-cli-ps-findimage.md).

2. Als u wilt bellen de methode die u eerder hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Stap 4: De resources verwijderen

Omdat u worden berekend voor resources die worden gebruikt in Azure wordt aangegeven, maar het is altijd een goede gewoonte om het verwijderen van resources die niet meer nodig zijn. Als u wilt verwijderen van de virtuele machines en alle ondersteunende bronnen, is enige u hoeft te doen verwijderen van het resourceveld groep.

1.  De als resourcegroep wilt verwijderen, door deze methode te toevoegen aan de klas programma:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Als u wilt bellen de methode die u eerder hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Stap 5: Voer de consoletoepassing

1. Als u wilt de consoletoepassing uitvoert, klikt u op **starten** in Visual Studio en meld u aan bij Azure AD met dezelfde gebruikersnaam en hetzelfde wachtwoord dat u met uw abonnement gebruikt.

2. Druk op **Enter** na elk statuscode elke resource maken. Nadat de virtuele machine is gemaakt, volgt u de volgende stap voordat u op Enter om het verwijderen van alle resources.

    Het moet over vijf minuten duren voordat deze consoletoepassing om uit te voeren volledig vanaf begin om te voltooien. Als u op Enter om te beginnen u resources verwijdert, kunt u een paar minuten om te controleren of het maken van de resources in de portal van Azure voordat u ze verwijderen kunt volgen.

3. Als u wilt zien van de status van de bronnen, gaat u naar de controlelogboeken bijhouden in de portal van Azure:

    ![Controlelogboeken bijhouden in Azure-portal bladeren](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Volgende stappen

- Maak gebruik van het gebruik van een sjabloon voor een virtuele machine maken met behulp van de gegevens aan [een Azure virtuele machines Deploy met C# en een resourcemanager-sjabloon](virtual-machines-windows-csharp-template.md).
- Informatie over het beheren van de virtuele machine die u aan de hand van [beheren virtuele machines met Azure resourcemanager en PowerShell](virtual-machines-windows-csharp-manage.md)hebt gemaakt.
