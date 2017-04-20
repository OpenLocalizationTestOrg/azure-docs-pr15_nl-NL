<properties
   pageTitle="Oplossingen in bewerkingen Management-Suite (OMS) | Microsoft Azure"
   description="De functionaliteit van bewerkingen Management Suite Kantoorbeheersysteem uitbreiden oplossingen doordat verpakt management-scenario's die klanten aan hun OMS-werkruimte toevoegen kunnen.  In dit artikel bevat informatie over hoe aangepaste oplossingen die zijn gemaakt door klanten en partners."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Oplossingen in bewerkingen Management Suite Kantoorbeheersysteem (Preview)

>[AZURE.NOTE]Dit is voorlopige documentatie voor managementoplossingen in OMS die momenteel beschikbaar in de Preview-versie zijn.    

De functionaliteit van bewerkingen Management Suite Kantoorbeheersysteem uitbreiden managementoplossingen doordat verpakt management-scenario's die klanten aan hun omgeving toevoegen kunnen.  Behalve [oplossingen geleverd door Microsoft](../log-analytics/log-analytics-add-solutions.md), partners en klanten oplossingen moet worden gebruikt in hun eigen omgeving kunnen maken of via de community aan klanten beschikbaar worden gemaakt.

## <a name="finding-and-installing-management-solutions"></a>Zoeken en installeren van oplossingen
Er zijn meerdere methoden voor het vinden en te installeren managementoplossingen, zoals in de volgende secties wordt beschreven.

### <a name="azure-marketplace"></a>Azure Marketplace
Oplossingen geleverd door Microsoft en vertrouwde partners van Azure Marketplace in de portal van Azure kunnen zijn geïnstalleerd.

