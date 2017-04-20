<properties
   pageTitle="Service principal in de portal maken | Microsoft Azure"
   description="Beschrijving van het maken van een nieuwe Active Directory-toepassing en service principal die kan worden gebruikt met het Rolgebaseerd toegangsbeheer in Azure resourcemanager voor het beheren van toegang tot bronnen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Portal gebruiken om Active Directory-toepassing en service principal die toegang bronnen tot te maken

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Wanneer u een toepassing dat u wilt openen of wijzigen van resources hebt, moet u een toepassing AD (Active Directory) instellen en de vereiste machtigingen toewijzen aan deze. In dit onderwerp ziet u hoe u deze stappen uitvoert via de portal. Momenteel, moet u de klassieke portal maken van een nieuwe Active Directory-toepassing en Ga naar de Azure-portal een rol toewijzen aan de toepassing. 

> [AZURE.NOTE] De stappen in dit onderwerp zijn alleen van toepassing wanneer u de AD-toepassing maken met de **klassieke portal** . **Als u de portal van Azure gebruikt voor het maken van de AD-toepassing, mislukt deze stappen.** 
>
> U wellicht handiger voor het instellen van uw AD-toepassing en service principal via [PowerShell](resource-group-authenticate-service-principal.md) of [Azure CLI](resource-group-authenticate-service-principal-cli.md), vooral als u wilt gebruiken van een certificaat voor verificatie. In dit onderwerp wordt niet weergegeven voor het gebruik van een certificaat.

