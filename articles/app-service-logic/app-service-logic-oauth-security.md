<properties
    pageTitle="OAUTH-beveiliging in SaaS verbindingslijnen en API Apps | Azure"
    description="Lees meer over het OAUTH-beveiliging in de verbindingslijnen en API-Apps in Azure App Service; microservices architectuur; SaaS"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>Meer informatie over OAUTH-beveiliging in SaaS verbindingslijnen

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie.

Veel van de Software als een Service (SaaS) verbindingslijnen zoals Facebook, Twitter, DropBox en dergelijke vereisen dat gebruikers om te verifiëren met het OAUTH-protocol.  Wanneer u deze verbindingslijnen SaaS van logica Apps gebruikt, bieden we een eenvoudigere gebruikerservaring waar u 'Autoriseren' op in de ontwerpfunctie voor logica Apps. Wanneer u **autoriseren**, wordt u gevraagd te melden in (als dat niet al) en geef toestemming verbinding maken met de service SaaS namens u. Nadat u toestemming geven en Autoriseer, hebben uw Apps logica vervolgens toegang tot deze SaaS-services.

## <a name="create-your-own-saas-app"></a>Uw eigen SaaS-app maken
Deze eenvoudigere functionaliteit is mogelijk omdat we eerder hebt gemaakt en de toepassing van deze services SaaS geregistreerd.  In sommige gevallen wilt u mogelijk registreren en u kunt uw eigen programma.  Dit is bijvoorbeeld nodig wanneer u wilt deze SaaS-connectors in uw aangepaste toepassingen gebruiken. In dit voorbeeld wordt de verbindingslijn DropBox, maar het is de procedure voor alle verbindingslijnen die afhankelijk van OAUTH zijn.

Zelfs in de context van logica Apps, kunt u uw eigen toepassing in plaats van de standaardtoepassing die wij bieden. Als de knop "Autoriseren" geen verbinding maakt, kunt u proberen uw eigen-app maken. Hieronder vindt u deze stappen voor het Twitter-connector:

1. Open uw Twitter-connector in de portal Azure preview. Ga naar het **Bladeren** > **API-Apps**. Selecteer de verbindingslijn Twitter:  
    ![][1]

2. Selecteer **Instellingen** > **verificatie**:  
    ![][2]

3. Kopieer de waarde **URI omleiden** .  
    ![][3]

4. Ga naar [Twitter-](http://apps.twitter.com) en **een nieuwe App maakt**. Plak de **URI omleiden** waarde hebt gekopieerd uit de verbindingslijn Twitter in de eigenschap **Terugbellen URL** : ![][4]  
5. Wanneer uw Twitter-app is gemaakt, selecteert u **sleutel en Access Tokens**. Kopieer de volgende waarden.
6. Plak deze waarden in de eigenschappen van **Client-ID** en **Geheim Client** in uw Twitter verbindingslijn verificatie-instellingen:   
    ![][5]  
7. Connectorinstellingen opslaan.  

Nu, moet u gebruikmaken van de verbindingslijn van logica Apps. Wanneer u deze connector van logica Apps gebruikt, wordt uw toepassing gebruikt in plaats van de standaardtoepassing.  

> [AZURE.NOTE] Als u een app eerder hebt gemachtigd, moet u mogelijk de app autoriseren.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
