<properties 
    pageTitle="Overzicht van de integratie accounts en de Enterprise-integratie Pack | App-Service van Microsoft Azure | Microsoft Azure" 
    description="Meer informatie over alle over integratie accounts, de Enterprise-integratie Pack en logica apps" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-integration-accounts"></a>Overzicht van de integratie-accounts

## <a name="what-is-an-integration-account"></a>Wat is een integratie-account?
Een account integratie is een Azure-account waarmee de integratie van de Enterprise-apps voor het beheren van onderdelen inclusief schema's, kaarten, certificaten, partners en overeenkomsten. Een integratie-app die u moet een integratie gebruikt voor toegang tot een schema, de map of het certificaat, bijvoorbeeld.

## <a name="create-an-integration-account"></a>Maak een account integratie 
1. Selecteer **Bladeren**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. **Integratie** invoeren in het zoekvak van het filter en selecteer **Integratie Accounts** in de lijst met resultaten     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. Knop *toevoegen* selecteren in het menu boven aan de pagina      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. Voer de **naam**, selecteer het **abonnement** dat u gebruiken wilt, een nieuwe **resourcegroep** maken of Selecteer een bestaande resourcegroep, selecteer een **locatie** waar uw integratie-account wordt beheerd, selecteert u een **laag prijzen**en selecteer vervolgens de knop **maken** .   

  Op dit moment het account van de integratie van gegevens in de locatie die u hebt geselecteerd. Dit moet worden voltooid binnen 1 minuut.    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. Vernieuw de pagina. Hier ziet u uw nieuwe integratie-account wordt vermeld. Gefeliciteerd!  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Hoe u een integratie-account koppelen aan een app logica
In de volgorde voor uw apps logica voor toegang tot kaarten, schema's, overeenkomsten en andere onderdelen die in uw account integratie bevinden zich, moet u eerst de integratie-account koppelen aan uw logica-app.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Hier volgen de stappen voor een integratie-account koppelen aan een app logica 

#### <a name="prerequisites"></a>Vereisten voor
- Een account integratie
- Een app logica

>[AZURE.NOTE]Zorg ervoor dat uw account van de integratie en logica-app in de **dezelfde Azure locatie** voordat u begint

1. Selecteer koppeling **Instellingen** in het menu van de logica-app  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. Selecteer het item **Integratie Account** in het blad instellingen  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. Selecteer het integratie-account dat u wilt koppelen aan uw app uit de **Selecteer een account van de integratie** van vervolgkeuzelijst logica  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. Sla uw werk  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. Hier ziet u een melding waarin wordt aangegeven dat uw account integratie is gekoppeld aan uw app logica en alle onderdelen in uw account integratie zijn nu beschikbaar voor uw app logica.  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

Nu dat uw account integratie is gekoppeld aan uw logica-app, kunt u gaat u naar uw app logica en B2B verbindingslijnen zoals de XML-validatie, plat bestand coderen/decoderen of transformeren gebruiken om apps te maken met B2B functies.  
    
## <a name="how-to-delete-an-integration-account"></a>Hoe een integratie-account verwijderen?
1. Selecteer **Bladeren**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integratie** invoeren in het zoekvak van het filter en selecteer **Integratie Accounts** in de lijst met resultaten     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Selecteer het **integratie-account** dat u wilt verwijderen  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Selecteer de koppeling **verwijderen** die in het menu bevindt zich   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. Bevestig uw keuze    

## <a name="how-to-move-an-integration-account"></a>Het verplaatsen van een integratie-account?
U kunt een account integratie eenvoudig verplaatsen naar een nieuw abonnement en een nieuwe resourcegroep. Volg deze stappen als u wilt verplaatsen van uw account integratie:

>[AZURE.IMPORTANT] U moet bijwerken van alle scripts voor de nieuwe resource-id's gebruiken nadat u een account integratie verplaatsen.

1. Selecteer **Bladeren**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integratie** invoeren in het zoekvak van het filter en selecteer **Integratie Accounts** in de lijst met resultaten     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Selecteer het **integratie-account** dat u wilt verwijderen  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Selecteer de koppeling voor **verplaatsen** die in het menu bevindt zich   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. Bevestig uw keuze    

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over overeenkomsten] (./app-service-logic-enterprise-integration-agreements.md "Algemene informatie over de enterprise-integratie overeenkomsten")  


 