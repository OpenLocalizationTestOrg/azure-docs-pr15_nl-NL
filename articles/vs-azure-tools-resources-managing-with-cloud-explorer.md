<properties 
   pageTitle="Azure bronnen beheren met Cloud Explorer | Microsoft Azure"
   description="Leer hoe u Cloud Explorer gebruiken om te bladeren en beheren van Azure bronnen binnen Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-azure-resources-with-cloud-explorer"></a>Azure bronnen beheren met Cloud Explorer

##<a name="overview"></a>Overzicht

Cloud Explorer is bedoeld om u kunt snel en eenvoudig onderzoeksverslagen bladeren en beheren van uw Azure bronnen binnen de Visual Studio IDE. U kunt, bijvoorbeeld het openen van een Web-app in de [portal van Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) of in een browser gebruiken of een foutopsporing bijvoegen, of u kunt de eigenschappen van een container blob weergeven en vervolgens opent in de Blob Container Editor.

Cloud Explorer is gebaseerd op de stapel Azure resource manager, net als de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Dit kunt u resources zoals Azure resourcegroepen en Azure services zoals logica apps en API-apps, en [Rolgebaseerd toegangsbeheer](./active-directory/role-based-access-control-configure.md) (RBAC) wordt ondersteund. Kies de knop **vernieuwen** op de werkbalk Cloud Explorer overzicht van Azure resources die zijn toegevoegd of gewijzigd.

Cloud Explorer is geïnstalleerd als onderdeel van de Visual Studio Tools voor Azure SDK 2.7. 

## <a name="prerequisites"></a>Vereisten voor

- Visual Studio 2015 RTM.

- De Visual Studio Tools for Azure SDK. 
- U moet ook een Azure-account hebben en zijn aangemeld bij het weergeven van Azure resources in de Cloud Explorer. Als u deze niet hebt, kunt u een account maken in een paar minuten. Als u een MSDN-abonnement hebt, raadpleegt u [Azure voordeel voor MSDN-abonnees](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Anders zien [een gratis proefabonnement-account maken](https://azure.microsoft.com/pricing/free-trial/).

- Als Cloud Explorer niet zichtbaar is, kunt u deze kunt weergeven door **weergave**, **Andere Windows,** **Cloud Explorer** op de menubalk te kiezen.

## <a name="manage-azure-accounts-and-subscriptions"></a>Azure-accounts en abonnementen beheren

Als u wilt zien van uw Azure resources in de Cloud Explorer, moet u aanmelden bij een Azure-account met een of meer actieve abonnementen. Als u meer dan één Azure-account hebt, kunt u deze toevoegen in de Cloud Explorer en kies vervolgens de abonnementen die u wilt opnemen in de weergave van de resource Cloud Explorer.

Als u Azure voordat u dit nog niet hebt gebruikt, of u de benodigde rekeningen niet hebt toegevoegd aan Visual Studio, wordt u gevraagd dat te doen.

## <a name="to-add-azure-accounts-to-cloud-explorer"></a>Azure-mailaccounts naar Cloud Explorer toevoegen

1. Kies het pictogram instellingen op de werkbalk Cloud Explorer.

1. Kies de koppeling **een account toevoegen** . Meld u aan bij de Azure-account waarvan bronnen die u wilt zoeken. Het account dat u zojuist hebt toegevoegd, moet zijn geselecteerd in de vervolgkeuzelijst van de account kiezer. De abonnementen voor dat account weergegeven onder de post.

    ![Azure abonnementen toevoegen](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Azure abonnementen kiezen](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Schakel de selectievakjes voor de account-abonnementen die u wilt bladeren en kies vervolgens de knop **toepassen** .

    De Azure bronnen voor de geselecteerde abonnementen weergegeven in Cloud Verkenner.

## <a name="to-remove-an-azure-account"></a>Een Azure-account verwijderen

1. Kies **bestand**, **Accountinstellingen** op de menubalk.

1. Kies in de sectie **Alle Accounts** in het dialoogvenster **Accountinstellingen** , de opdracht **verwijderen** naast het account dat u wilt verwijderen. Opmerking dat deze opdracht alleen verwijdert u het account uit Visual Studio – it heeft geen invloed op de Azure account zelf.

## <a name="view-resource-types-or-groups"></a>Resource-weergavetypen of groepen

Als u wilt uw Azure resources weergeven, kunt u **Resourcetypen** of **Resourcegroepen** weergave kiezen.

![Vervolgkeuzelijsten resource](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- **Resourcetypen** weergave, dat wil ook de algemene weergave in de [portal van Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040)gebruikt zeggen, ziet u uw Azure resources gecategoriseerd op type, zoals WebApps, opslag-accounts en virtuele machines. Dit is vergelijkbaar hoe Azure resources wilt weergeven in Server Explorer.

- Groepen resourceweergave gecategoriseerd Azure resources op Azure resourcegroep die ze bent verbonden.

 
    Een resourcegroep is een bundel van Azure resources, meestal gebruikt door een specifieke toepassing. Zie meer informatie over Azure resourcegroepen [Azure resourcemanager overzicht](./resource-group-overview.md).

## <a name="view-and-navigate-resources"></a>Bekijken en resources navigeren

Om te navigeren naar een Azure-bron en de informatie in de Cloud Explorer bekijken, vouwt u het type of de bijbehorende resourcegroep van het item en kies vervolgens de resource. Als u een resource kiest, wordt informatie weergegeven in de twee tabbladen onderaan in de Cloud Explorer.

![Kies een resourceweergave toe](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- Het tabblad **Acties** ziet u de acties die u kunt uitvoeren in de Cloud Explorer voor de geselecteerde resource. U kunt ook beschikbare acties bekijken in het snelmenu van de resource.

- Het tabblad **Eigenschappen** ziet u de eigenschappen van de resource, zoals de groep van het type, landinstelling en resource die is gekoppeld.

Elke resource heeft de actie **geopend in de portal**. Als u deze actie kiest, wordt de geselecteerde resource in Cloud Explorer weergegeven in de [portal van Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). Deze functie is vooral handig voor het navigeren naar nauw genest resources.

Aanvullende acties en eigenschapswaarden mogelijk ook weergegeven op basis van de Azure resource. Bijvoorbeeld tevens WebApps en logica apps de acties **in een browser** en **bijvoegen foutopsporing** naast **geopend in de portal**. Acties te openen editors weergegeven wanneer u een account-blob storage, wachtrij of tabel kiest. Azure apps hebben de eigenschappen van **URL** en de **Status** opslag resources sleutel en verbinding tekenreekseigenschappen.

## <a name="search-resources"></a>Resources zoeken

Als u wilt zoeken naar resources met een specifieke naam in uw abonnementen Azure-account, voert u de naam in het vak Zoeken in de Cloud Explorer.

![Zoeken van resources in de Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

Terwijl u tekens in het vak Zoeken invoert, wordt alleen de resources die overeenkomen met deze tekens worden weergegeven in de resourcestructuur.

