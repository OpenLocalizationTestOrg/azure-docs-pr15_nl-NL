<properties
    pageTitle="Altijd versleuteld: Gevoelige gegevens beschermen in Azure SQL-Database met versleuteling van de database | Microsoft Azure"
    description="Vertrouwelijke gegevens in uw SQL-database beveiligen in minuten."
    keywords="gegevensversleuteling, versleuteling toets, cloud-versleuteling"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Altijd versleuteld: Gevoelige gegevens in een SQL-Database beveiligen en bewaren van uw versleutelingssleutels in Azure-toets kluis

> [AZURE.SELECTOR]
- [Azure belangrijke kluis](sql-database-always-encrypted-azure-key-vault.md)
- [Windows-certificaat store](sql-database-always-encrypted.md)


In dit artikel leest u het beveiligen van vertrouwelijke gegevens in een SQL-database versleutelen van gegevens met de [Wizard altijd versleuteld](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Het bevat ook instructies die wordt beschreven hoe u voor de opslag van elke toets versleuteling in Azure-toets kluis.

Versleutelde is altijd een nieuwe versleutelingstechnologie van gegevens in Azure SQL-Database en SQL Server die voorkomen dat vertrouwelijke gegevens in rust op de server, tijdens verplaatst tussen client en server en terwijl de gegevens wordt gebruikt. Altijd versleutelde zorgt ervoor dat vertrouwelijke gegevens nooit wordt weergegeven als tekst zonder opmaak in de databasesysteem. Nadat u gegevensversleuteling configureren, alleen clienttoepassingen of app-servers die toegang tot de toetsen hebben toegang heeft tot leesbare gegevens. Zie [Altijd versleuteld (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx)voor gedetailleerde informatie.


Nadat u de database als u wilt gebruiken altijd versleuteld configureert, maakt u een clienttoepassing in C# met Visual Studio om te werken met de versleutelde gegevens.

Volg de stappen in dit artikel en leer hoe u altijd versleuteld instellen voor een Azure SQL-database. In dit artikel leert u hoe u de volgende taken uitvoeren:

- Gebruik de wizard altijd versleuteld in SSMS om [altijd versleuteld sleutels](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)te maken.
    - Een [kolom outmodel sleutel (CMK)](https://msdn.microsoft.com/library/mt146393.aspx)maakt.
    - Een [kolom versleutelingssleutel (CEK)](https://msdn.microsoft.com/library/mt146372.aspx)maakt.
- Een databasetabel maken en versleutelen kolommen.
- Maak een toepassing die wordt ingevoegd, selecteert u en gegevens uit de versleutelde kolommen worden weergegeven.


## <a name="prerequisites"></a>Vereisten voor

Voor deze zelfstudie hebt u het volgende nodig:

- Een Azure-account en abonnementen. Als u niet hebt, registreren voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) versie 13.0.700.242 of hoger.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) of hoger (op de clientcomputer).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
- [Azure PowerShell](../powershell-install-configure.md), versie 1.0 of hoger. Type **(Get-Module azure - ListAvailable). Versie** om te zien welke versie van PowerShell u uitvoert.



## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>De clienttoepassing voor toegang tot de service SQL-Database inschakelen

U moet de clienttoepassing voor toegang tot de service SQL-Database door de vereist verificatie instellen en bij het verkrijgen van het *ClientId* en *geheim* die u nodig hebt om te verifiëren van uw toepassing in de volgende code inschakelen.

1. Open de [portal van Azure klassieke](http://manage.windowsazure.com).
2. **Active Directory** en klik op het Active Directory-exemplaar dat uw toepassing moet gebruiken.
3. Klik op **toepassingen**en klik vervolgens op **toevoegen**.
4. Typ een naam voor uw toepassing (bijvoorbeeld: *myClientApp*), selecteert u **WEBTOEPASSING**en klik op de pijl om door te gaan.
5. U kunt een geldige (bijvoorbeeld *http://myClientApp*)-URL hebt getypt en doorgaan voor de **Aanmelding op URL** en de **APP-ID-URI** .
6. Klik op **configureren**.
7. Kopieer uw **klant-ID**. (U moet deze waarde in de code later.)
8. Selecteer in de sectie **toetsen** **1 jaar** uit de vervolgkeuzelijst **Selecteer duur** . (Kopieert u de toets nadat u hebt opgeslagen in stap 14.)
11. Schuif omlaag en klik op **toevoegen-toepassing**.
12. Laat waarde **weergeven** op de **Microsoft-Apps** en selecteer **Beheer van Microsoft Azure-Service**. Klik op het vinkje om door te gaan.
13. Selecteer **Toegangsbeheer Azure-Service** in de vervolgkeuzelijst **Gedelegeerde machtigingen** .
14. Klik op **Opslaan**.
15. Nadat de opslaan is voltooid, kopieert u de sleutelwaarde in de sectie **toetsen** . (U moet deze waarde in de code later.)



## <a name="create-a-key-vault-to-store-your-keys"></a>Een belangrijke kluis om op te slaan uw sleutels maken

Nu uw client-app is geconfigureerd en u uw klant-ID hebt, is het tijd een belangrijke kluis maken en configureren van de access-beleid, zodat u en uw toepassing toegang hebt tot van de kluis geheimen (de toetsen altijd versleuteld). Het *maken*, *krijgen*, *lijst*, *aanmelden*, *verifiëren*, *wrapKey*en *unwrapKey* machtigingen zijn vereist voor het maken van een nieuwe kolom outmodel sleutel en voor het instellen van versleuteling met SQL Server Management Studio.

U kunt snel een belangrijke kluis maken door het volgende script uit te voeren. Zie [aan de slag met Azure toets kluis](../Key-Vault/key-vault-get-started.md)voor een gedetailleerde uitleg van deze cmdlets en meer informatie over het maken en configureren van een belangrijke kluis.



    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).SubscriptionId
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup –Name $resourceGroupName –Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Een lege SQL-database maken
1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Ga naar de **nieuwe** > **gegevens + opslagruimte** > **SQL-Database**.
3. Maak een **lege** database met de naam **kliniek** op een nieuw of bestaand-server. Zie voor gedetailleerde instructies over het maken van een database in de portal van Azure [maken een SQL-database in minuten](sql-database-get-started.md).

    ![Maak een lege database](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Moet u de verbinding tekenreeks verderop in deze zelfstudie, dus nadat u de database hebt gemaakt, blader naar de nieuwe kliniek-database en kopieer de verbindingsreeks. U kunt de verbindingsreeks ophalen op elk gewenst moment, maar het is eenvoudig om deze te kopiëren in de portal van Azure.

1. Ga naar de **SQL-databases** > **kliniek** > **tekenreeksen voor databaseverbinding weergeven**.
2. Kopieer de verbindingsreeks voor **ADO.NET**.

    ![Kopieer de verbindingsreeks](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Verbinding maken met de database met SSMS

Open SSMS en verbinding maken met de server met de database kliniek.


1. Open SSMS. (Ga naar de **verbinding maken met** > **Database Engine** het **verbinding maken met de Server** -venster openen als dit nog niet geopend.)
2. Voer uw servernaam en de referenties. Naam van de server kan worden gevonden op het blad SQL-database en in de verbindingstekenreeks u eerder hebt gekopieerd. Typ de naam van de volledige server, inclusief *database.windows.net*.

    ![Kopieer de verbindingsreeks](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Als het **nieuwe** regelvenster uit een Firewall wordt geopend, meld u aan bij Azure en SSMS maken van een nieuwe firewallregel voor u laten.


## <a name="create-a-table"></a>Een tabel maken

In dit gedeelte maakt u een tabel naar patiënten gegevens bevatten. Het is niet in eerste instantie versleuteld--configureert u versleuteling in het volgende gedeelte.

1. **Databases**uitvouwen.
1. Met de rechtermuisknop op de database **kliniek** en klik op **Nieuwe Query**.
2. De nieuwe queryvenster en **uitvoeren** in de volgende Transact-SQL (T-SQL) plakt dit.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Kolommen (altijd versleuteld configureren) versleutelen

SSMS biedt een wizard waarmee u gemakkelijk geconfigureerd altijd versleuteld met het instellen van de kolom outmodel sleutel, kolom-versleutelingssleutel en versleutelde kolommen voor u.

1. **Databases**uitvouwen > **kliniek** > **tabellen**.
2. Met de rechtermuisknop op de tabel **patiënten** en selecteert u **Versleutelen kolommen** om de wizard altijd versleuteld te openen:

    ![Kolommen versleutelen](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

De wizard altijd versleuteld bevat de volgende secties: **Selectie van kolommen**, **Outmodel toets configuratie**, **gegevensvalidatie**en **Overzicht**.

### <a name="column-selection"></a>Kolom selecteren##

Klik op **volgende** op **de introductiepagina om de **Selectie van kolom** -pagina te openen** . Op deze pagina, selecteert u welke kolommen u versleutelen wilt, [het type codering, en welke kolom versleutelingssleutel (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) gebruiken.

Coderen **sofi-nummer** en **Geboortedatum** gegevens voor elke geduld. De kolom sofi-nummer wordt gebruikt in deterministic codering, waardoor ondersteunt gelijke zoekacties, joins en groeperen op. De geboortedatum kolom wordt gebruikt in willekeurig codering, waardoor wordt niet ondersteund.

Het **Type codering** voor de kolom sofi-nummer **Deterministic** en de kolom Geboortedatum **Randomized**instellen. Klik op **volgende**.

![Kolommen versleutelen](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Configuratie van de basispagina-toets###

De pagina **Configuratie van de basispagina sleutel** is waar u uw CMK instellen en selecteer de provider van de belangrijkste store waar de CMK worden opgeslagen. U kunt op dit moment een CMK opslaan in de Windows-certificaat store, Azure-toets kluis of een hardware HSM (security module).

Deze zelfstudie leert hoe u het opslaan van uw sleutels in Azure-toets kluis.

1.     Selecteer **Azure belangrijke kluis**.
1.     Selecteer de gewenste belangrijke kluis in de vervolgkeuzelijst.
1.     Klik op **volgende**.

![Configuratie van de basispagina-toets](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)


### <a name="validation"></a>Gegevensvalidatie###

U kunt nu de kolommen versleutelen of opslaan van een PowerShell-script later uit te voeren. Voor deze zelfstudie **Doorgaan om te voltooien nu** en klik op **volgende**.

### <a name="summary"></a>Overzicht ###

Controleer of de instellingen voor alle corrigeren en klik op **Voltooien** om de instellingen voor altijd versleuteld.


![Overzicht](./media/sql-database-always-encrypted-azure-key-vault/summary.png)


### <a name="verify-the-wizards-actions"></a>Controleer of de acties van de wizard

Nadat de wizard voltooid is, wordt uw database instellen voor het altijd versleuteld. De wizard heeft de volgende acties uitgevoerd:

- Een kolom outmodel sleutel gemaakt en opgeslagen in Azure-toets kluis.
- Een kolom versleutelingssleutel gemaakt en opgeslagen in Azure-toets kluis.
- De geselecteerde kolommen voor versleuteling geconfigureerd. De tabel Patiënten momenteel geen gegevens bevat, maar er bestaande gegevens in de geselecteerde kolommen worden nu versleuteld.

U kunt controleren of het maken van de toetsen in SSMS door uit te vouwen **kliniek** > **beveiliging** > **Toetsen altijd versleuteld**.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Een clienttoepassing die met het versleutelde gegevens werkt maken

Nu dat altijd versleuteld is ingesteld, kunt u een toepassing waarmee *wordt ingevoegd* en *selecteert u* op de versleutelde kolommen kunt maken.  

> [AZURE.IMPORTANT] Uw toepassing moet [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) -objecten gebruiken als tekst zonder opmaak gegevens worden doorgegeven naar de server met kolommen altijd versleuteld. Een uitzondering treden doorgeven van letterlijke waarden zonder SqlParameter objecten.

1. Open Visual Studio en maak een nieuwe C#-console-toepassing. Controleer of dat uw project is ingesteld op **.NET Framework 4.6** of hoger.
2. Naam van het project **AlwaysEncryptedConsoleAKVApp** en klik op **OK**.
![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/console-app.png)
3. De volgende NuGet-pakketten installeren door te gaan naar **Extra** > **NuGet Package Manager** > **Package Manager-Console**.

Deze twee regels met code worden uitgevoerd in de beheerconsole van pakket.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>De verbindingsreeks om in te schakelen altijd versleuteld wijzigen

In deze sectie wordt uitgelegd hoe altijd versleuteld inschakelen in de verbindingsreeks van uw database.


Als u wilt inschakelen op altijd versleuteld, moet u het trefwoord **Kolom versleuteling instelling** toevoegen aan uw verbindingsreeks en stel deze in op **ingeschakeld**.

U kunt deze rechtstreeks in de verbindingstekenreeks instellen of kunt u deze instellen met behulp van [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). De voorbeeldtoepassing in het volgende gedeelte ziet hoe u **SqlConnectionStringBuilder**gebruiken.



### <a name="enable-always-encrypted-in-the-connection-string"></a>Altijd versleuteld inschakelen in de verbindingstekenreeks

Het volgende trefwoord toevoegen aan uw verbindingsreeks.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Altijd versleuteld met SqlConnectionStringBuilder inschakelen

De volgende code ziet hoe u altijd versleuteld inschakelen door in te stellen [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) op [ingeschakeld](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>De provider Azure-toets kluis registreren

De volgende code ziet hoe u de provider kluis van Azure-toets met het stuurprogramma ADO.NET registreren.

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a>Altijd versleutelde steekproef console-toepassing

In dit voorbeeld wordt gedemonstreerd hoe u:

- Wijzig de verbindingsreeks om in te schakelen altijd versleuteld.
- Azure-toets kluis registreren als provider van de belangrijkste store van de toepassing.  
- Gegevens in de versleutelde kolommen invoegen.
- Een record met een filter op een bepaalde waarde in een versleutelde kolom selecteren.

De inhoud van **Program.cs** vervangen door de volgende code. Vervang de verbindingsreeks voor de globale connectionString variabele in de regel die rechtstreeks voorafgaat aan de methode Main met uw geldige verbindingsreeks van de Azure-portal. Dit is de enige wijziging die u wilt aanbrengen in deze code.

De app om te zien altijd versleuteld in actie uitvoeren.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }



## <a name="verify-that-the-data-is-encrypted"></a>Controleer of dat de gegevens worden gecodeerd

U kunt snel controleren dat de werkelijke gegevens op de server worden gecodeerd door de gegevens patiënten query's uitvoeren met SSMS (met de huidige verbinding waarvoor nog niet **Kolom versleuteling instelling** is ingeschakeld).

De volgende query uitvoeren op de kliniek-database.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

U kunt zien dat de versleutelde kolommen geen leesbare gegevens bevatten.

   ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)


Als u wilt gebruiken SSMS toegang tot de gegevens van de tekst zonder opmaak, kunt u toevoegen de *kolom versleuteling instelling = ingeschakeld* -parameter voor de verbinding.

1. In SSMS, met de rechtermuisknop op de server in **Object Explorer** en kies **verbinding verbreken**.
2. Klik op **verbinding maken met** > **Database Engine** om te openen van het venster **verbinding maken met de Server** en klikt u op **Opties**.
3. Klik op **Extra verbindingsparameters** en typ **kolom versleuteling instelling = ingeschakeld**.

    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)

4. De volgende query uitvoeren op de kliniek-database.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     U kunt nu de gegevens van de tekst zonder opmaak in de versleutelde kolommen zien.


    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Volgende stappen
Nadat u een database die altijd versleuteld gebruikt hebt gemaakt, wilt u mogelijk als volgt te werk:

- [Draaien en uw sleutels opschonen](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migreren gegevens die al is versleuteld met altijd versleuteld](https://msdn.microsoft.com/library/mt621539.aspx).


## <a name="related-information"></a>Gerelateerde informatie

- [Altijd versleuteld (client ontwikkeling)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server-versleuteling](https://msdn.microsoft.com/library/bb510663.aspx)
- [Altijd versleutelde wizard](https://msdn.microsoft.com/library/mt459280.aspx)
- [Altijd versleutelde blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
