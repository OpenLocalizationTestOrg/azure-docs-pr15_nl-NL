<properties 
    pageTitle="Verificatie met mobiele betrokkenheid REST API - handmatig instellen"
    description="Wordt beschreven hoe u de verificatie voor Mobile betrokkenheid REST API's handmatig instellen" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Verificatie met mobiele betrokkenheid REST API - handmatig instellen

Dit is een bijlage documentatie verificatie [bij Mobile betrokkenheid REST API's](mobile-engagement-api-authentication.md). Zorg ervoor dat u eerst om te zien van de context te lezen. Hiermee wordt een andere methode voor het eenmalige instellen voor het instellen van uw authenticatie voor Mobile betrokkenheid REST API's met behulp van de Portal Azure beschreven. 

>[AZURE.NOTE] De onderstaande instructies zijn op basis van deze [Active Directory-handleiding](../resource-group-create-service-principal-portal.md) en aangepast voor wat vereist voor verificatie voor Mobile betrokkenheid-API's is. Dus raadpleegt u deze als u wilt begrijpen van de onderstaande stappen uitgebreid beschreven. 

1. Meld u aan uw Azure-Account via de [klassieke portal](https://manage.windowsazure.com/).

2. Selecteer **Active Directory** in het linkerdeelvenster.

     ![Selecteer Active Directory][1]

3. Kies de **Active Directory standaard** in uw Azure-portal. 

     ![Kies map][2]

    >[AZURE.IMPORTANT] Deze methode werkt alleen als u in de standaard Active Directory van uw account werkt en niet werkt als u dit doen in een Active Directory die u hebt gemaakt in uw account. 

4. Als u wilt de toepassingen weergeven in uw adreslijst, klik op **toepassingen**.

     ![weergave-toepassingen][3]

5. Klik op **toevoegen**. 

     ![toepassing toevoegen][4]

6. Klik op de **toepassing het ontwikkelen van mijn organisatie toevoegen**

     ![nieuwe toepassing][5]

6. Vul de naam van de toepassing en selecteer het type van toepassing als **WEB APPLICATION en/of WEB API** en klikt u op de knop Volgende.

     ![de naam van toepassing][6]

7. U kunt een pop-URL's bieden voor **Aanmelding op URL** en **APP-ID-URI**. Ze niet worden gebruikt voor ons scenario en de URL's zelf niet worden gevalideerd.  

     ![Eigenschappen van een servicetoepassing][7]

8. Aan het einde van dit hebt u een AAD-app waarin de naam die u eerder hebt opgegeven als volgt te werk. Dit is uw **AD\_APP\_naam** en noteer deze.  

     ![App-naam][8]

9. Klik op de naam van de app en klik op **configureren**.

     ![app configureren][9]

10. Maak een notitie van de CLIENT-ID die wordt gebruikt als **CLIENT\_ID** oproepen voor uw API. 

     ![app configureren][10]

11. Schuif omlaag naar de sectie **toetsen** en voeg een sleutel met bij voorkeur 2 jaar (verstrijken) duur toe en klik op **Opslaan**. 

     ![app configureren][11]


12. De waarde die wordt weergegeven voor de sleutel zoals deze nu alleen weergegeven wordt en niet opgeslagen wordt zodat niet meer ooit weergegeven wordt direct kopiëren. Als u deze kwijtraakt hebt u een nieuwe sleutel genereren. Dit is de **CLIENT_SECRET** voor uw API-oproepen. 

     ![app configureren][12]

    >[AZURE.IMPORTANT] Deze toets verloopt aan het einde van de duur die u hebt opgegeven zodat Zorg ervoor dat u verlengen wanneer dat nodig is anders uw authenticatie API wordt niet meer werkt. U kunt ook verwijderen en opnieuw maken van deze toets als u denkt dat deze kent.
 
13. Klik op de knop **Weergave EINDPUNTEN** nu die wordt geopend in het dialoogvenster **App-eindpunten** . 

    ![][13]

14. Kopieer het **OAUTH 2.0 TOKEN EINDPUNT**in het dialoogvenster App-eindpunten. 

    ![][14]

15. Dit eindpunt niet in de volgende notatie waarbij de GUID in de URL uw **TENANT_ID** Controleer dus een notitie ervan: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Nu we wordt gaat u verder met de machtigingen configureren op deze app. U moet hiervoor openen van de [Azure-portal](https://portal.azure.com). 

17. Klik op **Resourcegroepen** en de resourcegroep **Mobile betrokkenheid** zoeken.  

    ![][15]

18. Klik op de resourcegroep **Mobile betrokkenheid** en Ga naar het blad **Instellingen** hier. 

    ![][16]

19. Klik op **gebruikers** in het blad instellingen en klik op **toevoegen** aan een gebruiker toevoegen. 

    ![][17]

20. Klik op **een rol selecteren**

    ![][18]

21. Klik op **eigenaar**

    ![][19]

22. Zoeken naar de naam van uw toepassing **AD\_APP\_naam** in het vak Zoeken. Worden er geen dit al dan niet standaard hier. Als u deze hebt gevonden, selecteert u deze en klik op het **selecteert u** onder aan het blad. 

    ![][20]

23. Klik op het blad **Access toevoegen** , wordt dit weergegeven als **1 gebruiker, 0 groepen**. Klik op **OK** op deze blade de wijziging te bevestigen. 

    ![][21]

U hebt nu de vereiste AAD-configuratie voltooid en u zijn al ingesteld om te bellen van de API's. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



