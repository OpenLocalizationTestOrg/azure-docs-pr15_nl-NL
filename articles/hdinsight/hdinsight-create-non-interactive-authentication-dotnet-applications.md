<properties
    pageTitle="Maken van niet-interactieve verificatie .NET HDInsight applciations | Microsoft Azure"
    description="Informatie over het maken van niet-interactieve verificatie .NET HDInsight-toepassingen."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Niet-interactieve verificatie .NET HDInsight-toepassingen maken

U kunt uw toepassing .NET Azure HDInsight onder van de toepassing identiteit (niet-interactieve) of onder de identiteit van de gebruiker die zijn aangemeld in van de toepassing (interactieve) uitvoeren. Zie voor een steekproef van de interactieve toepassing, [indienen component/varken/Sqoop taken met HDInsight .NET SDK](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Dit artikel leest u hoe u niet-interactieve verificatie .NET-toepassing verbinding maken met Azure HDInsight en het verzenden van een taak component maakt.

Vanuit uw .NET-toepassing, hebt u het volgende nodig:

- uw abonnement op Azure tenant-ID
- de client-ID van Azure-Directory-toepassing
- de Azure-Directory geheime toepassingstoets.  

Het belangrijkste proces bestaat uit de volgende stappen uit:

2. Maak een Azure-Directory-toepassing.
2. Rollen toewijzen aan de AD-toepassing.
3. Ontwikkel uw clienttoepassing.


##<a name="prerequisites"></a>Vereisten voor

- Cluster HDInsight. U kunt een volgens de instructies die zijn gevonden in de [handleiding aan de slag](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster)te maken. 




## <a name="create-azure-directory-application"></a>Azure-Directory-toepassing maken 
Wanneer u een Active Directory-toepassing maakt, deze daadwerkelijk Hiermee maakt u zowel de toepassing als een service principal. U kunt de toepassing onder de identiteit van de toepassing uitvoeren.

Momenteel, moet u de Azure klassieke portal gebruiken om een nieuwe Active Directory-toepassing te maken. Deze mogelijkheid worden, toegevoegd aan de Azure-portal in een latere versie. U kunt ook deze stappen via Azure PowerShell of Azure CLI uitvoeren. Zie voor meer informatie over het gebruik van PowerShell of CLI met de service principal [verifiëren service principal met Azure Resource Manager](../resource-group-authenticate-service-principal.md).

**Een Azure-Directory-toepassing maken**

1.  Meld u aan bij de [portal van Azure klassieke]( https://manage.windowsazure.com/).
2.  Selecteer **Active Directory** in het linkerdeelvenster.

    ![Azure klassieke portal active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Selecteer de map die u gebruiken wilt voor het maken van de nieuwe toepassing. Het is wel het bestaande bestand.
4.  Klik op **toepassingen** vanaf de bovenkant voor een overzicht van de bestaande toepassingen.
5.  Klik op **toevoegen** vanaf de onderkant naar een nieuwe toepassing toevoegen.
6.  Voer **naam**, selecteert u **webtoepassing en/of Web API**en klik vervolgens op **volgende**.

    ![nieuwe azure active directory-toepassing](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Voer **de URL-aanmelding** en **App -ID-URI**. Geef voor **Aanmelding op URL**, de URI naar een website waarop uw toepassing wordt beschreven. De aanwezigheid van de website niet worden gevalideerd. Geef de URI waarmee uw toepassing voor de APP-ID-URI. En klik vervolgens op **Voltooien**.
Duurt het even om de toepassing te maken.  Nadat de toepassing is gemaakt, ziet u in de portal de snelle Glace pagina van de nieuwe toepassing. Sluit de portal niet. 

    ![nieuwe eigenschappen van de azure active directory-toepassing](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Om de toepassingsclient-ID en de geheime sleutel**

1.  Klik op **configureren** in het bovenste menu op de pagina nieuw gemaakte AD-toepassing.
2.  Hiermee maakt u een kopie van **De Client-ID**. Dit moet u in uw .NET-toepassing.
3.  Onder **toetsen**, klik op de vervolgkeuzelijst **Selecteer duur** en selecteer **1 jaar** of **2 jaar**. De sleutelwaarde wordt niet weergegeven totdat u de configuratie opslaat.
4.  Klik op **Opslaan** vanaf de onderkant van de pagina. Wanneer de geheime sleutel wordt weergegeven, maakt u een kopie van de sleutel. Dit moet u in uw .NET-toepassing.

##<a name="assign-ad-application-to-role"></a>AD-toepassing aan rol toewijzen

U moet de toepassing aan een [rol](../active-directory/role-based-access-built-in-roles.md) toe te kennen machtigingen voor het uitvoeren van acties toewijzen. U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. De machtigingen worden overgenomen op lagere niveaus van reikwijdte (bijvoorbeeld het toevoegen van een toepassing aan de rol van de lezer voor een resourcegroep betekent dat deze de resourcegroep kunt lezen en er resources bevat). In deze zelfstudie wordt u het bereik instellen op het niveau van de groep resource.  Omdat de Azure klassieke portal geen resourcegroepen ondersteunt, heeft dit gedeelte worden uitgevoerd vanuit de Azure-portal. 

**De rol van eigenaar toevoegen aan de AD-toepassing**

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com).
2.  Klik op **Resourcegroep** in het linkerdeelvenster.
3.  Klik op de resourcegroep met het HDInsight cluster waar u uw query component verderop in deze zelfstudie wordt uitgevoerd. Als er te veel resourcegroepen, kunt u het filter.
4.  Klik op **toegang** van het blad cluster.

    ![pictogram cloud en thunderbolt = quickstart](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Klik op **toevoegen** vanuit het blad **gebruikers** .
6.  Volg de instructies in de rol van **eigenaar** toevoegen aan de AD-toepassing die u hebt gemaakt in de laatste procedure. Wanneer u deze met succes voltooit, ziet u mag de toepassing weergegeven in het blad gebruikers met de rol van de eigenaar.


##<a name="develop-hdinsight-client-application"></a>Ontwikkel HDInsight-clienttoepassing

Maak een C# .net-console-toepassing volgen de instructies in [indienen Hadoop-taken in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Vervang de methode GetTokenCloudCredentials door het volgende:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

Aan de Tenant-ID via PowerShell ophalen:

    Get-AzureRmSubscription

Of Azure CLI:

    azure account show --json

      
## <a name="see-also"></a>Zie ook

- [Hadoop-taken in HDInsight indienen](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Active Directory-toepassing en service principal met behulp van portal maken](../resource-group-create-service-principal-portal.md)
- [Service principal met Azure resourcemanager verifiëren](../resource-group-authenticate-service-principal.md)
- [Azure Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md)