Zie voor een uitleg van Active Directory concepten, [toepassingen en Service Principal objecten](./active-directory/active-directory-application-objects.md). Zie voor meer informatie over de verificatie van Active Directory, [Verificatie-scenario's voor Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Zie voor meer informatie over de integratie van een toepassing in Azure voor het beheren van resources [handleiding voor ontwikkelaars naar autorisatie met de API van Azure resourcemanager](resource-manager-api-authentication.md).

## <a name="create-an-active-directory-application"></a>Een Active Directory-toepassing maken

1. Meld u aan bij uw Account Azure via de [klassieke portal](https://manage.windowsazure.com/). 

2. Zorg ervoor dat u de standaard Active Directory weet voor uw abonnement. U kunt alleen toegang voor toepassingen in dezelfde map als uw abonnement. Selecteer **Instellingen** en zoek de naam van de map die is gekoppeld aan uw abonnement.  Zie [hoe Azure-abonnementen zijn gekoppeld aan Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)voor meer informatie.
   
     ![standaardmap zoeken](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. Selecteer **Active Directory** in het linkerdeelvenster.

     ![Selecteer Active Directory](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Selecteer de Active Directory die u gebruiken wilt voor het maken van de toepassing. Als u meer dan één Active Directory hebt, kunt u de toepassing maken in de standaardmap voor uw abonnement.   

     ![Kies map](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Als u wilt de toepassingen weergeven in uw adreslijst, selecteer **toepassingen**.

     ![weergave-toepassingen](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Als u een toepassing in die map voordat u dit nog niet hebt gemaakt, ziet u een vergelijkbare in de volgende afbeelding. Selecteer **een toepassing toevoegen**

     ![toepassing toevoegen](./media/resource-group-create-service-principal-portal/create-application.png)

     Of, klikt u op **toevoegen** in het onderste deelvenster.

     ![toevoegen](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Selecteer het type van toepassing die u wilt maken. Selecteer voor deze zelfstudie **toepassing het ontwikkelen van mijn organisatie toevoegen**. 

     ![nieuwe toepassing](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Geef een naam voor de toepassing en selecteer het type van toepassing die u wilt maken. Een **WEB APPLICATION en/of WEB API** maken en klik op de knop volgende voor deze zelfstudie. Als u **SYSTEEMEIGEN CLIENTTOEPASSING**selecteert, de overige stappen van dit artikel komen niet overeen met het gebruik van SharePoint.

     ![de naam van toepassing](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Vul de eigenschappen voor de app. Geef voor **Aanmelding op URL**, de URI naar een website waarop uw toepassing wordt beschreven. De aanwezigheid van de website niet worden gevalideerd. Geef de URI waarmee uw toepassing voor de **APP-ID-URI**.

     ![Eigenschappen van een servicetoepassing](./media/resource-group-create-service-principal-portal/app-properties.png)

U kunt uw toepassing hebt gemaakt.

## <a name="get-client-id-and-authentication-key"></a>Client-id en verificatie sleutel ophalen

Wanneer via programmacode aanmelden, moet u de id voor uw toepassing. Als de toepassing wordt uitgevoerd onder een eigen referenties, moet u ook een verificatiesleutel.

1. Selecteer het tabblad **configureren** van uw toepassing wachtwoord te configureren.

     ![een toepassing configureren](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Kopieer de **CLIENT-ID**.
  
     ![cliënt-id](./media/resource-group-create-service-principal-portal/client-id.png)

3. Als de toepassing wordt uitgevoerd onder een eigen referenties, schuif omlaag naar de sectie **toetsen** en geef aan hoe lang u wilt uw wachtwoord geldig.

     ![toetsen](./media/resource-group-create-service-principal-portal/create-key.png)

4. Selecteer **Opslaan** om uw sleutel te maken.

     ![opslaan](./media/resource-group-create-service-principal-portal/save-icon.png)

     De opgeslagen sleutel wordt weergegeven en kunt u deze kopiëren. U bent niet mogen de toets later ophalen dus nu kopiëren.

     ![toets opgeslagen](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Tenant id ophalen

Wanneer u via programmacode zich aanmeldt, moet u de tenant-id met uw authenticatie-verzoek doorgeeft. Voor de Web Apps en Web API-Apps, kunt u de tenant-id ophalen door **weergave eindpunten** onderaan in het scherm selecteren en het terughalen van de id zoals wordt weergegeven in de volgende afbeelding.  

   ![tenant-id](./media/resource-group-create-service-principal-portal/save-tenant.png)

U kunt ook de id van de tenant via PowerShell ophalen:

    Get-AzureRmSubscription

Of Azure CLI:

    azure account show --json

## <a name="set-delegated-permissions"></a>Machtigingen instellen die worden overgedragen

Als uw toepassing toegang krijgt resources namens een aangemelde gebruiker tot, moet u uw toepassing verlenen de gedelegeerde machtiging voor toegang tot andere toepassingen. U toegang deze in de sectie **machtigingen voor andere toepassingen** van het tabblad **configureren** . Standaard is al een gedelegeerd machtiging ingeschakeld voor de Azure Active Directory. Laat deze gedelegeerd machtiging ongewijzigd.

1. Selecteer **de toepassing toevoegen**.

2. Selecteer de **Windows Azure Service Management API**in de lijst. Selecteer het pictogram voltooid.

      ![app selecteren](./media/resource-group-create-service-principal-portal/select-app.png)

3. Selecteer in de vervolgkeuzelijst voor machtigingen voor gedelegeerd **Beheer van een Access-Azure-Service, als de organisatie**.

      ![Schakel het selectievakje machtiging](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Sla de wijziging.

## <a name="assign-application-to-role"></a>Toepassing aan rol toewijzen

Als uw toepassing wordt uitgevoerd onder een eigen referenties, moet u de toepassing aan een rol toewijzen. Bepalen welke rol de juiste machtigingen voor de toepassing vertegenwoordigt. Zie voor meer informatie over de beschikbare rollen [RBAC: rollen ingebouwd](./active-directory/role-based-access-built-in-roles.md). 

Als u wilt een rol toewijzen aan een toepassing, moet u de juiste machtigingen hebt. Specifiek, moet u `Microsoft.Authorization/*/Write` toegang is verleend tot en met de rol van de [eigenaar](./active-directory/role-based-access-built-in-roles.md#owner) of de [gebruiker toegang tot](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) de beheerdersrol. De rol Inzender heeft geen toegang.

U kunt het bereik instellen op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van reikwijdte. Bijvoorbeeld een toepassing aan de rol van de lezer voor een resourcegroep toe te voegen betekent dat deze kan worden gelezen resourcegroep en alle resources die deze bevat.

1. Als u wilt de toepassing aan een rol toewijzen, gaan in de klassieke portal bij de [portal van Azure](https://portal.azure.com)

1. Controleer uw machtigingen om te controleren of dat u kunt de hoofdsom service toewijzen aan een rol. Selecteer **Mijn machtigingen** voor uw account.

    ![Selecteer Mijn machtigingen](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. Bekijk de toegewezen machtigingen voor uw account. Zoals eerder is vermeld, moet u deel uitmaakt van de rollen eigenaar of beheerder van de gebruiker toegang of een aangepaste rol waarmee schrijftoegang voor Microsoft.Authorization hebt. De volgende afbeelding ziet u een account dat is toegewezen aan de rol Inzender voor het abonnement, dat wil zeggen niet voldoende machtigingen voor een toepassing aan een rol toewijzen.

    ![Mijn machtigingen weergeven](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Als u niet beschikt over de juiste machtigingen om toegang te verlenen aan een toepassing, moet u een verzoek om dat de beheerder van uw abonnement u aan de gebruiker toegang-beheerdersrol toevoegt of die aanvragen verleent een beheerder toegang tot de toepassing.

1. Ga naar het niveau van een bereik dat u wilt toewijzen van de toepassing. Als u wilt toewijzen een rol bij het abonnement bereik, selecteert u **abonnementen**.

     ![Selecteer abonnement](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Selecteer het bepaald abonnement de toepassing aan toewijzen.

     ![Selecteer abonnement voor toewijzing](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     Selecteer het **Access** -pictogram in de rechterbovenhoek.

     ![Selecteer van access](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Of, als u wilt een rol bij bereik resource toewijzen, gaat u naar een resourcegroep. Selecteer in het blad van de groep resource **beheren in Access**.

     ![Selecteer gebruikers](./media/resource-group-create-service-principal-portal/select-users.png)

     De volgende stappen zijn hetzelfde voor elk bereik.

2. Selecteer **toevoegen**.

     ![Selecteer toevoegen](./media/resource-group-create-service-principal-portal/select-add.png)

3. Selecteer de rol van de **lezer** (of welke rol die u wilt de toepassing toewijst aan).

     ![Selecteer rol](./media/resource-group-create-service-principal-portal/select-role.png)

4. Wanneer u de lijst met gebruikers die u aan de rol toevoegen kunt voor het eerst ziet, worden er geen toepassingen. U ziet alleen groeperen en gebruikers.

     ![gebruikers laten zien](./media/resource-group-create-service-principal-portal/show-users.png)

5. Als u wilt uw toepassing hebt gevonden, zoekt u deze. Typ de naam van uw toepassing en de lijst met beschikbare opties wordt gewijzigd. Selecteer uw toepassing wanneer dit wordt weergegeven in de lijst.

     ![toewijzen aan rol](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Selecteer **OK** om te voltooien voor het toewijzen van de rol. U ziet nu de toepassing in de lijst toegewezen aan een rol voor de resourcegroep doeleinden.


Zie voor meer informatie over het toewijzen van gebruikers en rollen via de portal-toepassingen [roltoewijzingen gebruiken voor het beheren van toegang tot uw resources Azure-abonnement](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## <a name="sample-applications"></a>Voorbeeldtoepassingen

De volgende toepassingen van de steekproef hoe meld u aan als de hoofdsom service.

**.NET**

- [Een SSH implementeren VM met een sjabloon met .NET ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure resources en resourcegroepen met .NET beheren](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Aan de slag met informatiebronnen - implementeren met Azure resourcemanager sjabloon - in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Aan de slag met informatiebronnen - beheren resourcegroep - in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Een SSH implementeren VM met een sjabloon in Python ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure Resource en resourcegroepen met Python beheren](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Een SSH implementeren VM met een sjabloon in Node.js ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure resources en resourcegroepen met Node.js beheren](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Een SSH implementeren VM met een sjabloon in Ruby ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure Resource en resourcegroepen met Ruby beheren](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het opgeven van beveiligingsbeleid voor apparaten, [Toegangsbeheer op basis van Azure rol](./active-directory/role-based-access-control-configure.md).  
- Zie voor een videodemonstratie over deze stappen, [Inschakelen via programmacode beheren van een Resource Azure met Azure Active Directory](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