1. Meld u aan bij de Azure-portal.
2. Selecteer in het linkerdeelvenster **meer services**.
3. Schuif omlaag naar **oplossingen** of *oplossingen* Typ in het dialoogvenster **Filter** .
4. Klik op de knop **+ toevoegen** .
5. Zoeken naar oplossingen waarin u geïnteresseerd door te bladeren bent, te klikken op de knop **Filter** of te typen in het vak **Zoeken Everthing** .
6. Klik op een item marketplace om gedetailleerde informatie weer te geven.
4. Klik op **maken** om het deelvenster **Oplossing toevoegen** .
5. U wordt gevraagd naar de vereiste gegevens, zoals [OMS workspace en automatisering account](#oms-workspace-and-automation-account) naast waarden voor parameters in de oplossing.
6. Klik op **maken** om te kunnen installeren van de oplossing.

### <a name="oms-portal"></a>OMS-Portal
Oplossingen geleverd door Microsoft kunnen vanuit de galerie met oplossingen in de portal OMS zijn geïnstalleerd.

1. Meld u aan bij de portal OMS.
2. Klik op de tegel van de **Galerie met oplossingen** .
2. Klik op de pagina Galerie met oplossingen OMS meer informatie over elke oplossing beschikbaar. Klik op de naam van de oplossing aan die u wilt toevoegen aan OMS.
3. Klik op de pagina voor de oplossing aan die u hebt gekozen wordt gedetailleerde informatie over de oplossing weergegeven. Klik op **toevoegen**.
4. Een nieuwe titel voor de oplossing die u hebt toegevoegd wordt weergegeven op de pagina in OMS en u kunt gaan gebruiken nadat u uw gegevens worden verwerkt door de service OMS overzicht.

### <a name="azure-quickstart-templates"></a>Azure Quickstart sjablonen
Leden van de community kunnen oplossingen voor Azure Quickstart sjablonen indienen.  U kunt deze sjablonen voor later installatie downloaden of controleren ze voor meer informatie over het [maken van uw eigen oplossingen](#creating-a-solution).

1. De stappen die worden beschreven in [OMS workspace en automatisering account](#oms-workspace-and-automation-account) naar een werkruimte en een account koppelen.
2. Ga naar [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates/).  
3. Zoek naar een oplossing waarin u geïnteresseerd bent.
4. Selecteer de oplossing in de resultaten om de berichtdetails te bekijken.
5. Klik op de knop **distribueren naar Azure** .
6. U wordt gevraagd om te leveren van gegevens, zoals de resourcegroep en de locatie naast waarden voor parameters in de oplossing.
7. Klik op **aanschaffen** om te kunnen installeren van de oplossing.

### <a name="deploy-azure-resource-manager-template"></a>Azure resourcemanager sjabloon implementeren
Oplossingen dat u van de community krijgen of als een sjabloon resourcemanager die u [maakt zelf](#creating-a-solution) worden geïmplementeerd en kunt u een van de standaard methoden voor het [implementeren van een sjabloon](../resource-group-template-deploy-portal.md).  Houd er rekening mee dat voordat u de oplossing installeert, moet u maken en de [OMS workspace en automatisering account](#oms-workspace-and-automation-account)koppelen.

## <a name="oms-workspace-and-automation-account"></a>OMS werkruimte en automatisering-account
Oplossingen voor de meeste vragen om een [OMS werkruimte](../log-analytics/log-analytics-manage-access.md) naar weergaven bevatten en een [account voor automatisering](../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante resources bevat. De werkruimte en de account moeten voldoen aan de volgende vereisten.

- Een oplossing kunt slechts één OMS workspace en één automatisering-account.  
- De werkruimte OMS en automatisering-account gebruikt door een oplossing moeten worden gekoppeld aan elkaar. Een werkruimte OMS kan alleen worden gekoppeld aan één automatisering-account en een account automatisering kan alleen worden gekoppeld aan één OMS-werkruimte.
- De werkruimte OMS en automatisering account om u te worden gekoppeld, moeten zich in dezelfde resourcegroep en regio.  Geldt niet voor een werkruimte OMS in Oost-Amerikaanse regio en en automatisering-account in Oost-VS 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Een koppeling tussen een OMS werkruimte en automatisering-account maken
Hoe geeft u de werkruimte OMS en automatisering account is afhankelijk van de installatiemethode voor uw oplossing.

- Wanneer u een Microsoft-oplossing via de portal OMS installeert, wordt dit is geïnstalleerd in de huidige OMS-werkruimte en is geen automatisering-account vereist.

- Wanneer u een oplossing tot en met de Azure Marketplace installeert, u wordt gevraagd om een OMS werkruimte en automatisering-account en de koppeling tussen deze voor u wordt gemaakt.  

- Voor oplossingen buiten de Azure Marketplace, moet u de OMS-werkruimte en automatisering account koppelen voordat u de oplossing installeert.  U kunt dit doen door te selecteren van een oplossing in de Azure Marketplace en de OMS-werkruimte en automatisering-account.  U hoeft niet te daadwerkelijk de oplossing worden geïnstalleerd omdat de koppeling wordt gemaakt zodra de OMS werkruimte en automatisering account zijn ingeschakeld.  Nadat de koppeling is gemaakt, kunt u gebruiken die OMS werkruimte en automatisering account voor elke oplossing. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>De koppeling tussen een OMS werkruimte en automatisering-account bevestigen
U kunt controleren of de koppeling tussen een werkruimte OMS en een automatisering-account met de volgende procedure.

1. Selecteer het account automatisering in de portal van Azure.
2. Blader naar de onderkant van het deelvenster **Instellingen** .
3. Als er een sectie **OMS-bronnen** in het deelvenster **Instellingen** , is klikt u vervolgens dit account gekoppeld aan een werkruimte OMS.
4. Selecteer **werkruimte** zodat u kunt de details van de werkruimte OMS is gekoppeld aan dit account automatisering bekijken.


## <a name="listing-management-solutions"></a>Vermelding van oplossingen
Gebruik de volgende procedure om de managementoplossingen weergeven in de werkruimten die is gekoppeld aan uw Azure-abonnement.

1. Meld u aan bij de Azure-portal.
2. Selecteer in het linkerdeelvenster **meer services**.
3. Schuif omlaag naar **oplossingen** of *oplossingen* Typ in het dialoogvenster **Filter** .
4. Oplossingen is geïnstalleerd in al uw werkruimten worden, vermeld.

Houd er rekening mee dat u alleen de Microsoft-oplossingen die zijn geïnstalleerd in de huidige werkruimte met behulp van de portal OMS kunt weergeven.

## <a name="removing-a-management-solution"></a>Een beheeroplossing verwijderen
Wanneer een beheeroplossing wordt verwijderd, worden alle bronnen in de oplossing worden ook verwijderd.  

1. Zoek de oplossing in de Azure-portal met de procedure in de [vermelding van oplossingen](#listing-solutions).
2. Selecteer de oplossing die u wilt verwijderen.
3. Klik op de knop **verwijderen** .

## <a name="creating-a-management-solution"></a>Een beheeroplossing maken
Voltooid richtlijnen voor het maken van oplossingen zijn beschikbaar bij [solutions maken in bewerkingen Management Suite Kantoorbeheersysteem](operations-management-suite-solutions-creating.md). 


## <a name="next-steps"></a>Volgende stappen

- Zoeken [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates) voor voorbeelden van verschillende resourcemanager sjablonen.
- Maak uw eigen [oplossingen](operations-management-suite-solutions-creating.md).
 