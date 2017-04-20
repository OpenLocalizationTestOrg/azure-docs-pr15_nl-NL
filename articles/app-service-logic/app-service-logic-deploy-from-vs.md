<properties 
    pageTitle="Samenstellen logica Apps in Visual Studio | Microsoft Azure" 
    description="Een project maken in Visual Studio maken en implementeren van uw app logica." 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Bouwen en implementeren van logica Apps in Visual Studio

Hoewel de [Portal van Azure](https://portal.azure.com/) u een uitstekende manier biedt om ontwerpen en beheren van uw apps logica, kunt u ook ontwerpen en implementeren van uw app logica in Visual Studio in plaats daarvan.  Logica Apps wordt geleverd met een uitgebreide Visual Studio set hulpmiddelen waarmee u om een logica-app met de designer te bouwen, configureren implementatie en automatisering sjablonen en implementeren in een omgeving.  

## <a name="installation-steps"></a>Installatiestappen

Hieronder vindt u de stappen voor het installeren en configureren van de Visual Studio tools voor logica-Apps.

### <a name="prerequisites"></a>Vereisten voor

- [Visual Studio-2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Meest recente Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 of hoger)
- [Azure PowerShell] (https://github.com/Azure/azure-powershell#installation)
- Toegang tot het web bij gebruik van de ingesloten designer

### <a name="install-visual-studio-tools-for-logic-apps"></a>Visual Studio tools voor logica Apps installeren

Zodra u de vereisten die is geïnstalleerd, heb 

1. Visual Studio-2015 Open naar het menu **Extra** en selecteer **extensies en Updates**
1. Selecteer de categorie **Online** online zoeken
1. Zoeken naar **Logica Apps** om weer te geven van de **Hulpmiddelen voor Apps van Azure-logica voor Visual Studio**
1. Klik op de knop **downloaden** om te downloaden en installeren van de extensie
1. Visual Studio na installatie opnieuw opstarten

> [AZURE.NOTE] U kunt ook de extensie downloaden rechtstreeks vanuit [deze koppeling](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9)

Eenmaal geïnstalleerde is mogelijk de resourcegroep Azure-project met de logica App Designer gebruiken.

## <a name="create-a-project"></a>Een project maken

1. Ga naar het menu **bestand** en selecteer **Nieuw** >  **Project** (of, kunt u Ga naar **toevoegen** en selecteer **Nieuw project** toe te voegen aan een bestaande oplossing):  ![menu bestand](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. Klik in het dialoogvenster **Cloud**Zoek en selecteer vervolgens **De resourcegroep Azure**. Typ een **naam** en klik vervolgens op **OK**.
    ![Nieuw project toevoegen](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. Selecteer de **logica app** -sjabloon. Hiermee maakt u een sjabloon voor een lege logica app implementatie beginnen.
    ![Selecteer Azure-sjabloon](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Zodra u uw **sjabloon**hebt geselecteerd, klik op **OK**.

    Uw project logica-app is nu toegevoegd aan uw oplossing. Hier ziet u de implementatie-bestand in de Verkenner oplossing:  

    ![Implementatie](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>De ontwerpfunctie voor logica-App gebruiken

Nadat u een resourcegroep Azure-project met een logica-app hebt, kunt u de ontwerpfunctie voor Visual Studio om u te helpen bij het maken van de werkstroom kunt openen.  De ontwerpfunctie is een internetverbinding vereist query's op de verbindingslijnen voor beschikbare eigenschappen en gegevens (bijvoorbeeld als de connector voor Dynamics CRM Online gebruikt, de ontwerpfunctie vraagt uw exemplaar CRM om een lijst met beschikbare aangepast en standaardeigenschappen).

1. Met de rechtermuisknop op de `<template>.json` bestand en selecteer **openen met logica App Designer** (of `Ctrl+L`)
1. Kies het abonnement, resourcegroep en locatie voor de implementatiesjabloon
    - Het is belangrijk te weten dat het ontwerpen van een app logica **API verbinding** resources om query's voor eigenschappen tijdens het ontwerp zal maken.  De resourcegroep geselecteerd is de resourcegroep gebruikt om te maken van deze verbindingen tijdens de ontwerpfase.  U kunt bekijken of wijzigen van alle verbindingen API door te gaan naar de Azure-Portal en bladeren naar **API verbindingen**.
    ![Abonnement kiezer](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. De ontwerpfunctie moet worden weergegeven op basis van de definitie in de `<template>.json` bestand.
1. U kunt nu maken en ontwerpen van uw app logica en wijzigingen zijn bijgewerkt in de sjabloon-implementatie.
    ![Designer in Visual Studio](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

U ziet ook `Microsoft.Web/connections` resources die worden toegevoegd aan uw bestand resource voor alle verbindingen die u nodig hebt voor de app logica-functie.  De eigenschappen van deze verbinding kunnen worden ingesteld wanneer u implementeert, en worden beheerd nadat u in **API-verbindingen** in de Portal Azure implementeren.

### <a name="switching-to-the-json-code-view"></a>Overschakelen naar de JSON-codeweergave

U kunt het tabblad **Codeweergave** vanaf de onderkant van de ontwerpfunctie wilt overschakelen naar de JSON-weergave van de app logica selecteren.  Als u wilt overschakelen naar de volledige resource JSON door met de rechtermuisknop op de `<template>.json` bestand en selecteer **openen**.

### <a name="saving-the-logic-app"></a>De logica-app opslaan

U kunt de logica-app in via de knop **Opslaan** op elk gewenst moment opslaan of `Ctrl+S`.  Als er fouten logica-App op het moment dat u opslaat, wordt deze weergegeven in het venster **uitvoer** van Visual Studio.

## <a name="deploying-your-logic-app"></a>Uw app logica implementeren

Tot slot nadat u uw app hebt geconfigureerd, kunt u implementeren rechtstreeks vanuit de Visual Studio in slechts een paar stappen. 

1. Met de rechtermuisknop op het project in de Solution Explorer en Ga naar **Deploy** > **Nieuwe implementatie...** 
     ![Nieuwe implementatie](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Aanmelden bij uw Azure abonnementen wordt u gevraagd. 

3. U moet nu de details van de resourcegroep die u wilt implementeren van de app logica om te kiezen. 
    ![Implementeren naar resourcegroep](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Zorg ervoor dat u selecteert u de juiste sjabloon en parameters bestanden voor de resourcegroep (bijvoorbeeld als u naar een productieomgeving implementeert wilt u het bestand van de parameters productie kiezen). 
4. Selecteer de knop distribueren
 
    
6. De status van de implementatie wordt weergegeven in **het uitvoervenster (mogelijk moet u Kies **Azure inrichten**.** 
    ![Uitvoer](./media/app-service-logic-deploy-from-vs/output.png)

U kunt in de toekomst herzien uw logica-app in een besturingselement voor gegevensbronnen en Visual Studio gebruiken om te implementeren van nieuwe versies. 

> [AZURE.NOTE] Als u de definitie in de Portal Azure rechtstreeks wijzigt, wordt de volgende keer dat u wilt in Visual Studio deze wijzigingen implementeren overschreven.

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen met logica Apps, volgt u de zelfstudie [een logica-App maken](app-service-logic-create-a-logic-app.md) .  
- [Algemene voorbeelden en scenario's weergeven](app-service-logic-examples-and-scenarios.md)
- [U kunt bedrijfsprocessen automatiseren met logica Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Meer informatie over het integreren van uw systemen met logica Apps](http://channel9.msdn.com/Events/Build/2016/P462)