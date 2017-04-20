<properties 
   pageTitle="Meer informatie over gegevens Lake Analytics en I-SQL Azure-Portal interactieve zelfstudies met | Azure" 
   description="Snel aan de slag met het leren werken met gegevens Lake analyses en I-SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Azure gegevens Lake Analytics interactieve handleiding gebruiken

De Portal Azure biedt een interactieve zelfstudie kunt aan de slag met gegevens Lake Analytics. In dit artikel leest u hoe de zelfstudie voor het analyseren van logboeken aan de website doorlopen.


>[AZURE.NOTE]Als u leest u over de dezelfde zelfstudie gebruik van Visual Studio wilt, raadpleegt u [analyseren website Logboeken door middel van gegevens Lake analyses](data-lake-analytics-analyze-weblogs.md).
>Meer interactieve zelfstudies moet worden toegevoegd aan de portal.


Zie voor andere zelfstudies:

- [Aan de slag met gegevens Lake analyses met behulp van Azure-Portal](data-lake-analytics-get-started-portal.md)
- [Aan de slag met gegevens Lake Analytics via Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Aan de slag met gegevens Lake analyses met .NET SDK](data-lake-analytics-get-started-net-sdk.md)
- [I-SQL-scripts met Data Lake Tools voor Visual Studio ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md) 

**Vereisten voor**

Voordat u deze zelfstudie begint, hebt u het volgende:

- **A gegevens Lake Analytics-account**.  Zie [Aan de slag met Azure gegevens Lake analyses met behulp van Azure-Portal](data-lake-analytics-get-started-portal.md).

##<a name="create-data-lake-analytics-account"></a>Gegevens Lake Analytics-account maken 

Voordat u kunt alle taken uitvoeren, moet u een gegevens Lake Analytics-account hebben.

Elke gegevens Lake Analytics-account heeft een afhankelijkheid [Gegevensopslag voor Lake Azure](../data-lake-store/data-lake-store-overview.md) -account.  Dit account wordt verwezen als het standaardaccount voor gegevensopslag Lake.  Vooraf of wanneer u uw gegevens Lake Analytics-account maakt, kunt u het account voor gegevensopslag Lake maken. In deze zelfstudie maakt u het account voor gegevensopslag Lake met het Analytics-account

**Een gegevens Lake Analytics-account maken**

1. Aanmelden bij de [Portal van Azure](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute).
2. Klik op **Microsoft Azure** in de linkerbovenhoek de StartBoard openen.
3. Klik op de tegel **Marketplace** .  
3. Typ **Azure gegevens Lake Analytics** in het zoekvak op het blad **Alles** en druk op **ENTER**. Er wordt **Azure gegevens Lake Analytics** in de lijst.
4. Klik op **Azure gegevens Lake Analytics** uit de lijst.
5. Klik op **maken** vanaf de onderkant van het blad.
6. Typ of Selecteer de volgende opties:

    ![Azure gegevens Lake Analytics portal blade](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Naam**: Geef een naam het Analytics-account.
    - **Gegevensopslag Lake**: elke gegevens Lake Analytics-account heeft een afhankelijke Lake gegevensopslag-account. De gegevens Lake Analytics-account en de afhankelijke Lake gegevensopslag-account moeten zich bevinden in het dezelfde Azure Datacenter. Volg de aanwijzingen op het maken van een nieuw account voor gegevensopslag Lake of Selecteer een bestaande eigenschap.
    - **Abonnement**: Kies het Azure abonnement dat is gebruikt voor het Analytics-account.
    - **Resourcegroep**. Selecteer een bestaande groep van Azure Resource of een nieuw account te maken. Toepassingen zijn meestal bestaat uit veel onderdelen, bijvoorbeeld een web-app, database, database-server, opslag en 3e partijen services. Azure Resource Manager (ARM) kunt u werken met de resources in uw toepassing als een groep, een resourcegroep Azure genoemd. U kunt implementeren, bijwerken, controleren of alle bronnen voor uw toepassing in één, gecoördineerde bewerking verwijderen. Een sjabloon te gebruiken voor implementatie en die sjabloon voor verschillende omgevingen zoals testen, ontwikkel- en kunt werken. U kunt de facturering voor uw organisatie verduidelijken door de samengevouwen kosten voor de hele groep weer te geven. Zie [Azure resourcemanager overzicht](azure-resource-manager/resource-group-overview.md)voor meer informatie. 
    - **Locatie**. Selecteer een Azure Datacenter voor de gegevens Lake Analytics-account. 
7. Selecteer **vastmaken aan Startboard**. Dit is vereist voor deze zelfstudie te volgen.
8. Klik op **maken**. Dit gaat u naar de portal StartBoard. Een nieuwe tegel wordt toegevoegd aan de startpagina van Lotus met het label 'Implementeert Azure gegevens Lake Analytics' weergegeven. Duurt het even naar een gegevens Lake Analytics-account maken. Wanneer het account is gemaakt, wordt het account in een nieuwe blade geopend in de portal.

    ![Azure gegevens Lake Analytics portal blade](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>Interactieve zelfstudie Website logboekanalyse uitvoeren

**De Website Log Analytics interactieve handleiding openen**

1. In de Portal klikt u op **Microsoft Azure** in het linkermenu de StartBoard openen.
2. Klik op de tegel dat is gekoppeld aan uw gegevens Lake Analytics-account.
3. Klik op **verkennen interactieve zelfstudies** op de balk voor **Essentials** .

    ![Gegevens Lake Analytics interactieve zelfstudies](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Als u ziet een oranje waarschuwing mededeling "voorbeelden niet instellen, klikt u op...', klikt u op **Voorbeeldgegevens kopiëren** als u wilt de voorbeeldgegevens kopiëren naar het standaardaccount voor gegevensopslag Lake. De interactieve zelfstudie moet de gegevens die moeten worden uitgevoerd.
5. Klik op het blad **Interactieve zelfstudies** op **Website Log Analytics**. De zelfstudie de portal geopend in een nieuwe portal blade.
5. Klik op **1 Inleiding** en volg de instructies

##<a name="see-also"></a>Zie ook

- [Overzicht van Microsoft Azure-gegevens Lake Analytics](data-lake-analytics-overview.md)
- [Aan de slag met gegevens Lake analyses met behulp van Azure-Portal](data-lake-analytics-get-started-portal.md)
- [Aan de slag met gegevens Lake Analytics via Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [I-SQL-scripts met Data Lake Tools voor Visual Studio ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md)
- [Logboeken aan de Website door middel van Azure gegevens Lake analyses analyseren](data-lake-analytics-analyze-weblogs.md)
