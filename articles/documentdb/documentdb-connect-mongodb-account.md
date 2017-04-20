<properties 
    pageTitle="Verbinding maken met een account DocumentDB met protocolondersteuning voor MongoDB | Microsoft Azure" 
    description="Informatie over het verbinding maken met een account DocumentDB met ondersteuning voor MongoDB, nu beschikbaar voor de preview-protocol. Verbinding maken met behulp van de verbindingsreeks MongoDB." 
    keywords="de verbindingsreeks mongodb"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Hoe u verbinding maakt met een account DocumentDB met protocolondersteuning voor MongoDB

Leer hoe u verbinding maken met een account in Azure DocumentDB bij protocolondersteuning voor MongoDB gebruiken standaard MongoDB URI indeling van de verbindingsreeks.  

## <a name="get-the-accounts-connection-string-information"></a>De accountgegevens van de tekenreeks verbinding

1. In een nieuw venster, meld u aan bij de [Portal van Azure](https://portal.azure.com).
2. Klik in de **Links** navigatiebalk van het blad Account, op **Verbindingsreeks**. Naar de **Account Blade**wilt gaan, klik op de Jumpbar Klik op **Meer Services**op **DocumentDB (NoSQL)** en selecteer vervolgens het account DocumentDB met protocolondersteuning voor MongoDB.

    ![Schermafbeelding van het blad alle instellingen](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. Het blad **Verbindingsinformatie** wordt geopend en alle informatie die verbinding maken met het account met behulp van een stuurprogramma voor MongoDB, met inbegrip van een vooraf samengestelde verbindingsreeks nodig heeft.

    ![Schermafbeelding van het blad van de tekenreeks verbinding](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Vereisten voor verbinding tekenreeks

Het is belangrijk te weten dat DocumentDB standaard MongoDB URI indeling van de verbindingsreeks, met een aantal specifieke vereisten ondersteunt: DocumentDB accounts verificatie en beveiligde communicatie via SSL vereist.  Indeling van de verbindingsreeks is dus:

    mongodb://username:password@host:port/[database]?ssl=true

Waar zijn de waarden van deze tekenreeks beschikbaar in het verbindingsreeks blad hierboven.

- Gebruikersnaam (vereist)
    - De naam van de DocumentDB-account
- Wachtwoord (vereist)
    - DocumentDB accountwachtwoord
- Host (vereist)
    - FQDN van DocumentDB-account
- Poort (vereist)
    - 10250
- Database (optioneel)
    - De standaard-database die wordt gebruikt door de verbinding
- SSL = true (vereist)

Stel het account weergegeven in de bovenstaande verbindingsinformatie.  Een geldige verbindingsreeks luidt als volgt:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Verbinding maken met het C#-stuurprogramma voor MongoDB
Zoals reeds vermeld, alle DocumentDB-accounts is verificatie en beveiligde communicatie via SSL vereist. Terwijl MongoDB URI indeling van de verbindingsreeks een ssl ondersteunt parameter waar queryreeks, werken met de MongoDB-C# = stuurprogramma hiervoor moet u het object MongoClientSettings gebruiken bij het maken van een MongoClient.  Het volgende codefragment ziet gegeven van de bovenstaande accountgegevens, u hoe u verbinding maken met het account en werken met de database 'Taken'.

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Volgende stappen


- Leer hoe u [MongoChef gebruiken](documentdb-mongodb-mongochef.md) met een account DocumentDB met protocol ondersteuning voor MongoDB.
- Uitzoeken DocumentDB met protocolondersteuning voor MongoDB [voorbeelden](documentdb-mongodb-samples.md).

 
