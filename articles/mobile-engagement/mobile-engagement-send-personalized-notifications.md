<properties 
    pageTitle="Gepersonaliseerde melding met Azure Mobile betrokkenheid verzenden" 
    description="Het verzenden van persoonlijke meldingen door gebruikersprofielinformatie op te nemen in de meldingen zoals hun namen"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Meldingen aanpassen met gebruikersnaam in te voeren

In uw voortdurende meldingen meer aantrekkelijker te maken voor uw app-gebruikers, moet u overwegen personaliseren van deze. Eén krachtige manier omvat van het gebruik van de namen van de gebruikers app selectief naar het adres van de meldingen zodat u ze meer persoonlijke. Waarschuwing hier - u moet benadert gebruikersnamen toe te voegen aan de meldingen zorgvuldig omdat als u deze strategie te veel gebruikmaakt vervolgens deze als griezelig voor sommige app-gebruikers tegenkomt kan. U moet er ook voor zorgen dat de gebruiker deelnemen aan de volgende persoonlijke informatie aan u verstrekken met volledige toestemming in de mobiele app voordat u begint met het gebruik deze opdracht laat. 

Technisch gesproken met Azure Mobile Resourceafspraak, kunt u uitvoeren voor het personaliseren van de meldingen die door de onderstaande stappen waarin we het scenario gebruikersnaam in te voeren op te nemen in de meldingen gebruiken. Gebruikt u het concept van het App-Info of labels waarvan de waarden op door de SDK's kunnen worden doorgegeven geïntegreerd in de Mobile-App of via API's. Deze App-Infos of labels kunnen vervolgens worden gebruikt:

1. voor doelgroepen meldingen aan specifieke gebruikers op basis van de waarden van de App-informatie of 
2. Als tijdelijke aanduidingen in de meldingen die wordt vervangen door de waarden die specifiek zijn voor het apparaat/gebruiker tijdens het verzenden van meldingen naar dat apparaat. 

> [AZURE.IMPORTANT] Houd er rekening mee dat de snelheid van het verzenden van meldingen te krijgen een wilt verkleinen vanwege deze aanvullende verwerking zien van het app-info waarden vervangen door elke meldingen. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>Register-App-Info in de Portal voor mobiel betrokkenheid

1) U begint met het registreren van App Info of labels in de portal van Azure. Ga naar **Instellingen** -> **Tag (App-Info)** voor deze.  

![][1]  

2) Klik op **Nieuw label (app-info)** en geef de naam als *gebruikersnaam* en het type als *tekenreeks* en klik op **verzenden**. 

![][2]

3) Ten slotte ziet u deze app-info geregistreerd als volgt uit:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>App-Info verzenden vanuit de client SDK

Hier wordt het Windows universele app voorbeeld gebruikt maar gelijkwaardige methoden voor onze andere SDK's ook. 

Als u een methode in de mobiele app, waar u de profielgegevens van de gebruiker zoals hun namen waarschijnlijk krijgen na het verifiëren van deze hebt, wordt u belt `SendAppInfo` methode hier en vullen de waarde van de `user_name` app info dat u eerder hebt geregistreerd in de Mobile-betrokkenheid service-end. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Persoonlijke meldingen verzenden

U bent nu alle instellen om meldingen met deze **gebruikersnaam**te verzenden. 

1) Ga naar mobiele betrokkenheid-Portal op het tabblad **hebt bereikt** maken van een melding en kunt u deze tijdelijke aanduiding in de volgende indeling ergens in de titel van de melding of de hoofdtekst. 

![][4]  

> [AZURE.NOTE] Alle gebruikers waarvoor de informatie van de app gebruikersnaam niet is ingesteld, wordt niet krijgt een melding. Als u de melding campagne uitgevoerd in de testmodus en als u nog geen app-info ingesteld we sturen '?' teken voor het vervangen van de tijdelijke aanduiding. 

2) Mobile betrokkenheid wordt wanneer een apparaat deze melding verzenden en vervolgens deze deze app-info bekijken en vervang de waarde in de tijdelijke aanduiding selecteren.  
Stel dat we hebben ingesteld `str = "Scott"` voor een gebruiker dan het apparaat registratie krijgt gekoppeld aan de gegevens van de app van **gebruikersnaam SCOTT =** voor deze gebruiker en deze gebruiker een automatisch niet app push-bericht in de volgende indeling zien. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

